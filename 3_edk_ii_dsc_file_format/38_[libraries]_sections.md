<!--- @file
  3.8 [Libraries] Sections

  Copyright (c) 2006-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

## 3.8 [Libraries] Sections

The section `[Libraries]` sections are optional in EDK II DSC files, although
if any EDK component is specified in the `[Components]` section, then the EDK
II DSC file must contain this section. EDK components can not use EDK II
Library Instances.

#### Summary

Defines the `[Libraries]` section tag which lists the libraries that are linked
against

EDK I components. Entries may appear in any order. Entries for EDK are a list
of INF files, with a path that is relative to the `$(EFI_SOURCE)`,
`$(EDK_SOURCE)` or `$(ECP_SOURCE)` directories (or a MACRO definition).

One or more `!include` statements may be used within the libraries sections. If
used, the file included must be a list of INF files and their paths relative to
the `$(EFI_SOURCE)`, `$(EDK_SOURCE)` or `$(ECP_SOURCE)` directories.

#### Prototype

```c
<Libraries>       ::= "[Libraries" [<attribs>] "]" <EOL> <LibStatements>*
<LibStatements>   ::= {<MacroDefinition>} {<IncludeStatement>}
                      {<Statement>}
<attribs>         ::= "." <arch> [", Libraries." <arch>]*
<Statement>       ::= <TS> <PathAndFilename> <EOL>
<PathAndFilename> ::= <LPATH> <Word> ".inf"
<LPATH>           ::= [{"$(EDK_SOURCE)"} {<MACROVAL>} "/"] <Path>*
<Path>            ::= <Word> "/"
```

#### Parameters

**_arch_**

The arch attribute must be specified in the `Conf/tools_def.txt` file for the
tool chain used to build the platform in order to be valid.

**_Path_**

If only the `<Path>` element is specified, the path is `WORKSPACE` relative.

#### Example

```ini
[Libraries]
  Foundation/Efi/Guid/EfiGuidLib.inf
  Foundation/Framework/Guid/EdkFrameworkGuidLib.inf
  Foundation/Guid/EdkGuidLib.inf
  Foundation/Library/EfiCommonLib/EfiCommonLib.inf
  Foundation/Cpu/Pentium/CpuIA32Lib/CpuIA32Lib.inf
  Foundation/Cpu/Itanium/CpuIA64Lib/CpuIA64Lib.inf
  Foundation/Library/CustomizedDecompress/CustomizedDecompress.inf

[Libraries.IA32]
  DEFINE PLATFORM_DIR = $(EDK_SOURCE)/Platform
  $(PLATFORM_DIR)/IntelEpg/Guid/IntelEpgGuidLib.inf
  $(PLATFORM_DIR)/IntelEpg/Ppi/IntelEpgPpiLib.inf
  $(PLATFORM_DIR)/Generic/Guid/GenericGuidLib.inf
  $(PLATFORM_DIR)/Generic/Lib/Port80MappingLib/PlatformPort80.inf
```
