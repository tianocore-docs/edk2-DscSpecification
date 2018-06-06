<!--- @file
  3.11 [Components] Sections

  Copyright (c) 2006-2018, Intel Corporation. All rights reserved.<BR>

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

## 3.11 [Components] Sections

The `[Components]` sections are required.

#### Summary

This section defines the modules and components that will be processed by
compilation tools and the EDK II tools used to generate PE32/PE32+/Coff image
files.

The `!include` statement is permitted in `[Components]` sections. however this
method is NOT recommended.

All EDK II file paths must be specified relative to a directory containing EDK
II Packages (as specified by the WORKSPACE or a directory listed in
PACKAGES_PATH system environment variable). EDK INF component and library files
may use `$(EDK_SOURCE)` or `$(EFI_SOURCE)` global environment variables. If the
environment variable is not specified, the INF file path is assumed to be
relative to the `WORKSPACE`.

The following is an example of specifying a `WORKSPACE` (MdeModulePkg is in the
directory pointed to by the WORKSPACE environment variable) relative Path:

`MdeModulePkg/Universal/Disk/DiskIo/Dxe`

The following is an example of specifying an Indirect Path:

```ini
DEFINE FOUNDATION_LIB = $(EDK_SOURCE)/Foundation/Library
$(FOUNDATION_LIB)/EdkIIGlueLib/EntryPoints
```

The permitted `DEFINE` statement must be a variable name assigned to a path.

EDK II DSC files allow specifying only one optional argument on an INF file
entry line. The argument, `EXEC = filename`, is used for User Defined
processing of an INF file. The EDK II parsing tools will call the program
listed by filename (which must either be in the OS `PATH` environment, or fully
qualified path and filename) with the INF filename (expanded) as the one and
only argument to filename.

EDK II modules can have scoped (scoping encapsulation between "{" and "}"
braces) sub-elements, `<LibraryClasses>`, `<Pcd*>` and/or `<BuildOptions>` that
allow individual modules to supersede previous definitions. The values
specified on the command-line have the highest precedence followed by values
specified in the sub-elements. Refer to _Section 2.4_ regarding how build
options are used by the EDK II build tools, as well as the _EDK II Build
Specification_.

The PCD access methods (and storage methods) are selected on a platform basis -
it is not permitted to have a PCD listed in one of the Pcd sections and use it
differently in an individual module. For example, if a PCD is listed in a
`[PcdsFixedAtBuild]` section, it is not permitted to list it in a
`<PcdsPatchableInModule>` sub-section of an INF file.

It is permitted to have a PatchableInModule PCD or FixedAtBuild PCD with
different values. If a PCD is listed in `[PcdsFixedAtBuild]` section with one
value, while the `<PcdsFixedAtBuild>` section of an INF use a different value.

The FeatureFlag PCD and the two dynamic forms of PCDs are common to a platform,
with the dynamic form PCD values stored in a "runtime database", read-only
memory location or an HII data store. Therefore, having different values is
prohibited for these access methods.

EDK components may have the scoped sub-element, `<SOURCE_OVERRIDE_PATH>` that
is used to virtually replace files in the component's directory.

The format for items listed in the sub-elements is the identical format for
content under the section.

Within the context of an EDK II module sub-element, the `<LibraryClasses>`
entries must appear before `<Pcds*>` entries; the `<LibraryClasses>` entries
terminate with the start of either the `<Pcds*>` or `<BuildOptions>`
sub-section header or the end of the scope defined by the right curly "}"
brace. The `<BuildOptions>` sub-element must be the last sub-entry of an EDK II
module's scoped section. Entries for `<LibraryClasses>`, `<Pcds*>` and
`<BuildOptions>` are used to replace the platform or global definition entries
listed elsewhere. `LibraryClass` and PCDs are globally defined in the DSC
file's `[LibraryClasses]` and `[Pcds*]` sections, while global BuildOptions
may be specified in either the DSC file's `[BuildOptions]` section or in the
`$(WORKSPACE)/Conf/tools_def.txt` file.

Components and modules listed here will be processed during the MAKE phase of
the build. Binary EDK II only modules do not need to be listed in this section,
but can be put into the FDF file. If a Binary INF listed in the FDF file has
dynamic or PatchableInModule PCDs, the INF should be listed in the DSC
file.Build tools will use the order of files specified in this section for
performing a build (Library Class Instances will be built prior to the module's
sources,) however the ordering in this file has no effect on the ordering of
modules in a binary image (the FDF file describes that ordering).

#### Prototype

