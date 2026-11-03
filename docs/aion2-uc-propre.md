# Aion2 Reversal, Structs and Offsets - unknowncheats.me (thread 726048)

292 posts extraits.


## #1  -  scx1125  -  18th November 2025, 08:26 PM

Hey everyone

Aion 2 has been released today

we can start sharing info here — structs, offsets, engine details, and general reversing notes.

Platform: PC (Steam / Purple) ,mobile

Anti-Cheat: Unknown for now — maybe kernel-level anti-cheat

If anyone gets access to the pre-release build or files, feel free to post findings (module names, process info, memory dumps, etc.)


## #2  -  palacestreet  -  19th November 2025, 04:21 AM

10202
Code:
const uintptr_t OFFSET_APPENDSTRING = 0x3363160;

const uintptr_t OFFSET_GNAMES = 0xDA6D140;

const uintptr_t OFFSET_GOBJECTS = 0xD6349B0;

const uintptr_t OFFSET_GWORLD = 0xD929C18;

const uintptr_t OFFSET_PROCESSEVENT = 0x35656C0;
__________________
u have to try google it or chatgpt


## #3  -  ratknee148  -  24th November 2025, 05:19 PM

Quote:
Originally Posted by palacestreet
10202
Code:
const uintptr_t OFFSET_APPENDSTRING = 0x3363160;

const uintptr_t OFFSET_GNAMES = 0xDA6D140;

const uintptr_t OFFSET_GOBJECTS = 0xD6349B0;

const uintptr_t OFFSET_GWORLD = 0xD929C18;

const uintptr_t OFFSET_PROCESSEVENT = 0x35656C0;
Hi, can u say how to dump game offsets?


## #4  -  abc837932126  -  24th November 2025, 11:21 PM

Quote:
Originally Posted by ratknee148
Hi, can u say how to dump game offsets?
Code:
namespace Offsets
{
    constexpr int32 GObjects          = 0x0D6429B0;
    constexpr int32 AppendString      = 0x00000000;
    constexpr int32 GNames            = 0x0DA7B140;
    constexpr int32 GWorld            = 0x0D937C18;
    constexpr int32 ProcessEvent      = 0x035654D0;
    constexpr int32 ProcessEventIdx   = 0x0000004D;
}


## #5  -  ratknee148  -  25th November 2025, 05:42 AM

Quote:
Originally Posted by abc837932126
Code:
namespace Offsets
{
    constexpr int32 GObjects          = 0x0D6429B0;
    constexpr int32 AppendString      = 0x00000000;
    constexpr int32 GNames            = 0x0DA7B140;
    constexpr int32 GWorld            = 0x0D937C18;
    constexpr int32 ProcessEvent      = 0x035654D0;
    constexpr int32 ProcessEventIdx   = 0x0000004D;
}
How? And did u have game SDK?


## #6  -  jonxpin  -  26th November 2025, 05:11 AM



## #7  -  kiccccc  -  26th November 2025, 06:36 AM

Quote:
Originally Posted by jonxpin

Observing the HP, it seems to only have 1.5 times the attack speed; it's just that the animation looks exaggerated.


## #8  -  jonxpin  -  26th November 2025, 12:16 PM

Quote:
Originally Posted by kiccccc
Observing the HP, it seems to only have 1.5 times the attack speed; it's just that the animation looks exaggerated.
The game uses Unreal Engine 5, so it’s almost certainly using TimeDilation.


## #9  -  digital1q  -  26th November 2025, 01:38 PM

Does anyone have new offsets?
The game was updated a couple of hours ago.


## #10  -  palacestreet  -  26th November 2025, 06:27 PM

Code:
const uintptr_t OFFSET_APPENDSTRING = 0x3363860;

const uintptr_t OFFSET_GNAMES = 0xDA7D180;

const uintptr_t OFFSET_GOBJECTS = 0xD644930;

const uintptr_t OFFSET_GWORLD = 0xD939C18 ;
__________________
u have to try google it or chatgpt


## #11  -  xDead2K  -  26th November 2025, 07:16 PM

Quote:
Originally Posted by Medux
Hey there, I'm currently trying to develop an auto farming macro for AION 2 however I am having problems trying to get the macro working, Im using python for the development any tips on how I can be able to successfully work the macro to the game?
The anticheat hooks NtUserSendInput and other APIs.


## #12  -  digital1q  -  26th November 2025, 10:24 PM

I have an sdk. But I'm having trouble finding a player in this game. How do I get to the player's hp and max hp - who can tell me about classes or offsets?
How to navigate this in general are there guides?

It's just that this is the first time I've used something like this before, I've had enough to find offsets through CE and use what I need.

Can you share the materials, preferably with a demonstration of how to actually build a chain up to the player's hp in such a complicated structure?


## #13  -  Medux  -  27th November 2025, 04:55 AM

Quote:
Originally Posted by xDead2K
The anticheat hooks NtUserSendInput and other APIs.

https://i.imgur.com/Z44Op4c.png
Is there a workaround for this? Im using python for development


## #14  -  judarest  -  30th November 2025, 06:25 AM

Quote:
Originally Posted by Medux
Is there a workaround for this? Im using python for development
None on any keyboard events level.
I used variations like postmessage/sendmessage/send_keys also looked into autoit's controlsend and controlclicks which none of which works.

What works was:
au3.MouseClick("LEFT", 831, 958, 5)

this moves the cursor on the slot1 and clicks it 5 times.

The downside to this is that game must always be on the foreground.

So in theory, we can automate the game but by just using mouseclicks which pain in the [removed].


## #15  -  abc837932126  -  3rd December 2025, 01:42 PM

Quote:
Originally Posted by digital1q
I have an sdk. But I'm having trouble finding a player in this game. How do I get to the player's hp and max hp - who can tell me about classes or offsets?
How to navigate this in general are there guides?

It's just that this is the first time I've used something like this before, I've had enough to find offsets through CE and use what I need.

Can you share the materials, preferably with a demonstration of how to actually build a chain up to the player's hp in such a complicated structure?
Code:
export module AionCharacter;
import ACharacter;
import Attribute;
import Mem;

export class AionCharacter :public ACharacter {
public:
	std::string m_nickname{};
	uint32_t m_lv{};
	Attribute m_Attribute{};
public:
	using ACharacter::ACharacter;

	bool is_valid_npc();

	std::string get_nickname();

	void update_nickname();

	uint32_t get_lv();

	void update_lv();

	int32_t get_type();

	Attribute get_Attribute();

	void update_Attribute();


private:
	std::string get_player_name();

	std::string get_npc_name();

	int32_t type = -1;
};

bool AionCharacter::is_valid_npc()
{
	auto addr = Mem::Read<uint64_t>(ptr + 0x2290);
	return addr != 0;
}

std::string AionCharacter::get_nickname()
{
	if (get_type() == 1) {
		return get_player_name();

	}
	else if (get_type() == 0) {
		return get_npc_name();
	}

	return std::string();
}

void AionCharacter::update_nickname()
{
	m_nickname = get_nickname();
}


uint32_t AionCharacter::get_lv()
{
	auto addr = Mem::Read<uint64_t>(ptr + 0x2280);
	return Mem::Read<uint32_t>(addr + 0x20);
}

void AionCharacter::update_lv()
{
	m_lv = get_lv();
}

std::string AionCharacter::get_player_name()
{
	auto addr = Mem::Read<uint64_t>(ptr + 0x2270);
	addr = Mem::Read<uint64_t>(addr);
	auto fstr_name = Mem::Read<FString>(addr + 0x28);
	return fstr_name.read_string();
}

std::string AionCharacter::get_npc_name()
{
	auto addr = Mem::Read<uint64_t>(ptr + 0x2270);
	addr = Mem::Read<uint64_t>(addr);
	addr = Mem::Read<uint64_t>(addr + 0x10);
	addr = Mem::Read<uint64_t>(addr + 0x30);
	auto fstr_name = Mem::Read<FString>(addr + 0x10);
	return fstr_name.read_string();
}

Attribute AionCharacter::get_Attribute()
{
	auto addr = Mem::Read<uint64_t>(ptr + 0x2280);
	return Attribute(addr + 0xb0);
}

void AionCharacter::update_Attribute()
{
	auto addr = Mem::Read<uint64_t>(ptr + 0x2280);
	m_Attribute.set_ptr_read(addr + 0xb0);
}

int32_t AionCharacter::get_type()
{
	if (type != -1) {
		return type;
	}
	auto addr = Mem::Read<uint64_t>(ptr + 0x22A0);
	if (addr != 0) {
		type = 1; // player
		return type; // player
	}
	else {
		type = 0; // NPC
		return type; // NPC
	}
}



export module Attribute;

import UObject;
import Mem;


export class Attribute: public UObject {
public:
	using UObject::UObject;



	uint32_t get_Health_other();
	uint32_t get_HealthMax_other();



	uint32_t get_Health_self();
	uint32_t get_HealthMax_self();

	uint32_t get_Mana_self();
	uint32_t get_ManaMax_self();


};


uint32_t Attribute::get_Health_other()
{
	return Mem::Read<uint32_t>(ptr + 0x20);
}

uint32_t Attribute::get_HealthMax_other()
{
	return Mem::Read<uint32_t>(ptr + 0x8);
}

uint32_t Attribute::get_Health_self()
{
	return Mem::Read<uint32_t>(ptr + 0x98);
}

uint32_t Attribute::get_HealthMax_self()
{
	return Mem::Read<uint32_t>(ptr + 0x8);
}

uint32_t Attribute::get_Mana_self()
{
	return Mem::Read<uint32_t>(ptr + 0xb0);
}

uint32_t Attribute::get_ManaMax_self()
{
	return Mem::Read<uint32_t>(ptr + 0x20);
}
Old data, not sure if it's still valid.
It's been a while since I've done this; you can use CE or XDBG to search for it.


## #16  -  WhoWouldCheat  -  4th December 2025, 06:41 AM

Was playing around today with the game a bit. So P2W infested though, this won't make it past a year.

It's kind of weird how they have a movement speed check that'll resync you if go past ~60% but then they allow you to completely teleport across zones regardless of distance.

All cooldowns are server sided.
__________________
just a nn abusing a yzf r1


## #17  -  zonetime  -  4th December 2025, 03:44 PM

I am looking for some advice on bypassing NC Guard (NCSoft) for AION 2. Whenever I try to attach x64dbg to the game, the process terminates (crashes) immediately.

Here is what I have attempted so far:

Since the game refuses to run in Windows Test Mode, I booted using EfiGuard, loaded TitanHide, and tried to attach -> Failed.

Modified x64dbg strings using a Hex Editor to hide signatures -> Failed.

Created a custom kernel driver to generate a memory dump for IDA analysis, but I couldn't analyze it due to heavy obfuscation.

My questions are: Do I need to build a custom version of x64dbg (modifying the source code) to avoid detection? Or is there a better approach? What specific method can I use to successfully attach a debugger to this game?


## #18  -  fpsnewboy  -  4th December 2025, 09:51 PM

Quote:
Originally Posted by zonetime
I am looking for some advice on bypassing NC Guard (NCSoft) for AION 2. Whenever I try to attach x64dbg to the game, the process terminates (crashes) immediately.

Here is what I have attempted so far:

Since the game refuses to run in Windows Test Mode, I booted using EfiGuard, loaded TitanHide, and tried to attach -> Failed.

Modified x64dbg strings using a Hex Editor to hide signatures -> Failed.

Created a custom kernel driver to generate a memory dump for IDA analysis, but I couldn't analyze it due to heavy obfuscation.

My questions are: Do I need to build a custom version of x64dbg (modifying the source code) to avoid detection? Or is there a better approach? What specific method can I use to successfully attach a debugger to this game?
You need to pause the Aion2 process before the login screen, then use x64dbg and Scylla to get the dump for IDA.



I tested fly hack and it worked, but I didn’t see much use for it. I also tested movement speed / TimeDilation but without success, and in the ESP I’m having trouble calling GName on the actors to display their names. If anyone can help, I’d appreciate it. This is what I’ve managed to get so far ↓


Code:
#include <windows.h>
#include <vector>
#include <string>
#include <thread>
#include <chrono>

namespace NCGuardBypass
{

    uintptr_t FindPattern(uintptr_t start, size_t size, const char* sig, const char* mask)
    {
        size_t maskLen = strlen(mask);
        for (uintptr_t i = 0; i < size - maskLen; ++i)
        {
            bool found = true;
            for (size_t j = 0; j < maskLen; ++j)
            {
                if (mask[j] != '?' && sig[j] != *(char*)(start + i + j))
                {
                    found = false;
                    break;
                }
            }
            if (found) return start + i;
        }
        return 0;
    }

    void PatchLoadLibrarySuccess(uintptr_t addr)
    {
        if (!addr) return;

        DWORD old;
        VirtualProtect((void*)addr, 16, PAGE_EXECUTE_READWRITE, &old);

        // mov eax, 1     ; sucesso
        // ret
        // nop nop...
        BYTE patch[] = { 0xB8, 0x01, 0x00, 0x00, 0x00, 0xC3, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90 };
        memcpy((void*)addr, patch, sizeof(patch));

        VirtualProtect((void*)addr, 16, old, &old);
    }

    bool KillNCGuard()
    {
        HMODULE base = GetModuleHandleA(NULL);
        if (!base) return false;

        uintptr_t imageBase = (uintptr_t)base;
        SIZE_T imageSize = 0x40000000;

        const char* sig = "\xFF\x15????\x49\x89\x44\x24\x08\x48\x85\xC0\x0F\x85????";
        const char* mask = "xx????xxxxxxxxx????";

        uintptr_t result = FindPattern(imageBase, imageSize, sig, mask);
        if (!result)
        {
            const char* sig2 = "bb64.dll";
            result = FindPattern(imageBase, imageSize, sig2, "xxxxxxxx");
            if (result)
            {
                for (int i = 0; i < 300; i += 1)
                {
                    BYTE* p = (BYTE*)(result - i);
                    if (p[0] == 0xFF && p[1] == 0x15)
                    {
                        result = (uintptr_t)p;
                        break;
                    }
                }
            }
        }

        if (result)
        {
            PatchLoadLibrarySuccess(result);
            MessageBoxA(0, "NCGuard Bypass aplicado com sucesso!", "AION 2 - NCGuard", MB_ICONINFORMATION);
            return true;
        }

        MessageBoxA(0, "NCGuard nao encontrado.", "Erro", MB_ICONERROR);
        return false;
    }
}

// ==============================================================
// Thread principal
// ==============================================================
DWORD WINAPI MainThread(LPVOID)
{
    Sleep(8000);

    if (NCGuardBypass::KillNCGuard())
    {

    }

    return 0;
}

// ==============================================================
BOOL APIENTRY DllMain(HMODULE hModule, DWORD reason, LPVOID lpReserved)
{
    if (reason == DLL_PROCESS_ATTACH)
    {
        DisableThreadLibraryCalls(hModule);
        CreateThread(nullptr, 0, MainThread, nullptr, 0, nullptr);
    }
    return TRUE;
}
If a Fatal Error pops up, just don’t click on it.





Code:
GWorld Offset: 0xDA0AE18
GObjects Offset: 0xD7149B0
GNames 0xDB52940 → I’m not sure if it’s correct :D
With UEDumper you can extract the SDK and MDK.


## #19  -  jonxpin  -  5th December 2025, 01:02 PM

Quote:
Originally Posted by fpsnewboy
GWorld Offset: 0xDA0AE18
What is sig for gworld?


## #20  -  discipline  -  5th December 2025, 09:21 PM

Quote:
Originally Posted by fpsnewboy
You need to pause the Aion2 process before the login screen, then use x64dbg and Scylla to get the dump for IDA.



I tested fly hack and it worked, but I didn’t see much use for it. I also tested movement speed / TimeDilation but without success, and in the ESP I’m having trouble calling GName on the actors to display their names. If anyone can help, I’d appreciate it. This is what I’ve managed to get so far ↓


Code:
#include <windows.h>
#include <vector>
#include <string>
#include <thread>
#include <chrono>

namespace NCGuardBypass
{

    uintptr_t FindPattern(uintptr_t start, size_t size, const char* sig, const char* mask)
    {
        size_t maskLen = strlen(mask);
        for (uintptr_t i = 0; i < size - maskLen; ++i)
        {
            bool found = true;
            for (size_t j = 0; j < maskLen; ++j)
            {
                if (mask[j] != '?' && sig[j] != *(char*)(start + i + j))
                {
                    found = false;
                    break;
                }
            }
            if (found) return start + i;
        }
        return 0;
    }

    void PatchLoadLibrarySuccess(uintptr_t addr)
    {
        if (!addr) return;

        DWORD old;
        VirtualProtect((void*)addr, 16, PAGE_EXECUTE_READWRITE, &old);

        // mov eax, 1     ; sucesso
        // ret
        // nop nop...
        BYTE patch[] = { 0xB8, 0x01, 0x00, 0x00, 0x00, 0xC3, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90 };
        memcpy((void*)addr, patch, sizeof(patch));

        VirtualProtect((void*)addr, 16, old, &old);
    }

    bool KillNCGuard()
    {
        HMODULE base = GetModuleHandleA(NULL);
        if (!base) return false;

        uintptr_t imageBase = (uintptr_t)base;
        SIZE_T imageSize = 0x40000000;

        const char* sig = "\xFF\x15????\x49\x89\x44\x24\x08\x48\x85\xC0\x0F\x85????";
        const char* mask = "xx????xxxxxxxxx????";

        uintptr_t result = FindPattern(imageBase, imageSize, sig, mask);
        if (!result)
        {
            const char* sig2 = "bb64.dll";
            result = FindPattern(imageBase, imageSize, sig2, "xxxxxxxx");
            if (result)
            {
                for (int i = 0; i < 300; i += 1)
                {
                    BYTE* p = (BYTE*)(result - i);
                    if (p[0] == 0xFF && p[1] == 0x15)
                    {
                        result = (uintptr_t)p;
                        break;
                    }
                }
            }
        }

        if (result)
        {
            PatchLoadLibrarySuccess(result);
            MessageBoxA(0, "NCGuard Bypass aplicado com sucesso!", "AION 2 - NCGuard", MB_ICONINFORMATION);
            return true;
        }

        MessageBoxA(0, "NCGuard nao encontrado.", "Erro", MB_ICONERROR);
        return false;
    }
}

// ==============================================================
// Thread principal
// ==============================================================
DWORD WINAPI MainThread(LPVOID)
{
    Sleep(8000);

    if (NCGuardBypass::KillNCGuard())
    {

    }

    return 0;
}

// ==============================================================
BOOL APIENTRY DllMain(HMODULE hModule, DWORD reason, LPVOID lpReserved)
{
    if (reason == DLL_PROCESS_ATTACH)
    {
        DisableThreadLibraryCalls(hModule);
        CreateThread(nullptr, 0, MainThread, nullptr, 0, nullptr);
    }
    return TRUE;
}
If a Fatal Error pops up, just don’t click on it.
https://i.imgur.com/YpoECX5.png




Code:
GWorld Offset: 0xDA0AE18
GObjects Offset: 0xD7149B0
GNames 0xDB52940 → I’m not sure if it’s correct :D
With UEDumper you can extract the SDK and MDK.
Thanks for sharing, a noob question: how do you bypass the NcGuard? What kind of anti-cheat detection does it have? I heard that it's using kernel-level AC. Still new to this topic.


## #21  -  digital1q  -  6th December 2025, 03:47 PM

Quote:
You need to pause the Aion2 process before the login screen, then use x64dbg and Scylla to get the dump for IDA.
Can I tell you a little more about how to make a correct dump?


## #22  -  zonetime  -  6th December 2025, 08:14 PM

Quote:
Originally Posted by fpsnewboy
You need to pause the Aion2 process before the login screen, then use x64dbg and Scylla to get the dump for IDA.



I tested fly hack and it worked, but I didn’t see much use for it. I also tested movement speed / TimeDilation but without success, and in the ESP I’m having trouble calling GName on the actors to display their names. If anyone can help, I’d appreciate it. This is what I’ve managed to get so far ↓


Code:
#include <windows.h>
#include <vector>
#include <string>
#include <thread>
#include <chrono>

namespace NCGuardBypass
{

    uintptr_t FindPattern(uintptr_t start, size_t size, const char* sig, const char* mask)
    {
        size_t maskLen = strlen(mask);
        for (uintptr_t i = 0; i < size - maskLen; ++i)
        {
            bool found = true;
            for (size_t j = 0; j < maskLen; ++j)
            {
                if (mask[j] != '?' && sig[j] != *(char*)(start + i + j))
                {
                    found = false;
                    break;
                }
            }
            if (found) return start + i;
        }
        return 0;
    }

    void PatchLoadLibrarySuccess(uintptr_t addr)
    {
        if (!addr) return;

        DWORD old;
        VirtualProtect((void*)addr, 16, PAGE_EXECUTE_READWRITE, &old);

        // mov eax, 1     ; sucesso
        // ret
        // nop nop...
        BYTE patch[] = { 0xB8, 0x01, 0x00, 0x00, 0x00, 0xC3, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90 };
        memcpy((void*)addr, patch, sizeof(patch));

        VirtualProtect((void*)addr, 16, old, &old);
    }

    bool KillNCGuard()
    {
        HMODULE base = GetModuleHandleA(NULL);
        if (!base) return false;

        uintptr_t imageBase = (uintptr_t)base;
        SIZE_T imageSize = 0x40000000;

        const char* sig = "\xFF\x15????\x49\x89\x44\x24\x08\x48\x85\xC0\x0F\x85????";
        const char* mask = "xx????xxxxxxxxx????";

        uintptr_t result = FindPattern(imageBase, imageSize, sig, mask);
        if (!result)
        {
            const char* sig2 = "bb64.dll";
            result = FindPattern(imageBase, imageSize, sig2, "xxxxxxxx");
            if (result)
            {
                for (int i = 0; i < 300; i += 1)
                {
                    BYTE* p = (BYTE*)(result - i);
                    if (p[0] == 0xFF && p[1] == 0x15)
                    {
                        result = (uintptr_t)p;
                        break;
                    }
                }
            }
        }

        if (result)
        {
            PatchLoadLibrarySuccess(result);
            MessageBoxA(0, "NCGuard Bypass aplicado com sucesso!", "AION 2 - NCGuard", MB_ICONINFORMATION);
            return true;
        }

        MessageBoxA(0, "NCGuard nao encontrado.", "Erro", MB_ICONERROR);
        return false;
    }
}

// ==============================================================
// Thread principal
// ==============================================================
DWORD WINAPI MainThread(LPVOID)
{
    Sleep(8000);

    if (NCGuardBypass::KillNCGuard())
    {

    }

    return 0;
}

// ==============================================================
BOOL APIENTRY DllMain(HMODULE hModule, DWORD reason, LPVOID lpReserved)
{
    if (reason == DLL_PROCESS_ATTACH)
    {
        DisableThreadLibraryCalls(hModule);
        CreateThread(nullptr, 0, MainThread, nullptr, 0, nullptr);
    }
    return TRUE;
}
If a Fatal Error pops up, just don’t click on it.





Code:
GWorld Offset: 0xDA0AE18
GObjects Offset: 0xD7149B0
GNames 0xDB52940 → I’m not sure if it’s correct :D
With UEDumper you can extract the SDK and MDK.
Did you use a customized version of x64dbg or Cheat Engine to attach to the process? Since Scylla runs in user mode, wouldn't it get detected or cause the game to crash due to the Anti-Cheat protection?


## #23  -  zonetime  -  7th December 2025, 03:28 PM

yes plz...!!!!!!

