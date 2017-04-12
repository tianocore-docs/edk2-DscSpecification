<!--- @file
  3.9 [LibraryClasses] Sections

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

## 3.9 [LibraryClasses] Sections

The `[LibraryClasses]` sections are optional if no library classes are defined
for any of the components, or if only EDK modules are used.

#### Summary

This section defines the `[LibraryClasses]` tag required for EDK II module INF
files, and is new for EDK II extended DSC files. This is a mapping of library
class names to the EDK II module instances that provide the library class.

The one or more `!include` statements may be used within the library class
section.

The !include files may contain `LibraryClass|Library Instance` statements as
well as complete Library Class sections.

The library class entry is a formatted string with two fields, separated by
the pipe "|" character.

When parsing the DSC file, the precedence rules must be followed.

1. If a Library Class Instance (INF) is specified in the EDK II `[Components]`
   section (INF file's `<LibraryClasses>` sub-section,) then it will be used.

2. If not specified in the `[Components]` section, then the Library Class
   Instance that is defined in the `[LibraryClasses.$(ARCH).$(MODULE_TYPE)]`
   section will be used.

3. If not specified in the `[LibraryClasses.$(ARCH).$(MODULE_TYPE)]` section,
   then the Library Class Instance that is defined in the
   `[LibraryClasses.Common.$(MODULE_TYPE)]` section will be used.

4. If not specified in the `[LibraryClasses.Common.$(MODULE_TYPE)]` section,
   then the Library Class Instance that is defined in the
   `[LibraryClasses.$(ARCH)]` section will be used.

5. If not specified in the `[LibraryClasses.$(ARCH)]` section, then the Library
   Class Instance that is defined in the `[LibraryClasses]` Section or
   `[LibraryClasses.Common]` section will be used.

6. It is an error if it has not been specified in one of the above sections.

#### Prototype

```c
<LibraryClasses>   ::= "[LibraryClasses" [<attribs>] "]" <EOL> <LcStatements>*
<attribs>          ::= <attrs> ["," "LibraryClasses" <attrs>]*
<attrs>            ::= "." <arch> ["." <Edk2ModuleType>]
<LcStatements>     ::= {<MacroDefinition>} {<IncludeStatment>}
                       {<TS> <LcEntry>}
<LcEntry>          ::= <LibraryClassName> <FS> <LibraryInstance> <EOL>
<LibraryClassName> ::= (A-Z)(a-zA-Z0-9)*
<LibraryInstance>  ::= <InfFileName> <EOL>
<InfFileName>      ::= <PATH> <Word> ".inf"
```

#### Example

```ini
[LibraryClasses.common]
  BaseLib|MdePkg/Library/BaseLib.inf
  BaseMemoryLib|MdePkg/Library/BaseMemoryLib/BaseMemoryLib.inf
  DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf PcdLib|MdePkg/Library/BasePcdLib/BasePcdLibNull.inf

[LibraryClasses.common.PEI_CORE, LibraryClasses.common.PEIM]
  DEFINE MDIR = MdePkg/Library
  DebugLib|$(MDIR)/PeiReportStatusCode/PeiReportStatusCodeLib.inf

[LibraryClasses.common.UEFI_DRIVER,
  LibraryClasses.common.UEFI_APPLICATION]
  DebugLib|MdePkg/Library/UefiDebugLibStdErr/UefiDebugLibStdErr.inf
```
