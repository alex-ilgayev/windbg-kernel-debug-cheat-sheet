# WinDBG Kernel Debug Cheat-Sheet

I won't cover the basic commands (`lm`/`dt` and more), but try to focus other unique but useful WinDBG utilities.

## General Useful Commands

`kd> ln fffff8014b60e180` - Shows the symbol for given address</br>
`kd> .writemem <dump_path> 0x80f5e000 L 0xA000` - Dumps a PE file to disk</br>
`kd> uf /c 0x833cfb43` - Unassemble Function - show only function calls.</br>
`kd> !dh 0x80f5e000` - Tries to parse a PE image header</br>
`kd> !dh -e ndis` - List export functions for loaded module</br>
`kd> lm m ndis` - Show specific module</br>

## Processes

`kd> !process 0 0` - List all processes</br>
`kd> !process 0 0 cmd.exe` - Find process</br>
`kd> !process -1 0` - Current process context</br>
`kd> .process /i ffffd302f03020c0` - Switch to usermode process VAS</br>
`kd> .reload -user` - Reload user-mode symbols</br>
`kd> bp /p ffffd302f03020c0 ntdll!NtCreateFile` - Breakpoint in context of usermode process</br>
`kd> !token 0xffffaa08ba807635` - Info about a process token</br>
`kd> !pte fffff8016037a9f8` - Convert VA to PA</br>

## Drivers Specific

`kd> !drvobj \Driver\kbdclass 2` - Extensive info about driver object</br>
`kd> !devobj 0x83272ca0` - Device object info</br>
`kd> !devstack 0x83272ca0` - Device stack for a device object</br>
`kd> !vpb 0x8325c298` - Volume Parameter Block: map between dev objects and physical devices</br>
`kd> !object \Device\Ide\IdeDeviceP0T0L0-0` - Query driver object</br>
`kd> !object \global??\c:` - Query symbolic link object</br>
`kd> .trap ffffa482a55cd990` - Set to trap frame context (to observe kernel registers). trap frame could be observed using `kv` command</br>

## Memory

`kd> !pool 0x833cfb43` - Info about pool page region</br>
`kd> !poolfind Driv` - Scans pools for specific tag</br>
`kd> !address 0x833cfb43` - Info about address region</br>
`kd> s-b 833cb000 833cb000+0x57000 4d 5a 90 00` - Searching for MZ in a region</br>

## Extensions

`kd> !fltkd.help` - Various commands for dealing with filter drivers</br>
`kd> !fltkd.filters` - Show filter list</br>
`kd> !ndiskd.help` - Various commands for dealing with NDIS drivers</br>
`kd> !ndiskd.miniports` - List of miniport NDIS drivers</br>
`kd> !ndiskd.minidriver ffffd7050de514f0 -handler` - Lists all handlers for specific driver</br>
`kd> !ndiskd.filters` - Query NDIS filters for each adapter</br>
`kd> !wdfkd.help` - Various commands for dealing with WDF drivers</br>

## Useful Exported NTOSKRNL Symbols

`kd> dp nt!PsInitialSystemProcess L1` - SYSTEM EPROCESS pointer</br>
`kd> dp nt!MmPteBase L1` - Page table base address. The value is being randomized in boot process</br>
`kd> dp nt!MmHighestUserAddress L1` - Highest address for userspace VA</br>
`kd> dp nt!MmSystemRangeStart L1` - Lowest address for kernel VA</br>
`kd> x nt!PsLoadedModuleList` - Loaded modules in kernel (_LIST_ENTRY)</br>
`kd> dt <list_base> nt!_KLDR_DATA_TABLE_ENTRY -l InLoadOrderLinks.Flink FullDllName` - Traversing loaded modules in kernel</br>
`kd> dq nt!SeLoadDriverPrivilege L1` - Privilege for loading a driver</br>
`kd> dps nt!HalDispatchTable` - HAL dispatch table</br>
`kd> dp nt!PsJobType L1` - Job creation configuration (_OBJECT_TYPE)</br>
`kd> dp nt!PsProcessType L1` - Process creation configuration (_OBJECT_TYPE)</br>
`kd> dp nt!PsThreadType L1` - Thread creation configuration (_OBJECT_TYPE)</br>
`kd> db nt!PspNotifyEnableMask L1` - Flag which can disable kernel notify routines</br>
`kd> rdmsr 0xc0000082` - System call handler (`KiSystemCall64Shadow`)</br>