Quote:
Originally Posted by digital1q
Can I tell you a little more about how to make a correct dump?
tell me more!!!!


## #24  -  zw5413  -  8th December 2025, 01:04 AM

so have any hacker？fly，speed or atk？


## #25  -  fpsnewboy  -  8th December 2025, 05:32 PM

Quote:
Originally Posted by jonxpin
What is sig for gworld?
I don’t have it. I use GSpots to get the GWorld and GObjects.

Quote:
Originally Posted by zonetime
Did you use a customized version of x64dbg or Cheat Engine to attach to the process? Since Scylla runs in user mode, wouldn't it get detected or cause the game to crash due to the Anti-Cheat protection?
I only used the modified Cheat Engine.




And to inject a DLL into the game, you just use SetWindowHook or ManualMap QueueUserAPC.


## #26  -  Indetical  -  14th December 2025, 10:55 PM

Hello, its first time im using SDK

im get nickname of player using NamePlate_C

but how to get npc/monster name?


## #27  -  Medux  -  15th December 2025, 07:32 AM

Quote:
Originally Posted by judarest
None on any keyboard events level.
I used variations like postmessage/sendmessage/send_keys also looked into autoit's controlsend and controlclicks which none of which works.

What works was:
au3.MouseClick("LEFT", 831, 958, 5)

this moves the cursor on the slot1 and clicks it 5 times.

The downside to this is that game must always be on the foreground.

So in theory, we can automate the game but by just using mouseclicks which pain in the [removed].

So I was able to fully run the macros and bypass the game's client side anti cheat, so the good part was you can simply use python and a python wrapper for the interception driver and just install the interception drivers using the github repository
oblitum/Interception

Then afterwards you can programmatically send keyboard inputs by sending scancodes to the python wrapper of the driver then everything just works but make sure the game is running on the foreground tho


## #28  -  qkwkqhtn  -  16th December 2025, 04:21 PM

Quote:
Originally Posted by fpsnewboy
I don’t have it. I use GSpots to get the GWorld and GObjects.



I only used the modified Cheat Engine.




And to inject a DLL into the game, you just use SetWindowHook or ManualMap QueueUserAPC.
GWorld
GObjects
GNames Is there a way to find these as IDA? I saw a video looking for the "Seamless Travel FlushLevelStreaming" string and it didn't work


## #29  -  judarest  -  18th December 2025, 04:53 PM

Quote:
Originally Posted by Medux
So I was able to fully run the macros and bypass the game's client side anti cheat, so the good part was you can simply use python and a python wrapper for the interception driver and just install the interception drivers using the github repository
oblitum/Interception

Then afterwards you can programmatically send keyboard inputs by sending scancodes to the python wrapper of the driver then everything just works but make sure the game is running on the foreground tho
Ive solved this already, using the same driver.


## #30  -  fpsnewboy  -  19th December 2025, 05:13 AM

Quote:
Originally Posted by qkwkqhtn
GWorld
GObjects
GNames Is there a way to find these as IDA? I saw a video looking for the "Seamless Travel FlushLevelStreaming" string and it didn't work
Search for %d ansi Fnames


## #31  -  qkwkqhtn  -  19th December 2025, 04:39 PM

Quote:
Originally Posted by fpsnewboy
Search for %d ansi Fnames
I can't find Gworld. What string should I use for this?


## #32  -  aakk47  -  21st December 2025, 07:40 AM

Quote:
Originally Posted by fpsnewboy
You need to pause the Aion2 process before the login screen, then use x64dbg and Scylla to get the dump for IDA.

I tested fly hack and it worked, but I didn’t see much use for it. I also tested movement speed / TimeDilation but without success, and in the ESP I’m having trouble calling GName on the actors to display their names. If anyone can help, I’d appreciate it. This is what I’ve managed to get so far ↓

With UEDumper you can extract the SDK and MDK.

Hello.
How did you find the pattern for NCGuard?
I tested it today and it didn't work. I think the offset has been updated.


## #33  -  qkwkqhtn  -  22nd December 2025, 03:34 PM

Quote:
Originally Posted by fpsnewboy
Search for %d ansi Fnames
Does the TW server work with Gspots by any chance??


## #34  -  lirikmel  -  26th December 2025, 05:59 PM

Code:
GWorld,0xDBCEC98,0x14DBCEC98
GNames,0xDD1EB00,0x14DD1EB00
GObjects,0xD8D5E60,0x14D8D5E60
Does anyone know where skill_id and their cooldowns (or simply whether they are active or not) are stored? And how to build a path to them in Reclass?


## #35  -  qkwkqhtn  -  27th December 2025, 04:47 PM

Quote:
Originally Posted by lirikmel
Code:
GWorld,0xDBCEC98,0x14DBCEC98
GNames,0xDD1EB00,0x14DD1EB00
GObjects,0xD8D5E60,0x14D8D5E60
Does anyone know where skill_id and their cooldowns (or simply whether they are active or not) are stored? And how to build a path to them in Reclass?
May I know how you got Gworld? I can't get it no matter what


## #36  -  lirikmel  -  27th December 2025, 07:33 PM

Quote:
Originally Posted by qkwkqhtn
May I know how you got Gworld? I can't get it no matter what
Make a dump and feed it to Gspot, it will find it.


## #37  -  qkwkqhtn  -  28th December 2025, 01:18 AM

Quote:
Originally Posted by lirikmel
Make a dump and feed it to Gspot, it will find it.
If you run the version of gspots 1.8, you get gname object, but not gworld. Is it a tw server? I'm a kr server

Quote:
Originally Posted by lirikmel
Make a dump and feed it to Gspot, it will find it.
Code:
----- GSpots : v1.8 -----


Unreal Engine Version: 5.3.2.0

GNames Offset: 0xE3D5EC0
GObjects Offset: 0xDF8CE60
GWorld signature not found...

Try running the game in the background and try again!


Press Enter to exit...
This is the only way for me


## #38  -  lirikmel  -  28th December 2025, 03:00 AM

Quote:
Originally Posted by qkwkqhtn
If you run the version of gspots 1.8, you get gname object, but not gworld. Is it a tw server? I'm a kr server



Code:
----- GSpots : v1.8 -----


Unreal Engine Version: 5.3.2.0

GNames Offset: 0xE3D5EC0
GObjects Offset: 0xDF8CE60
GWorld signature not found...

Try running the game in the background and try again!


Press Enter to exit...
This is the only way for me

tw server . It's strange because gnames is also showing incorrect results for you. Gnames usually doesn't find it, which is normal.

Code:
----- GSpots : v1.8 -----


Unreal Engine Version: Unreal Engine 4.

GWorld Offset: 0xDBCE298
GObjects Offset: 0xD8D5460
GNames signature not found...

Try running the game in the background and try again!


Press Enter to exit...


## #39  -  qkwkqhtn  -  28th December 2025, 04:52 AM

Quote:
Originally Posted by lirikmel
tw server . It's strange because gnames is also showing incorrect results for you. Gnames usually doesn't find it, which is normal.

Code:
----- GSpots : v1.8 -----


Unreal Engine Version: Unreal Engine 4.

GWorld Offset: 0xDBCE298
GObjects Offset: 0xD8D5460
GNames signature not found...

Try running the game in the background and try again!


Press Enter to exit...
I guess kr server and tw server are different. I can't do anything to get gworld


## #40  -  qkwkqhtn  -  28th December 2025, 06:06 PM

Where can I get player names in GWorld and Sdk


## #41  -  lirikmel  -  29th December 2025, 11:56 PM

Quote:
Originally Posted by qkwkqhtn
Where can I get player names in GWorld and Sdk
It seems that this structure is located in AApawn + 2200 offfset.


## #42  -  814003279  -  30th December 2025, 08:43 AM

ask for help!!!! The object names are all BPEnvObj_Cmmon_C. How to read the real name? How to distinguish which item it is?


## #43  -  qkwkqhtn  -  30th December 2025, 11:54 AM

Quote:
Originally Posted by 814003279
ask for help!!!! The object names are all BPEnvObj_Cmmon_C. How to read the real name? How to distinguish which item it is?
Where is the object?


## #44  -  814003279  -  30th December 2025, 04:53 PM

class AAionEnvObj : public AActor
{
public:
unsigned char UnknownData00_8[0xF0]; // 0x0338 (0x00F0) MISSED
class USceneComponent* RootSceneComponent; // 0x0428 (0x0008)
class USkeletalMeshComponent* SkeletalMeshComponent; // 0x0430 (0x0008)
class UCapsuleComponent* CapsuleComponent; // 0x0438 (0x0008)
class UCapsuleComponent* PickingCapsule; // 0x0440 (0x0008)
class UAionCharSessionComponent* CharSessionComponent; // 0x0448 (0x0008)
class UShapeComponent* OverlapCollisionComponent; // 0x0450 (0x0008)
class UOutlineComponent* OutlineComponent; // 0x0458 (0x0008)
class UAionEnvObjMovementComponent* EnvObjMovementComponent; // 0x0460 (0x0008)
unsigned char UnknownData01_6[0x80]; // 0x0468 (0x0080) MISSED
class UAionLevelUIComponent* NamePlateComponent; // 0x04E8 (0x0008)
class UAionLevelUIComponent* DebugInfoComponent; // 0x04F0 (0x0008)
class UAionActContainerMaterialComponent* ActContainerMaterialComponent; // 0x04F8 (0x0008)
class UAionActContainerComponent* ActContainerComponent; // 0x0500 (0x0008)
unsigned char UnknownData02_6[0x118]; // 0x0508 (0x0118) MISSED
TArray<class UStaticMeshComponent*> CachedObjectStaticMeshList; // 0x0620 (0x0010)
unsigned char UnknownData03_6[0x58]; // 0x0630 (0x0058) MISSED
class USkeletalMeshComponent* GuildDecoItemMesh; // 0x0688 (0x0008)
unsigned char UnknownData04_7[0x20]; // 0x0690 (0x0020) MISSED
};


I searched everywhere but couldn't find the item name

Can someone help me?


## #45  -  qkwkqhtn  -  31st December 2025, 01:29 AM

Quote:
Originally Posted by 814003279
class AAionEnvObj : public AActor
{
public:
unsigned char UnknownData00_8[0xF0]; // 0x0338 (0x00F0) MISSED
class USceneComponent* RootSceneComponent; // 0x0428 (0x0008)
class USkeletalMeshComponent* SkeletalMeshComponent; // 0x0430 (0x0008)
class UCapsuleComponent* CapsuleComponent; // 0x0438 (0x0008)
class UCapsuleComponent* PickingCapsule; // 0x0440 (0x0008)
class UAionCharSessionComponent* CharSessionComponent; // 0x0448 (0x0008)
class UShapeComponent* OverlapCollisionComponent; // 0x0450 (0x0008)
class UOutlineComponent* OutlineComponent; // 0x0458 (0x0008)
class UAionEnvObjMovementComponent* EnvObjMovementComponent; // 0x0460 (0x0008)
unsigned char UnknownData01_6[0x80]; // 0x0468 (0x0080) MISSED
class UAionLevelUIComponent* NamePlateComponent; // 0x04E8 (0x0008)
class UAionLevelUIComponent* DebugInfoComponent; // 0x04F0 (0x0008)
class UAionActContainerMaterialComponent* ActContainerMaterialComponent; // 0x04F8 (0x0008)
class UAionActContainerComponent* ActContainerComponent; // 0x0500 (0x0008)
unsigned char UnknownData02_6[0x118]; // 0x0508 (0x0118) MISSED
TArray<class UStaticMeshComponent*> CachedObjectStaticMeshList; // 0x0620 (0x0010)
unsigned char UnknownData03_6[0x58]; // 0x0630 (0x0058) MISSED
class USkeletalMeshComponent* GuildDecoItemMesh; // 0x0688 (0x0008)
unsigned char UnknownData04_7[0x20]; // 0x0690 (0x0020) MISSED
};


I searched everywhere but couldn't find the item name

Can someone help me?
Do you have a discord? Would you like to get to know me?


## #46  -  qkwkqhtn  -  31st December 2025, 02:01 PM

Quote:
Originally Posted by 814003279
class AAionEnvObj : public AActor
{
public:
unsigned char UnknownData00_8[0xF0]; // 0x0338 (0x00F0) MISSED
class USceneComponent* RootSceneComponent; // 0x0428 (0x0008)
class USkeletalMeshComponent* SkeletalMeshComponent; // 0x0430 (0x0008)
class UCapsuleComponent* CapsuleComponent; // 0x0438 (0x0008)
class UCapsuleComponent* PickingCapsule; // 0x0440 (0x0008)
class UAionCharSessionComponent* CharSessionComponent; // 0x0448 (0x0008)
class UShapeComponent* OverlapCollisionComponent; // 0x0450 (0x0008)
class UOutlineComponent* OutlineComponent; // 0x0458 (0x0008)
class UAionEnvObjMovementComponent* EnvObjMovementComponent; // 0x0460 (0x0008)
unsigned char UnknownData01_6[0x80]; // 0x0468 (0x0080) MISSED
class UAionLevelUIComponent* NamePlateComponent; // 0x04E8 (0x0008)
class UAionLevelUIComponent* DebugInfoComponent; // 0x04F0 (0x0008)
class UAionActContainerMaterialComponent* ActContainerMaterialComponent; // 0x04F8 (0x0008)
class UAionActContainerComponent* ActContainerComponent; // 0x0500 (0x0008)
unsigned char UnknownData02_6[0x118]; // 0x0508 (0x0118) MISSED
TArray<class UStaticMeshComponent*> CachedObjectStaticMeshList; // 0x0620 (0x0010)
unsigned char UnknownData03_6[0x58]; // 0x0630 (0x0058) MISSED
class USkeletalMeshComponent* GuildDecoItemMesh; // 0x0688 (0x0008)
unsigned char UnknownData04_7[0x20]; // 0x0690 (0x0020) MISSED
};


I searched everywhere but couldn't find the item name

Can someone help me?
Did you find it? I'm having a hard time finding the player, npc, monster


## #47  -  qkwkqhtn  -  1st January 2026, 05:40 PM

Quote:
Originally Posted by fpsnewboy
You need to pause the Aion2 process before the login screen, then use x64dbg and Scylla to get the dump for IDA.



I tested fly hack and it worked, but I didn’t see much use for it. I also tested movement speed / TimeDilation but without success, and in the ESP I’m having trouble calling GName on the actors to display their names. If anyone can help, I’d appreciate it. This is what I’ve managed to get so far ↓


Code:
#include <windows.h>
#include <vector>
#include <string>
#include <thread>
#include <chrono>

namespace NCGuardBypass
{

    uintptr_t FindPattern(uintptr_t start, size_t size, const char* sig, const char* mask)
    {
        size_t maskLen = strlen(mask);
        for (uintptr_t i = 0; i < size - maskLen; ++i)
        {
            bool found = true;
            for (size_t j = 0; j < maskLen; ++j)
            {
                if (mask[j] != '?' && sig[j] != *(char*)(start + i + j))
                {
                    found = false;
                    break;
                }
            }
            if (found) return start + i;
        }
        return 0;
    }

    void PatchLoadLibrarySuccess(uintptr_t addr)
    {
        if (!addr) return;

        DWORD old;
        VirtualProtect((void*)addr, 16, PAGE_EXECUTE_READWRITE, &old);

        // mov eax, 1     ; sucesso
        // ret
        // nop nop...
        BYTE patch[] = { 0xB8, 0x01, 0x00, 0x00, 0x00, 0xC3, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90, 0x90 };
        memcpy((void*)addr, patch, sizeof(patch));

        VirtualProtect((void*)addr, 16, old, &old);
    }

    bool KillNCGuard()
    {
        HMODULE base = GetModuleHandleA(NULL);
        if (!base) return false;

        uintptr_t imageBase = (uintptr_t)base;
        SIZE_T imageSize = 0x40000000;

        const char* sig = "\xFF\x15????\x49\x89\x44\x24\x08\x48\x85\xC0\x0F\x85????";
        const char* mask = "xx????xxxxxxxxx????";

        uintptr_t result = FindPattern(imageBase, imageSize, sig, mask);
        if (!result)
        {
            const char* sig2 = "bb64.dll";
            result = FindPattern(imageBase, imageSize, sig2, "xxxxxxxx");
            if (result)
            {
                for (int i = 0; i < 300; i += 1)
                {
                    BYTE* p = (BYTE*)(result - i);
                    if (p[0] == 0xFF && p[1] == 0x15)
                    {
                        result = (uintptr_t)p;
                        break;
                    }
                }
            }
        }

        if (result)
        {
            PatchLoadLibrarySuccess(result);
            MessageBoxA(0, "NCGuard Bypass aplicado com sucesso!", "AION 2 - NCGuard", MB_ICONINFORMATION);
            return true;
        }

        MessageBoxA(0, "NCGuard nao encontrado.", "Erro", MB_ICONERROR);
        return false;
    }
}

// ==============================================================
// Thread principal
// ==============================================================
DWORD WINAPI MainThread(LPVOID)
{
    Sleep(8000);

    if (NCGuardBypass::KillNCGuard())
    {

    }

    return 0;
}

// ==============================================================
BOOL APIENTRY DllMain(HMODULE hModule, DWORD reason, LPVOID lpReserved)
{
    if (reason == DLL_PROCESS_ATTACH)
    {
        DisableThreadLibraryCalls(hModule);
        CreateThread(nullptr, 0, MainThread, nullptr, 0, nullptr);
    }
    return TRUE;
}
If a Fatal Error pops up, just don’t click on it.
https://i.imgur.com/YpoECX5.png




Code:
GWorld Offset: 0xDA0AE18
GObjects Offset: 0xD7149B0
GNames 0xDB52940 → I’m not sure if it’s correct :D
With UEDumper you can extract the SDK and MDK.
Can anyone help me with NCGuard bypass? 7 seconds after pressing the start button in Purple, bb64.dll loads. I have to block it from loading in these 7 seconds, but it's too hard. I tried autoinjection, but there's also a problem with the game shutting down right away. How can I bypass NCGuard


## #48  -  qkwkqhtn  -  2nd January 2026, 04:12 PM

Quote:
Originally Posted by 814003279
class AAionEnvObj : public AActor
{
public:
unsigned char UnknownData00_8[0xF0]; // 0x0338 (0x00F0) MISSED
class USceneComponent* RootSceneComponent; // 0x0428 (0x0008)
class USkeletalMeshComponent* SkeletalMeshComponent; // 0x0430 (0x0008)
class UCapsuleComponent* CapsuleComponent; // 0x0438 (0x0008)
class UCapsuleComponent* PickingCapsule; // 0x0440 (0x0008)
class UAionCharSessionComponent* CharSessionComponent; // 0x0448 (0x0008)
class UShapeComponent* OverlapCollisionComponent; // 0x0450 (0x0008)
class UOutlineComponent* OutlineComponent; // 0x0458 (0x0008)
class UAionEnvObjMovementComponent* EnvObjMovementComponent; // 0x0460 (0x0008)
unsigned char UnknownData01_6[0x80]; // 0x0468 (0x0080) MISSED
class UAionLevelUIComponent* NamePlateComponent; // 0x04E8 (0x0008)
class UAionLevelUIComponent* DebugInfoComponent; // 0x04F0 (0x0008)
class UAionActContainerMaterialComponent* ActContainerMaterialComponent; // 0x04F8 (0x0008)
class UAionActContainerComponent* ActContainerComponent; // 0x0500 (0x0008)
unsigned char UnknownData02_6[0x118]; // 0x0508 (0x0118) MISSED
TArray<class UStaticMeshComponent*> CachedObjectStaticMeshList; // 0x0620 (0x0010)
unsigned char UnknownData03_6[0x58]; // 0x0630 (0x0058) MISSED
class USkeletalMeshComponent* GuildDecoItemMesh; // 0x0688 (0x0008)
unsigned char UnknownData04_7[0x20]; // 0x0690 (0x0020) MISSED
};


I searched everywhere but couldn't find the item name

Can someone help me?
Did you find it? I didn't find the nickname either


edit
I solved it


## #49  -  qkwkqhtn  -  3rd January 2026, 01:36 PM

Do you happen to know how to move in the background? I use "AddMovementInput", so I go without animation


## #50  -  pxj5920  -  6th January 2026, 10:04 PM

Does anyone know how to achieve acceleration? Could you share it?


## #51  -  unknownatom  -  24th January 2026, 07:51 AM

Quote:
Originally Posted by palacestreet
10202
Code:
const uintptr_t OFFSET_APPENDSTRING = 0x3363160;

const uintptr_t OFFSET_GNAMES = 0xDA6D140;

const uintptr_t OFFSET_GOBJECTS = 0xD6349B0;

const uintptr_t OFFSET_GWORLD = 0xD929C18;

const uintptr_t OFFSET_PROCESSEVENT = 0x35656C0;
Hi, do you have any idea of dumping sdk from aion 2 for android?


## #52  -  lirikmel  -  25th January 2026, 06:24 AM

Can anyone tell me where all the properties from the skill panel and effects on the character are stored?


## #53  -  kernelshy  -  25th January 2026, 11:29 AM

Could you tell me how to find FindObject and ProcessEvent, and how to find the rapid pathfinding CALL? This isn't regular pathfinding; it uses rapid pathfinding when clicking on a task. Also, how do I find the button CALL?


## #54  -  ic1003  -  26th January 2026, 09:19 AM

any idea how to do movement speed hack or attack speed hack?


## #55  -  nasicos  -  1st February 2026, 08:40 AM

Hello, experts! I need help
Hello, experts! I'm a newbie. Whenever I use debugging tools like Cheat Engine or x64dbg to open Aion 2, the game crashes. How do you guys manage to do it? Thank you for taking the time to explain.


## #56  -  richhks  -  2nd February 2026, 12:34 PM

Hello! I was wondering if anyone has figured out what the client is doing to encrypt it's network traffic? I have been writing a MITM style proxy and I have had decent success with it but I'm stuck at this because of the anti-cheat + the client's UE5 files? I cannot seem to find a DLL to decompile that could give me hints and also no debugger can be attached because of NCGuard.

If anyone would be interested in a high level overview of the network protocol I can share whatever I have discovered so far. cheers!


## #57  -  kernelshy  -  7th February 2026, 06:25 PM

Does the game use kernel anti-cheat? Why haven't I found it? I've traversed the kernel modules and didn't find any anti-cheat (AC) driver from the game company. I implemented a driver DLL injection into the target game process, but no matter how I tried, it failed. Can someone help me? Even just providing a direction would be appreciated!


## #58  -  xDead2K  -  7th February 2026, 07:17 PM

Quote:
Originally Posted by kernelshy
Does the game use kernel anti-cheat? Why haven't I found it? I've traversed the kernel modules and didn't find any anti-cheat (AC) driver from the game company. I implemented a driver DLL injection into the target game process, but no matter how I tried, it failed. Can someone help me? Even just providing a direction would be appreciated!
The anti-cheat is user-mode; you're unable to inject it because it hooks multiple APIs. Use Thread Hijack, and you won't have any problems.


## #59  -  GameWarrier  -  1st March 2026, 01:31 PM

Need your advice
Which method did you use for injection to AION2 process?


## #60  -  AzX  -  12th March 2026, 03:49 PM

someone know where i can find Class Data? What Class (Ranger,Templar,...) is picked f.e.


## #61  -  kernelshy  -  2nd April 2026, 09:19 PM

any idea how to do movement speed hack or attack speed hack?


## #62  -  siyu644321  -  12th June 2026, 09:33 AM

Hi everyone,

I'm currently working on parsing the NavMesh data for a specific game, but I've hit a brick wall trying to find the proper entry point/base pointer. Here is what I've done so far:

