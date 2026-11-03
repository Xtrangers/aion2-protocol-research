# Rapport technique : `lunixxx37/aion2-protocol-research`

**Source** : https://github.com/lunixxx37/aion2-protocol-research
**License** : MIT (code original, utilisable chez nous)
**Snapshot** : 10/10/2026, Global Windows client révisions `3526` et `3527`
**Clone local** : `C:\IA\Game\aion2-protocol-research\` (872 Ko, 20+ commits)

Ce dépôt résout **la totalité du problème C2S** : framing, handshake RSA, chiffrement RC4, découverte dynamique de la clé, catalogage des opcodes, parsing des actions joueur.

---

## 1. État des connaissances dans le repo

| Domaine | Statut |
|---|---|
| Réassemblage TCP | **compris** |
| Framing varint externe non signé | **compris** |
| Bundles LZ4 | **compris** |
| Redirect lobby → world | **compris** |
| Opcodes et payloads S2C | partiellement compris |
| Handshake RSA C2S | **entièrement reconstruit** |
| Reconstruction clé privée RSA | **reproductible via mémoire read-only** |
| Padding RSA + plaintext handshake | **OAEP-SHA1, exactement 214 bytes** |
| Chiffrement C2S symétrique | **RC4 standard, entièrement reconstruit** |
| Décryptage PCAP C2S offline | **reproduit sur 6 sessions indépendantes** |
| Premier paquet chiffré `13 36` | layout byte-par-byte reconstruit, handoff `authn_token` tracé |
| Sémantique opcodes C2S | visible ; la plupart des payloads en cours de mapping |

---

## 2. La découverte centrale : AES-GCM, c'était faux. C'est RC4.

**Nos notes session 29/09 disaient AES-128-GCM.** C'était faux. Le dépôt prouve :

- **Pas d'AES-GCM, pas de ChaCha, pas de PRF custom.**
- **RC4 standard** sans IV, sans drop, sans KDF, sans reset per-frame.
- **La clé RC4 = les 214 bytes exacts** de la sortie RSA-OAEP-SHA1 du handshake serveur. Pas de slicing, pas de dérivation, pas de hash. Les 214 bytes bruts sont directement passés au KSA de RC4.
- **RC4 est continu** sur tous les corps C2S de la connexion : le state ne reset jamais entre frames.
- **Les varints clairs externes (longueur) ne consomment PAS le keystream.** Seuls les body bytes avancent le PRGA.

Validé sur **172 178 frames + 1 950 077 bytes RC4 corps** sur 6 sessions indépendamment chiffrées. Les disassembles statiques du KSA/PRGA du client et les décryptages PCAP offline concordent **byte à byte**.

---

## 3. Framing (couche extérieure)

```
uvarint(encoded_length) || frame_body