```c
<Components>       ::= "[Components" [<attribs>] "]" <EOL>
                       <ModuleStatements>*
<attribs>          ::= <attrs> ["," <TS> "Components" <attrs>]*
<attrs>            ::= "." <arch>
<ModuleStatements> ::= {<MacroDefinition>}
                       {<IncludeStatement>} {<TS> <InfFiles>}
<InfFiles>         ::= <InfFilename> [<MTS> <Options>] <EOL>
<Options>          ::= {<Exec>} {<Edk2Struct>} {<EdkStruct>}
<InfFilename>      ::= <PATH> <Word> ".inf"
<Exec>             ::= "EXEC" <Eq> <ExecFilename>
<ExecFilename>     ::= <PATH> <Word> ["." <ExecExtension>]
<ExecExtension>    ::= <Word> # An OS recognisable extension that will #
                       automatically be run.
<EdkStruct>        ::= "{" <EOL>
                       <TS> "<SOURCE_OVERRIDE_PATH>" <EOL>
                       <TS> <PATH>
                       <TS> "}" <EOL>
<Edk2Struct>       ::= "{" <EOL>
                       [<TS> <DefSec>]
                       [<TS> <LibraryClasses>]
                       [<TS> <PcdsFeatureFlag>]
                       [<TS> <PcdsFixed>]
                       [<TS> <PcdsPatchable>]
                       [<TS> <BuildOptions>] "}"
<DefSec>           ::= "<Defines>" <EOL>
                       <TS> "FILE_GUID" <EQ> <RegistryFormatGuid> <EOL>
<LibraryClasses>   ::= "<LibraryClasses>" <EOL> <LcEntries>*
<LcEntries>        ::= {<MacroDefinition>} {<IncludeStatement>} {<TS>
                       <LibraryInstances>}
<LibraryInstances> ::= {<ClassInstanceMap>} {<NullLibInstances>}
<ClassInstanceMap> ::= <ClassName> <FS> <InfFilename> <EOL>
<ClassName>        ::= (A-Z)(a-zA-Z0-9)*
<NullLibInstances> ::= "NULL" <FS> <InfFilename> <EOL>
<PcdsFeatureFlag>  ::= "<PcdsFeatureFlag>" <EOL> 
                        <PcdsFFEntries>*
<PcdsFFEntries>    ::= {<MacroDefinition>} {<IncludeStatement>} {<TS>
                       <PcdFeatureEntry>}
<PcdFeatureEntry>  ::= <PcdName> <FS> <PcdFeatureValue> <EOL>
<PcdFeatureValue>  ::= {<BoolType>} {<MACROVAL>} {<Expression>}
<PcdsFixed>        ::= "<PcdsFixedAtBuild>" <EOL>
                        <PcdEntries>*
<PcdEntries>       ::= {<MacroDefinition>} {<IncludeStatement>}
                       {<TS> <PcdEntry>}
<PcdsPatchable>    ::= "<PcdsPatchableInModule>" <EOL>
                       <PcdEntries>*
<PcdEntry>         ::= <PcdName> [<FS> <PcdValue>] <EOL>
<PcdValue>         ::= if (pcddatumtype == "BOOLEAN"):
                         {<BoolType>} {<Expression>}
                       elif (pcddatumtype == "UINT8"):
                         {<NumValUint8>} {<Expression>}
                       elif (pcddatumtype == "UINT16"): 
                         {<NumValUint16>} {<Expression>}
                       elif (pcddatumtype == "UINT32"):
                         {<NumValUint32>} {<Expression>}
                       elif (pcddatumtype == "UINT64"):
                         {<NumValUint64>} {<Expression>}
                       else:
                         <StringValue> [<MaxSize>]
<MaxSize>          ::= <FS> "VOID*" [<FS> <SizeValue>]
<SizeValue>        ::= {<Number>} {<Expression>}
<StringValue>      ::= {<StringVal>} {<MACROVAL>} {<Expression>}
<BuildOptions>     ::= "<BuildOptions>" <EOL>
                       [<DefineStatements>]*
                       [<TS> <ToolFlags>]+
                       [<TS> <ToolPath>]*
                       [<TS> <ToolCmd>]*
                       [<TS> <Other>]*
<ToolFlags>        ::= [<Family> ":"] <FlagSpec> <Equal> <Flags> <EOL>
<ToolSpec>         ::= <Target> "_" <TagName> "_" <Tarch> "_" <ToolCode>
<FlagSpec>         ::= <ToolSpec> "_FLAGS"
<ToolPath>         ::= [<Family> ":"] <PathSpec> <Equal> <PATH> <EOL>
<PathSpec>         ::= <ToolSpec> "_DPATH"
<ToolCmd>          ::= [<Family> ":"] <CmdSpec> <ReplaceEq> <PathCmd> <EOL>
<CmdSpec>          ::= <ToolSpec> "_PATH"
<PathCmd>          ::= <PATH> <Word> ["." <Extension>]
<Extension>        ::= (a-zA-Z)(a-zA-Z0-9_-)*
<Other>            ::= [<Family> ":"] <OtherSpec>
<OtherSpec>        ::= <ToolSpec> "_" <Attribute> <Equal> <String>
<Equal>            ::= {<AppendEq>} {<ReplaceEq>}
<AppendEq>         ::= <Eq>
<ReplaceEq>        ::= <TS> "==" <TS>
<Tarch>            ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {*} {<OA>}
<OA>               ::= (A-Z) (A-Z0-9)*
<Family>           ::= _Family_
<Attribute>        ::= _Attribute_
<Target>           ::= _Target_
<TagName>          ::= _TagName_
<ToolCode>         ::= _ToolCode_
<Flags>            ::= _FlagString_
```