Static Analysis & Dump: I dumped the game data, but when inspecting it in IDA, the relevant data or strings appear as encrypted/obfuscated .ncg files, making it hard to trace back to the navigation structures.

Dynamic Analysis: I attempted to locate GWorld dynamically in memory to use it as a breakthrough point, but I haven't been able to find the genuine GWorld pointer or signature yet.

Since the traditional approaches aren't panning out due to the custom encryption/obfuscation, what other strategies or bypasses would you recommend?

Are there any reliable alternative patterns for locating FNavigationOctree / UNavigationSystemV1 directly without relying on GWorld, or any known tricks to handle the .ncg wrapper?

Any pointers or insights would be greatly appreciated. Thanks in advance!


## #63  -  NullReference  -  22nd June 2026, 03:18 AM

Quote:
Originally Posted by siyu644321
Hi everyone,

I'm currently working on parsing the NavMesh data for a specific game, but I've hit a brick wall trying to find the proper entry point/base pointer. Here is what I've done so far:

Static Analysis & Dump: I dumped the game data, but when inspecting it in IDA, the relevant data or strings appear as encrypted/obfuscated .ncg files, making it hard to trace back to the navigation structures.

Dynamic Analysis: I attempted to locate GWorld dynamically in memory to use it as a breakthrough point, but I haven't been able to find the genuine GWorld pointer or signature yet.

Since the traditional approaches aren't panning out due to the custom encryption/obfuscation, what other strategies or bypasses would you recommend?

Are there any reliable alternative patterns for locating FNavigationOctree / UNavigationSystemV1 directly without relying on GWorld, or any known tricks to handle the .ncg wrapper?

Any pointers or insights would be greatly appreciated. Thanks in advance!
This game has almost no encryption or obfuscation, the SDK is easy to dump.

GEngine: 0x0e7e87e0

Code:
uintptr_t MyClass::GetWorld()
{
    auto imageBase = _process.MainModule.Address;
    auto engine = _process[imageBase].Read<uintptr_t>(Offsets::GEngine);

    if (engine == 0)
        return 0;

    auto contextArray = _process[engine].Read<TArray<uintptr_t>>(Offsets::Engine::NextWorldContextHandle - 0x10);

    std::vector<uintptr_t> contexts = contextArray.Read(_process);

    for (uintptr_t context : contexts)
    {
        uintptr_t world = _process[context].Read<uintptr_t>(Offsets::WorldContext::ActiveNetDrivers + 0xB0);

        if (world != 0 && _process[world].Read<uintptr_t>(Offsets::World::GameState) != 0)
            return world;
    }

    return 0;
}


## #64  -  raldenmei  -  1st July 2026, 02:37 AM

Dear Sir,
I used FModel to unpack game assets from .pak/.utoc archives, but all the extracted .dat files show as garbled, unreadable binary data. I cannot tell whether they are encrypted, compressed with a proprietary algorithm, or just a custom format that FModel does not natively support.
I would really appreciate it if you could give me a brief hint on how to approach this issue, or recommend any tools for further analysis of these .dat files.https://www.gildor.org/smf/index.php...ach=6887;image


## #65  -  hyphenzo  -  16th September 2026, 01:25 AM

Does anyone here has any idea how to send keyboard & mouse inputs into the minimized game or at least if game windows is unfocused?


## #66  -  Kenneth Rodrigo  -  28th September 2026, 09:05 PM

Kenneth Rodrigo's Avatar

Join Date: Apr 2018
Posts: 424
Reputation: 1438
Rep Power: 213
Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.
Points: 11,602, Level: 13
Points: 11,602, Level: 13	Points: 11,602, Level: 13	Points: 11,602, Level: 13
Level up: 47%, 698 Points needed
Level up: 47%	Level up: 47%	Level up: 47%
Activity: 2.4%
Activity: 2.4%	Activity: 2.4%	Activity: 2.4%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
hopefully there will be stuff to use at release
Kenneth Rodrigo is offline


## #67  -  jonxpin  -  28th September 2026, 11:03 PM

I tested it on the TW server. Cheats don't really work, maybe just a 15-20% boost of speed. The only useful features i can think of are ESP and bot farming.


## #68  -  fpsnewboy  -  29th September 2026, 01:13 AM

Quote:
Originally Posted by jonxpin
I tested it on the TW server. Cheats don't really work, maybe just a 15-20% boost of speed. The only useful features i can think of are ESP and bot farming.

You can do super jump, a dash multiplier, a short-range teleport, and increase move speed by around 40–50%. I think you’re probably using time dilation right now. It’s also useful for animation canceling and can give you around a 20% DPS increase.

For move speed though, you need to find the walk value and multiply that. The run value is separate, so you have to find and multiply that one too. For example: walk 200, run 500, sprint 800. Then you just do the same thing for flying sprint, dash, and swimming.

You can also make an auto-skill feature for around +10% DPS, and a visual skin changer for outfits, pets, and stuff like that.



For anyone who wants to use an internal or make a bypass, try using ReflectiveDLLInjection with OpenProcess → VirtualAllocEx → WriteProcessMemory → CreateRemoteThread / NtCreateThreadEx.

You’ll need to hook bb64.dll. You don’t have to close it, just hook a few parts, for example ncgpelib64.dll, path_blacklist, and RtlAddVectoredExceptionHandler.

That said, I’d still recommend using an external unless you specifically want to support multi-instance.


## #69  -  yurigagarin  -  29th September 2026, 12:01 PM

Quote:
Originally Posted by fpsnewboy
You can do super jump, a dash multiplier, a short-range teleport, and increase move speed by around 40–50%. I think you’re probably using time dilation right now. It’s also useful for animation canceling and can give you around a 20% DPS increase.

For move speed though, you need to find the walk value and multiply that. The run value is separate, so you have to find and multiply that one too. For example: walk 200, run 500, sprint 800. Then you just do the same thing for flying sprint, dash, and swimming.

You can also make an auto-skill feature for around +10% DPS, and a visual skin changer for outfits, pets, and stuff like that.

https://imgur.com/a/tgS4R9K

For anyone who wants to use an internal or make a bypass, try using ReflectiveDLLInjection with OpenProcess → VirtualAllocEx → WriteProcessMemory → CreateRemoteThread / NtCreateThreadEx.

You’ll need to hook bb64.dll. You don’t have to close it, just hook a few parts, for example ncgpelib64.dll, path_blacklist, and RtlAddVectoredExceptionHandler.

That said, I’d still recommend using an external unless you specifically want to support multi-instance.
hey, new to mmo cheating. what features for cheats is best for mmo games like aion 2? aside from bot farming, and also, is external possible for doing speed, etc.


## #70  -  jonxpin  -  29th September 2026, 01:48 PM

Quote:
Originally Posted by yurigagarin
hey, new to mmo cheating. what features for cheats is best for mmo games like aion 2? aside from bot farming, and also, is external possible for doing speed, etc.
Animation speed (remove animation locks and faster dodging).
Increase attack speed by 15-20%.
Increase movement speed by about 20-40%.
Increase jump height by 30%.
Increase dash/evade distance by 30%.
Teleport behind target for back attacks (PVP) - Long teleport is not possible.
Field of View

fpsnewboy mentioned that the movement speed (not sprint) could be set to 40-50%, but I can't exceed 700-750 without causing my movement to rubberband.


## #71  -  yurigagarin  -  29th September 2026, 03:49 PM

Quote:
Originally Posted by jonxpin
Animation speed (remove animation locks and faster dodging).
Increase attack speed by 15-20%.
Increase movement speed by about 20-40%.
Increase jump height by 30%.
Increase dash/evade distance by 30%.
Teleport behind target for back attacks (PVP) - Long teleport is not possible.
Field of View

fpsnewboy mentioned that the movement speed (not sprint) could be set to 40-50%, but I can't exceed 700-750 without causing my movement to rubberband.
gotcha thank you. these can be done with external and doesnt need any bypass or turning off some stuff?


## #72  -  fpsnewboy  -  29th September 2026, 04:10 PM

Quote:
Originally Posted by yurigagarin
gotcha thank you. these can be done with external and doesnt need any bypass or turning off some stuff?
Yeah, with an external you can still do those features

Quote:
Originally Posted by jonxpin
Animation speed (remove animation locks and faster dodging).
Increase attack speed by 15-20%.
Increase movement speed by about 20-40%.
Increase jump height by 30%.
Increase dash/evade distance by 30%.
Teleport behind target for back attacks (PVP) - Long teleport is not possible.
Field of View

fpsnewboy mentioned that the movement speed (not sprint) could be set to 40-50%, but I can't exceed 700-750 without causing my movement to rubberband.
walk 200 -> 290
run 500 -> 745
sprint 800 -> 1192

And teleport up to 8m. You can use it in PvE too, so it can be useful for bosses.

I’m trying to make a skin changer, but it’s pretty annoying, especially with an external. If anyone can help, I’d appreciate it.

Right now I can only get skins from the store.

-> ResourceDatas[ECostumeMainMeshType]
-> AionCostumeArmorMeshData (352 bytes)
+0x08 SkeletalMesh FSoftObjectPtr
+0x108 EquipPartsSettingData FSoftObjectPtr
+0x15C EquipSlotOverrideMask


## #73  -  jonxpin  -  29th September 2026, 06:00 PM

Quote:
Originally Posted by fpsnewboy
Yeah, with an external you can still do those features



walk 200 -> 290
run 500 -> 745
sprint 800 -> 1192

And teleport up to 8m. You can use it in PvE too, so it can be useful for bosses.

I’m trying to make a skin changer, but it’s pretty annoying, especially with an external. If anyone can help, I’d appreciate it.

Right now I can only get skins from the store.

-> ResourceDatas[ECostumeMainMeshType]
-> AionCostumeArmorMeshData (352 bytes)
+0x08 SkeletalMesh FSoftObjectPtr
+0x108 EquipPartsSettingData FSoftObjectPtr
+0x15C EquipSlotOverrideMask
Ah yes, skin changer... it would be awesome to get that working, even if it's only visual.


## #74  -  yurigagarin  -  29th September 2026, 11:26 PM

Quote:
Originally Posted by fpsnewboy
Yeah, with an external you can still do those features



walk 200 -> 290
run 500 -> 745
sprint 800 -> 1192

And teleport up to 8m. You can use it in PvE too, so it can be useful for bosses.

I’m trying to make a skin changer, but it’s pretty annoying, especially with an external. If anyone can help, I’d appreciate it.

Right now I can only get skins from the store.

-> ResourceDatas[ECostumeMainMeshType]
-> AionCostumeArmorMeshData (352 bytes)
+0x08 SkeletalMesh FSoftObjectPtr
+0x108 EquipPartsSettingData FSoftObjectPtr
+0x15C EquipSlotOverrideMask
thanks a lot guys. any chance you guys have a code block example on how changing speed is being done externally? is there a decent UE mmorpg source to take inspiration of?


## #75  -  Engksanity  -  30th September 2026, 03:38 AM

These are fruitful information, I hope you guys release a project for this. I do try to make my own too! BTW do you have a dump for this game, Thanks!


## #76  -  AhmedPharaoh  -  30th September 2026, 06:44 AM

good work guys i wish u release it after global release


## #77  -  PraetorEaz  -  30th September 2026, 08:45 AM

Will be waiting for some release on 5th by some legend...................


## #78  -  yurigagarin  -  30th September 2026, 05:24 PM

Code:
Unreal Engine Version: 5.3.2.0
GWorld Offset: 0xEF73B08
GNames Offset: 0xF110380
GObjects Offset: 0xEC68920


## #79  -  Kenneth Rodrigo  -  30th September 2026, 07:58 PM

Kenneth Rodrigo's Avatar

Join Date: Apr 2018
Posts: 424
Reputation: 1438
Rep Power: 213
Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.
Points: 11,602, Level: 13
Points: 11,602, Level: 13	Points: 11,602, Level: 13	Points: 11,602, Level: 13
Level up: 47%, 698 Points needed
Level up: 47%	Level up: 47%	Level up: 47%
Activity: 2.4%
Activity: 2.4%	Activity: 2.4%	Activity: 2.4%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Im rdy for this game
Kenneth Rodrigo is offline


## #80  -  HX73D  -  30th September 2026, 08:30 PM

Forum Moderator & File Analyzer

HX73D's Avatar

Join Date: Dec 2022
Posts: 629
Reputation: 17350
Rep Power: 118
HX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UCHX73D Will always be a legend at UC
Points: 51,590, Level: 34
Points: 51,590, Level: 34	Points: 51,590, Level: 34	Points: 51,590, Level: 34
Level up: 6%, 3,410 Points needed
Level up: 6%	Level up: 6%	Level up: 6%
Activity: 99.2%
Activity: 99.2%	Activity: 99.2%	Activity: 99.2%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
File Approved
SHA256: d139d856e07b25ac13bb565a024b5c0514b449f246da7d45563efc9f78fa9e05 - SDK AION2.zip


Download: https://www.unknowncheats.me/forum/d...=file&id=58134

Thanks for sharing!
__________________
——————————————————————————————
Downloads • Rules • FAQ

New here? Quick help: Upload a File · Can't Download? · Can't Post / PM?

Got questions? Send me a PM
Want to help our team? See how you can join
——————————————————————————————


## #81  -  yurigagarin  -  30th September 2026, 08:37 PM

anyone have idea to remove the dodge animation delay?


## #82  -  yurigagarin  -  2nd October 2026, 09:46 PM

Quote:
Originally Posted by 814003279
ask for help!!!! The object names are all BPEnvObj_Cmmon_C. How to read the real name? How to distinguish which item it is?
anyone got a fix for this?


## #83  -  bankysgenius  -  3rd October 2026, 08:26 AM

SDK Has anyone made a tutorial video for beginners?


## #84  -  yurigagarin  -  3rd October 2026, 02:58 PM

Quote:
Originally Posted by bankysgenius
SDK Has anyone made a tutorial video for beginners?
dumpspace


## #85  -  HyanJae  -  3rd October 2026, 05:19 PM

Just a reminder. Aion 2 Anti Cheat checks what you are searching for in the background and can flag your account for just searching for something sus


## #86  -  Shard999  -  3rd October 2026, 05:25 PM

Quote:
Originally Posted by HyanJae
Just a reminder. Aion 2 Anti Cheat checks what you are searching for in the background and can flag your account for just searching for something sus
From my research so far, this seems like the most invasive non-kernel AC ever. Its kinda insane what it tracks xD


## #87  -  M7MDxx  -  3rd October 2026, 06:41 PM

Quote:
Originally Posted by HyanJae
Just a reminder. Aion 2 Anti Cheat checks what you are searching for in the background and can flag your account for just searching for something sus
How do we avoid this?


## #88  -  yurigagarin  -  3rd October 2026, 07:20 PM

Quote:
Originally Posted by HyanJae
Just a reminder. Aion 2 Anti Cheat checks what you are searching for in the background and can flag your account for just searching for something sus
been using anydesk from my main to my 2nd pc. didnt get anything yet


## #89  -  Shard999  -  3rd October 2026, 10:29 PM

Quote:
Originally Posted by yurigagarin
been using anydesk from my main to my 2nd pc. didnt get anything yet
Virtual input is also detected / flagged btw, it checks device information by hooking WH_CALLWNDPROC or just generally during WM_INPUT and then matches the input device for all kinds of known virtual / emulated devices. But probably wont lead to a ban on its own?


## #90  -  yurigagarin  -  3rd October 2026, 11:58 PM

Quote:
Originally Posted by Shard999
Virtual input is also detected / flagged btw, it checks device information by hooking WH_CALLWNDPROC or just generally during WM_INPUT and then matches the input device for all kinds of known virtual / emulated devices. But probably wont lead to a ban on its own?
what i meant was i was playing while building my cheat on my second pc through anydesk.


## #91  -  Danielci58  -  4th October 2026, 12:05 AM

Quote:
Originally Posted by yurigagarin
what i meant was i was playing while building my cheat on my second pc through anydesk.
How far did you got ? Im still at around 30% managed to decrypt everything with windbg but still lots of work to do.


## #92  -  yurigagarin  -  4th October 2026, 06:25 AM

Quote:
Originally Posted by Danielci58
How far did you got ? Im still at around 30% managed to decrypt everything with windbg but still lots of work to do.
there is no decryption


## #93  -  SigPhreak  -  4th October 2026, 05:15 PM

He's confusing encryption with packing. ^


## #94  -  hornochse89  -  4th October 2026, 10:25 PM

Hey there fellow Aion2 Devs.

Im currently running my own project with a injected Dll which works fine, i can read memory via manually mapped Dll and everything runs fine.
Radar is working, i can read names...

But im having problems hooking basically anything besides the swapbuffer for my imgui.
Basically any way i tried results in a 0xE0000014 Ac Crash.
Inline hooking, Veh Hooking, Midhook everything crashes the game (Integrity check i assume).

I dont really play around with anticheat enabled games normally so hitting integrity checks this often is something new to me.

Can someone give me a little hint into which direction i should look ?
Not looking for hand holding just a little hint.

Keep being awesome and keep having fun with the game guys.


## #95  -  Kenneth Rodrigo  -  4th October 2026, 10:40 PM

Kenneth Rodrigo's Avatar

Join Date: Apr 2018
Posts: 424
Reputation: 1438
Rep Power: 213
Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.
Points: 11,602, Level: 13
Points: 11,602, Level: 13	Points: 11,602, Level: 13	Points: 11,602, Level: 13
Level up: 47%, 698 Points needed
Level up: 47%	Level up: 47%	Level up: 47%
Activity: 2.4%
Activity: 2.4%	Activity: 2.4%	Activity: 2.4%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
FREE RELEASE TMR, hopefully some stuff will be useable by then.
Kenneth Rodrigo is offline


## #96  -  Shard999  -  4th October 2026, 11:00 PM

Am I the only who thinks its really harder to make a classic screen / pixelbot for this game than it is to make a cheat? The AC is so easy to bypass when you want to actually cheat, but trying to stay undetected by using YOLO / image recognition or even simple macros seems really difficult. It flags so many devices, inputs, drivers, etc.

Theres also an input collector for heuristic detection I assume, but it looks like its not enabled atm.


## #97  -  yurigagarin  -  4th October 2026, 11:09 PM

whats the best best chain for interactable resources?


## #98  -  hornochse89  -  4th October 2026, 11:36 PM

Quote:
Originally Posted by yurigagarin
whats the best best chain for interactable resources?
Code:
    enum class GatherSourceType : std::uint8_t {
    None      = 0,
    Metal     = 1,
    RareMetal = 2,
    Jewelry   = 3,
    Tree      = 4,
    Flower    = 5,
    Herb      = 6,
    Berry     = 7,
    Vegetable = 8,
    Shellfish = 9,
    Od        = 10,
    Max       = 11
};
PersistentLevel->Actors[i]

Code:
envData = ReadPtr(actor + 0x588);
envId   = Read<uint32_t>(envData + 0x28);

envRecord = LookupById(gameBase + 0xF29DFB0, envId);
usage     = Read<uint8_t>(envRecord + 0x34);

if (usage == 3) { // Gatherables only, not including wings
    key = Read<uint64_t>(envRecord + 0x38);
    gatherRecord = LookupByName(gameBase + 0xF2A4EC0, key);
    type = Read<uint8_t>(gatherRecord + 0x1C);
}


## #99  -  Blonde94  -  5th October 2026, 10:35 AM

Someone can advice me a good dumper? Or dumper 7 work?


## #100  -  vova1210  -  5th October 2026, 10:51 AM

Is the game worth making something out of it?


## #101  -  Kenneth Rodrigo  -  5th October 2026, 10:51 AM

Kenneth Rodrigo's Avatar

Join Date: Apr 2018
Posts: 424
Reputation: 1438
Rep Power: 213
Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.
Points: 11,602, Level: 13
Points: 11,602, Level: 13	Points: 11,602, Level: 13	Points: 11,602, Level: 13
Level up: 47%, 698 Points needed
Level up: 47%	Level up: 47%	Level up: 47%
Activity: 2.4%
Activity: 2.4%	Activity: 2.4%	Activity: 2.4%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
yes sir
Kenneth Rodrigo is offline


## #102  -  vova1210  -  5th October 2026, 11:00 AM

anyone got references for it or like a free source , just to take mainly references from it?


## #103  -  HyanJae  -  5th October 2026, 11:34 AM

I'd be happy with just a Radar tbh. that shows Hidden Cubes and Mats.


## #104  -  Gata  -  5th October 2026, 03:31 PM

GNames 0xF122440
GObjects 0xEC7A9E0
GWorld 0xEF85BC8
GEngine 0xF259A10
LocalPlayer 0xF3B57C0
GEngineLoop 0
AppendString 0
StaticFindObject 0
SkillBar 0
AbilitySystem 0
Telegraphs 0
BlockWindow 0
ProcessEvent 0

UObject_Vtable 0x00
UObject_Flags 0x08
UObject_InternalIndex 0x0C
UObject_Class 0x10
UObject_Name 0x18
UObject_Outer 0x20

AActor_CustomTimeDilation 0x84
AActor_InputComponent 0x180
AActor_RootComponent 0x1C0
AActor_InstanceComponents 0x280
AActor_BlueprintCreatedComponents 0x290
APawn_BaseEyeHeight 0x2B4
APawn_PlayerState 0x2C8
APawn_Controller 0x2D8
ACharacter_Mesh 0x3C0
ACharacter_CharacterMovement 0x3C8
ACharacter_CapsuleComponent 0x3D0
AionCharacter_CharActPlayRateControl 0x22B0
Mesh_GlobalAnimRateScale 0xAF0

UWorld_PersistentLevel 0x30
UWorld_Levels 0x178
UWorld_OwningGameInstance 0x1E8
UGameInstance_LocalPlayers 0x38
UGameInstance_UISystem 0x3C8
UGameInstance_InputSystem 0x418
AionUISystem_WidgetTree 0x680
ULevel_Actors 0xA0
ULevel_WorldSettings 0x2C0
AWorldSettings_TimeDilation 0x468
AWorldSettings_CinematicTimeDilation 0x46C

UPlayer_PlayerController 0x30
APlayerController_AcknowledgedPawn 0x378
APlayerController_AHUD 0x3F0
APlayerController_PlayerCameraManager 0x3F8
APlayerCameraManager_DefaultFOV 0x2C0
APlayerCameraManager_CameraCachePrivate 0x1410
FCameraCacheEntry_POV 0x10
FMinimalViewInfo_FOV 0x30
FMinimalViewInfo_DesiredFOV 0x34
AController_ControlRotation 0x320
APlayerState_PlayerNamePrivate 0x340

USceneComponent_RelativeLocation 0x128
USceneComponent_RelativeRotation 0x140
USceneComponent_RelativeScale3D 0x158
UCharacterMovementComponent_JumpZVelocity 0x198
UCharacterMovementComponent_MovementMode 0x221
UCharacterMovementComponent_MaxWalkSpeed 0x268
UCharacterMovementComponent_MaxSwimSpeed 0x270
UCharacterMovementComponent_MaxFlySpeed 0x274
UCharacterMovementComponent_MaxClimbSpeed -1
UPrimitiveComponent_BodyInstance 0x348
FBodyInstance_CollisionEnabled 0x17
UCapsuleComponent_CapsuleHalfHeight 0x508
UCapsuleComponent_CapsuleRadius 0x50C
USkeletalMeshComponent_CachedComponentSpaceTransforms 0x960