body_length         = encoded_length - 4
physical_frame_size = varint_width + encoded_length - 4
```

- **uvarint = LEB128 non signé**, encodage minimal obligatoire.
- Le décalage de 4 est **logique**, pas un header de 4 bytes.
- Les décodeurs skippent le padding zéro entre frames.
- **TCP boundaries ≠ frame boundaries** : il faut gérer retransmissions, overlaps, fragments, multi-frames par segment, et gaps de capture.

**Exemple 1 byte** :
```
06 03 36      // encoded_length=6, physical=3, body = "03 36"
```

**Exemple 2 bytes** :
```
9F 02 ...     // encoded_length=287, physical=285, body_length=283
```

Port serveur jeu observé : **13328** (mais dynamique via redirect lobby `0F 39`).

---

## 4. Handshake world (3 étapes clés)

### Étape 1 : `10 36` — Client → Serveur (clair)

Le client génère une **nouvelle paire RSA-2048 à chaque connexion world** avec **exposant public = 3** (!). Envoie la clé publique sérialisée en PKCS#1 `RSAPublicKey` DER (268 bytes pour une 2048 bits).

```
outer_length:uvarint
10 36                         // opcode
rsa_der_length:uvarint        // = 268
rsa_public_key:PKCS1 DER      // SEQUENCE { modulus 257 bytes, e=3 }
client_revision:u32le         // ex: 3527
04 06 05 02                   // constante fixe
region_ascii[2]               // ex: "DE", "EU", "KR"
02                            // trailing byte
```

Dans le corpus de 171 handshakes capturés : **171 modules RSA distincts, aucun partage de facteur premier**. Pas de key reuse, pas de Håstad broadcast attaquable (167 fenêtres testées).

### Étape 2 : `11 36` — Serveur → Client (clair)

```
outer_length:uvarint
11 36
status:u16le                  // 0 observé
timeout_ms:u32le              // 60000 observé
80 02                         // uvarint(256)
rsa_ciphertext[256]           // bloc RSA chiffré
reserved:u64le                // 0 observé
sentinel_or_result:i32le      // -7 observé
```

### Étape 3 : décryptage RSA-OAEP-SHA1

```
ciphertext byte order = big endian
padding               = OAEP-SHA1 / MGF1-SHA1
label                 = empty
decoded message length = 214 bytes
```

Pour RSA-2048 avec OAEP-SHA1, 214 bytes est le maximum : `256 - 2*20 - 2`. Validé manuellement : leading zero + `SHA1("")` + MGF1 unmasking + zero padding + séparateur `01`.

**Les 214 bytes complets sont la clé RC4**, sans transformation.

---

## 5. L'algorithme C2S post-handshake

```
C2S: clear_uvarint || RC4(body)
S2C: clear_uvarint || opcode || payload
```

### KSA (initialisation du state RC4)

```python
key = OAEP_SHA1_RSA_DECRYPT(server_block)  # exactement 214 bytes
S = [0, 1, ..., 255]
j = 0
for i in 0..255:
    j = (j + S[i] + key[i mod 214]) mod 256
    swap(S[i], S[j])
```

### PRGA (keystream pour chaque body)

```python
i = 0
j = 0
# Pour CHAQUE body byte, en continu entre frames :
for b in c2s_body_stream:
    i = (i + 1) mod 256
    j = (j + S[i]) mod 256
    swap(S[i], S[j])
    output = b XOR S[(S[i] + S[j]) mod 256]