#### Parameters

**_Target_**

Must match a target identifier in the EDK II `tools_def.txt` file - the first
field, where fields are separated by the underscore character. The "*"
character is a valid wildcard.

**_TagName_**

Must match a tag name field in the EDK II `tools_def.txt` file - second field.
The "*" character is a valid wild-card.

**_TargetArch_**

Must match the architecture field in the EDK II `tools_def.txt` file - third
field. The "*" character is a valid wild-card.

**_ToolCode_**

Must match a tool code field in the EDK II `tools_def.txt` file - fourth field.
Use of a wild-card character is not permitted.

**_AttributeName_**

Must match a tool attribute field in the EDK II `tools_def.txt` file - fifth
field. Use of a wild-card character is not permitted. The attributes, `_Flag`,
`_PATH` and `_DPATH` are defined elsewhere and cannot be used with the
`<OtherSpec>` definition.

**_FlagString_**

Must be a valid string for the tool specified. The string will be appended to
the end of the tool's flags (from the `tools_def.txt`). Both Microsoft and GCC
evaluate options from left to right on the command line. This allows disabling
some flags that may have been specified in the `tools_def.txt` by providing an
alternate flag, i.e., if the `tools_def.txt` `CC_FLAGS` defines /O2 and an /O1
options is specified for this module, the module will compile with /O1 (size)
not with `/O2` (speed). Use of the quote characters around options is required
when specifying string values with spaces, path names with spaces or values
containing the hash "#" character not within a string. Note that a macro
named `MDEPKG_NDEBUG` is reserved for size reduction purposes. The user must
not use this keyword to define new macro.

**_Pcd Values_**

PCD elements follow the exact format defined for `<PcdEntry>` elements in the
PCD Sections. Since the Dynamic and DynamicEx access method PCDs are common
values to all modules in the platform, the values cannot be overridden for
individual modules.

**_ClassName_**

A Library Class Keyword defined in DEC files. The Keyword must also be present
in the defines section `LIBRARY_CLASS` entry of the INF file

#### Example Using EDK components in an EDK II DSC build

```
[Components]
DEFINE EDK=$(EDK_SOURCE)/Edk
DEFINE MDE=MdePkg/Library
DEFINE STATUS_CODE=$(MDE)/PeiDxeDebugLibReportStatusCode

$(EDK)/Foundation/Core/Pei/PeiMain.inf
DEFINE NT32 = $(EDK)/Sample/Platform
$(NT32)/Generic/MonoStatusCode/Pei/Nt32/MonoStatusCode.inf
$(NT32)/Nt32/Pei/BootMode/BootMode.inf
$(NT32)/Nt32/Pei/FlashMap/FlashMap.inf
MdeModulePkg/Core/Dxe/DxeMain.inf
...
MdeModulePkg/Universal/Debugger/Debugport/Dxe/DebugPort.inf
MdeModulePkg/Cpu/DebugSupport/Dxe/DebugSupport.inf
...

DEFINE MDEMODUNI = MdeModulePkg/Universal
$(MDEMODUNI)/DataHub/DataHubStdErr/Dxe/DataHubStdErr.inf
MdeModulePkg/Universal/Disk/DiskIo/Dxe/DiskIo.inf {
  <LibraryClasses>
    DebugLib|$(STATUS_CODE)/PeiDxeDebugLibReportStatusCode.inf
    BaseMemoryLib|$(MDE)/DxeMemoryLib/DxeMemoryLib.inf
    MemoryAllocationLib|$(MDE)/DxeMemoryAllocationLib/DxeMemoryAllocationLib.inf
  <PcdsFeatureFlag>
    PcdDriverDiagnosticsDisable|gEfiMdePkgTokenSpaceGuid|FALSE
}
MdeModulePkg/Universal/Ebc/Dxe/Ebc.inf
$(MDEMODUNI)/GenericMemoryTest/Dxe/NullMemoryTest.inf
$(MDEMODUNI)/StatusCode/Pei/PeiStatusCode.inf {
  <BuildOptions>
    MSFT:RELEASE_MYTOOLS_IA32_DLINK_FLAGS = Kernel32.lib MSVCRTD.lib Gdi32.lib User32.lib Winmm.lib
    DEBUG_MYTOOLS_IA32_DLINK_FLAGS = Kernel32.lib MSVCRTD.lib Gdi32.lib User32.lib Winmm.lib
    DEBUG_WINDDK3790x1830_IA32_DLINK_FLAGS = Kernel32.lib MSVCRTD.lib Gdi32.lib User32.lib Winmm.lib
    RELEASE_WINDDK3790x1830_IA32_DLINK_FLAGS = Kernel32.lib MSVCRTD.lib Gdi32.lib User32.lib Winmm.lib
    DEBUG_VS2003_IA32_DLINK_FLAGS = Kernel32.lib MSVCRTD.lib Gdi32.lib User32.lib Winmm.lib
    RELEASE_VS2003_IA32_DLINK_FLAGS = Kernel32.lib MSVCRTD.lib Gdi32.lib User32.lib Winmm.lib
}
MdeModulePkg/Logo/Logo.inf
...
```