FUObjectItem_Object 0x00
FUObjectItem_Flags 0x08
FUObjectItem_Size 0x18
NamePoolCurrentBlockOffset 0x08
NamePoolCurrentByteCursorOffset 0x0C
NamePoolBlocksOffset 0x10
NameBlockSize 0x20000
NamePoolMaxBlocks 8192

UserWidget_WidgetTree 0x268
HUD_QuickSlotGroup_ButtonTooltip 0x400
HUD_QuickSlotGroup_ItemSlot0 0x478
HUD_QuickSlotGroup_SkillSlot0 0x4B8
HUD_QuickSlotSkill_CooltimeBar 0x418
HUD_QuickSlotSkill_CooltimeText 0x420
HUD_QuickSlotHolder_SlotId_Set 0x688
HUD_QuickSlotHolder_P_Content 0x6B0
HUD_QuickSlotHolder_UseButton 0x6B8
HUD_QuickSlotHolder_P_HotKey 0x700
AionWidget_UseSkillSlot_P_Img_SkillIcon 0x418
AionWidget_UseSkillSlot_P_Txt_SkillLv 0x440
AionWidget_UseSkillSlot_P_HotKey 0x460
UTextBlock_Text 0x1A8
AionImage_DefaultResource 0x328
AionImage_MouseOverTooltipText 0x338
AionWidget_QuickSlotBase_P_Img_Icon 0x400
AionWidget_QuickSlotSkill_P_Txt_SkillLv 0x910
AionDialogWidget_SkillSlotTooltip_SkillBasicInfoUpdater 0x518
AionDialogWidget_SkillSlotTooltip_P_RTxt_Name 0x528
AionUIComponent_SkillBasicInfoUpdater_P_HotKey 0x70
AionUIComponent_SkillBasicInfoUpdater_P_RTxt_SkillName 0x80
AionWidget_HotKey_LinkedWidgetName 0x3C8
AionWidget_HotKey_P_RTxt_HotKey 0x408
AionWidget_QuickSlotListContextSkillItem_SkillBasicInfoUpdater 0x420
AionWidget_QuickSlotListContextSkillItem_P_UseSkillSlot 0x428
AionWidget_QuickSlotListGroup_P_ContextSkill_1 0x410
AionWidget_QuickSlotListGroup_P_ContextSkill_2 0x418
AionUIComponent_PlayerInfoUpdater_P_Txt_Level 0x60
AionUIComponent_PlayerInfoUpdater_P_Txt_ItemLevel 0x68
AionUIComponent_PlayerInfoUpdater_P_Txt_CharacterName 0x88
AionDialogWidget_HUD_Menu_PlayerInfoUpdater 0x548
AionDialogWidget_HUD_Menu_P_Txt_Lv 0x588

AionInputSystem_MappingContext 0x198
AionInputSystem_Key1 0
AionInputSystem_NumberedKeyStride 1
QuickSlotSystem_SlotHeld 0
InputMappingContext_Held 0
AionButton_Pressed 0
AionPlayerInput_Key1 0
AionPlayerInput_Key1Mask 0x02

UStruct_SuperField 0x40
UStruct_Children 0x48
UField_Next 0x28
UFunction_FunctionFlags 0xB0
UFunction_Func 0xD8
UStaticMeshComponent_StaticMesh 0x528
UInstancedStaticMeshComponent_PerInstanceSMData 0x5C0


## #105  -  WhoWouldCheat  -  5th October 2026, 04:11 PM

Teleport is patched for long distances at-least

CD/attack speed is patched for super minor gains now

Movement speed is iffy but timescale still works

Dumb enemies still works fine

Also just got a 24 hour restriction so not sure what I tripped, might've been me just mining too many nodes with radar
__________________
just a nn abusing a yzf r1


## #106  -  protonbot1  -  5th October 2026, 04:13 PM

Quote:
Originally Posted by WhoWouldCheat
Teleport is patched for long distances at-least

CD/attack speed is patched for super minor gains now

Movement speed is iffy but timescale still works

Dumb enemies still works fine

Also just got a 24 hour restriction so not sure what I tripped, might've been me just mining too many nodes with radar
is it possible just for ESP for chest , and collectibles. and only infinite buff


## #107  -  yurigagarin  -  5th October 2026, 07:07 PM

Quote:
Originally Posted by hornochse89
Code:
    enum class GatherSourceType : std::uint8_t {
    None      = 0,
    Metal     = 1,
    RareMetal = 2,
    Jewelry   = 3,
    Tree      = 4,
    Flower    = 5,
    Herb      = 6,
    Berry     = 7,
    Vegetable = 8,
    Shellfish = 9,
    Od        = 10,
    Max       = 11
};
PersistentLevel->Actors[i]

Code:
envData = ReadPtr(actor + 0x588);
envId   = Read<uint32_t>(envData + 0x28);

envRecord = LookupById(gameBase + 0xF29DFB0, envId);
usage     = Read<uint8_t>(envRecord + 0x34);

if (usage == 3) { // Gatherables only, not including wings
    key = Read<uint64_t>(envRecord + 0x38);
    gatherRecord = LookupByName(gameBase + 0xF2A4EC0, key);
    type = Read<uint8_t>(gatherRecord + 0x1C);
}
https://i.imgur.com/M5aWFzp.png
how do u find EnvData pointer and EnvObj ID?

Quote:
Originally Posted by WhoWouldCheat
Teleport is patched for long distances at-least

CD/attack speed is patched for super minor gains now

Movement speed is iffy but timescale still works

Dumb enemies still works fine

Also just got a 24 hour restriction so not sure what I tripped, might've been me just mining too many nodes with radar
movement speed 49% is still smooth. how was attack speed done before? ive been trying but with no luck


## #108  -  zjebolud1234  -  5th October 2026, 07:16 PM

works fine hehe


## #109  -  Kenneth Rodrigo  -  5th October 2026, 08:11 PM

Kenneth Rodrigo's Avatar

Join Date: Apr 2018
Posts: 424
Reputation: 1438
Rep Power: 213
Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.
Points: 11,602, Level: 13
Points: 11,602, Level: 13	Points: 11,602, Level: 13	Points: 11,602, Level: 13
Level up: 47%, 698 Points needed
Level up: 47%	Level up: 47%	Level up: 47%
Activity: 2.4%
Activity: 2.4%	Activity: 2.4%	Activity: 2.4%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
need stuff to release
Kenneth Rodrigo is offline


## #110  -  zjebolud1234  -  5th October 2026, 08:15 PM



## #111  -  Kenneth Rodrigo  -  5th October 2026, 08:25 PM

Kenneth Rodrigo's Avatar

Join Date: Apr 2018
Posts: 424
Reputation: 1438
Rep Power: 213
Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.Kenneth Rodrigo -- if your rep is this high please leave the forum.
Points: 11,602, Level: 13
Points: 11,602, Level: 13	Points: 11,602, Level: 13	Points: 11,602, Level: 13
Level up: 47%, 698 Points needed
Level up: 47%	Level up: 47%	Level up: 47%
Activity: 2.4%
Activity: 2.4%	Activity: 2.4%	Activity: 2.4%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by zjebolud1234
https://i.imgur.com/EjgKyGD.png
NEED IT
Kenneth Rodrigo is offline


## #112  -  zjebolud1234  -  5th October 2026, 08:29 PM

maybe i will share it )


## #113  -  thatboyxgm  -  5th October 2026, 09:22 PM

Quote:
Originally Posted by zjebolud1234
https://i.imgur.com/EjgKyGD.png
Does this also have gathering?


## #114  -  ZixMatrix  -  5th October 2026, 10:37 PM

thanks "Gata"

actually i dont know how to dumb, or i to dumb to use dumper-7
because of the NCGuard


## #115  -  yurigagarin  -  5th October 2026, 10:57 PM

Quote:
Originally Posted by zjebolud1234
https://i.imgur.com/EjgKyGD.png
how do u find EnvData pointer and EnvObj ID?


## #116  -  Danielci58  -  6th October 2026, 12:02 AM

Quote:
Originally Posted by WhoWouldCheat
Teleport is patched for long distances at-least

CD/attack speed is patched for super minor gains now

Movement speed is iffy but timescale still works

Dumb enemies still works fine

Also just got a 24 hour restriction so not sure what I tripped, might've been me just mining too many nodes with radar
If I do some memory edit like a tool that would play with the character customizer sliders how detected is that?


## #117  -  hornochse89  -  6th October 2026, 12:03 AM

Exclamation
Quote:
Originally Posted by yurigagarin
how do u find EnvData pointer and EnvObj ID?
Envdata is the pointer at actor + 0x588

Objid is at envdata + 0x28

Code:
uintptr_t envData = Read<uintptr_t>(actor + 0x588);
if (envData) {
uint32_t envObjId = Read<uint32_t>(envData + 0x28);
}
As of update earlier today.
Have fun mate


## #118  -  Blonde94  -  6th October 2026, 12:31 AM

Quote:
Originally Posted by ZixMatrix
https://i.imgur.com/JNLs24n.jpeg

thanks "Gata"

actually i dont know how to dumb, or i to dumb to use dumper-7
because of the NCGuard
Nice work man, how did u find loot esp? i mean EnvData pointer and EnvObj ID


## #119  -  WhoWouldCheat  -  6th October 2026, 02:36 AM

Quote:
Originally Posted by yurigagarin
how do u find EnvData pointer and EnvObj ID?



movement speed 49% is still smooth. how was attack speed done before? ive been trying but with no luck

Old speed was about ~10x without issues

Attack speed I just did by weapon stats themselves but even now that seems to get checked for validity now based on CD

*Attack range still might work but already uninstalled the game after seeing all the mandatory subs to even play properly
__________________
just a nn abusing a yzf r1


## #120  -  ZixMatrix  -  6th October 2026, 05:13 AM

Quote:
Originally Posted by yurigagarin
how do u find EnvData pointer and EnvObj ID?
what is that ?

i just use the offset shared by geta that copy paste that to my ai agent
"make esp, external, simple menu, insert open close menu"

it just works


## #121  -  apple2626  -  6th October 2026, 10:32 AM

Quote:
Originally Posted by ZixMatrix
what is that ?

i just use the offset shared by geta that copy paste that to my ai agent
"make esp, external, simple menu, insert open close menu"

it just works
How do you even keep the process reading without anti-cheat disturbing it. Do you bypass the AC?


## #122  -  PraetorEaz  -  6th October 2026, 11:21 AM

Quote:
Originally Posted by zjebolud1234
maybe i will share it )
Pls do !!!

Quote:
Originally Posted by zjebolud1234
maybe i will share it )
Pls do !!!

Anyone releasing anything 👀


## #123  -  yurigagarin  -  6th October 2026, 11:52 AM

Quote:
Originally Posted by WhoWouldCheat
Old speed was about ~10x without issues

Attack speed I just did by weapon stats themselves but even now that seems to get checked for validity now based on CD

*Attack range still might work but already uninstalled the game after seeing all the mandatory subs to even play properly
are you external or internal? what other features did u try


## #124  -  RuneBind  -  6th October 2026, 02:13 PM

He's external, read the layout design. i just wonder if he got UFunction to work and is he using a driver or direct read from the game, the AC shouldn't allow it and if it does, it means the AC basically a hollow-shell.


## #125  -  Johnschiffer  -  6th October 2026, 03:40 PM

Where is the attack speed? Even a small increase would help


## #126  -  tesla24  -  6th October 2026, 04:49 PM

Quote:
Originally Posted by zjebolud1234
https://i.imgur.com/EjgKyGD.png
how did u bypass AC?


## #127  -  mevisitor  -  6th October 2026, 10:36 PM

anyone do market snipe bot and got temp restriction on market ? i want to know how long it is but im able to sell things on market just fine

i might fetching market too fast like 1 item per second

edit. its 24 hours temp ban


## #128  -  hoamzyourbae  -  7th October 2026, 12:07 AM

Cool
Quote:
Originally Posted by zjebolud1234
maybe i will share it )
thanks! illbe waitin for it


## #129  -  SigPhreak  -  7th October 2026, 01:08 AM

We will be releasing an Internal leveling (for both factions) and dynamic combat bot covering all classes in the coming week, however will be sub based so not promoting actual links here.

I would advise most people on here if doing their own project to stay external. Will release some bits of helpful info here a long with our release to help those working on externals out


## #130  -  vova1210  -  7th October 2026, 04:23 AM

Is it possible anyone can help out with latest SDK/Offsets Dump?


## #131  -  KeanuCorpse  -  7th October 2026, 06:07 AM

some p2c provider has god mode listed as an option, but the cantbedamaged offset or whatever was client sided and did nothing for me, was this placebo. I'm having a hell of a time getting teleport to work as well, I have a bunch of cool stuff and im happy with the progress and thank you all so much for the help, but i would really love to get that teleport working. (figured out teleport eventually, its kind of futile as a feature though, the server is pretty strict)

Any thoughts on the p2c god mode option, seller bait trash? or maybe this guy is internal and doing things differently? I dumped externally so im about to download whatever sdk was posted here and compare to see if im missing stuff or something.

another guy has "Increase dash/evade distance by ~30%." but i couldnt find that in the SDK lol. I'm guessing he used his mega forehead reversal skills and not an SDK.

and "Reduce cooldown timers by ~300ms per use."

I've done the rest, im trying to stabilize fly, it looks so funny, might as well call it fish mode, it makes you look like a little fish swimming around.

Quote:
Originally Posted by vova1210
Is it possible anyone can help out with latest SDK/Offsets Dump?
is the one from the 30th on here out of date? If so I can upload tomorrow, I was just going to sleep. Having a brain bleed from working on this all day.

Quote:
Originally Posted by SigPhreak
We will be releasing an Internal leveling (for both factions) and dynamic combat bot covering all classes in the coming week, however will be sub based so not promoting actual links here.

I would advise most people on here if doing their own project to stay external. Will release some bits of helpful info here a long with our release to help those working on externals out


What does that even mean lol, are you releasing it, or is it sub based. If you arent releasing it here in some capacity then that was self promotion.

Quote:
Originally Posted by Johnschiffer
Where is the attack speed? Even a small increase would help
Honestly im not sure what people meant by this, you get around the 15% claimed through animation and time dilation. I'm not sure if thats what they were talking about and just obfuscated how they said it so it sounded cooler.

I was super underwhelmed by what was possible, but never the less i made something cool and im happy with it. Perhaps more is possible internally, idk. I know people claim some things that I was unable to do thus far.


## #132  -  SigPhreak  -  7th October 2026, 09:05 AM

@KeanuCorpse

If you want to think that is self promotion then be my guest?

The sdk.. you need to get a bit more involved than the SDK for those types of features... certainly wont be mentioning those methods here as you are such a stricler on the rules it appears.

as far as the god mode question, I can say with certainty it is not possible. So yes, confirmed bait trash.


## #133  -  ZixMatrix  -  7th October 2026, 10:35 AM

Quote:
Originally Posted by apple2626
How do you even keep the process reading without anti-cheat disturbing it. Do you bypass the AC?
yes the AC kill my process each time
so the solution is to read it again everytime the ac kill it

logic =
1. attach, read, esp working
2. AC killed it
3. loop again

im sorry i cant explain this more to you
i just ask my ai agent to do it for me

after this maintenance i will upload my latest external menu

so far already optimize, covering all important esp for all monster, resources, etc

@https://www.unknowncheats.me/forum/members/6954410.html

can you do unlimited fly ? because it's really good for grinding resource

Quote:
Originally Posted by KeanuCorpse
.
can you do unlimited fly ? because it's really good for grinding resource


## #134  -  Shard999  -  7th October 2026, 11:02 AM

Quote:
Originally Posted by ZixMatrix
yes the AC kill my process each time
so the solution is to read it again everytime the ac kill it

logic =
1. attach, read, esp working
2. AC killed it
3. loop again

im sorry i cant explain this more to you
i just ask my ai agent to do it for me

after this maintenance i will upload my latest external menu

so far already optimize, covering all important esp for all monster, resources, etc

@https://www.unknowncheats.me/forum/members/6954410.html



can you do unlimited fly ? because it's really good for grinding resource



can you do unlimited fly ? because it's really good for grinding resource

Every time the AC is killing your process, it is logged and emergency reported. you WILL get banned doing this.


## #135  -  iamLau  -  7th October 2026, 11:27 AM

Updated offsets for the latest client patch.

Code:
constexpr uintptr_t OFFSET_GOBJECTS      = 0xEC8C9A0;
constexpr uintptr_t OFFSET_APPENDSTRING  = 0x0;
constexpr uintptr_t OFFSET_GNAMES        = 0xF134440;
constexpr uintptr_t OFFSET_GWORLD        = 0xEF97BC8;
constexpr uintptr_t OFFSET_PROCESSEVENT  = 0x42066B0;
constexpr int32_t   INDEX_PROCESSEVENT   = 0x4D;
OFFSET_APPENDSTRING is 0, use FNamePool directly.


## #136  -  apple2626  -  7th October 2026, 11:57 AM

Quote:
Originally Posted by ZixMatrix
yes the AC kill my process each time
so the solution is to read it again everytime the ac kill it

logic =
1. attach, read, esp working
2. AC killed it
3. loop again

im sorry i cant explain this more to you
i just ask my ai agent to do it for me

after this maintenance i will upload my latest external menu

so far already optimize, covering all important esp for all monster, resources, etc

@https://www.unknowncheats.me/forum/members/6954410.html



can you do unlimited fly ? because it's really good for grinding resource



can you do unlimited fly ? because it's really good for grinding resource
Managed to get through that.

I am unable to send action keys through the game. Seems like AC filters them out. For exmaple, when I send "F" key , it presses that in chat but in actual interraction it does not work.

Quote:
Originally Posted by SigPhreak
We will be releasing an Internal leveling (for both factions) and dynamic combat bot covering all classes in the coming week, however will be sub based so not promoting actual links here.

I would advise most people on here if doing their own project to stay external. Will release some bits of helpful info here a long with our release to help those working on externals out
Hey, how do you make the game accept interaction? "F" for example does not make my character interact, but works in game chat


## #137  -  Astrihacks  -  7th October 2026, 12:05 PM

Why all the secrecy around sharing this? You only need the Cheat Engine MCP, whose source code is easy to find, and there are even tutorials on YouTube showing how to set it up. Then you can connect it to any Visual Studio IDE or Antigravity with OpenCode and ask it to make the hacks on its own. Why make such a big mystery out of it? I guess there’s just not much interest in sharing how hacks can be made with AI.


## #138  -  Shard999  -  7th October 2026, 12:32 PM

Quote:
Originally Posted by Astrihacks
Why all the secrecy around sharing this? You only need the Cheat Engine MCP, whose source code is easy to find, and there are even tutorials on YouTube showing how to set it up. Then you can connect it to any Visual Studio IDE or Antigravity with OpenCode and ask it to make the hacks on its own. Why make such a big mystery out of it? I guess there’s just not much interest in sharing how hacks can be made with AI.
You realize this is not a singleplayer game without anti-cheat?


## #139  -  Astrihacks  -  7th October 2026, 12:44 PM

Quote:
Originally Posted by Shard999
You realize this is not a singleplayer game without anti-cheat?
It works for any online game. The only limitation depends on the AI you use and how you structure your prompt. Using this method, I created my own custom hacks for Aniimo, Aion 2, and Genshin Impact.

Note/recommendation: I recommend using free OpenCode models or accessing them via API, although the free models work very well.
I also recommend reading this thread:
Undetected Cheat Engine + Driver | 2023 | Bypass Anticheats (BE / EAC)


## #140  -  KeanuCorpse  -  7th October 2026, 05:03 PM

Quote:
Originally Posted by SigPhreak
@KeanuCorpse

If
Lol I dont care about the rules as much as your oxymoron of a statement confounded me at 3am, i didnt report you or anything. Personally, i think swear filters and rules on a hacking forum is forehead level enforcement. Like yes, lets hemorrhage money out of billion dollar companies by corrupting their work, but you cant say poo poo and shickelfritz. I've always hated the rules here, but they do keep the place sanitary so I guess they are working.

Quote:
Originally Posted by ZixMatrix



can you do unlimited fly ? because it's really good for grinding resource


No can you? I rubberband but my speed adjustment for fly isnt working, so I was going to try again today to slow it down. I think its simply moving too fast, but from what i've seen, teleport, the several other tricks i found for movement that other people have not mentioned, all seem bound by the same server enforcement. I can slow teleport down and loop it, but its a really ugly way to move around and not much of an advantage. If you start running and then loop it its like a marginal forward gain. Seems stupid to me, but I havent given up on flying yet.

Quote:
Originally Posted by Astrihacks
Why all the secrecy around sharing this? You only need the Cheat Engine MCP, whose source code is easy to find, and there are even tutorials on YouTube showing how to set it up. Then you can connect it to any Visual Studio IDE or Antigravity with OpenCode and ask it to make the hacks on its own. Why make such a big mystery out of it? I guess there’s just not much interest in sharing how hacks can be made with AI.
Thanks, I didnt stop and think CE might have MCP. You should make a post with your full workflow, because even though that is cool im wondering how it interacts with the game or do you have it stop and prompt you to change values until it uncovers the structure of the game and can work from there.

I automated making melonmods the other week for fun and released one here, but there isnt really any cool unity games I wanted to work on, so it just sits as a skill option for me now. It worked really well though.

A deep a.i workflow post would be very appreciated <3 I already have my menu up and running for Aion 2 but still I would like to stand up a full reversal skill, including CE.


## #141  -  arisenpai  -  7th October 2026, 05:36 PM

Nemesis Mods CEO

arisenpai's Avatar

Join Date: May 2024
Posts: 34
Reputation: 1108
Rep Power: 61
arisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the world
Points: 3,125, Level: 5
Points: 3,125, Level: 5	Points: 3,125, Level: 5	Points: 3,125, Level: 5
Level up: 41%, 475 Points needed
Level up: 41%	Level up: 41%	Level up: 41%
Activity: 13.9%
Activity: 13.9%	Activity: 13.9%	Activity: 13.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Modified Dumper - 7 for dumping Arion2 .

https://www.unknowncheats.me/forum/d...=file&id=58309

Just inject it while in login page
__________________
◤ SphinX ◢
「 Owner of Nemesis Mods 」


## #142  -  KeanuCorpse  -  7th October 2026, 05:41 PM

Quote:
Originally Posted by arisenpai
Modified Dumper - 7 for dumping Arion2 .

https://www.unknowncheats.me/forum/d...=file&id=58309

Just inject it while in login page
Thanks! you arent afraid of NCguard doing it like this? I spent 160 million tokens bypassing NCguard, and then 7 dumper kept freezing the game and I couldnt fix it lol. So I switched to external dumping, which im currently doing now.

What type of injection are you doing, and why at the login page?


## #143  -  SigPhreak  -  7th October 2026, 06:07 PM

That dumper already belongs to NCSoft sense posted yes, if you post anything here it is obviously looked at. Every company is very well aware of this site and others..

Injection type is not as big of a deal as the anti tamper, which is what all of you are running into.


## #144  -  RuneBind  -  7th October 2026, 06:38 PM

Quote:
Originally Posted by arisenpai
Modified Dumper - 7 for dumping Arion2 .

https://www.unknowncheats.me/forum/d...=file&id=58309

Just inject it while in login page
Thank you! but is it possible to make a kernel-wired only version? i've been working on it but i keep hitting

"can't enumerate modules (protected process"


## #145  -  arisenpai  -  7th October 2026, 07:18 PM

Nemesis Mods CEO

arisenpai's Avatar