```

### Objet runtime dans la RAM du client

Dans le process AION2.exe, chaque état RC4 est stocké ainsi :

```
+0x00  vtable
+0x08  status/flags
+0x10  key_buffer pointer
+0x18  key_length = 214
+0x1C  RC4 index i
+0x20  RC4 index j
+0x24  S[256]  (permutation)
```

Le client construit **2 objets RC4 identiques** depuis la même sortie OAEP (A pour décoder le body entrant, B pour le restaurer après dispatch). D'où 2 states synchronisés visibles en RAM.

### Chaîne d'ownership runtime

```
network_owner + 0xE10 -> RSA handler
network_owner + 0xE20 -> RC4 state A
network_owner + 0xE30 -> RC4 state B
```

Localisation : via la string plaintext `g.network.HandlerRecvBuffer` dans l'image mappée.

---

## 6. Récupération rapide de la clé RC4 live

C'est **le gros gain pratique**. Au lieu de scanner 17-22 Go de mémoire virtuelle :

- `locate_session_key.py` : dérive la vtable RC4 d'une signature de constructeur, suit la vtable adjacente du network owner pour trouver l'allocation courante, vérifie les 2 states RC4 (permutations 256 bytes valides + même clé 214 bytes).
- **269-297 ms interne (280 ms moyenne)**, 348-385 ms wall-clock avec startup Python.
- Pas d'injection, pas de pause du process, read-only via `VirtualQueryEx` + `ReadProcessMemory`.

- `watch_session_key.py` : monitor continu. Première découverte en 285 ms. Polls suivants cached : **0.40-0.76 ms**. Détecte reconnect/restart automatiquement.
- Fallback full-memory : 2-7 secondes dans les gros cas, après prioritisation des régions 64 KiB de l'allocator.

---

## 7. Premier paquet chiffré : `13 36` CLIENT_SESSION_SETUP

Byte grammar reverse (158 bytes dans les 6 sessions) :

```
13 36
primary_length:uvarint              // observé 43
primary_identifier:ascii[43]        // "decimal_text:36-UUID"
00
token_length:uvarint                // observé 100
encoded_token:base64-ascii[100]     // décode en 74 bytes
00
setup_flags:u8                      // bit 0 = flag caller, bit 1 = optional_value > 0
optional_value:u64le
```

Le `encoded_token` décodé en Base64 = `stable_identifier':'connection_identifier\x00` (2 UUIDs 36-char).

**Source du token (tracé byte à byte)** :
- Handler à `AION2.exe+0x9509C50` demande à un result NC Platform SDK sa vue étendue
- Virtual populate à `AION2.exe+0x7AE8970` cherche le champ nommé `authn_token`, vérifie que c'est une string, stocke à `+0x58`
- Builder à `AION2.exe+0x9001BA0` copie depuis session object `+0x15C0`
- Fallback cmdline Unreal : `-authnToken:` reconnu par `AION2.exe+0x94F7A50`

Le `stable_identifier` matche le champ `guid_` d'un sidecar NCCR Purple dans `%LOCALAPPDATA%\NCSOFT\NccrData\com.ncsoft.nccr.purpleonp.live\`.

---

## 8. Launcher parameter `-lp` (AES-256-ECB)

Configuration séparée (pas l'auth). Codec :

```
Base64 decode
-> require non-zero multiple of 16 bytes
-> AES-256-ECB decrypt (clé ASCII = "LaunchParameterMakingKeyforAion2")
-> strict PKCS#7 unpadding
-> UTF-8 text
```

Clé AES-256-ECB = **32 bytes ASCII `LaunchParameterMakingKeyforAion2`** (visible dans le binaire).

Contenu décodé : `GamePlatformType`, `NcUpdaterConfigType`, `NCCRAppIDKey`, `ServerListType`. Pas d'auth_token. `tools/decode_launcher_parameter.py` fait le job avec redaction.

---

## 9. Catalogue d'opcodes C2S documentés

### Opcodes `locally-confirmed` (reproduits sur multiple sessions ou vérifiés byte-byte contre le code runtime)

**Système :**
- `01 36` — `CLIENT_TIME` (10 bytes) : `opcode || client_unix_ms:u64le`. **159 676 instances** dans le corpus.
- `00 36` S2C — `SERVER_TICK` : tick serveur régulier.

**Handshake :**
- `10 36` — `CLIENT_RSA_HANDSHAKE` (283 bytes).
- `11 36` S2C — `SERVER_RSA_HANDSHAKE` (278 bytes).

**Mouvement sol :**
- `00 37` — `CLIENT_MOVEMENT_UPDATE_VARIANT` (30 bytes) : variant de 01 37, suit souvent les skill-movement.
- `01 37` — `CLIENT_MOVEMENT_UPDATE` (29/30 bytes) : forme haute fréquence 10 Hz. Flags 0x02 en mouvement, 0x00 à l'arrêt. Position XYZ + heading compressé 16 bit + facing float + mode + timestamp.

Format `01 37` (30 bytes moving) :
```
0:2    opcode
2      movement_flags (0x02 moving, 0x00 idle)
3:15   position_xyz (f32le × 3)
15:17  movement_heading_compressed:u16le  (deg = val * 360/65536)
17:21  facing_heading_degrees:f32le
21     optional_movement_mode:u8 (0x01, absent si flags=0)
body_end-8:body_end  client_unix_ms:u64le
```

**Saut / airborne :**
- `02 37` — `CLIENT_AIRBORNE_TRANSITION` (40/41 bytes) : takeoff +1000 velocity, apex markers 0x02/0x03/0x04.
- `03 37` — `CLIENT_AIRBORNE_MOVEMENT` (40 bytes) : updates airborne 10 Hz.

**Vol :**
- `0A 37` — `CLIENT_FLIGHT_TRANSITION` (40/41 bytes).
- `0B 37` — `CLIENT_FLIGHT_MOVEMENT` (39 bytes) : updates seulement en vol actif, pas en hover.

**Esquive :**
- `0E 37` — `CLIENT_DODGE_REQUEST` (61 bytes) : skill_id 15000100 (Dodge) ou 15000101.
- `0F 37` — `CLIENT_DODGE_MOVEMENT` (39/40 bytes).

**Combat :**
- `00 38` — `CLIENT_SKILL_REQUEST` (36-49 bytes) : request_flags + skill_id (u32le) + target varint + aim xyz + optional params. Les 16 skill_id distincts observés correspondent tous à des skill IDs publiés (Dodge, Flame Arrow, Firestorm, Blaze, Pyroclasm...).
- `1A 38` — `CLIENT_LOCATION_SKILL_REQUEST` (27 bytes) : skill ciblé par position.
- `3A 38` — `CLIENT_SKILL_REQUEST_BEGIN` (2 bytes) : enveloppe début.
- `3C 38` — `CLIENT_SKILL_REQUEST_END` (2 bytes) : enveloppe fin, 67-107 ms après 3A 38.
- `30 38` — `CLIENT_SKILL_TARGET_CONTEXT` (4/6 bytes) : target du cast en cours.
- `33 38` — `CLIENT_TARGET_SELECTION` (4/6 bytes) : sélection TAB ou clic.

**Interaction monde :**
- `20 56` — `CLIENT_LOOT_NEARBY_REQUEST` (12/20 bytes) : touche F, batch loot. Format `4 + 8 * loot_target_count`.
- `30 8D` — `CLIENT_GATHER_REQUEST` (5 bytes) : resource_entity_id en varint.
- `3D 36` — `CLIENT_GATHER_FOLLOWUP` (5 bytes) : conditionnel.
- `04 90` — `CLIENT_DISMOUNT_REQUEST` (3 bytes) : toujours `04 90 00`.

**Carte :**
- `00 91` — `CLIENT_MAP_PERIODIC_VALUE` (6 bytes) : u32le = 1110, cadence ~1.05-1.10s pendant que la map est ouverte.
- `0B 91` — `CLIENT_MAP_PERIODIC_PULSE` (2 bytes) : opcode seul, cadence ~1.204s.

**Divers (opcodes-only observés) :**
- `10 56` — `CLIENT_STARTUP_SIGNAL`
- `51 36` — `CLIENT_STARTUP_FLOAT_51` : floats 2000, 2500, 3000, 3500, 3800 à ~1 s d'intervalle au login.

### Opcodes `inferred` (preuves structurelles, meaning à confirmer)

- `04 37`, `05 37` : timestamp markers (10 bytes, u64le millis seul).
- `18 37` : `CLIENT_ACTION_POSITION` snapshot avant skills.
- `10 37` / `11 37` : landing boundary + landing movement après atterrissage.
- `1D 38` : follow-up Hellfire.
- `42 8D` : quest auto-nav candidate.
- `0C 37` / `0D 37` : mouvement spécial (control points).

### Opcodes S2C `parser-based` (vus dans parseurs publics, non revérifiés)

Lobby : `01 39`, `03 39`, `06 39`, `09 39`, `0B 39`, `0D 39`, `0F 39` (redirect world).
World : `00 36` tick, `03 36` ping, `04 38` DIRECT_DAMAGE, `05 38` DOT, `09 38` SKILL_CAST, `0E 38` TARGET_SELECTED, `2A/2B/2C 38` STATUS, `33 36` LOCAL_PLAYER, `41 36` SPAWN, `42 36` ENTITY_REMOVE_OR_DEATH, `45 36` PLAYER_INFO, `1B 92` HP_STATE, `00 8D` HP_UPDATE, `04 8D` NICKNAME, `2F 8D` NOTICE, `01 61` DUNGEON_RESULT, `02 97` PARTY_STATE...

### 47 opcodes C2S restants `observed` (shape visible, sémantique inconnue)

Dont `A1 FF` périodique ~30s, `17 90`, `59 E2`, `22 36`, familles `8A`, `43`, `56`, `E2`, `E3`...

---

## 10. Compressions

### 10.1 Bundles LZ4 externes (S2C surtout)

```
FF FF || plain_size:u32le || raw_lz4_block
```

Le flux décompressé contient des frames Aion complètes et se traite **récursivement**. Caps obligatoires : taille décompressée, ratio compression, profondeur récursion.

Forme marquée observée :
```
flag:F0..FE || FF FF || plain_size:u32le || raw_lz4_block
```

### 10.2 C2S `40 8D` : zlib

```
40 8D
compressed_blob_length:uvarint
plain_size:u32le
zlib_stream                 // commence par 78 9C
```

Décompresse en **UTF-16LE JSON-like** config (sens applicatif pas encore mappé).

---

## 11. Outillage Python livré (23 scripts)

### Crypto/handshake
- **`aion2_client_crypto.py`** : primitives complètes pour un client standalone (génération RSA, sérialisation `10 36`, parsing `11 36`, OAEP, RC4 continu, framing varint, parse/build `13 36`, launcher `-lp`). Self-test couvre tous les vecteurs.
- `decrypt_handshake.py` : décrypte un handshake capturé avec une clé privée PEM.
- `decrypt_c2s_rc4.py` : décrypte tout le stream C2S d'un PCAP, TCP reassembly inclus.

### Localisation runtime (read-only)
- **`locate_session_key.py`** : trouve la clé RC4 live en 280 ms.
- **`watch_session_key.py`** : monitor continu avec cache.
- `locate_openssl_rsa.py` : trouve le wrapper OpenSSL (RSA_private_decrypt + 3 copains).
- `scan_process_rsa.py` : scan modulus dans la RAM.
- `scan_process_rsa_public.py`, `scan_process_range.py`, `scan_live_buffers.py` : scanners auxiliaires.
- `export_openssl_rsa.py` : export + validation RSA/CRT depuis les BIGNUM.
- `disassemble_process.py`, `find_x64_calls.py`, `find_x64_rip_refs.py` : disass + refs RIP.

### GUI live
- **`live_opcode_viewer.py`** : GUI Windows qui démarre un `dumpcap` filtré, le monitor de clé, affiche opcodes C2S décryptés en live avec label d'action manuel. Code vert = mappé, jaune = catalogué sans sémantique, rouge = inconnu.

### Analyse PCAP
- `analyze_pcaps.py` : batch analyser multi-captures.
- `capture_rsa_plaintext.py` : extraction handshakes.
- `decode_launcher_parameter.py` : decoder AES-256-ECB `-lp` avec redaction.

### Probes historiques
- `probe_keystream_prf.py`, `probe_keystream_prng.py`, `probe_legacy_aion_cipher.py`, `probe_stream_ciphers.py`, `probe_tick_plaintext.py` : tests négatifs qui ont permis d'écarter AES/ChaCha/HMAC/vieux cipher Aion.

### Opcodes
- `generate_opcode_docs.py` : regénère `OPCODES.md` depuis `opcodes.json`.

---

## 12. Workflow typique

### Client standalone (celui qui nous intéresse pour un bot)

Pas besoin de reverser la RAM, le client fait tout lui-même :

```
1. Génère une paire RSA-2048 avec e=3
2. Envoie la clé publique dans `10 36` (clair)
3. Lit `11 36` (clair), extrait les 256 bytes RSA
4. Décrypte avec RSA-OAEP-SHA1, MGF1-SHA1, label vide
5. Vérifie que la sortie fait exactement 214 bytes
6. Initialise RC4 avec les 214 bytes
7. Pour chaque C2S body : RC4-transform, puis prefixer uvarint(len(body)+4)
8. Parse S2C en clair (ou LZ4 bundles si FF FF)
```

### Companion externe (notre cas pour spy/ESP enrichi)

```
1. Lance dumpcap/wireshark filtré sur AION2.exe
2. Lance watch_session_key.py (280 ms pour trouver la clé courante)
3. Chaque frame C2S : decrypt_c2s_rc4 continu
4. Chaque frame S2C : parse clair
5. Live GUI avec labels d'actions
```

### Offline (analyse d'un PCAP historique)

```
1. Dumper le process pendant la session (pour la clé RSA privée)
2. export_openssl_rsa.py : extraire PEM
3. decrypt_handshake.py : récupérer OAEP 214 bytes
4. decrypt_c2s_rc4.py : décrypter tout le PCAP
```

---

## 13. Captures d'actions contrôlées déjà faites

Le repo documente des **tests labellisés** (le joueur appuie sur une touche en marqueur) :

| Action | Opcodes émis |
|---|---|
| Jump stationnaire | `02 37 → 03 37 ×6 → 02 37 → 03 37 ×3 → 18 37` (877 ms) |
| Dodge | `00 38 → 0E 37 → 0F 37 ×3 → 18 37` (394 ms) |
| Deploy wings | `0A 37 → 0B 37 ×3 → 0A 37 → 18 37` (402 ms) |
| Fly up/down vertical | `0A 37 → 0B 37 ×38 → 18 37` (3898 ms) |
| Fold wings | `02 37 → 03 37 ×4 → 18 37` (465 ms) |
| TAB cycling (6 mobs) | 16 × `33 38` seulement |
| Gather resource ×3 | `33 38 (sel) → 30 8D (req) → ... → 3D 36 (?) → 33 38 (clear)` |
| Loot 1 mob F | 1 × `20 56` (12 bytes, count=1) |
| Loot 2 mobs F | 1 × `20 56` (20 bytes, count=2) |
| Dismount | `04 90 00` |
| Mount up | **rien** (local only) |
| Flame Arrow / Blaze / Firestorm cast | `3A 38 → 30 38 → 00 38 → 3C 38` (67-107 ms) |
| Hellfire | `1A 38 → 1D 38 ×n` |

---

## 14. Ce qui reste ouvert dans le repo

1. Sémantique de la plupart des opcodes `inferred` et `observed`.
2. Mapping d'actions joueur spécifiques sur des C2S exacts (ex: ouverture inventaire, achat shop...).
3. Source applicative de `optional_value` dans `13 36`.
4. Meaning du zlib `40 8D`.
5. Flow de login/token lobby complet (le repo s'arrête au world).
6. Minimum de paquets pour keepalive, character selection, world join (pour un bot standalone).
7. Comportement client sur S2C inconnu ou omis.

---

## 15. Ce que ça change pour nous (impact pratique)

### Immédiat

1. **Toutes nos notes crypto obsolètes** : le repo écrase nos séances du 29/09 (AES-GCM, flot custom, hook `aesni_encrypt`). RC4 avec clé = OAEP 214 bytes brut, point.
2. **Plus besoin de hooker `EVP_PKEY_decrypt`** avant zeroize. `locate_session_key.py` récupère la clé **après coup** depuis les states RC4 vivants (280 ms, read-only).
3. **Workflow externe = passe NCGuard sans bypass**. Dumpcap + ReadProcessMemory, aucune injection. C'est pourquoi `lunixxx37` n'a jamais déclenché NCGuard pendant son analyse.
4. **Un bot standalone AION 2 est faisable** : le client peut générer sa propre paire RSA, faire son handshake, parler le protocole chiffré. Reste à régler le login lobby (dépend du token `authn_token`).
5. **Nos offsets AC inutiles pour cette couche** : la crypto est en user-mode OpenSSL standard, pas de magie NCsoft côté AC.

### Pour SysUtil / AION2-STUDIO

- Intégrer `aion2_client_crypto.py` comme librairie Python → script de **décryptage temps réel des PCAP** capturés par Aion2PacketTools côté **C2S** (jusqu'ici on avait que S2C).
- Le workflow "capture réseau + dump UMD en 1 geste" peut gagner une **colonne C2S opcodes** dans l'onglet Session, avec les noms canoniques de `opcodes.json`.
- Pour les tâches de bot (gathering, auto-skill, mount control), on a maintenant le **grammaire exacte** des requêtes à envoyer.

### Pour I.03 (hook NetDeltaSerialize sur CharState)

Reste **toujours pertinent** mais pour une raison différente : le repo ne décode pas les S2C sauf les basiques. Les stats HP/MP/FP viennent de `DevPacketData_Common_CharState` **côté S2C** (packet dumps serveur → client), hors scope du repo. Nos chemins ESP Tools fil 618 restent la bonne route pour les attributs joueur en RAM.

### Pour la détection

- Le repo documente `NCGuard DeFe_ScanHandle` qui ferme les handles `VM_READ`/`VM_OPERATION` du process. **Mais** `locate_session_key.py` fait son RPM depuis une session **non-game** (le shell élevé). NCGuard ferme le handle **du scanner externe** s'il détecte ses patterns, pas le handle depuis le process shell.
- `lunixxx37` dit explicitement : **une tentative d'attach de debugger natif a déclenché un kill NCGuard avec exit `0xE0000011`**. Donc : passive-only. Pas d'injection.
- `Frida` est rejeté par le process. Hook direct impossible.

---

## 16. Prochaines actions recommandées

1. **Lire `aion2_client_crypto.py`** en détail (c'est le fichier canonique, auto-testé).
2. **Tester `watch_session_key.py` sur AION2.exe live** pour voir si ça marche sur notre build Steam EU (le repo cible rev 3527 Global).
3. **Captures C2S étalonnées** : pour chaque action que SysUtil veut automatiser (gathering, auto-dodge, auto-skill), capturer sur notre client, décrypter, vérifier qu'on retrouve bien les opcodes du catalogue → compléter notre propre registre.
4. **Fork + README** : on fait un fork `Xtrangers/aion2-protocol-research` qu'on versionne pour nos ajouts, en remontant les trouvailles upstream via PR (MIT license, c'est OK).
5. **Pour un bot standalone** : la plus grosse pièce manquante est **le login NC Platform SDK** qui fournit `authn_token`. Ça demande de reverser la couche HTTP de Purple, hors scope du repo.

---

## 17. Risques et limites

- **Snapshot 10/10/2026** : les offsets runtime (ex `+0xE20`, `+0xE30`, `+0x15C0`, `+0x9001BA0`...) peuvent avoir bougé avec les patches 14/10, 22/10, 28/10. Les constantes **cryptographiques** (RC4, OAEP, framing, opcodes) sont stables.
- **Monitor key** demande une **shell élevée** pour ouvrir le process AION2.exe.
- Le repo publie des outils, pas des captures : raw PCAP, PEM, OAEP decrypted sont dans `.gitignore`. Il faut rejouer la méthode avec son propre client.
- **MIT license** mais le dépôt est "unofficial, not affiliated with NCSOFT". Usage privé = OK, redistribution d'un bot utilisant ces outils = zone grise comme toujours.
- Un **debugger natif attaché crashe le jeu** avec `0xE0000011`. Donc on reste strictement passif / ReadProcessMemory.

---

## 18. Fichier à lire en priorité pour nous

Classement d'importance :

1. **`tools/aion2_client_crypto.py`** — toutes les primitives, self-test complet. C'est la lib à intégrer.
2. **`docs/PROTOCOL.md`** — spec canonique du protocole, byte-par-byte.
3. **`protocol/opcodes.json`** — registre machine lisible (parseurs auto possibles).
4. **`tools/locate_session_key.py`** + **`tools/watch_session_key.py`** — pour la clé RC4 live.
5. **`tools/decrypt_c2s_rc4.py`** — pour décrypter nos PCAP historiques (session 29/09, etc).
6. **`docs/C2S_RESEARCH.md`** — toute la méthode RE, les erreurs d'approche à ne pas refaire.
7. **`docs/OPCODES.md`** — catalogue humain-lisible.

Tout le reste (`probe_*.py`, `scan_*.py`) = outils d'investigation, utiles seulement si on reverse une nouvelle couche.
