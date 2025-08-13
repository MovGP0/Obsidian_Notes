---
title: Debugging in .NET
---

## Options for debugging

### .NET CLR Debugging API (`ICorDebug`)

This is the low-level interface that Visual Studio’s debugger uses.

Requires implementing the COM-based debug interfaces or using a wrapper library

### Windows Debugger (WinDbg)

[WinDbg](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/) (and its console counterpart CDB) use the Windows Debugger Engine (DbgEng) for controlling processes.

```sh
winget install Microsoft.WinDbg
```

For debugging of managed code (.NET/UWP) the [SOS debugging extension](https://learn.microsoft.com/en-us/dotnet/framework/tools/sos-dll-sos-debugging-extension) is required.

See also: 
- [Debugging Managed Code Using the Windows Debugger](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-managed-code)

### .NET Framework Command-Line Debugger (MDbg.exe)

See also:
- [.NET Framework Command-Line Debugger](https://learn.microsoft.com/en-us/dotnet/framework/tools/mdbg-exe)

### [[ClrDebug]]

[ClrDebug](https://github.com/lordmilko/ClrDebug) is a managed wrapper around common debugging APIs:

- CorDebug (`ICorDebug*`)
- Metadata (`IMetaData*`)
- Profiling (`ICorProfiler*`)
- Diagnostics Symbol Store (`ISym*`)
- `IXCLR*`/`ISOS*`/DAC
- DbgEng (`IDebug*`)
- DIA
- PDB1
- TTD