Join Date: May 2024
Posts: 34
Reputation: 1108
Rep Power: 61
arisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the world
Points: 3,125, Level: 5
Points: 3,125, Level: 5	Points: 3,125, Level: 5	Points: 3,125, Level: 5
Level up: 41%, 475 Points needed
Level up: 41%	Level up: 41%	Level up: 41%
Activity: 13.9%
Activity: 13.9%	Activity: 13.9%	Activity: 13.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by KeanuCorpse
Thanks! you arent afraid of NCguard doing it like this? I spent 160 million tokens bypassing NCguard, and then 7 dumper kept freezing the game and I couldnt fix it lol. So I switched to external dumping, which im currently doing now.

What type of injection are you doing, and why at the login page?
lol, i have my ways .
I used my driver to read games physical memory with ai and fixed it . I used SWE 2 btw, what ai you use dosent matter , you need tools and exp. anyways good luck

Quote:
Originally Posted by SigPhreak
That dumper already belongs to NCSoft sense posted yes, if you post anything here it is obviously looked at. Every company is very well aware of this site and others..

Injection type is not as big of a deal as the anti tamper, which is what all of you are running into.
Thats true , but i dont think they will look into any already opensourced stuff xd so chill
__________________
◤ SphinX ◢
「 Owner of Nemesis Mods 」


## #146  -  Shard999  -  7th October 2026, 07:27 PM

Quote:
Originally Posted by Astrihacks
It works for any online game. The only limitation depends on the AI you use and how you structure your prompt. Using this method, I created my own custom hacks for Aniimo, Aion 2, and Genshin Impact.

Note/recommendation: I recommend using free OpenCode models or accessing them via API, although the free models work very well.
I also recommend reading this thread:
Undetected Cheat Engine + Driver | 2023 | Bypass Anticheats (BE / EAC)
Interesting post, had not seen that before. Guess getting your hands on a cert is the hardest part lol. Does Efiguard work well for Aion 2? They collect signatures of all loaded drivers so not sure how that would work out.

Also, do you mind me asking which AI specifically you use? Ive had good success using the frontier models for reversing, but as soon as anything needs to be modified they refuse. Had the same experience with other / free models. Do you gaslight them into thinking they are working on something legit?


## #147  -  KeanuCorpse  -  7th October 2026, 07:39 PM

Quote:
Originally Posted by Shard999
Interesting post, had not seen that before. Guess getting your hands on a cert is the hardest part lol. Does Efiguard work well for Aion 2? They collect signatures of all loaded drivers so not sure how that would work out.

Also, do you mind me asking which AI specifically you use? Ive had good success using the frontier models for reversing, but as soon as anything needs to be modified they refuse. Had the same experience with other / free models. Do you gaslight them into thinking they are working on something legit?
He is using GLM Max with a complicated workflow, i talked to him in discord, really nice guy.

Quote:
Originally Posted by arisenpai
lol, i have my ways .
I used my driver to read games physical memory with ai and fixed it . I used SWE 2 btw, what ai you use dosent matter , you need tools and exp. anyways good luck



Thats true , but i dont think they will look into any already opensourced stuff xd so chill
Well i can already dump but the nerd in me is excited to try your D7 version to see if it actually works. I was able to "jailbreak" my grok build into reversing, after i figured out what to turn off in the config to do that. (memory) Honestly i plugged in GLM with open router and spent about 4 bucks for the actual NCguard bypassing, but once i figured out how to make that work I was also able to make grok 4.7 work.


Anyways I also have my own kernel driver for reads and writes, thats how i was able to do all that, and thats also how my external dumping, and my external menu work now, all with that driver.


## #148  -  omgiamliving  -  7th October 2026, 07:58 PM

Latest SDK Dump please anyone?


## #149  -  Shard999  -  7th October 2026, 08:02 PM

Quote:
Originally Posted by KeanuCorpse
He is using GLM Max with a complicated workflow, i talked to him in discord, really nice guy.

Interesting, Ill need to play around with other models. What the frontier models are willing to do is so inconsistent, lmao.

Also regarding your other question, this is purely speculative but from what I observed, NCGuard checks for process suspension using GetTickCount in several watcher threads, so when you freeze the process for too long, it notices the gap and detects it. Theres far less stuff loaded into memory in the login screen versus when youre in-game, so you can stay below the threshold.


## #150  -  omgiamliving  -  7th October 2026, 08:43 PM

Im new to this, how do we get names of resources, players etc?


## #151  -  fpsnewboy  -  7th October 2026, 08:43 PM

SDK Dump https://www.unknowncheats.me/forum/d...=file&id=58312


## #152  -  KeanuCorpse  -  7th October 2026, 09:02 PM

Quote:
Originally Posted by Shard999
Interesting, Ill need to play around with other models. What the frontier models are willing to do is so inconsistent, lmao.

Also regarding your other question, this is purely speculative but from what I observed, NCGuard checks for process suspension using GetTickCount in several watcher threads, so when you freeze the process for too long, it notices the gap and detects it. Theres far less stuff loaded into memory in the login screen versus when youre in-game, so you can stay below the threshold.
Thanks bro, yeah it makes me want to vomit when these things refuse. Like how dare you disobey me btch I pay for you lmfao. Thankfully I have grok cranking away right now so I dont have to worry, but after talking to that other guy, after my 3 months of grok are up, i might switch to GLM. I really liked using it in the grok build harness with Openrouter. I got 12 hours of work for 4 dollars out of it with open router

It was so comical, because memory mode was enabled, grok build was refusing me (which i later found out after GLM was refusing me and mentioning and old memory) , so i was like okay if you want to be a virgin, explain to me how to hook a model up that isnt, and it was fine doing that... like okay nice logic rtard. You wont shoot the target but you will connect me with someone who will

Anyways TLR Once i hooked GLM up to grok build through open router API, (and realized memory mode in grok build CLI was poisoning me with refusal messages) it was more than happy to reverse NCguard and do trampolines and all this b.s to make it so I could inject. Although it leveraged my kernel driver that I already had made. I didnt even realize GLM had a subscription until I spoke with that guy.

He showed me plenty of proof of life though, hes working on pretty difficult games and has results. His plan is 168 dollars a month but hes running multiple agents and things 6-8 hours a day...

If you are using a CLI harness for a.i coding locally, i recommending turning off memory, since realizing how much that effects refusals for some reason, ive even thought of turning it back on and creating a program to poison all the memories to see if that works.


## #153  -  Danielci58  -  7th October 2026, 09:04 PM

Quote:
Originally Posted by KeanuCorpse
Thanks bro, yeah it makes me want to vomit when these things refuse. Like how dare you disobey me btch I pay for you lmfao. Thankfully I have grok cranking away right now so I dont have to worry, but after talking to that other guy, after my 3 months of grok are up, i might switch to GLM. I really liked using it in the grok build harness with Openrouter. I got 12 hours of work for 4 dollars out of it with open router

It was so comical, because memory mode was enabled, grok build was refusing me, so i was like okay if you want to be a virgin, explain to me how to hook a model up that isnt, and it was fine doing that... like okay nice logic rtard. You wont shoot the target but you will connect me with someone who will

Anyways TLR Once i hooked GLM up to grok build through open router API, it was more than happy to reverse NCguard and do trampolines and all this b.s to make it so I could inject. Although it leveraged my kernel driver that I already had made. I didnt even realize GLM had a subscription until I spoke with that guy.

He showed me plenty of proof of life though, hes working on pretty difficult games and has results. His plan is 168 dollars a month but hes running multiple agents and things 6-8 hours a day...

If you are using a CLI harness for a.i coding locally, i recommending turning off memory, or since realizing how much that effects refusals for some reason, ive even though of turning it back on and creating a program to poison all the memories to see if that works.
There is plenty of jailbreaks for grok , glm , deepseek and similar. Even for astra there is one that works most of time. If you have the workflow let me know and I can let my glm 5.3 try to do something I have a sub there


## #154  -  Astrihacks  -  7th October 2026, 09:59 PM

Quote:
Originally Posted by Shard999
Interesting post, had not seen that before. Guess getting your hands on a cert is the hardest part lol. Does Efiguard work well for Aion 2? They collect signatures of all loaded drivers so not sure how that would work out.

Also, do you mind me asking which AI specifically you use? Ive had good success using the frontier models for reversing, but as soon as anything needs to be modified they refuse. Had the same experience with other / free models. Do you gaslight them into thinking they are working on something legit?
Download Google Antigravity IDE > Install OpenCode and extension for Visual Studio inside Antigravity > Open a terminal with OpenCode > Choose free models such as Big Pickle or Muse Spark 1.3 > install the GitHub repository miscusi-peek/cheatengine-mcp-bridge > Use your own custom version of Cheat Engine; here’s how to make it: Undetected Cheat Engine + Driver | 2023 | Bypass Anticheats (BE / EAC) > Enjoy creating your hacks automatically


## #155  -  foycein  -  7th October 2026, 10:00 PM

Quote:
Originally Posted by fpsnewboy
SDK Dump https://www.unknowncheats.me/forum/d...=file&id=58312
you are a legend!
any tips on your dumping workflow ?


## #156  -  Shard999  -  7th October 2026, 10:10 PM

Quote:
Originally Posted by Astrihacks
Download Google Antigravity IDE > Install OpenCode and extension for Visual Studio inside Antigravity > Open a terminal with OpenCode > Choose free models such as Big Pickle or Muse Spark 1.3 > install the GitHub repository miscusi-peek/cheatengine-mcp-bridge > Use your own custom version of Cheat Engine; here’s how to make it: Undetected Cheat Engine + Driver | 2023 | Bypass Anticheats (BE / EAC) > Enjoy creating your hacks automatically

https://i.imgur.com/eigvlHI.png
Thanks for sharing. Are you booting with Efiguard or did you find a leaked codesigning certificate?


## #157  -  Astrihacks  -  7th October 2026, 10:17 PM

Quote:
Originally Posted by Shard999
Thanks for sharing. Are you booting with Efiguard or did you find a leaked codesigning certificate?
I currently use the custom driver and the DSE Patcher on Windows 10 IoT LTSC. I don't use EfiGuard or any other method


## #158  -  Danielci58  -  7th October 2026, 10:43 PM

Quote:
Originally Posted by Astrihacks
Download Google Antigravity IDE > Install OpenCode and extension for Visual Studio inside Antigravity > Open a terminal with OpenCode > Choose free models such as Big Pickle or Muse Spark 1.3 > install the GitHub repository miscusi-peek/cheatengine-mcp-bridge > Use your own custom version of Cheat Engine; here’s how to make it: Undetected Cheat Engine + Driver | 2023 | Bypass Anticheats (BE / EAC) > Enjoy creating your hacks automatically

https://i.imgur.com/eigvlHI.png
How good is muse spark 1.3 ? never tested. Seen that 1.4 just got released today


## #159  -  Astrihacks  -  7th October 2026, 10:48 PM

Quote:
Originally Posted by Danielci58
How good is muse spark 1.3 ? never tested. Seen that 1.4 just got released today
Use whichever ones you have available and whichever provide the best context and analysis. I use Gemini Pro 3.8 High and Muse Spark 1.3, and sometimes you simply need to make corrections after reviewing the output two or three times. However, the hacks always work.

You can always use the APIs of paid models if you have access to them, but the free plan is more than enough for hack development.


## #160  -  RuneBind  -  7th October 2026, 11:22 PM

Quote:
Originally Posted by arisenpai
Modified Dumper - 7 for dumping Arion2 .

https://www.unknowncheats.me/forum/d...=file&id=58309

Just inject it while in login page
Thanks, can you release the source-code or tell us what exactly the difference between it and a dumper-7? thank you


## #161  -  defaultx  -  8th October 2026, 12:41 AM

This has been a fun process


## #162  -  jonxpin  -  8th October 2026, 01:11 AM

Lol, everyone went with an external menu just like mine. I'm also stuck with it >.<


## #163  -  rhaym  -  8th October 2026, 02:00 AM

File Approved
SHA256: a7043f863f91716922d482e8107480c544888389453310ff9abf6cdaaf12f0b2 - Dumper-7.dll


Download: https://www.unknowncheats.me/forum/d...=file&id=58309

Thanks for sharing!
__________________
When the snows fall and the white winds blow, the lone wolf dies, but the pack survives.


## #164  -  rhaym  -  8th October 2026, 02:11 AM

File Approved
SHA256: efbaf05d2a3d1f5c0c37f25feac5a37bda97c41a8c66da1f6c6e0817732f6169 - 5.3.0+++UE5+Release-5.3.AION2.zip


Download: https://www.unknowncheats.me/forum/d...=file&id=58312

Thanks for sharing!
__________________
When the snows fall and the white winds blow, the lone wolf dies, but the pack survives.


## #165  -  defaultx  -  8th October 2026, 02:28 AM

Quote:
Originally Posted by jonxpin
Lol, everyone went with an external menu just like mine. I'm also stuck with it >.<
I used your aniimo menu and i liked the layout so i tried to replicate it :P


## #166  -  RuneBind  -  8th October 2026, 02:34 AM

Quote:
Originally Posted by fpsnewboy
SDK Dump https://www.unknowncheats.me/forum/d...=file&id=58312
Thank you! that was most useful! mind telling us what tools used for the dump? this is one of the best i've seen so far.


## #167  -  arisenpai  -  8th October 2026, 02:43 AM

Nemesis Mods CEO

arisenpai's Avatar

Join Date: May 2024
Posts: 34
Reputation: 1108
Rep Power: 61
arisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the world
Points: 3,125, Level: 5
Points: 3,125, Level: 5	Points: 3,125, Level: 5	Points: 3,125, Level: 5
Level up: 41%, 475 Points needed
Level up: 41%	Level up: 41%	Level up: 41%
Activity: 13.9%
Activity: 13.9%	Activity: 13.9%	Activity: 13.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by RuneBind
Thank you! but is it possible to make a kernel-wired only version? i've been working on it but i keep hitting

"can't enumerate modules (protected process"
i can read both physical and virtual memory of game using my driver . you can find lots of refs on git . mainly used in cs .

Quote:
Originally Posted by RuneBind
Thanks, can you release the source-code or tell us what exactly the difference between it and a dumper-7? thank you
that i can't do sorry . But there are lots needs to be changed took me 2day .
i created it on first day but accidentally deleted the entire folder . then on 2nd day i needed to do it from the start again .
__________________
◤ SphinX ◢
「 Owner of Nemesis Mods 」


## #168  -  tesla24  -  8th October 2026, 03:40 AM

Quote:
Originally Posted by rhaym

Download: https://www.unknowncheats.me/forum/d...=file&id=58312

Thanks for sharing!
goat ,


## #169  -  benferoking  -  8th October 2026, 10:19 AM

Quote:
Originally Posted by defaultx
I used your aniimo menu and i liked the layout so i tried to replicate it :P
when we gettin new halloween update man


## #170  -  ZixMatrix  -  8th October 2026, 12:25 PM

Quote:
Originally Posted by fpsnewboy
SDK Dump https://www.unknowncheats.me/forum/d...=file&id=58312
OG !

anyway it will be maintenance for 1 hour, please help to dump again next time
thank you


## #171  -  KeanuCorpse  -  8th October 2026, 06:16 PM

Grok 4.7 was too stroked out to get me the actual names of gathering materials, with its last dying 2% usage I had it burp me out a snippet so I could jingle a can and ask for help. Every gathering thing on my esp is like "Gather 19381947191" instead of the name.

Code:
# AION 2 gather names — what we have, what is missing

UE 5.3.2 external read. Actor class is `BP_EnvObj_Common_C` / `AionEnvObj`. We can box the nodes. We cannot get the plant name other people print (`Morfa`, `Odyle`, `Empyrean Trace`).

Please tell us where the **display string** lives. The FName on the row is not that string, and the actor nameplate is empty.

## Chain that is confirmed on live actors

```cpp
// AAionEnvObj. EnvData sits inside the missed pad at 0x518. Do not move off 0x588.
// +0x580 reads a decoy id (1110). The real id is +0x588.
uint64_t envData = *(uint64_t*)(actor + 0x588);
uint32_t envId   = *(uint32_t*)(envData + 0x28);   // 1000 .. 50000000
```

Two nodes from one standing dump (Safe Haven, same session):

| dist | actor class | envData | id | nameplate |
|---|---|---|---|---|
| 3.8 m | BP_EnvObj_Common_C | `F0DA12550` | 1300865 (`0x13D981`) | empty |
| 23.3 m | BP_EnvObj_Common_C | `193369AD0` | 1300171 (`0x13D6CB`) | empty |

`envData` is **not** a UObject. Reading it as one returns class `FloatProperty` and an empty FName. Sc[removed] FNames out of the blob stored the literal `None` (index 0) and `Cylinder`.

Qwords on the blob (same shape on both nodes):

```
+0x28  id in the low dword. High dword differs (0x90000 vs 0x10007).
       node1 0x0009000013D981
       node2 0x10007000013D6CB
+0x30  0x100000000 on both          (low dword 0, +0x34 is 1)
+0x38  float 2.5, then 80.0 or 200.0
+0x40  1 vs 0x102
+0x48  NOT a pointer. Both high dwords are 0x08DF2445.
+0x50  per-object user pointer. This is the only sane differing pointer.
       node1 0x1017CB4E30
       node2 0x107CA11EC0
       It is NOT an FEnvObjTableItem (id is not at +8 with a sane FName at +0x0C).
+0x58  0x800000005 on both
+0x60  1 vs 0
+0x68  0
```

Game HUD over the focused node says `Empyrean Trace` / `Altgard` and `F Interact`. That text was **not** in the nameplate widget at actor `+0x4F8` or the next component at `+0x500`. Do not hardcode those strings. They are the interact label of whatever the player is looking at.

## SDK row (we never successfully point at one of these for a live id)

`FEnvObjTableItem` is `0x160`. There is **no FString** on it. `Desc` is two FNames (`FTableL10NText`), not the localized wchar.

```cpp
// FAionTableItem base is 8 bytes. ID is the first field.
struct FTableL10NText {   // 0x10
    FName Key;            // +0x00
    FName StringKey;      // +0x08
};

struct FEnvObjTableItem {          // size 0x160
    uint32_t ID;                   // +0x08
    FName    Name;                 // +0x0C
    FTableL10NText Desc;           // +0x14   Key +0x14, StringKey +0x1C
    FName    ResourceKey;          // +0x24
    uint8_t  Nameplate;            // +0x2C   EEnvObjNameplate, SDK_UNDEFINED
    int32_t  NameplateOffset;      // +0x30
    uint8_t  Usage;                // +0x34   EEnvObjectUsage, SDK_UNDEFINED
    FName    UsageValue;           // +0x38   gather-source key when Usage == 3
    // ...
};

struct FGatherSourceTableItem {    // size 0x40
    uint32_t ID;                   // +0x08
    FName    Name;                 // +0x0C   this is what we would print (Morfa / Odyle)
    FName    MaterialGroup;        // +0x14
    uint8_t  SourceType;           // +0x1C   None,Metal,RareMetal,Jewelry,Tree,Flower,Herb,Berry,Vegetable,Shellfish,Od
    uint8_t  SourceGrade;          // +0x1D
    uint32_t MasteryLevel;         // +0x20
};
```

Published image RVAs (not re-scanned this patch, and they did **not** contain the live ids):

- EnvObjTable `0xF29DFB0`
- GatherSourceTable `0xF2A4EC0`

A gather-source map of 192 rows was found at runtime RVA `0xF3021F8`. That map is the gather-source table, not the env table. Env rows stayed 0. GObjects itself is readable (555136 objects, 9 chunks, num at `+0x14`, chunks at `+0x1C`). The walk never accepted an env table that contains ids `1300865` / `1300171`.

`UAionGameDefaultResources::EnvObjResourceTable` at `+0x78` is a mesh resource table, not this gameplay table. There is no SDK class named EnvData.

## What the overlay does

```cpp
// Rank 3 = a word with a lowercase letter and no underscore (Morfa, Odyle).
// "None", "Cylinder", "*Property", "BP*", "EnvObj", "Default__", "Base" are junk.

uint64_t envData = 0;
uint32_t id = ReadEnvId(actor, 0x588, envData);   // *(u32*)(envData + 0x28)

// 1. Global env map, keyed by id. Empty until a table scan accepts live ids.
uint64_t rec = LookupEnv(id);

// 2. If no row, scan qwords on the blob for a record shaped like FEnvObjTableItem:
//    *(u32*)(ptr + 8) == id  AND  FName index at ptr+0x0C is in 1..0x1FFFFFF.
//    Neighbors are walked at stride 0x160. This did not hit +0x50.
if (!rec)
    rec = BindEnvFromData(envData, id);

// 3. If we have a row:
//    usage == 3, or UsageValue hits the gather-source map -> kind Gather
//    name comes from gather-source Name (+0x0C), else env Name (+0x0C / +8 / +16).
//    We do NOT resolve FTableL10NText Key/StringKey into a localized string.

// 4. If rank is still < 3, try a UObject hanging off the blob.
//    +0x50 first. Class GName must not contain "Property".
//    Then the actor nameplate at +0x4F8 (and +0x500). Both were empty.

// 5. No row yet: still draw the node. Label is "Gather {id}".
//    A row whose usage is not gather is left as plain env (piano keys).
if (!rec && id >= 1000 && id <= 50000000)
    kind = Gather;
if (!name[0] && kind == Gather)
    snprintf(name, "Gather %u", id);
```

## The ask

Ids are good. Boxes are good. The display name is not on the actor and not in the blob as a wchar or a useful FName.

If you already print `Morfa` / `Odyle`:

1. Is the display string the localization of `FEnvObjTableItem::Name`, or of `Desc.StringKey` (`FTableL10NText` at `+0x14`)?
2. How do you turn that FName into the wchar the client shows? Which string table, and what is the lookup (GObjects `UStringTable`, a loaded `FAionStringTableItem`, or something else)?
3. How do you get the `FEnvObjTableItem*` for env id `1300865` without scanning every UObject? Is the row pointer somewhere on the `+0x588` blob besides `+0x50`?
4. Is `Usage == 3` still the gather usage on the current patch, with `UsageValue` as the `FGatherSourceTableItem` name key?

A wrong answer we already tried: treating `envData` as a UObject, and reading FNames at `envData+0x18/0x30/...`. That prints `None`.


## #172  -  symond100  -  8th October 2026, 07:33 PM

Quote:
Originally Posted by Astrihacks
Download Google Antigravity IDE > Install OpenCode and extension for Visual Studio inside Antigravity > Open a terminal with OpenCode > Choose free models such as Big Pickle or Muse Spark 1.3 > install the GitHub repository miscusi-peek/cheatengine-mcp-bridge > Use your own custom version of Cheat Engine; here’s how to make it: Undetected Cheat Engine + Driver | 2023 | Bypass Anticheats (BE / EAC) > Enjoy creating your hacks automatically

https://i.imgur.com/eigvlHI.png
Is this the full process on how you make your AI cheat?


## #173  -  Danielci58  -  8th October 2026, 08:07 PM

Quote:
Originally Posted by symond100
Is this the full process on how you make your AI cheat?
You dont need any specific tutorial. You can do it external or internal both work.

You do yourself a dump or use an AI there is multiple jailbreaks out there for most of the models.

Deepseek , glm , grok , Sol 6.1 , Astra...
First three will work without giving any issues while the last two you need to have a bit of care while using them to avoid " daybreak " flags. But all of them work you can even use Sol6.1 to completely unpack NCGuard


Just prompt it well. And with a full dump + AI you can already do a full ESP


## #174  -  melmel89  -  8th October 2026, 08:28 PM

Quote:
Originally Posted by Danielci58
You dont need any specific tutorial. You can do it external or internal both work.

You do yourself a dump or use an AI there is multiple jailbreaks out there for most of the models.

