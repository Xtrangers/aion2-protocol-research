# AION 2 Reversal — Resume complet de la discussion UnknownCheats

**Source** : thread 726048 sur unknowncheats.me (18/11/2025 - 03/11/2026, 292 posts).
**Jeu** : AION 2, Steam / Purple + mobile, moteur **Unreal Engine 5.3.2.0**, editeur NCSoft.
**Anticheat** : **NCGuard**, user-mode uniquement (confirme par plusieurs reversers).

---

## 1. Moteur et SDK

- **UE 5.3.2.0**, dumpable via Dumper-7 modifie (release par `arisenpai` dans le post #141) ou via dump manuel IDA + Scylla sur un process suspendu avant le login.
- Plusieurs **SDK zip publies** sur le forum :
  - 30/09/2026 (`HX73D` approve, file id 58134).
  - 07/10/2026 (`fpsnewboy` -> `rhaym` approve, file id 58312).
  - 15/10/2026 (`s4ncak1`, file id 58554).
  - 28/10/2026 (`Dankboose` -> `rhaym` approve, file id 59014, dernier au moment du thread).
- Dump UE classique via **GSpots v1.8** (trouve GNames/GObjects, souvent pas GWorld, surtout sur KR). Alternative : patch binary + reflexion.

### Methodes de dump que le forum valide

1. **Dumper-7 modifie** (`arisenpai`, file id 58309) : injection pendant la page de login, pas en jeu. L'anti-tamper est moins strict a ce moment-la.
2. **Dump externe x64dbg + Scylla** : suspendre AION2.exe juste apres le splash (avant la login screen qui charge NCGuard). OEP = AION2.exe MainEntryPoint.
3. **Driver kernel BYOVD + lecture memoire physique** (methodes des reversers avances type `arisenpai`, `KeanuCorpse`, `vova1210`).

Un user (`unfairuser67`) decrit que Scylla IAT Search echoue, les sections sont preservees mais IDA plante : signe que le binaire est **VM-packed** / obfusque et qu'un simple dump ne suffit pas.

---

## 2. Offsets globaux UE (historique des patches)

| Date | GNames | GObjects | GWorld | ProcessEvent | AppendString | Auteur |
|---|---|---|---|---|---|---|
| 19/11/2025 | 0xDA6D140 | 0xD6349B0 | 0xD929C18 | 0x35656C0 | 0x3363160 | palacestreet |
| 24/11/2025 | 0x0DA7B140 | 0x0D6429B0 | 0x0D937C18 | 0x035654D0 | 0x00000000 | abc837932126 |
| 26/11/2025 | 0xDA7D180 | 0xD644930 | 0xD939C18 | — | 0x3363860 | palacestreet |
| 04/12/2025 | 0xDB52940 | 0xD7149B0 | 0xDA0AE18 | — | — | fpsnewboy |
| 26/12/2025 | 0xDD1EB00 | 0xD8D5E60 | 0xDBCEC98 | — | — | lirikmel (TW) |
| 28/12/2025 (KR) | 0xE3D5EC0 | 0xDF8CE60 | not found | — | — | qkwkqhtn |
| 30/09/2026 | 0xF110380 | 0xEC68920 | 0xEF73B08 | — | — | yurigagarin |
| 05/10/2026 | 0xF122440 | 0xEC7A9E0 | 0xEF85BC8 | — | — | Gata |
| 07/10/2026 | 0xF134440 | 0xEC8C9A0 | 0xEF97BC8 | 0x42066B0 | 0 | iamLau |
| 09/10/2026 | 0xF120440 | 0xEC78990 | 0xEF83BC8 | 0x04205CD0 | — | ZixMatrix |
| 14/10/2026 | 0xF12B5C0 | 0xEC83990 | 0xEF8EC38 | 0x4206400 | — | qwesdeq |
| 21/10/2026 | 0xF132600 | 0xEC8A990 | 0xEF95C78 | — | — | Mevsa |
| 22/10/2026 | 0xF132200 | 0xEC8A5A0 | 0xF3CEEB0 | 0x04206550 | — | iamLau |
| 26/10/2026 | 0xF132600 | 0xEC8A9A0 | 0xEF95C78 | 0x04206550 | — | syvix |
| 28/10/2026 | 0xF137700 | 0xEC8F910 | 0xEF9AE08 | 0x4206430 | — | nosquare (dernier) |

- **INDEX_PROCESSEVENT = 0x4D** stable depuis le debut.
- **AppendString = 0** : donc on utilise **FNamePool directement** (`iamLau`).
- **Signature GWorld** : "Search for `%d ansi Fnames`" dans IDA (`fpsnewboy`).

### GEngine route (post #63, NullReference)

Alternative a GWorld pour l'external :
- `GEngine` = 0x0e7e87e0 (early)
- `WorldContext` : `engine + (NextWorldContextHandle - 0x10)` = `TArray<WorldContext*>`
- `ActiveNetDrivers + 0xB0` -> `UWorld*`
- Valider : `World.GameState != 0`

---

## 3. Dump d'offsets de structure complet (post #104, auteur `Gata`, 05/10/2026)

C'est **le dump le plus complet du thread**. Classes principales :

### UObject / UField / UStruct / UFunction
```
UObject_Vtable         0x00
UObject_Flags          0x08
UObject_InternalIndex  0x0C
UObject_Class          0x10
UObject_Name           0x18
UObject_Outer          0x20

UStruct_SuperField  0x40
UStruct_Children    0x48
UField_Next         0x28
UFunction_FunctionFlags 0xB0
UFunction_Func      0xD8
```

### AActor / APawn / ACharacter / AionCharacter
```
AActor_CustomTimeDilation       0x84
AActor_InputComponent           0x180
AActor_RootComponent            0x1C0
AActor_InstanceComponents       0x280
AActor_BlueprintCreatedComponents 0x290
APawn_BaseEyeHeight             0x2B4
APawn_PlayerState               0x2C8
APawn_Controller                0x2D8
ACharacter_Mesh                 0x3C0
ACharacter_CharacterMovement    0x3C8
ACharacter_CapsuleComponent     0x3D0
AionCharacter_CharActPlayRateControl  0x22B0   ← anim speed
Mesh_GlobalAnimRateScale        0xAF0
```

Note (`qwesdeq` 14/10/2026 confirme) :
```
AActor->ClassPrivate = 0x10
AActor->FName        = 0x18
AActor->RootComponent= 0x1C0
AActor->Nickname     = 0x26B8   ← pseudo joueur !
```

### UWorld / Levels / GameInstance / Players
```
UWorld_PersistentLevel    0x30
UWorld_Levels             0x178
UWorld_OwningGameInstance 0x1E8
UGameInstance_LocalPlayers 0x38
UGameInstance_UISystem    0x3C8
UGameInstance_InputSystem 0x418
AionUISystem_WidgetTree   0x680
ULevel_Actors             0xA0
ULevel_WorldSettings      0x2C0
AWorldSettings_TimeDilation          0x468
AWorldSettings_CinematicTimeDilation 0x46C

UPlayer_PlayerController       0x30
APlayerController_AcknowledgedPawn   0x378
APlayerController_AHUD               0x3F0
APlayerController_PlayerCameraManager 0x3F8
APlayerCameraManager_DefaultFOV      0x2C0
APlayerCameraManager_CameraCachePrivate 0x1410
FCameraCacheEntry_POV               0x10
FMinimalViewInfo_FOV                0x30
FMinimalViewInfo_DesiredFOV         0x34
AController_ControlRotation         0x320
APlayerState_PlayerNamePrivate      0x340
```

Mise a jour `qwesdeq` 14/10/2026 : `POV_Location = 0x3E0`, `POV_FOV = 0x410` (sur APlayerCameraManager en direct).

### SceneComponent / Movement / Capsule / Mesh
```
USceneComponent_RelativeLocation 0x128  (ou 0x148 selon patch 14/10)
USceneComponent_RelativeRotation 0x140
USceneComponent_RelativeScale3D  0x158
UCharacterMovementComponent_JumpZVelocity    0x198
UCharacterMovementComponent_MovementMode     0x221
UCharacterMovementComponent_MaxWalkSpeed     0x268
UCharacterMovementComponent_MaxSwimSpeed     0x270
UCharacterMovementComponent_MaxFlySpeed      0x274
UPrimitiveComponent_BodyInstance             0x348
FBodyInstance_CollisionEnabled               0x17
UCapsuleComponent_CapsuleHalfHeight          0x508
UCapsuleComponent_CapsuleRadius              0x50C
USkeletalMeshComponent_CachedComponentSpaceTransforms 0x960
```

Post #243 (yurigagarin) :
```
ComponentToWorld FTransform3d  = RootComponent + 0x270
Translation                    = RootComponent + 0x290
```

### FUObjectArray / NamePool
```
FUObjectItem_Object  0x00
FUObjectItem_Flags   0x08
FUObjectItem_Size    0x18
NamePoolCurrentBlockOffset   0x08
NamePoolCurrentByteCursorOffset 0x0C
NamePoolBlocksOffset         0x10
NameBlockSize                0x20000
NamePoolMaxBlocks            8192
```

**Note cle** (post #152, `Shard999`) : `GObjects` compte **555 136 objects**, 9 chunks, num a `+0x14`, chunks a `+0x1C`.

### HUD / QuickSlots (chaine complete pour auto-skill)

Tres long bloc dans post #104 (Gata) : `AionWidget_QuickSlotHolder`, `UseButton`, `P_Content`, `AionWidget_QuickSlotSkill`, `AionDialogWidget_SkillSlotTooltip`, `AionUIComponent_SkillBasicInfoUpdater`, `AionWidget_HotKey_P_RTxt_HotKey`, etc. Voir aussi **post #228** (fpsnewboy) qui donne code C++ complet pour lire la barre de skills (chain state, cooldown, icon via Material.TextureParameterValues).

**Trap `fpsnewboy` post #228** : `SlotId_Set` **n'est pas unique** (~3 holders live par position). `InputAction` est le vrai id (seul un holder par position a `action != 0`, range 93-100 skills, 101-108 items, 109-112 stigma).

---

## 4. Classe AionCharacter (post #15, abc837932126)

Classe complete pour lire HP/MP/nickname/level d'un joueur ou NPC :

```cpp
// Depuis AionCharacter* ptr :
ptr + 0x2270  → player name chain (FString a +0x28, ou NPC via +0x10 → +0x30 → +0x10)
ptr + 0x2280  → Attribute (lv a +0x20, HP/MP a +0x98/+0xb0, HPMax a +0x8)
ptr + 0x2290  → is_valid_npc check (!= 0 si npc)
ptr + 0x22A0  → != 0 si player, == 0 si npc
```

Attribute offsets :
- `get_Health_other()`  = ptr + 0x20
- `get_HealthMax_other()`= ptr + 0x8
- `get_Health_self()`   = ptr + 0x98
- `get_HealthMax_self() `= ptr + 0x8
- `get_Mana_self()`     = ptr + 0xb0
- `get_ManaMax_self()`  = ptr + 0x20

**Confirme** (post #41, lirikmel) : "cette structure est a `AApawn + 0x2200`".

---

## 5. Costume / SkinChanger (post #72, fpsnewboy)

```
AionCostumeArmorMeshData (352 bytes) :
+0x08  SkeletalMesh         FSoftObjectPtr
+0x108 EquipPartsSettingData FSoftObjectPtr
+0x15C EquipSlotOverrideMask
```
Via `ResourceDatas[ECostumeMainMeshType]`. Externe = tres difficile selon lui, seulement les skins du shop.

---

## 6. EnvObj / Gathering (resources mondes) — tres documente

### Post #42-48, 814003279 + qkwkqhtn

Classe `AAionEnvObj : public AActor` :
```
0x0428 RootSceneComponent
0x0430 SkeletalMeshComponent
0x0438 CapsuleComponent
0x0440 PickingCapsule
0x0448 UAionCharSessionComponent
0x0450 UShapeComponent
0x0458 UOutlineComponent
0x0460 UAionEnvObjMovementComponent
0x04E8 NamePlateComponent          (UAionLevelUIComponent)
0x04F0 DebugInfoComponent
0x04F8 ActContainerMaterialComponent
0x0500 ActContainerComponent
0x0620 CachedObjectStaticMeshList  (TArray<UStaticMeshComponent*>)
0x0688 GuildDecoItemMesh
```

### Post #98 et #117, hornochse89 — chain complete pour les objets gathering

```cpp
PersistentLevel->Actors[i] = actor

uintptr_t envData = Read<uintptr_t>(actor + 0x588);
if (envData) {
    uint32_t envObjId = Read<uint32_t>(envData + 0x28);
}

envRecord = LookupById(gameBase + 0xF29DFB0, envId);  // EnvObjTable
usage     = Read<uint8_t>(envRecord + 0x34);

if (usage == 3) { // Gatherables (hors wings)
    key = Read<uint64_t>(envRecord + 0x38);
    gatherRecord = LookupByName(gameBase + 0xF2A4EC0, key);  // GatherSourceTable
    type = Read<uint8_t>(gatherRecord + 0x1C);
}
```

Enum `GatherSourceType` :
```
0=None 1=Metal 2=RareMetal 3=Jewelry 4=Tree 5=Flower 6=Herb
7=Berry 8=Vegetable 9=Shellfish 10=Od  (Max=11)
```

### Post #171 (KeanuCorpse) — probleme non resolu des noms affiches

Les actors sont `BP_EnvObj_Common_C`, nameplate vide. Pour avoir le vrai nom (Morfa, Odyle, Empyrean Trace), il faut :
- Resoudre `FEnvObjTableItem` depuis `envObjId`
- Puis via `UsageValue` -> `FGatherSourceTableItem` -> `Name` (FName a +0x0C)
- **Mais** : les Name sont des `FName` qui pointent vers `FTableL10NText` (Key + StringKey), et personne n'a trouve la lookup string vers wchar.

Structures :
```cpp
struct FTableL10NText {   // 0x10
    FName Key;            // +0x00
    FName StringKey;      // +0x08
};

struct FEnvObjTableItem {          // size 0x160
    uint32_t ID;                   // +0x08
    FName    Name;                 // +0x0C
    FTableL10NText Desc;           // +0x14
    FName    ResourceKey;          // +0x24
    uint8_t  Nameplate;            // +0x2C   EEnvObjNameplate
    int32_t  NameplateOffset;      // +0x30
    uint8_t  Usage;                // +0x34   EEnvObjectUsage
    FName    UsageValue;           // +0x38
};

struct FGatherSourceTableItem {    // size 0x40
    uint32_t ID;                   // +0x08
    FName    Name;                 // +0x0C
    FName    MaterialGroup;        // +0x14
    uint8_t  SourceType;           // +0x1C
    uint8_t  SourceGrade;          // +0x1D
    uint32_t MasteryLevel;         // +0x20
};
```

RVAs des tables (patch early 10/2026, a re-scanner) :
- EnvObjTable      = `0xF29DFB0`
- GatherSourceTable= `0xF2A4EC0`
- Gather source map runtime : `0xF3021F8` (192 rows)

---

## 7. Mob / NPC runtime (post #274, nosquare)

```cpp
// Relative to the monster actor:
OFFSET_MOB_RUNTIME_KEY = 0x26F0  // int32_t  (instance id spawnee)
OFFSET_MOB_NATIVE_DATA = 0x2728  // ptr vers NativeData

// Relative to NativeData:
OFFSET_NATIVE_RUNTIME_KEY = 0x00  // int32_t
OFFSET_NATIVE_NPC_ID      = 0x04  // NPC/template ID
```

---

## 8. Race / Faction (post #289, yurigagarin — tres recent)

```
PlayerActor + 0x3C0         → Character Mesh
Mesh + 0x988                → AnimScriptInstance
AnimScriptInstance + 0x596  → PcRace uint8

ERace: 0=None 1=Light 2=Dark 3=All
```

Verif pour radar PvP (differentRace) :
```cpp
(localRace == 1 || localRace == 2) &&
(remoteRace == 1 || remoteRace == 2) &&
remoteRace != localRace
```

**Mais** `lancelot9999` (#290) rapporte que ca ne marche pas dans l'Abyss : le serveur ne pousse que sa propre faction pour tous les joueurs proches. **Server-side filter**. Il n'y a pas de contournement partage dans le thread.

---

## 9. Auto-dodge / AoE PvE et PvP (post #222, mevisitor + darksidexc #220)

### PvE auto-dodge
```
AionSkillNoticeObj  (actors proches)
  → DecalComponentArray    # world transform, AoE location, shape, visual progress
```
Donne la position de l'AoE, sa forme, et sa progression visuelle (0-100%).

### PvP auto-dodge
```
enemy actor → rootaction → AionCharacterAction_Skill → skill id (filtrer CC) → target entity
```

Darksidexc a livre une **video** de l'auto-evade (#220, imgur mp4), "no skills/mobs database, just real time math". Yurigagarin confirme l'approche.

---

## 10. Hotbar / Skill chain state (post #228, fpsnewboy)

Code C++ complet pour lire :
- Chaque slot de la barre (hotkey, icone, niveau, cooldown)
- Chain-skill (OnChainSkill, OnConditionSkill, OnCooldown)
- InputAction identite (93-112)
- Icon : `Material.TextureParameterValues[0]` (pas directement texture, c'est une material instance avec params !)

Visibility values de UE : `ESlateVisibility 0=Visible, 3=HitTestInvisible, 4=SelfHitTestInvisible` = on screen.

---

## 11. NCGuard (anticheat) — reversing

### Composants identifies (post #210, Zeuby ; #211 Danielci58 ; #212 Shard999)

- **bb64.dll** : visible scan / telemetry layer, PAS le coeur AC
- **BBt.dll** : vrai moteur AC, manuellement mappe dans AION2.exe au runtime, **PE header efface**, invisible via PEB
- **ncghdll64.dll** : injection agent, droppe dans `%TEMP%\ncgtmp-*` seulement quand une policy decide d'un inspection poussee (**pas deploye** la plupart du temps)
- **ncgpelib64.dll** / **ncghdll32.dll** : wrappers
- **PDB path** interne : `D:\ncc_jenkins\workspace\Release_NCGPE\bin\x64\Obfuscate\pdb\BBt.pdb`
- Markers : `NcgObfuscationBegin/End`

### ~16 features DeFe_* dans BBt :
```
ProcessScanner, ScanHandle, HookChecker, MemoryChecker,
MemoryProtector, Debugger, VMChecker, RemoteDesktopChecker,
ThirdPartyChecker, LauncherChecker, MultiClientChecker, ScanRulePattern,
ScanThread, CallbackManager, ReplaceMessageProcedure,
ProcessAccessChecker, PreventSuspendProcess, BypassNCGH (heuristique)
```

### Verifications documentees

- Hooks **NtUserSendInput** et autres input APIs → impossible SendInput, PostMessage, SendMessage, autoit ControlSend/ControlClick (#11, #14, #27, #136).
- Hook **WH_CALLWNDPROC** + **WM_INPUT** pour matcher les devices virtuels/emules → virtual input flagge (#89, Shard999).
- **DeFe_ScanHandle** : enumere les handles via `NtQuerySystemInformation(SystemHandleInformation)` et appelle `CloseProcessHandle` sur tout ce qui tient `VM_READ`/`VM_OPERATION` sur le process (#210).
- **GetTickCount watcher threads** : check de suspension du process. Si on gele trop longtemps, detection (#149, Shard999).
- Verif du **chemin de recherche** systeme : AION2 flagge quand on cherche "des mots sus" en arriere-plan (#85, HyanJae).
- `ProcessScanner.injector.switch` : actuellement **desactive dans la policy embarquee** (#212, Shard999).
- Watch des **integrity checks** : inline hooking / VEH / midhook crash avec `0xE0000014` et `0xC0000409 __fastfail` (#94, hornochse89).
- **Pas de kernel-mode AC** confirme par plusieurs (#57, #58, #249).

### Patch / Bypass NCGuard (post #18, fpsnewboy)

Code DLL complet qui patche `LoadLibrary` sur bb64.dll (pattern `FF 15 ? ? ? ? 49 89 44 24 08 48 85 C0 0F 85 ? ? ? ?`), remplace par `mov eax, 1 ; ret` → `LoadLibrary(bb64.dll)` retourne succes sans charger la DLL. Mais c'est le **wrong dll** selon SigPhreak #186/208.

Alternative plus propre : patcher **BBt** lui-meme (la DLL cachee), pas bb64.

### Comment attacher x64dbg

- `fpsnewboy` #18 : suspend AION2 **avant le login screen**, puis x64dbg + Scylla pour le dump. Ne clique pas sur Fatal Error popup.
- `arisenpai` #141 : injecter Dumper-7 modifie pendant la **page de login** (bb64 pas encore totalement actif).
- `KeanuCorpse` #152 : NCGuard check GetTickCount → tenir sur la login screen (moins de choses chargees = seuil plus bas).
- EfiGuard + TitanHide **ne marche pas** (#17 zonetime).

### Injection techniques validees

- **Thread Hijack** (xDead2K #58) : contourne les hooks d'input.
- **ReflectiveDLLInjection** : OpenProcess → VirtualAllocEx → WriteProcessMemory → CreateRemoteThread / NtCreateThreadEx (#68).
- **Hooker bb64.dll** : hooker `ncgpelib64.dll`, `path_blacklist`, `RtlAddVectoredExceptionHandler` (sans fermer bb64).
- **SetWindowHook ou ManualMap QueueUserAPC** (#25 fpsnewboy).
- **BYOVD + NtLoadDriver → physical R/W → manual map → NtCreateThreadEx SKIP_THREAD_ATTACH** (#233, vova1210). Important : SKIP_THREAD_ATTACH evite que la TLS callback de bb64 crash avec `__fastfail 0xC0000409`.
- **Vtable Hooking** plutot qu'inline (#233).

### Interception driver (user-mode)

- `oblitum/Interception` sur GitHub : driver qui envoie des scancodes clavier. Fonctionne pour les macros tant que la fenetre est au premier plan (#27 Medux, #29 judarest).

### Fausses pistes et P2W

- **God mode** : impossible, option de cheats commerciaux = bait trash (#132, SigPhreak).
- **Long teleport** : patche early (#105).
- **Instant gathering** : server-side, impossible officiellement (#264 Shard999, confirme).
- **Cooldowns** : tous server-side (#16 WhoWouldCheat).

---

## 12. Movement / Speed / Teleport — ce qui marche

### Resume effets (post #70, jonxpin ; #68 fpsnewboy)

| Effet | Gain | Technique |
|---|---|---|
| Attack speed | 15-20 % | manip weapon stats (patche partiellement) |
| Movement speed | 20-40 % (jusqu'a ~45 % smooth) | `MaxWalkSpeed`/`MaxFlySpeed` + walk/run/sprint multipliee ou TimeDilation |
| Walk 200 → 290 | +45 % | fpsnewboy |
| Run 500 → 745 | +49 % | fpsnewboy |
| Sprint 800 → 1192 | +49 % | fpsnewboy |
| Jump height | +30 % | JumpZVelocity |
| Dash/evade distance | +30 % | non precise |
| Teleport | <= 8-10 m par saut, loopable | #71 fpsnewboy, #204 hunchojacks |
| FOV | custom | APlayerCameraManager_DefaultFOV |
| Animation canceling | +20 % DPS | TimeDilation |
| Auto-skill | +10 % DPS | OnQuickSlotPressed/Released |

**Rubberbanding** au-dela de 700-750 unites de MaxWalkSpeed (#70 jonxpin), server resync a ~60 %.

### Patches silencieux

- **Early 10/2026** : teleport long distance patche, CD/attack speed minor seulement, movement iffy mais **TimeDilation fonctionne toujours** (#105 WhoWouldCheat).
- **13/10/2026** : silent bb64.dll update (#224).
- Chaque patch casse les offsets globaux (nouveaux offsets poses chaque jour par iamLau, nosquare, syvix, etc.).

### Fly / infinite fly

- `AionWidget_HUD_ActionButtonFly` + `FpUpdater` + `P_Img_FpGauge` + `FlightAction_NotEnoughFP` dans SDK (#248 nosquare).
- Chercher FP et MaxFP via `ECharPointType::FP` / `MaxFP` + `fp_point_fly_cost` / `fp_point_fly_dash_cost` / `fp_point_flying_sprint_cost`.
- Personne n'a release de code public dans ce thread.

---

## 13. Chiffrement / reseau

- Post #191 (cracksuxer200) : clef AES = `06038EF544B6007614F8574F1B7C2A3F0D565F74CDCC1B366B4EA1A17B97CBFF` (une seule ligne, sans contexte).
- Post #292 (dollyXtoddy) : repo GitHub **"aion2-protocol-research"** contient "presque tout pour Client → Server".
- Post #277 / #291 (prixone) : "**preset style shop**" chiffre dans `reserved4` du JSON de nc.com/s/<id>. Blob base64 (environ 1700 octets). Personne n'a trouve la cle.
- Post #260 (zEw00) : idem, dump memoire sans succes pour trouver la cle.
- Post #56 (richhks) : MITM proxy fonctionnel partiellement.

---

## 14. Bans / detection / strategies

### Detection

- Flagge : hooks input, debuggers, VM, Remote Desktop, "ThirdPartyChecker", multi-client.
- Account can be flagged **pour avoir cherche** des termes suspects en arriere-plan (#85).
- **Mining massif + radar** : 24h ban temporaire (#105 WhoWouldCheat).
- **Market snipe trop rapide** (1 req/sec) : 24h ban temporaire (#127 mevisitor).
- Chaque fois que NCGuard tue un process attache = **loggue et reporte en emergency** (#134 Shard999) → bannissement eventuel.

### Strategies partagees

- **AnyDesk depuis 2e PC** pour builder le cheat tout en jouant (#88 yurigagarin).
- **External par defaut** (`darksidexc`, `fpsnewboy`, `Shard999`) : plus safe pour les comptes. External + kernel driver pour R/W.
- **Internal** : `WhoWouldCheat`, `arisenpai` : plus simple pour gathering names, pas de crash si bien fait.
- Deux ou trois tiers de comptes (cheat / cheat-sans-ban / main) (#284).
- Steam game ban = visible sur le compte Steam.

---

## 15. AI-assisted reversal (meta)

Thread tres marque par l'usage d'AI (grok 4.6/4.7, GLM 5.3, DeepSeek, Sol 6.1, Astra, Gemini Pro, Muse Spark, Antigravity IDE, Big Pickle) pour reverser NCGuard et generer du code cheat.

- Workflow `Astrihacks` #154 : Google Antigravity IDE + OpenCode + modeles gratuits (Big Pickle, Muse Spark 1.3) + `miscusi-peek/cheatengine-mcp-bridge` + Cheat Engine custom (bypass BE/EAC thread 2023) → cheats generes auto.
- `KeanuCorpse` : grok Build harness + openrouter + GLM 5.3 pour reverse NCGuard. Memory mode = **a desactiver** pour eviter que l'AI "se poisonne" avec un ancien refus. 160 M tokens juste pour bypasser NCGuard.
- `arisenpai` utilise **SWE 2** + son propre kernel driver pour lire la memoire physique.
- `Danielci58` : jailbreaks publics pour grok / glm / deepseek. Sol 6.1 et Astra "cyber safeguard" se declenchent → "daybreak" flags.
- Mention `togg53192-cmd` sur GitHub pour la base de jailbreak grok.

---

## 16. Choses non resolues (fin du thread)

1. **Noms des objets gathering** (Morfa/Odyle/Empyrean Trace) : chaine FTableL10NText non resolue vers wchar.
2. **Fraction/race PvP** : ERace retourne toujours sa propre faction en Abyss. Pas de contournement partage.
3. **Preset style shop decryption** : blob base64 ~1700 octets, cle introuvable.
4. **Attack speed strict** : validation par CD, gains tres minces apres patch 10/2026.
5. **Infinite fly** : classe + updater identifies, pas de code release.
6. **Combat speed** : question ouverte (#271).
7. **Damage meter / boss HP** : non partage (#252).
8. **Skin changer externe** : partiel (store seulement).

---

## 17. Fichiers downloadables et outils partages

| File ID | Date | Contenu | Auteur | SHA256 |
|---|---|---|---|---|
| 58134 | 30/09/2026 | SDK AION2.zip | fpsnewboy | d139d856e07b25ac13bb565a024b5c0514b449f246da7d45563efc9f78fa9e05 |
| 58309 | 07/10/2026 | Dumper-7.dll modifie | arisenpai | a7043f863f91716922d482e8107480c544888389453310ff9abf6cdaaf12f0b2 |
| 58312 | 07/10/2026 | 5.3.0+++UE5+Release-5.3.AION2.zip (SDK) | fpsnewboy | efbaf05d2a3d1f5c0c37f25feac5a37bda97c41a8c66da1f6c6e0817732f6169 |
| 58554 | 15/10/2026 | SDK.zip | s4ncak1 | 9b99ee140329d2b66a355219a6c3d13829845a09a9108540f3202cc4e002959e |
| 59014 | 28/10/2026 | AION2 SDK | Dankboose | 9ba4ad5f4eae3e3df2ad0b43b0f9e9f65cf8544f51f221349d8110f3bc8f4a55 |

Repos / outils externes :
- `oblitum/Interception` : driver clavier
- `miscusi-peek/cheatengine-mcp-bridge` : AI auto-cheats
- `aion2-protocol-research` sur GitHub : protocole reseau Client→Server
- `togg53192-cmd` : jailbreak grok

---

## 18. Reversers principaux a suivre

| Pseudo | Specialite | Posts cles |
|---|---|---|
| **yurigagarin** | Movement, PvP auto-dodge, race, radar | 24 posts, tres actif |
| **fpsnewboy** | NCGuard bypass DLL, SDK, HUD skillbar, costumes | #18, #68, #72, #228 |
| **qkwkqhtn** | Debug SDK, offsets KR | 13 posts |
| **Danielci58** | AI-assisted RE, bypass BBt | 13 posts |
| **Shard999** | NCGuard features/limits, DeFe_* reverse | #146, #149, #212 |
| **KeanuCorpse** | AI workflow + external + gather names research | #171 (le plus technique du thread) |
| **Gata** | Dump d'offsets complet | #104 (base canonique) |
| **Zeuby** | Nommage BBt, markers obfusques | #205, #210 |
| **darksidexc** | Auto-dodge, external avec kernel driver | #220 |
| **arisenpai** | Dumper-7 modifie, driver physical R/W | #141, #167 |
| **nosquare** | Offsets recents, Mob runtime key, FP question | #257, #274, #278 |
| **hornochse89** | Gather chain complete, env usage | #98, #117 |
| **mevisitor** | AoE auto-dodge chain | #222 |
| **SigPhreak** | Internal bot (sub-based, pas release ici) | #129, #186 |
| **WhoWouldCheat** | Internal, YZF R1 | #16, #105 |

---

## 19. Pour notre projet (InjectorXXX / AION2-STUDIO)

Ce qui est **nouveau** par rapport a ce qu'on sait deja :

1. **NCGuard = BBt (manuellement mappe, PE wipe)**, pas bb64.dll. Les futurs bypass doivent viser BBt (`D:\ncc_jenkins\...BBt.pdb`), pas bb64.
2. **`AActor->Nickname = 0x26B8`** (patch oct 2026) : offset direct du pseudo joueur. Potentiellement plus simple que la chain `+0x2270` qu'on connait.
3. **`AionCharacter_CharActPlayRateControl = 0x22B0`** : anim speed control.
4. **`AnimScriptInstance + 0x596 = PcRace uint8`** : race via mesh animation, mais serveur restreint les races vues en Abyss.
5. **Mob/NPC** : `OFFSET_MOB_RUNTIME_KEY = 0x26F0`, `OFFSET_MOB_NATIVE_DATA = 0x2728`, puis `NPC_ID a +0x04` sur NativeData → relie au catalogue drops/materials.
6. **Table RVAs** : EnvObjTable `0xF29DFB0`, GatherSourceTable `0xF2A4EC0`, gather map runtime `0xF3021F8` (a re-scanner par version).
7. **AoE/SkillNotice** : `AionSkillNoticeObj` → `DecalComponentArray` donne la forme et la progression visuelle des telegraph zones (0-100 %).
8. **PlayerCameraManager POV** : offsets POV_Location = `+0x3E0`, POV_FOV = `+0x410` directs sur la cam manager (plus simple que la chaine Pov).
9. Confirmation : **NCGuard user-mode only**, pas de driver kernel NCSoft. Un **kernel driver externe cote cheat** passe sans probleme.
10. Confirmation : **chiffrement reseau** confirme AES, cle hardcoded citee par un user mais sans contexte de session/handshake.

Ce qu'on peut **recouper** avec ce qu'on fait :
- Notre hook `NetDeltaSerialize` sur `DevPacketData_Common_CharState` est **la bonne approche** : HP/MP/FP ne sont pas en RAM fixe (`qkwkqhtn` et autres galeraient 50 posts a les trouver). Notre fil ESP Tools 618 cadrait bien le probleme.
- Notre OffsetFinder qui ecrit `offsets-aion2.json` par patch client est la **bonne strategie** (le thread montre que les offsets changent tous les 3-5 jours).
- Notre `AionWidget_HUD_UserInfo` -> HpUpdater etc. est confirme par `Gata` post #104 et `fpsnewboy` post #228.
- Notre `AionCharacter_CharActPlayRateControl` sur 0x22B0 — a ajouter dans nos offsets.