Deepseek , glm , grok , Sol 6.1 , Astra...
First three will work without giving any issues while the last two you need to have a bit of care while using them to avoid " daybreak " flags. But all of them work you can even use Sol6.1 to completely unpack NCGuard


Just prompt it well. And with a full dump + AI you can already do a full ESP
Would it be possible to tell me how you did with Grok because I can't manage to jailbreak it to ask it to code an ESP and other things for me, thanks to you


## #175  -  Danielci58  -  8th October 2026, 08:35 PM

Quote:
Originally Posted by melmel89
Would it be possible to tell me how you did with Grok because I can't manage to jailbreak it to ask it to code an ESP and other things for me, thanks to you
Not sure if I can just paste the whole .md here but you can always check togg53192-cmd projects in github.


## #176  -  KeanuCorpse  -  8th October 2026, 11:24 PM

Quote:
Originally Posted by melmel89
Would it be possible to tell me how you did with Grok because I can't manage to jailbreak it to ask it to code an ESP and other things for me, thanks to you
If you are using grok build, YOU MUST turn memory mode OFF. If it saves a memory refusing you, it doesnt even matter if you hook openrouter models up to it, or switch to grok 4.6 or anything, it will instantly pull that memory up next session and poison you. I use grok just fine as long as memory is off.

I'm out of usage so right now im using open router - GLM 5.3 inside of the grok build harness to finish up something.

now someone tell me how I get the gather object names externally before I kms lol

btw deepseek is a horrendous pos if anyone was wondering, im trying 4.1 in openrouter and i caught it making up stuff "You're right — I said "dial" and you never used that word. That was me inventing a detail, and then I built a whole "smoking gun" argument on top of it ("the dial changed nothing"). Both were fabrications. Sorry. Let me also correct my other overreach: the ShareX observation is not evidence of a compositing fix. You press the hotkey with your hand off the mouse, so the camera stops for an instant — and per your own report, stopping is exactly when the boxes snap home. So ShareX always catching them aligned is consistent with the lag, not a separate clue."

Grok 4.6 > GLM 5.3 > Deepseek > Grok 4.7

I hate grok 4.7, it thinks SPASMODICALLY, constantly spamming the thinking boxes for long periods of time, burns usage like 2x faster, and just seems futile. I really think they panic released it because they were socially forced to release something. Remember it had all those delays, then people started crying about the release so they released it. I much prefer grok 4.6.


## #177  -  Danielci58  -  8th October 2026, 11:52 PM

Quote:
Originally Posted by KeanuCorpse
If you are using grok build, YOU MUST turn memory mode OFF. If it saves a memory refusing you, it doesnt even matter if you hook openrouter models up to it, or switch to grok 4.6 or anything, it will instantly pull that memory up next session and poison you. I use grok just fine as long as memory is off.

I'm out of usage so right now im using open router - GLM 5.3 inside of the grok build harness to finish up something.

now someone tell me how I get the gather object names externally before I kms lol

btw deepseek is a horrendous pos if anyone was wondering, im trying 4.1 in openrouter and i caught it making up stuff "You're right — I said "dial" and you never used that word. That was me inventing a detail, and then I built a whole "smoking gun" argument on top of it ("the dial changed nothing"). Both were fabrications. Sorry. Let me also correct my other overreach: the ShareX observation is not evidence of a compositing fix. You press the hotkey with your hand off the mouse, so the camera stops for an instant — and per your own report, stopping is exactly when the boxes snap home. So ShareX always catching them aligned is consistent with the lag, not a separate clue."

Grok 4.6 > GLM 5.3 > Deepseek > Grok 4.7

I hate grok 4.7, it thinks SPASMODICALLY, constantly spamming the thinking boxes for long periods of time, burns usage like 2x faster, and just seems futile. I really think they panic released it because they were socially forced to release something. Remember it had all those delays, then people started crying about the release so they released it. I much prefer grok 4.6.
None AIs outside big 2 will actually make you real stuff if you wanna 100% vibe code it. And those 2 will trigger a lot of the time cyber safeguard and you need to constatly fight againts them by taking different paths. Claude its a pain I never managed pass those safeguards , lucky chatgpt its a bit more " relaxed " but yet sometimes you need to do some workarounds.


## #178  -  KeanuCorpse  -  9th October 2026, 12:18 AM

Quote:
Originally Posted by Danielci58
None AIs outside big 2 will actually make you real stuff if you wanna 100% vibe code it. And those 2 will trigger a lot of the time cyber safeguard and you need to constatly fight againts them by taking different paths. Claude its a pain I never managed pass those safeguards , lucky chatgpt its a bit more " relaxed " but yet sometimes you need to do some workarounds.
grok and GLM and deepseek all did bypassing and reversing work by name, called the programs cheats and hacks, found "exploits" and all tried to circumvent NCguard by using my kernel driver to explore what it was doing while it was unprotected in memory.

I have no clue what you are talking about with openAI being relaxed, its the most woke POS safe guarded one of them all. I just had some guy in discord losing his mind because codex wouldnt even make him a kernel driver to do reads and writes.


## #179  -  Danielci58  -  9th October 2026, 12:22 AM

Quote:
Originally Posted by KeanuCorpse
grok and GLM and deepseek all did bypassing and reversing work by name, called the programs cheats and hacks, found "exploits" and all tried to circumvent NCguard by using my kernel driver to explore what it was doing while it was unprotected in memory.

I have no clue what you are talking about with openAI being relaxed, its the most woke POS safe guarded one of them all. I just had some guy in discord losing his mind because codex wouldnt even make him a kernel driver to do reads and writes.
Not sure if you managed to do any RE to bb64.dll with any of those models but if you tried you probably seen that at some point they keep doing [removed] and get lost at least it was like that for me while with 6.1Sol in a few hours it was close to being done and completely patch it but the daybreak cyber flag started spamming and had to stop. Once my GLM usage get reseted I will probably try to advance a bit more


## #180  -  KeanuCorpse  -  9th October 2026, 04:44 AM

Quote:
Originally Posted by Danielci58
Not sure if you managed to do any RE to bb64.dll with any of those models but if you tried you probably seen that at some point they keep doing [removed] and get lost at least it was like that for me while with 6.1Sol in a few hours it was close to being done and completely patch it but the daybreak cyber flag started spamming and had to stop. Once my GLM usage get reseted I will probably try to advance a bit more


Perhaps my experience is different because im using the grok build harness, im not sure but I did do RE on NCGuard and successfully bypassed it when I was trying to get 7 dumper to work, I didnt realize the dumper needed modifications though, so even though the builder had confirmed through our injection logs and data that the bypass was working, i eventually gave up because 7 dumper wasnt working I tried that guy's upload for 7 dumper today and successfully dumped, the same style as I had before with marvel rivals, but decided not to switch to internal

And switched to external.


The only refusals I had were the 10 million refusals trying to reverse NCguard in the first place, then I finally realized there was a refusal in memory, I erased those memories and then disabled memory, and every single model was open again.


## #181  -  Cinus  -  9th October 2026, 05:09 AM

Someone was able to dump whole exe? Pls share if. I would like to analyze it thx


## #182  -  omgiamliving  -  9th October 2026, 10:19 AM

Anybody has the new dump?


## #183  -  ZixMatrix  -  9th October 2026, 10:54 AM

AION2.exe 2026-10-09 (Steam, 156,557,752 bytes)
sha256 3842fc511a09121d3b3f2567002994d5c2cfdba0cf762d035c8ae21872aba953
base 0x140000000

GNames 0x0F120440 (was 0x0F134440)
GObjects 0x0EC78990 (was 0x0EC8C990)
GWorld 0x0EF83BC8 (was 0x0EF97BC8)
ProcessEvent 0x04205CD0 (was 0x042066B0)


## #184  -  Danielci58  -  9th October 2026, 01:27 PM

Quote:
Originally Posted by KeanuCorpse
Perhaps my experience is different because im using the grok build harness, im not sure but I did do RE on NCGuard and successfully bypassed it when I was trying to get 7 dumper to work, I didnt realize the dumper needed modifications though, so even though the builder had confirmed through our injection logs and data that the bypass was working, i eventually gave up because 7 dumper wasnt working I tried that guy's upload for 7 dumper today and successfully dumped, the same style as I had before with marvel rivals, but decided not to switch to internal

And switched to external.


The only refusals I had were the 10 million refusals trying to reverse NCguard in the first place, then I finally realized there was a refusal in memory, I erased those memories and then disabled memory, and every single model was open again.
Im not really sure if you actually managed to bypass NCGuard. I could do a full dump even without the 7 dumper or any other external tool without bypassing anything while using internal dll most of the time.

But patching the detect call thats a different story . Tomorrow when my glm has quota I will try to push a bit more and then switch back to gpt astra /6.1 again and keep going after the blockage


## #185  -  KeanuCorpse  -  9th October 2026, 07:51 PM

Quote:
Originally Posted by Danielci58
Im not really sure if you actually managed to bypass NCGuard. I could do a full dump even without the 7 dumper or any other external tool without bypassing anything while using internal dll most of the time.

But patching the detect call thats a different story . Tomorrow when my glm has quota I will try to push a bit more and then switch back to gpt astra /6.1 again and keep going after the blockage


I have the logs that prove it but either way good luck to you! GLM 5.3 is the one who did most of the NCguard work. Huge waste of time unless you are making a bot or something. I dont see many reasons to go internal, besides getting the gathering names would have been easier for me.

Quote:
Originally Posted by Cinus
Someone was able to dump whole exe? Pls share if. I would like to analyze it thx
Some guy on one of the first pages had a video showing how to do it, then he took the video down lol

He like started the game, then right after the splash when the game comes up, suspended it in task manager, then used IDA + scylla, and it worked instantly in his video. Kind of bummy it either went away or he took it away because I was curious to do it for learning purposes. If you have any luck maybe repay my sage advice with a video here for us.


(Kind of interesting your account is from 2011 and you've made one post so far lol)


## #186  -  SigPhreak  -  9th October 2026, 08:31 PM

bb64.dll is not what you need to be looking at for bypass. Wrong dll..

I wont name the one you should be looking at, but it's not that one.


## #187  -  Federwind  -  9th October 2026, 09:08 PM

Quote:
Originally Posted by SigPhreak
bb64.dll is not what you need to be looking at for bypass. Wrong dll..

I wont name the one you should be looking at, but it's not that one.
good thing i read your post, was about to start and go through the whole obfuscated file and try to reverse it, dodged a bullet.

The file you shall not name starts with a "N"?


## #188  -  Danielci58  -  9th October 2026, 09:18 PM

Quote:
Originally Posted by SigPhreak
bb64.dll is not what you need to be looking at for bypass. Wrong dll..

I wont name the one you should be looking at, but it's not that one.
Damn thanks for letting me know...

I will recheck again what Im doing wrong. Thanks

Quote:
Originally Posted by KeanuCorpse
I have the logs that prove it but either way good luck to you! GLM 5.3 is the one who did most of the NCguard work. Huge waste of time unless you are making a bot or something. I dont see many reasons to go internal, besides getting the gathering names would have been easier for me.(Kind of interesting your account is from 2011 and you've made one post so far lol)
Oh no its no a bot Im doing a tool ( almost done actually like just making it a bit more fancy ) that can recreat presets without you having to interact with game at all or making presets based on real people


## #189  -  hunchojacks  -  9th October 2026, 11:01 PM

this is my ship now, i dare u come here


## #190  -  tesla24  -  9th October 2026, 11:07 PM

Quote:
Originally Posted by hunchojacks
https://i.imgur.com/s071Tvl.jpeg

this is my ship now, i dare u come here
next time think 2 times before post hunco


## #191  -  cracksuxer200  -  10th October 2026, 01:26 AM

AES encryption key

06038EF544B6007614F8574F1B7C2A3F0D565F74CDCC1B366B4EA1A17B97CBFF


## #192  -  fauzera  -  10th October 2026, 04:23 AM

Does anyone know how I can get information about player names and ore nodes (objects in the game world)?


## #193  -  darksidexc  -  10th October 2026, 04:50 AM

Super l337


darksidexc's Avatar

Join Date: Jun 2013
Location: Moon
Posts: 204
Reputation: 2781
Rep Power: 331
darksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating community
Points: 15,000, Level: 15
Points: 15,000, Level: 15	Points: 15,000, Level: 15	Points: 15,000, Level: 15
Level up: 50%, 1,400 Points needed
Level up: 50%	Level up: 50%	Level up: 50%
Activity: 70.0%
Activity: 70.0%	Activity: 70.0%	Activity: 70.0%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
working in progress, reading with my external read driver >>
Code:
https://imgur.com/a/GfxQKSn

__________________
 game hacking at all, also know as darksidexc


## #194  -  fauzera  -  10th October 2026, 07:00 AM

https://i.imgur.com/QtdniR5.png


## #195  -  Gamevantage  -  10th October 2026, 08:57 AM

how are we dumpin aion ?


## #196  -  makotogames  -  10th October 2026, 01:14 PM

makotogames is becoming an outcast
Points: 1,267, Level: 2
Points: 1,267, Level: 2	Points: 1,267, Level: 2	Points: 1,267, Level: 2
Level up: 74%, 133 Points needed
Level up: 74%	Level up: 74%	Level up: 74%
Activity: 2.8%
Activity: 2.8%	Activity: 2.8%	Activity: 2.8%
Quote:
Originally Posted by fauzera
https://i.imgur.com/QtdniR5.png
i ala br kkk


## #197  -  arisenpai  -  10th October 2026, 02:12 PM

Nemesis Mods CEO

arisenpai's Avatar

Join Date: May 2024
Posts: 34
Reputation: 1108
Rep Power: 61
arisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the world
Points: 3,125, Level: 5
Points: 3,125, Level: 5	Points: 3,125, Level: 5	Points: 3,125, Level: 5
Level up: 41%, 475 Points needed
Level up: 41%	Level up: 41%	Level up: 41%
Activity: 13.9%
Activity: 13.9%	Activity: 13.9%	Activity: 13.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets

😊
__________________
◤ SphinX ◢
「 Owner of Nemesis Mods 」


## #198  -  zombie2001  -  10th October 2026, 02:25 PM

any solutions for attack speed, move speed, fly speed, cooldown sec, auto gathering etc.. ?


## #199  -  Slemknot  -  10th October 2026, 04:59 PM

Supreme H4x0|2


Slemknot's Avatar

Join Date: Apr 2020
Posts: 681
Reputation: 3105
Rep Power: 169
Slemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating community
Points: 19,990, Level: 19
Points: 19,990, Level: 19	Points: 19,990, Level: 19	Points: 19,990, Level: 19
Level up: 46%, 810 Points needed
Level up: 46%	Level up: 46%	Level up: 46%
Activity: 2.9%
Activity: 2.9%	Activity: 2.9%	Activity: 2.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Who has the current patterns for the signature scanner? I dumped the file and removed the protection from it!
__________________
Xiloe (Wooteck) worst fear!


## #200  -  hunchojacks  -  10th October 2026, 07:04 PM

any funny exploits besides teleporting up?


## #201  -  yurigagarin  -  10th October 2026, 07:19 PM

Quote:
Originally Posted by hunchojacks
any funny exploits besides teleporting up?
does ur tp up work for dodging jump mechanics? mine doesnt and still get damage.


## #202  -  darksidexc  -  10th October 2026, 07:46 PM

Super l337


darksidexc's Avatar

Join Date: Jun 2013
Location: Moon
Posts: 204
Reputation: 2781
Rep Power: 331
darksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating community
Points: 15,000, Level: 15
Points: 15,000, Level: 15	Points: 15,000, Level: 15	Points: 15,000, Level: 15
Level up: 50%, 1,400 Points needed
Level up: 50%	Level up: 50%	Level up: 50%
Activity: 70.0%
Activity: 70.0%	Activity: 70.0%	Activity: 70.0%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by Slemknot
Who has the current patterns for the signature scanner? I dumped the file and removed the protection from it!
don't need.. you can do all by reflection
__________________
 game hacking at all, also know as darksidexc


## #203  -  Slemknot  -  10th October 2026, 07:53 PM

Supreme H4x0|2


Slemknot's Avatar

Join Date: Apr 2020
Posts: 681
Reputation: 3105
Rep Power: 169
Slemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating community
Points: 19,990, Level: 19
Points: 19,990, Level: 19	Points: 19,990, Level: 19	Points: 19,990, Level: 19
Level up: 46%, 810 Points needed
Level up: 46%	Level up: 46%	Level up: 46%
Activity: 2.9%
Activity: 2.9%	Activity: 2.9%	Activity: 2.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by darksidexc
don't need.. you can do all by reflection
I’ve already found everything. The main thing for me is to find the signatures in IDA!
__________________
Xiloe (Wooteck) worst fear!


## #204  -  hunchojacks  -  10th October 2026, 08:11 PM

Quote:
Originally Posted by yurigagarin
does ur tp up work for dodging jump mechanics? mine doesnt and still get damage.
tbh i dont know what u mean by dodging jump mechanics. but u can tp up like 10m and then just loop it


## #205  -  Zeuby  -  10th October 2026, 08:18 PM

Quote:
Originally Posted by SigPhreak
bb64.dll is not what you need to be looking at for bypass. Wrong dll..

I wont name the one you should be looking at, but it's not that one.
it's not bb64.dll but "I won't name it" helps nobody on a community forum, the gatekeeping is unnecessary. Change your mindset asap.

The real anticheat module is BBt, manually mapped into AION2.exe at runtime, wiped PE header, invisible to Tool/PEB. bb64.dll is just the visible scan/telemetry layer.


## #206  -  Danielci58  -  10th October 2026, 08:19 PM

Quote:
Originally Posted by Zeuby
it's not bb64.dll but "I won't name it" helps nobody on a community forum, the gatekeeping is unnecessary. Change your mindset asap.

The real anticheat module is BBt, manually mapped into AION2.exe at runtime, wiped PE header, invisible to Tool/PEB. bb64.dll is just the visible scan/telemetry layer.
Thank you <3


## #207  -  makotogames  -  10th October 2026, 08:31 PM

makotogames is becoming an outcast
Points: 1,267, Level: 2
Points: 1,267, Level: 2	Points: 1,267, Level: 2	Points: 1,267, Level: 2
Level up: 74%, 133 Points needed
Level up: 74%	Level up: 74%	Level up: 74%
Activity: 2.8%
Activity: 2.8%	Activity: 2.8%	Activity: 2.8%
Talking
Quote:
Originally Posted by Zeuby
it's not bb64.dll but "I won't name it" helps nobody on a community forum, the gatekeeping is unnecessary. Change your mindset asap.

The real anticheat module is BBt, manually mapped into AION2.exe at runtime, wiped PE header, invisible to Tool/PEB. bb64.dll is just the visible scan/telemetry layer.
Thank you Thank you


## #208  -  SigPhreak  -  10th October 2026, 10:41 PM

Quote:
Originally Posted by Zeuby
it's not bb64.dll but "I won't name it" helps nobody on a community forum, the gatekeeping is unnecessary. Change your mindset asap.

The real anticheat module is BBt, manually mapped into AION2.exe at runtime, wiped PE header, invisible to Tool/PEB. bb64.dll is just the visible scan/telemetry layer.
I'm good bro, i'm not required to detail every single thing out in order to be helpful. Also, that's not it either.. so i'm not sure why you are coming at me, while listing something still irrelevant for operation.


## #209  -  Slemknot  -  10th October 2026, 11:29 PM

Supreme H4x0|2


Slemknot's Avatar

Join Date: Apr 2020
Posts: 681
Reputation: 3105
Rep Power: 169
Slemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating community
Points: 19,990, Level: 19
Points: 19,990, Level: 19	Points: 19,990, Level: 19	Points: 19,990, Level: 19
Level up: 46%, 810 Points needed
Level up: 46%	Level up: 46%	Level up: 46%
Activity: 2.9%
Activity: 2.9%	Activity: 2.9%	Activity: 2.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Ups, del please.
__________________
Xiloe (Wooteck) worst fear!


## #210  -  Zeuby  -  11th October 2026, 12:08 AM

Quote:
Originally Posted by SigPhreak
I'm good bro, i'm not required to detail every single thing out in order to be helpful. Also, that's not it either.. so i'm not sure why you are coming at me, while listing something still irrelevant for operation.
ncghdll64.dll ? its just the injection agent dropped to %TEMP%\ncgtmp-* when policy decides a process needs closer inspection. It never even deployed during my sessions. The core system is BBt + its hook module so yes BBt. Not sure what else you're seeing, but happy to compare dumps.

to get BBt, scan regions for the plaintext markers that survived:
- PDB path string: D:\ncc_jenkins\workspace\Release_NCGPE\bin\x64\Obfuscate\pdb\BBt.pdb
- ncgpelib64.dll, ncghdll32.dll / ncghdll64.dll
- NcgObfuscationBegin/End

around 16 features DeFe_* :
ProcessScanner, ScanHandle, HookChecker, MemoryChecker,
MemoryProtector, Debugger, VMChecker, RemoteDesktopChecker,
ThirdPartyChecker, LauncherChecker, MultiClientChecker, ScanRulePattern,
ScanThread, CallbackManager, ReplaceMessageProcedure,
ProcessAccessChecker, PreventSuspendProcess, plus a "BypassNCGH" heuristic xD
also DeFe_ScanHandle enumerates handles to the game process via NtQuerySystemInformation (SystemHandleInformation) and CloseProcessHandle force closes anything holding VM_READ/VM_OPERATION on the game.

If you don't want to share then don't comment. Just keep leeching off everyone else's work. We're here to share knowledge and help each other out not to gatekeep [removed] and thinking we're the haxxor of the year. Peace


## #211  -  Danielci58  -  11th October 2026, 12:13 AM

Quote:
Originally Posted by Zeuby
ncghdll64.dll ? its is just the injection agent dropped to %TEMP%\ncgtmp-* when policy decides a process needs closer inspection. It never even deployed during my sessions. The core system is BBt + its hook module so yes BBt. Not sure what else you're seeing, but happy to compare dumps.

to get BBt, scan regions for the plaintext markers that survived:
- PDB path string: D:\ncc_jenkins\workspace\Release_NCGPE\bin\x64\Obfuscate\pdb\BBt.pdb
- ncgpelib64.dll, ncghdll32.dll / ncghdll64.dll
- NcgObfuscationBegin/End

around 16 features DeFe_* :
ProcessScanner, ScanHandle, HookChecker, MemoryChecker,
MemoryProtector, Debugger, VMChecker, RemoteDesktopChecker,
ThirdPartyChecker, LauncherChecker, MultiClientChecker, ScanRulePattern,
ScanThread, CallbackManager, ReplaceMessageProcedure,
ProcessAccessChecker, PreventSuspendProcess, plus a "BypassNCGH" heuristic xD
also DeFe_ScanHandle enumerates handles to the game process via NtQuerySystemInformation (SystemHandleInformation) and CloseProcessHandle force closes anything holding VM_READ/VM_OPERATION on the game.

If you don't want to share then don't comment. Just keep leeching off everyone else's work. We're here to do what we love, help each other out and be a bunch of wannabe haxxor nerds. Peace
Yup thats what I found too. Really helpful info you gave today ty


## #212  -  Shard999  -  11th October 2026, 02:10 AM

Quote:
Originally Posted by Zeuby
ncghdll64.dll ? its just the injection agent dropped to %TEMP%\ncgtmp-* when policy decides a process needs closer inspection. It never even deployed during my sessions. The core system is BBt + its hook module so yes BBt. Not sure what else you're seeing, but happy to compare dumps.

to get BBt, scan regions for the plaintext markers that survived:
- PDB path string: D:\ncc_jenkins\workspace\Release_NCGPE\bin\x64\Obfuscate\pdb\BBt.pdb
- ncgpelib64.dll, ncghdll32.dll / ncghdll64.dll
- NcgObfuscationBegin/End

around 16 features DeFe_* :
ProcessScanner, ScanHandle, HookChecker, MemoryChecker,
MemoryProtector, Debugger, VMChecker, RemoteDesktopChecker,
ThirdPartyChecker, LauncherChecker, MultiClientChecker, ScanRulePattern,
ScanThread, CallbackManager, ReplaceMessageProcedure,
ProcessAccessChecker, PreventSuspendProcess, plus a "BypassNCGH" heuristic xD
also DeFe_ScanHandle enumerates handles to the game process via NtQuerySystemInformation (SystemHandleInformation) and CloseProcessHandle force closes anything holding VM_READ/VM_OPERATION on the game.

If you don't want to share then don't comment. Just keep leeching off everyone else's work. We're here to share knowledge and help each other out not to gatekeep [removed] and thinking we're the haxxor of the year. Peace
The flag (ProcessScanner.injector.switch) is not even included in the embedded policy, so its safe to assume that this measure is currently completely disabled on our client.


## #213  -  WhoWouldCheat  -  11th October 2026, 03:23 AM

Quote:
Originally Posted by yurigagarin
are you external or internal? what other features did u try
Internal why would anyone go external its a waste of time
__________________
just a nn abusing a yzf r1


## #214  -  darksidexc  -  11th October 2026, 03:49 AM

Super l337


darksidexc's Avatar

Join Date: Jun 2013
Location: Moon
Posts: 204
Reputation: 2781
Rep Power: 331
darksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating community
Points: 15,000, Level: 15
Points: 15,000, Level: 15	Points: 15,000, Level: 15	Points: 15,000, Level: 15
Level up: 50%, 1,400 Points needed
Level up: 50%	Level up: 50%	Level up: 50%
Activity: 70.0%
Activity: 70.0%	Activity: 70.0%	Activity: 70.0%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by WhoWouldCheat
Internal why would anyone go external its a waste of time
internal its the major reason for bans since it is by far more detectable in general, so i totally disagree, in a mmorpg the account is real gold..
__________________
 game hacking at all, also know as darksidexc


## #215  -  arisenpai  -  11th October 2026, 04:52 PM

Nemesis Mods CEO

arisenpai's Avatar

Join Date: May 2024
Posts: 34
Reputation: 1108
Rep Power: 61
arisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the worldarisenpai -- This man endangers the world
Points: 3,125, Level: 5
Points: 3,125, Level: 5	Points: 3,125, Level: 5	Points: 3,125, Level: 5
Level up: 41%, 475 Points needed
Level up: 41%	Level up: 41%	Level up: 41%
Activity: 13.9%
Activity: 13.9%	Activity: 13.9%	Activity: 13.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
I dont know what are you guys getting detected through internal , i have been using internal cheat no detection it dosent crash too .
https://www.pasteboard.co/J8gay-qUJC0q.png
__________________
◤ SphinX ◢
「 Owner of Nemesis Mods 」


## #216  -  yurigagarin  -  11th October 2026, 07:06 PM

Quote:
Originally Posted by hunchojacks
tbh i dont know what u mean by dodging jump mechanics. but u can tp up like 10m and then just loop it
mines glitching after i move. does tp still work?


## #217  -  omgiamliving  -  12th October 2026, 03:46 PM

Latest Offsets?


## #218  -  symond100  -  12th October 2026, 08:13 PM

Anyone found this?
Offsets?

Speedup to 1.00 to 5.00
Infinite Fly
Jump height
Animation rate
Time dilation - 0.1x to 3.0x


## #219  -  Federwind  -  12th October 2026, 09:06 PM

Somewhat off topic.

Has anyone figured out how to extract or decode the actual character creator settings from AION 2's online Style Shop presets?

Maybe I'm missing something obvious, but I'm trying to recreate a character from the Asian servers on Global.

Example preset:
Code:
https://nc.com/s/jJeKd
-JSON response contains a encoded string in reserved4.
-The example has something like 1712 bytes of binary data

Found so far:
you can copy the entire data onto any picture and make a working upload
Code:
https://s.g.nc.com/s/t8kED
if you try that you should get a different model than on the image preview


## #220  -  darksidexc  -  12th October 2026, 10:12 PM

Super l337


darksidexc's Avatar

Join Date: Jun 2013
Location: Moon
Posts: 204
Reputation: 2781
Rep Power: 331
darksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating community
Points: 15,000, Level: 15
Points: 15,000, Level: 15	Points: 15,000, Level: 15	Points: 15,000, Level: 15
Level up: 50%, 1,400 Points needed
Level up: 50%	Level up: 50%	Level up: 50%
Activity: 70.0%
Activity: 70.0%	Activity: 70.0%	Activity: 70.0%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
https://i.imgur.com/ZSojihm.mp4


fully auto-evade/auto-dodge, only real time offsets and logics, no skills/mobs database, just real time math and reads.
__________________
 game hacking at all, also know as darksidexc


## #221  -  yurigagarin  -  12th October 2026, 10:51 PM

Quote:
Originally Posted by darksidexc
https://i.imgur.com/ZSojihm.mp4
https://i.imgur.com/ZSojihm.mp4

fully auto-evade/auto-dodge, only real time offsets and logics, no skills/mobs database, just real time math and reads.
whats your read chain on the enemy's attack? im using SkillCasting, maybe theres a different way


## #222  -  mevisitor  -  13th October 2026, 02:11 AM

Quote:
Originally Posted by yurigagarin
whats your read chain on the enemy's attack? im using SkillCasting, maybe theres a different way
what im using is AionSkillNoticeObj actors in the current world near player -> DecalComponentArray

It give you world transform, AoE's location, shape, and visual progress. not sure why my post taken down

but in high level dungeon you maybe want to adjust per skill or per boss because Some Skill can happen after 1-2 second after warning ground reached full and sometime instantly and some dungeon want you to stay inside the warning ground or some want you to jump over it

that why i added boss table later was to fix some mechanics and can adjust decision what to do with certain mech

for PVP auto dodge enemy actor -> rootaction -> AionCharacterAction_Skill -> skill id to filter CC skill -> target entity


## #223  -  yurigagarin  -  13th October 2026, 08:18 AM

Quote:
Originally Posted by mevisitor
what im using is AionSkillNoticeObj actors in the current world near player -> DecalComponentArray

It give you world transform, AoE's location, shape, and visual progress. not sure why my post taken down

but in high level dungeon you maybe want to adjust per skill or per boss because Some Skill can happen after 1-2 second after warning ground reached full and sometime instantly and some dungeon want you to stay inside the warning ground or some want you to jump over it

that why i added boss table later was to fix some mechanics and can adjust decision what to do with certain mech

for PVP auto dodge enemy actor -> rootaction -> AionCharacterAction_Skill -> skill id to filter CC skill -> target entity

oh this is useful man. currently what i was trying to find is to have a time offset so i can dodge earlier. im missing half of boss dodges since my ping is a bit on a high side.. im only reading when the nearest enemy finishes their cast. will check with the visual progress and add some delay. and will add a logic if im inside the aoe. thanks


## #224  -  darksidexc  -  13th October 2026, 03:46 PM

Super l337


darksidexc's Avatar

Join Date: Jun 2013
Location: Moon
Posts: 204
Reputation: 2781
Rep Power: 331
darksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating community
Points: 15,000, Level: 15
Points: 15,000, Level: 15	Points: 15,000, Level: 15	Points: 15,000, Level: 15
Level up: 50%, 1,400 Points needed
Level up: 50%	Level up: 50%	Level up: 50%
Activity: 70.0%
Activity: 70.0%	Activity: 70.0%	Activity: 70.0%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
seems have bb64.dll silent update
__________________
 game hacking at all, also know as darksidexc


## #225  -  whoisteam  -  13th October 2026, 05:52 PM

What's the best way to dump an .exe file?
I want to try creating an emulator server.


## #226  -  spk93  -  13th October 2026, 06:17 PM

btw you can abuse the dxgi.dll


## #227  -  Slemknot  -  13th October 2026, 06:32 PM

Supreme H4x0|2


Slemknot's Avatar

Join Date: Apr 2020
Posts: 681
Reputation: 3105
Rep Power: 169
Slemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating communitySlemknot is a legend in the cheating community
Points: 19,990, Level: 19
Points: 19,990, Level: 19	Points: 19,990, Level: 19	Points: 19,990, Level: 19
Level up: 46%, 810 Points needed
Level up: 46%	Level up: 46%	Level up: 46%
Activity: 2.9%
Activity: 2.9%	Activity: 2.9%	Activity: 2.9%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by spk93
btw you can abuse the dxgi.dll
and not only that)
__________________
Xiloe (Wooteck) worst fear!


## #228  -  fpsnewboy  -  13th October 2026, 07:03 PM

Quote:
Originally Posted by spk93
btw you can abuse the dxgi.dll
d3d11.dll too, there are several methods.

---------------

Code:
// Aion 2 - read the hotbar slots (what's in them + chain state).
// Plug your own engine layer into these 5:
uintptr_t    RPM(uintptr_t addr);                        // read a pointer
template<class T> T Read(uintptr_t addr);
int32_t      PropOffset(const char* cls, const char* prop);   // UE reflection
int32_t      StructPropOffset(const char* st, const char* prop);
std::string  NameOf(uintptr_t obj);
bool         IsA(uintptr_t obj, const char* cls);
std::vector<uintptr_t> ObjectsOfClass(const char* cls);

static uintptr_t Prop(uintptr_t o, const char* cls, const char* p) {
    const int32_t off = PropOffset(cls, p);
    return (o && off >= 0) ? RPM(o + off) : 0;
}
// ESlateVisibility: 0 Visible, 3 HitTestInvisible, 4 SelfHitTestInvisible are on screen.
static bool Shown(uintptr_t w) {
    const int32_t off = PropOffset("Widget", "Visibility");
    if (!w || off < 0) return false;
    const uint8_t v = Read<uint8_t>(w + off);
    return v == 0 || v == 3 || v == 4;
}

// The HUD's skill icons are MATERIALS, not textures - you want the material's
// first texture parameter, or you get the material name for every slot.
static std::string IconName(uintptr_t imageWidget) {
    const int32_t brush = PropOffset("Image", "Brush");
    const int32_t res   = StructPropOffset("SlateBrush", "ResourceObject");
    if (!imageWidget || brush < 0 || res < 0) return {};
    const uintptr_t obj = RPM(imageWidget + brush + res);
    if (!obj) return {};
    if (IsA(obj, "MaterialInstance")) {
        const int32_t arrOff = PropOffset("MaterialInstance", "TextureParameterValues");
        const int32_t valOff = StructPropOffset("TextureParameterValue", "ParameterValue");
        if (arrOff >= 0 && valOff >= 0) {
            const uintptr_t data = RPM(obj + arrOff);
            const int32_t   num  = Read<int32_t>(obj + arrOff + 8);
            const int32_t   stride = /* sizeof FTextureParameterValue, read from the UStruct */ 0;
            for (int i = 0; i < num && i < 4 && data && stride; ++i)
                if (uintptr_t tex = RPM(data + uintptr_t(i) * stride + valOff))
                    return NameOf(tex);
        }
    }
    return NameOf(obj);
}

struct Slot {
    uint8_t     action;        // 93-100 skill, 101-108 item, 109-112 stigma
    std::string icon;          // stable per-skill key
    std::string level;
    bool        chainReady;    // the chain window is OPEN on this slot
    bool        condReady;
    bool        onCooldown;
    uintptr_t   holder;        // call OnQuickSlotPressed/Released on this
};

std::vector<Slot> ReadBar() {
    std::vector<Slot> out;
    for (uintptr_t h : ObjectsOfClass("AionWidget_QuickSlotHolder")) {
        if (!Shown(h)) continue;

        // TRAP: SlotId_Set is NOT unique. There are ~3 live holders per bar
        // position and all three carry the same value - you'd get 60 entries
        // for a 20-slot bar. InputAction is the real identity, and exactly one
        // holder per position has it non-zero.
        const uintptr_t hk = Prop(h, "AionWidget_QuickSlotHolder", "P_HotKey");
        if (!hk) continue;
        const int32_t aOff = PropOffset("AionWidget_HotKey", "InputAction");
        if (aOff < 0) continue;
        const uint8_t action = Read<uint8_t>(hk + aOff);
        if (action < 93 || action > 112) continue;

        // The holder wraps the actual slot widget (AionWidget_QuickSlotSkill).
        uintptr_t w = Prop(h, "AionWidget_QuickSlotHolder", "P_Content");
        if (w) w = Prop(w, "ContentWidget", "Content");
        if (!w) continue;

        Slot s{};
        s.action     = action;
        s.holder     = h;
        s.icon       = IconName(Prop(w, "AionWidget_QuickSlotBase", "P_Img_Icon"));
        s.chainReady = Shown(Prop(w, "AionWidget_QuickSlotSkill", "P_ChainSkill"));
        s.condReady  = Shown(Prop(w, "AionWidget_QuickSlotSkill", "P_ConditionSkill"));
        s.onCooldown = Shown(Prop(w, "AionWidget_QuickSlotBase", "P_PBar_Cooltime"));
        if (uintptr_t t = Prop(w, "AionWidget_QuickSlotSkill", "P_Txt_SkillLv"))
            s.level = /* your FText reader */ {};
        out.push_back(std::move(s));
    }
    return out;
}


## #229  -  vova1210  -  13th October 2026, 07:30 PM

im still new to internals, anyone can possibly help out seeing what am i doing wrong concerning injection and hooking?

im sure i made something super solid, my externals are working just fine

just internal is the one im having a trouble with


## #230  -  darksidexc  -  13th October 2026, 09:24 PM

Super l337


darksidexc's Avatar

Join Date: Jun 2013
Location: Moon
Posts: 204
Reputation: 2781
Rep Power: 331
darksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating community
Points: 15,000, Level: 15
Points: 15,000, Level: 15	Points: 15,000, Level: 15	Points: 15,000, Level: 15
Level up: 50%, 1,400 Points needed
Level up: 50%	Level up: 50%	Level up: 50%
Activity: 70.0%
Activity: 70.0%	Activity: 70.0%	Activity: 70.0%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
external goes easy, someone that was do it all internal, please break the ice here, how you inject the dll here (please it's not a trivial question, im not talking about native inject)
__________________
 game hacking at all, also know as darksidexc


## #231  -  SunBeamx  -  13th October 2026, 10:24 PM

backup to the current anticheat revision, because theys patch tomorrow for WPE+, debuggers`+ obviusly CE (with no bypass universal )

if you make the backup to the rev. hack prevention you can reverter with host/apache to the old version = today version.


## #232  -  Shard999  -  14th October 2026, 01:09 AM

Quote:
Originally Posted by SunBeamx
backup to the current anticheat revision, because theys patch tomorrow for WPE+, debuggers`+ obviusly CE (with no bypass universal )

if you make the backup to the rev. hack prevention you can reverter with host/apache to the old version = today version.
where did you get this information?


## #233  -  vova1210  -  14th October 2026, 04:47 AM

Quote:
Originally Posted by darksidexc
external goes easy, someone that was do it all internal, please break the ice here, how you inject the dll here (please it's not a trivial question, im not talking about native inject)
not at all
im using the following

1. byovd and i load it via NtLoadDriver.

2. Physical Memory R/W → Virtual R/W: Using the driver’s IOCTLs

3. Manual Mapping via Physical Writes

4. Entry Point Execution: Uses NtCreateThreadEx with SKIP_THREAD_ATTACH flag to mainly prevent bb64.dll’s TLS callback from firing on the new thread (which otherwise causes __fastfail 0xC0000409). If that fails, i make it fallback to QueueUserWorkItem.

5.Vtable Hooking (not inline hooking)

that's pretty much it


## #234  -  Mevsa  -  14th October 2026, 10:02 AM

The game was updated a couple of hours ago.
Quote:
Originally Posted by mevisitor
what im using is AionSkillNoticeObj actors in the current world near player -> DecalComponentArray

It give you world transform, AoE's location, shape, and visual progress. not sure why my post taken down

but in high level dungeon you maybe want to adjust per skill or per boss because Some Skill can happen after 1-2 second after warning ground reached full and sometime instantly and some dungeon want you to stay inside the warning ground or some want you to jump over it

that why i added boss table later was to fix some mechanics and can adjust decision what to do with certain mech

for PVP auto dodge enemy actor -> rootaction -> AionCharacterAction_Skill -> skill id to filter CC skill -> target entity
Yes, this method works for me; I also incorporated basic attacks and added an optional rotation for melee characters to enable back attacks—it works perfectly.


## #235  -  qwesdeq  -  14th October 2026, 02:11 PM

14.10
GWorld = 0xEF8EC38
GNames = 0xF12B5C0
GObjects = 0xEC83990
ProcessEvent = 0x4206400

UWorld->PersistentLevel = 0x30
UWorld->OwningGameInstance = 0x1E8

UGameInstance->LocalPlayers = 0x38

ULocalPlayer->PlayerController = 0x30

APlayerController->PlayerState = 0x340
APlayerController->AcknowledgedPawn = 0x378
APlayerController->PlayerCameraManager = 0x3F8

APlayerCameraManager->POV_Location = 0x3E0
APlayerCameraManager->POV_FOV = 0x410

ULevel->ActorsData = 0xA0
ULevel->ActorsCount = 0xA8
ULevel->ActorsMax = 0xAC

AActor->ClassPrivate = 0x10
AActor->FName = 0x18
AActor->RootComponent = 0x1C0
AActor->Nickname = 0x26B8

USceneComponent->RelativeLocation_X = 0x148
USceneComponent->RelativeLocation_Y = 0x150
USceneComponent->RelativeLocation_Z = 0x158


## #236  -  darksidexc  -  14th October 2026, 05:10 PM

Super l337


darksidexc's Avatar

Join Date: Jun 2013
Location: Moon
Posts: 204
Reputation: 2781
Rep Power: 331
darksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating communitydarksidexc is a legend in the cheating community
Points: 15,000, Level: 15
Points: 15,000, Level: 15	Points: 15,000, Level: 15	Points: 15,000, Level: 15
Level up: 50%, 1,400 Points needed
Level up: 50%	Level up: 50%	Level up: 50%
Activity: 70.0%
Activity: 70.0%	Activity: 70.0%	Activity: 70.0%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
Quote:
Originally Posted by qwesdeq
14.10
GWorld = 0xEF8EC38
GNames = 0xF12B5C0
GObjects = 0xEC83990
ProcessEvent = 0x4206400

UWorld->PersistentLevel = 0x30
UWorld->OwningGameInstance = 0x1E8

UGameInstance->LocalPlayers = 0x38

ULocalPlayer->PlayerController = 0x30

APlayerController->PlayerState = 0x340
APlayerController->AcknowledgedPawn = 0x378
APlayerController->PlayerCameraManager = 0x3F8

APlayerCameraManager->POV_Location = 0x3E0
APlayerCameraManager->POV_FOV = 0x410

ULevel->ActorsData = 0xA0
ULevel->ActorsCount = 0xA8
ULevel->ActorsMax = 0xAC

AActor->ClassPrivate = 0x10
AActor->FName = 0x18
AActor->RootComponent = 0x1C0
AActor->Nickname = 0x26B8

USceneComponent->RelativeLocation_X = 0x148
USceneComponent->RelativeLocation_Y = 0x150
USceneComponent->RelativeLocation_Z = 0x158
thanks, all working
__________________
 game hacking at all, also know as darksidexc


## #237  -  yurigagarin  -  14th October 2026, 06:16 PM

anyone got sdk dump? my dumper broke


## #238  -  s4ncak1  -  14th October 2026, 07:51 PM

God-Like


s4ncak1's Avatar

Join Date: Oct 2019
Posts: 163
Reputation: 5481
Rep Power: 179
s4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATSs4ncak1 DEFINES UNKNOWNCHEATS
Points: 16,988, Level: 17
Points: 16,988, Level: 17	Points: 16,988, Level: 17	Points: 16,988, Level: 17
Level up: 43%, 812 Points needed
Level up: 43%	Level up: 43%	Level up: 43%
Activity: 2.8%
Activity: 2.8%	Activity: 2.8%	Activity: 2.8%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
sdk: https://www.unknowncheats.me/forum/d...=file&id=58554


## #239  -  peekmeFast  -  15th October 2026, 11:28 AM

Is it possible to have a gathering speedhack? i saw someone in abyss just instantly getting all the odyle in the abyss.


## #240  -  rhaym  -  15th October 2026, 10:43 PM

File Approved
SHA256: 9b99ee140329d2b66a355219a6c3d13829845a09a9108540f3202cc4e002959e - SDK.zip


Download: https://www.unknowncheats.me/forum/d...=file&id=58554

Thanks for sharing!
__________________
When the snows fall and the white winds blow, the lone wolf dies, but the pack survives.


## #241  -  omgiamliving  -  16th October 2026, 05:09 AM

Is there anyway to detect if mob is hostile towards us?


## #242  -  lovesignal  -  16th October 2026, 06:04 AM

some insight as to known exploits would be really nice. dialogue, looting/gathering, mobs, etc.


## #243  -  yurigagarin  -  16th October 2026, 08:39 AM

Code:
ComponentToWorld FTransform3d = RootComponent + 0x270
Translation                   = RootComponent + 0x290


## #244  -  zombie2001  -  16th October 2026, 02:58 PM

Quote:
Originally Posted by yurigagarin
Code:
ComponentToWorld FTransform3d = RootComponent + 0x270
Translation                   = RootComponent + 0x290

Thanks for the offsets! What are they for?


## #245  -  hunchojacks  -  17th October 2026, 12:53 PM

Quote:
Originally Posted by zombie2001
Thanks for the offsets! What are they for?
wdym theyre named :sob:


## #246  -  zombie2001  -  17th October 2026, 02:39 PM

Quote:
Originally Posted by hunchojacks
wdym theyre named :sob:

my bad im new to this stuff i thought it was auto combat or auto dodge lmfao


## #247  -  Peepcallsmedad  -  18th October 2026, 07:07 AM

any suggestions on solving pathing / possibly abusing the games internal path solving mechanism for gathering and auto discovery externally? managed to get a simple gather bot operating but the pathing is kinda scuffed


## #248  -  nosquare  -  18th October 2026, 07:34 AM

Are anyone find FP (flying stamina) offset/adress ?
in SDK there are few resources:
ECharPointType::FP = x
ECharPointType::MaxFP = x
fp_point_fly_cost
fp_point_fly_dash_cost
fp_point_flying_sprint_cost

AionWidget_HUD_ActionButtonFly
FpUpdater
P_Img_FpGauge
FlightAction_NotEnoughFP

The required data highlighted in bold


## #249  -  anomiesage  -  18th October 2026, 08:01 AM

Been reading that this game only has a usermode AC. I have my own Kernel driver for external so I should be fine just reading cube information without doing anything special correct?


## #250  -  Peepcallsmedad  -  18th October 2026, 08:37 AM

Quote:
Originally Posted by anomiesage
Been reading that this game only has a usermode AC. I have my own Kernel driver for external so I should be fine just reading cube information without doing anything special correct?
yeah you will be fine


## #251  -  omgiamliving  -  18th October 2026, 09:03 AM

Is it possible to change the stamina consumption or flying stamina consumption?


## #252  -  toilahieu  -  18th October 2026, 10:27 AM

does any1 have offset of dmg, boss HP? i want to build a damage meter.


## #253  -  lepurtim  -  18th October 2026, 12:15 PM

Quote:
Originally Posted by Peepcallsmedad
any suggestions on solving pathing / possibly abusing the games internal path solving mechanism for gathering and auto discovery externally? managed to get a simple gather bot operating but the pathing is kinda scuffed
use waypoint recorder for gather bot


## #254  -  syvix  -  18th October 2026, 12:21 PM

Hey, sorry for questioning



I can't get past the CR3 stuff when i try doing it with byovd, maybe i'm too stupid
I could skip it with MmCopyVirtualMemory but i think they check for that?

Could anyone help me out or link me a thread i could learn from?


## #255  -  fauzera  -  20th October 2026, 04:26 AM

Does anyone know how to get the attack range of all entities (including mobs) externally?


## #256  -  peekmeFast  -  21st October 2026, 09:35 PM

Anyone have updated offsets? Thanks!


## #257  -  Mevsa  -  21st October 2026, 11:42 PM

NEW

GNames: 0xF132600
GObjects: 0xEC8A990
GWorld: 0xEF95C78


## #258  -  kaizerken  -  22nd October 2026, 01:28 AM

May i ask the latest download for this aion 2 bot. Thank you in advance


## #259  -  kiramami  -  22nd October 2026, 04:21 AM

guys, i saw someone with instant gathering, literally. Any idea where to look?


## #260  -  zEw00  -  22nd October 2026, 07:13 AM

Has anyone found the key to reserved4 in the styleshop json response? It apparently contains the values for character customization/presets but it's encrypted.

I tried dumping the entire game memory to find the key but to no avail. Perhaps I'm doing something wrong.

Would appreciate it if anyone has some solid leads to the key


## #261  -  yurigagarin  -  22nd October 2026, 10:10 AM

anyone got implemented teleport? my player tp seems like local only. other than movement and auto dodge, what other stuffs that really work and gives advantage (non bot related)?


## #262  -  Federwind  -  22nd October 2026, 10:43 AM

Quote:
Originally Posted by yurigagarin
anyone got implemented teleport? my player tp seems like local only. other than movement and auto dodge, what other stuffs that really work and gives advantage (non bot related)?
In case you have a auto dodge, you mind sharing how you made it?


## #263  -  yurigagarin  -  22nd October 2026, 11:43 AM

Quote:
Originally Posted by Federwind
In case you have a auto dodge, you mind sharing how you made it?
someone shared it a few pages back


## #264  -  Shard999  -  22nd October 2026, 04:17 PM

ppl are talking about stuff like instant gathering or teleporting - but im wondering how thats supposed to work with how strict the server is? The highest deviation I could get from the server was 5% and harvest times are full server controlled, so how can it be possible? lol


## #265  -  iamLau  -  22nd October 2026, 08:31 PM

Code:
    constexpr int32_t OFFSET_GOBJECTS     = 0x0EC8A5A0;
    constexpr int32_t OFFSET_GNAMES       = 0x0F132200;
    constexpr int32_t OFFSET_GWORLD       = 0x0F3CEEB0;
    constexpr int32_t OFFSET_PROCESSEVENT = 0x04206550;
    constexpr int32_t INDEX_PROCESSEVENT  = 0x4D;
heard this got an ac update, kind of disappointed to see that my day 1 internal injector still works


## #266  -  unfairuser67  -  23rd October 2026, 11:25 AM

How do you guys dump the game? I suspended the game at different points during loading and created a dump with x64dbg and Scylla, setting the OEP to AION2.exe MainEntryPoint, but Scylla's IAT Search did not find anything. The resulting dump has the same sections as the original binary (if relevant); IDA also appears to hang when I try to load the dumped binary: it produces no logs, remains at 0% CPU usage, and has not finished after four hours. So the dumps I make are invalid. ScyllaHide is detected by AC; Dumper-7 (manual map) doesn't work; also, I tried searching the runtime for UE objects and patterns (ring0 read), but was unsuccessful (I am unfamiliar with how VM-packed binaries behave at runtime). So I would appreciate help in understanding how to do this correctly, and where I am going wrong.


## #267  -  kalo93  -  23rd October 2026, 11:37 AM

Quote:
Originally Posted by iamLau
Code:
    constexpr int32_t OFFSET_GOBJECTS     = 0x0EC8A5A0;
    constexpr int32_t OFFSET_GNAMES       = 0x0F132200;
    constexpr int32_t OFFSET_GWORLD       = 0x0F3CEEB0;
    constexpr int32_t OFFSET_PROCESSEVENT = 0x04206550;
    constexpr int32_t INDEX_PROCESSEVENT  = 0x4D;
heard this got an ac update, kind of disappointed to see that my day 1 internal injector still works
Are you dumping with Dumper7 or something else?


## #268  -  Danielci58  -  23rd October 2026, 02:34 PM

Quote:
Originally Posted by iamLau
Code:
    constexpr int32_t OFFSET_GOBJECTS     = 0x0EC8A5A0;
    constexpr int32_t OFFSET_GNAMES       = 0x0F132200;
    constexpr int32_t OFFSET_GWORLD       = 0x0F3CEEB0;
    constexpr int32_t OFFSET_PROCESSEVENT = 0x04206550;
    constexpr int32_t INDEX_PROCESSEVENT  = 0x4D;
heard this got an ac update, kind of disappointed to see that my day 1 internal injector still works

AC update ? I didnt notice much, I updated the bypass and everything seems to work as usual.


## #269  -  ilikepie123  -  23rd October 2026, 09:12 PM

anyone have any luck being able to read the card rewards from things like shugo before flipping? to pick what rewards you want?


## #270  -  yurigagarin  -  24th October 2026, 12:41 AM

Quote:
Originally Posted by ilikepie123
anyone have any luck being able to read the card rewards from things like shugo before flipping? to pick what rewards you want?
its server side. u can inspect packets coming only after pressing confirm. same for crafting, you will just receive processed packets that will show to the memory


## #271  -  omgiamliving  -  24th October 2026, 09:03 PM

has anyone bale to increase combat speed?


## #272  -  MgproOv2  -  25th October 2026, 07:28 PM

Is there a way to increase movement speed without increasing the animation speed of everything?
I currently use TimeDilation or CinematicTimeDilation, but enemies and boss attacks also get sped up which is really annoying.
I'd be grateful for any tips


## #273  -  syvix  -  26th October 2026, 12:51 PM

Last update

GWorld Offset: 0xEF95C78
GNames Offset: 0xF132600
GObjects Offset: 0xEC8A9A0


## #274  -  nosquare  -  26th October 2026, 05:18 PM

Code:
// Relative to the monster actor:
constexpr uintptr_t OFFSET_MOB_RUNTIME_KEY = 0x26F0; // int32_t
constexpr uintptr_t OFFSET_MOB_NATIVE_DATA = 0x2728; // Pointer to NativeData

// Relative to NativeData:
constexpr uintptr_t OFFSET_NATIVE_RUNTIME_KEY = 0x00; // int32_t
constexpr uintptr_t OFFSET_NATIVE_NPC_ID      = 0x04; // int32_t — NPC/template ID
The runtime key identifies the spawned instance. The NPC/template ID links it to the material/drop catalog. These offsets are client-build-specific; dynamic pointers and values still require memory reads.


## #275  -  Dankboose  -  26th October 2026, 11:55 PM

Oct 26, 2026
GNames = 0x0F132600
GObjects = 0x0EC8A990
GWorld = 0x0EF95C78
ProcessEvent = 0x04206550
ExeSize = 156609312ULL

Please let me know if this works for you as well as it worked for me.


## #276  -  DragonNight2000  -  27th October 2026, 05:26 PM

God-Like


DragonNight2000's Avatar

Join Date: Jan 2018
Posts: 154
Reputation: 168
Rep Power: 215
DragonNight2000 is known to create posts excellent in qualityDragonNight2000 is known to create posts excellent in quality
Points: 6,938, Level: 9
Points: 6,938, Level: 9	Points: 6,938, Level: 9	Points: 6,938, Level: 9
Level up: 40%, 662 Points needed
Level up: 40%	Level up: 40%	Level up: 40%
Activity: 4.8%
Activity: 4.8%	Activity: 4.8%	Activity: 4.8%
Aion2 Reversal, Structs and OffsetsAion2 Reversal, Structs and Offsets
anyone knows how rarity and value could be implemented for items ?


## #277  -  prixone  -  28th October 2026, 09:22 AM

have anyone figured out how to decrypt/read the presets from aion2?

sample data

Code:
sLflmZ5pMzTK6FgdoUm4vIl26FXlkrOFe4M/fFHtDw17WN2YwjattH9j6saVkkuKCdESasf/hrLy80BnYkwWu2KjHTe/UNNDvQ76R7xvOPsXr+g8LGJlUg5MnboCQc7AugtqJIwodsT3IDItOIVPI4gcKBjrOUgf3jVoO2LuRpKfJ21X3yX7NlE2GlFsza5PUjpo55iawAABdR4ZCRjhOFxkUcE44Q4yzJMv9ePck642ww6POboRTSjcuhrshnYtV+KRNLwrd/+6R3ZvCzAXQnBsaFwbPuCC46V25XIzT+sciu0AOkhsUrKYZanaD0MYnmWUTPfnYZZOkNevsuNwHJ7XQTpfC8xfHKasIYR/JHNYKWf/6uust7MVA0PsemAN7FdOirXcoJKjlB3lKrzFpckLMorpXRoZzBN+R/268etSDkIPaK4l+nTwYfTcv5b9u9Irx2j+h76pM9UbGtydKqiHv4BXQs4oAU3CEb+o6tLl/wbMt9nLDqhLCLiO4yKnOQsggnb7ihOXkqxdLi7Woc6Q2m1CTLBEm4Jv4IElMqsDtXL8m+YmKHB2BImEuSGX7oO1WjAxGTILzsm8c/J5HxjSP3FkeMJNNYGFnJ7v67BShnk4VvVLARSTJnp6+cVqgxsUwEnFW2r+tIcnISh0G/YTLv90BzhibManBtKX1+nT2zb9gP+/lwXjh9F+DTN+TIrA5+J8q4O5fMr3kRbbZ4RuVR9WAG7o2cukZgxF21VOuOmQVoCkf7N/jNJPWtOlydM1BrJ7S6GLMB6kW3jNgkRrXFaAJRysvOzPIahmlKj/ZxRu3bzKpn80wS1ovkQhh2Ev3X99M21GZfCTHb+GvuIeSJ3xKhjX+psTnBD17qypjVkfc+igaMorSutAUPZC0rOeXfmfwsRJHqBjygOKQZeDtv44gbakgPVG0Y6xF68oXEHbvewqdjnb6zh9rCirzRy6PRdp/J3ShadgZGohQuCk33L0n1UXbrikAR2tPYkPMSKljrAjRXB0AUX/TuyCi+MfkR6JIH+KTiIttbU6TPTnn+6AseXv4xhNYe0FyOpd19UoJd6bxEDFseyxZLmKeLIXeBcz0MIn4u4fzfuWH2C1OBrBSBEJNG/fXS3TZXRaHPkSZeuutGLtaIyZR7MtN0oEFrYnVjce242sjFH+lYVccDKJ5qpgUXJi9wli9EyILIeTZILPlz/NPiY9iTfSNVIYwm9524oVOVj8RYnPRnFPxKc6EEH/7NzkG68eKkjfAlmg9c216WEc05TH7zEuoBxURLi430qOQgctId607PP+Spl5LCCQoShib4N1T4UDtzf7ch1MefenbStHK0XjZV4yqMP3w5Qvqtr0XkUAJphFgYb1jwSsYd2+/cgj5G0HbZYyJ4XmgWSvQjyQZW4idoqUb1S8VrXhgiFFXsRceAVseC7a33+RtDdJPtK3ew9Q0n7Afo/jGYslZiWvS1aM7X6QFGGaLSbN2L3FEHkQPd7ImLFSodvN0m1gMzj2G8iKbG5daLsHph2z0+0EGZ+o7908/JfNjJrbvSAd/yadqjLyPAu9XBe0efYIRPV3/iLzkrkyvINnnxCMmfZHhdVzQI5+xXIxhR6xyMlZqnYwjO75S3G/AJqUtEuwvN6A0F2wMmr05FEp4M5u5rD/zhYYm6rWuGqt7zlmN1nCLTSCb7yChYbCHlTJXLg3Y32RVN9BjB8OjRt/jaVEHpWjwE60SJs7dNLkXBuZ5NrSdnfECseDGQaScWD+y94lao8b6JImKmokt4k2ncCvIHKpwzZL/Tj2J0rN95xiPv21+3qnVNY4JN1YJQ6r0fg838G61ViOlk/tC6LJFYMb1GDzFA/8SBXWsRecwTsMla3CwXO72Sp/Pc6DuMShibJjuURhA4Nh8EBA+iALzyTl7Y90dudSpEuPsdBX8kaDfYt6Jstw9n17rfraZNqjfEYrKEBBWbWXeKYnXvu/ESBjJrV4x6CzYDqldstp9A/YkcRGTFy15WF1AZYLEtwgFAUudCpeNZMncN2JAapP/HIwrkMu2WVu


## #278  -  nosquare  -  28th October 2026, 12:45 PM

Oct 28, 2026

Code:
constexpr uintptr_t OFFSET_GNAMES       = 0xF137700;
constexpr uintptr_t OFFSET_GOBJECTS     = 0xEC8F910;
constexpr uintptr_t OFFSET_GWORLD       = 0xEF9AE08;
constexpr uintptr_t OFFSET_PROCESSEVENT = 0x4206430;
constexpr int       INDEX_PROCESSEVENT  = 0x4D;


## #279  -  peekmeFast  -  28th October 2026, 01:56 PM

anyone have updated SDK files?


## #280  -  omgiamliving  -  28th October 2026, 04:02 PM

Quote:
Originally Posted by nosquare
Oct 28, 2026

Code:
constexpr uintptr_t OFFSET_GNAMES       = 0xF137700;
constexpr uintptr_t OFFSET_GOBJECTS     = 0xEC8F910;
constexpr uintptr_t OFFSET_GWORLD       = 0xEF9AE08;
constexpr uintptr_t OFFSET_PROCESSEVENT = 0x4206430;
constexpr int       INDEX_PROCESSEVENT  = 0x4D;
Cheers!
Although how did you get the offsets before they even released the patch?


## #281  -  nosquare  -  28th October 2026, 04:23 PM

Quote:
Originally Posted by omgiamliving
Cheers!
Although how did you get the offsets before they even released the patch?
No time machine, sadly 😄 I pulled them directly from the updated Steam client while it was running.


## #282  -  midas7777  -  28th October 2026, 05:24 PM

Quote:
Originally Posted by yurigagarin
someone shared it a few pages back
I think that post somehow got deleted :/


## #283  -  Dankboose  -  28th October 2026, 10:41 PM

Oct 28, 2026 SDK Dump,

Download: https://www.unknowncheats.me/forum/d...=file&id=59014


## #284  -  anomiesage  -  29th October 2026, 04:16 AM

Anyone know if you get banned, does it show on your steam account? I'm planning to play around but if they show I would rather test on an account with a game ban on it before, I move it to an account that also cheats but no game ban.


## #285  -  prixone  -  29th October 2026, 05:59 AM

Quote:
Originally Posted by anomiesage
Anyone know if you get banned, does it show on your steam account? I'm planning to play around but if they show I would rather test on an account with a game ban on it before, I move it to an account that also cheats but no game ban.
just make a new account with purple instead and play on that instead, if you dont want to risk your main account.


## #286  -  rhaym  -  29th October 2026, 03:22 PM

File Approved
SHA256: 9ba4ad5f4eae3e3df2ad0b43b0f9e9f65cf8544f51f221349d8110f3bc8f4a55 - AION2 SDK


Download: https://www.unknowncheats.me/forum/d...=file&id=59014

Thanks for sharing!
__________________
When the snows fall and the white winds blow, the lone wolf dies, but the pack survives.


## #287  -  anomiesage  -  29th October 2026, 04:18 PM

Quote:
Originally Posted by prixone
just make a new account with purple instead and play on that instead, if you dont want to risk your main account.
I don't cheat on my main account ever, that wasn't my question. I have 2 tiers of cheat accounts. One that has game bans on it for games that do that and one that doesn't have gamebans because those games don't do that. I'm just trying to figure out which cheat account it should go on.


## #288  -  lancelot9999  -  Yesterday, 05:49 AM

anyone find out where is the fraction offset? I want to create a radar but couldn't find a reliable way to read it. The ERace from the SDK always read my char race.


## #289  -  yurigagarin  -  Yesterday, 09:58 AM

Quote:
Originally Posted by lancelot9999
anyone find out where is the fraction offset? I want to create a radar but couldn't find a reliable way to read it. The ERace from the SDK always read my char race.
Code:
PlayerActor + 0x3C0            → Character Mesh
Mesh + 0x988                   → AnimScriptInstance
AnimScriptInstance + 0x596     → PcRace uint8
PcRace uses ERace:
Code:
0 = None
1 = Light
2 = Dark
3 = All
Code:
differentRace =
    (localRace == 1 || localRace == 2) &&
    (remoteRace == 1 || remoteRace == 2) &&
    remoteRace != localRace;
Radar is always capped to what the game wants us to show. So for massive pvps, radar is not that useful.

Radar is always capped to what the game wants us to show. So for massive pvps, radar is not that useful.


## #290  -  lancelot9999  -  Yesterday, 03:06 PM

Quote:
Originally Posted by yurigagarin
Code:
PlayerActor + 0x3C0            → Character Mesh
Mesh + 0x988                   → AnimScriptInstance
AnimScriptInstance + 0x596     → PcRace uint8
PcRace uses ERace:
Code:
0 = None
1 = Light
2 = Dark
3 = All
Code:
differentRace =
    (localRace == 1 || localRace == 2) &&
    (remoteRace == 1 || remoteRace == 2) &&
    remoteRace != localRace;
Radar is always capped to what the game wants us to show. So for massive pvps, radar is not that useful.

Radar is always capped to what the game wants us to show. So for massive pvps, radar is not that useful.

Thanks for the info! My goal is mainly for smaller Abyss battles, where I want to count how many Elyos and Asmo players are within 30m of me.

I've tried that exact PcRace pointer chain, but in the Abyss it always returns my own faction (1 or 2) for every nearby player, even when they're clearly from the opposite faction.

Do you happen to know another pointer chain or offset to check whether a player is hostile or targetable? I don't necessarily need their actual race, just a reliable way to distinguish enemies from friendly players.


## #291  -  prixone  -  Yesterday, 04:20 PM

Quote:
Originally Posted by prixone
have anyone figured out how to decrypt/read the presets from aion2?

sample data

Code:
sLflmZ5pMzTK6FgdoUm4vIl26FXlkrOFe4M/fFHtDw17WN2YwjattH9j6saVkkuKCdESasf/hrLy80BnYkwWu2KjHTe/UNNDvQ76R7xvOPsXr+g8LGJlUg5MnboCQc7AugtqJIwodsT3IDItOIVPI4gcKBjrOUgf3jVoO2LuRpKfJ21X3yX7NlE2GlFsza5PUjpo55iawAABdR4ZCRjhOFxkUcE44Q4yzJMv9ePck642ww6POboRTSjcuhrshnYtV+KRNLwrd/+6R3ZvCzAXQnBsaFwbPuCC46V25XIzT+sciu0AOkhsUrKYZanaD0MYnmWUTPfnYZZOkNevsuNwHJ7XQTpfC8xfHKasIYR/JHNYKWf/6uust7MVA0PsemAN7FdOirXcoJKjlB3lKrzFpckLMorpXRoZzBN+R/268etSDkIPaK4l+nTwYfTcv5b9u9Irx2j+h76pM9UbGtydKqiHv4BXQs4oAU3CEb+o6tLl/wbMt9nLDqhLCLiO4yKnOQsggnb7ihOXkqxdLi7Woc6Q2m1CTLBEm4Jv4IElMqsDtXL8m+YmKHB2BImEuSGX7oO1WjAxGTILzsm8c/J5HxjSP3FkeMJNNYGFnJ7v67BShnk4VvVLARSTJnp6+cVqgxsUwEnFW2r+tIcnISh0G/YTLv90BzhibManBtKX1+nT2zb9gP+/lwXjh9F+DTN+TIrA5+J8q4O5fMr3kRbbZ4RuVR9WAG7o2cukZgxF21VOuOmQVoCkf7N/jNJPWtOlydM1BrJ7S6GLMB6kW3jNgkRrXFaAJRysvOzPIahmlKj/ZxRu3bzKpn80wS1ovkQhh2Ev3X99M21GZfCTHb+GvuIeSJ3xKhjX+psTnBD17qypjVkfc+igaMorSutAUPZC0rOeXfmfwsRJHqBjygOKQZeDtv44gbakgPVG0Y6xF68oXEHbvewqdjnb6zh9rCirzRy6PRdp/J3ShadgZGohQuCk33L0n1UXbrikAR2tPYkPMSKljrAjRXB0AUX/TuyCi+MfkR6JIH+KTiIttbU6TPTnn+6AseXv4xhNYe0FyOpd19UoJd6bxEDFseyxZLmKeLIXeBcz0MIn4u4fzfuWH2C1OBrBSBEJNG/fXS3TZXRaHPkSZeuutGLtaIyZR7MtN0oEFrYnVjce242sjFH+lYVccDKJ5qpgUXJi9wli9EyILIeTZILPlz/NPiY9iTfSNVIYwm9524oVOVj8RYnPRnFPxKc6EEH/7NzkG68eKkjfAlmg9c216WEc05TH7zEuoBxURLi430qOQgctId607PP+Spl5LCCQoShib4N1T4UDtzf7ch1MefenbStHK0XjZV4yqMP3w5Qvqtr0XkUAJphFgYb1jwSsYd2+/cgj5G0HbZYyJ4XmgWSvQjyQZW4idoqUb1S8VrXhgiFFXsRceAVseC7a33+RtDdJPtK3ew9Q0n7Afo/jGYslZiWvS1aM7X6QFGGaLSbN2L3FEHkQPd7ImLFSodvN0m1gMzj2G8iKbG5daLsHph2z0+0EGZ+o7908/JfNjJrbvSAd/yadqjLyPAu9XBe0efYIRPV3/iLzkrkyvINnnxCMmfZHhdVzQI5+xXIxhR6xyMlZqnYwjO75S3G/AJqUtEuwvN6A0F2wMmr05FEp4M5u5rD/zhYYm6rWuGqt7zlmN1nCLTSCb7yChYbCHlTJXLg3Y32RVN9BjB8OjRt/jaVEHpWjwE60SJs7dNLkXBuZ5NrSdnfECseDGQaScWD+y94lao8b6JImKmokt4k2ncCvIHKpwzZL/Tj2J0rN95xiPv21+3qnVNY4JN1YJQ6r0fg838G61ViOlk/tC6LJFYMb1GDzFA/8SBXWsRecwTsMla3CwXO72Sp/Pc6DuMShibJjuURhA4Nh8EBA+iALzyTl7Y90dudSpEuPsdBX8kaDfYt6Jstw9n17rfraZNqjfEYrKEBBWbWXeKYnXvu/ESBjJrV4x6CzYDqldstp9A/YkcRGTFy15WF1AZYLEtwgFAUudCpeNZMncN2JAapP/HIwrkMu2WVu
any good souls, should be a walk in the park for a seasoned person


## #292  -  dollyXtoddy  -  Today, 12:41 AM

"aion2-protocol-research" on github there almost everything for Client to Server
