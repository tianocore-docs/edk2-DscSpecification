<!--- @file
  3.10 PCD Sections

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

## 3.10 PCD Sections

The PCD sections are optional.

PCD Values listed in the DSC file must be absolute values, macro names or
expressions which may include other PCD names and/or macro names that have been
previously defined.While each PCD type has its own section definition, it is
possible for PCDs accessed using either the `FixedAtBuild` or
`PatchableInModule` methods to have different values for different modules.

It is not permitted to list a PCD in different access method sections. A PCD
can only be listed under one access method. All PCDs listed in these sections
must be declared in a DEC file if the PCD is used in any modules listed in the
DSC or FDF file.

If a PCD is listed in multiple PCD access method sections in the DEC file, the
platform integrator can decide which access method to use. If not listed in the
DSC file, the tools are allowed to select a type, with the `FixedAtBuild`
access method being preferred.

It is not permissible to mix Dynamic*Default with Dynamic*Vpd or Dynamic*Hii,
nor is it permissible to mix Dynamic*Vpd with Dynamic*Hii - only one storage
method for a PCD is permitted.

**********
**Note:** The format for listing PCDs under Default, Vpd and HII sections
differ significantly.
**********

Only FeatureFlag PCDs can be listed in `[PcdsFeatureFlag]` sections.

PCD Values specified in the DSC file override any values specified in INF files
or DEC files.

If no value is provided in the DSC file, then the value will come from INF
files, provided all INF files define the same preferred value.

If the INF values differ (or are not listed), and the method for the PCD in all
of the INF files is `FixedAtBuild`, then each module will use the value
specified in the INF file or the default value listed in the DEC file (for
modules that do not list a preferred value in the INF). Likewise, if the method
is `PatchableInModule` in all INF files, then each module will also use the
value specified in the INF file or the default value listed in the DEC file
(for modules that do not list a preferred value). This same rule applies to
`FeatureFlag` PCDs.

If the Source INF values differ (or are not listed) and the access methods are
different, then the build must break. All source modules in a platform must use
the same PCD same access method.

Binary modules included in a platform build are permitted to use the
PatchableInModule or DynamicEx access methods (the Binary module must specify
which of these two methods were used to create the binary module) regardless of
the method used for a given PCD in modules built from source. The build supports
binary modules that use the same or different PCD access method than the source
modules or other binary modules. The build parser must break with an error if a
PCD is listed as FixedAtBuild or Dynamic (not DynamicEx) in the Binary INF.

If no value is entered in the DSC file, and no INF files provide a preferred
value, then the DEC file's default value must be used.

It is possible to have different default values based on architecture, and it
is permissible to list multiple architectures in a single method section as in:

`[PcdsFixedAtBuild.IA32, PcdsFixedAtBuild.X64]`

It is permissible to list a PCD in a common architecture section and also list
it in an architecturally modified section. In this case, the value in the
architectural section overrides the value specified in the common section.

The PCD values must match the datum type declared for a given PCD in the DEC
file.

PCDs with a data type of `VOID`* can optionally provide the maximum size of the
value. If not provided, the maximum length will be calculated as the largest of
the size of the data in the DSC file, the size of the data in the INF file or
the size of the data in the DEC file that declares the PCD.

PCDs with a C strucutre type is also a VOID* PCD. Its value can be specified like
normal VOID* PCD, and also be specified by its structure field.

Refer to the _EDK II Build Specification_ for the description of the PCD
processing rules.

### 3.10.1 [PcdsFeatureFlag] Sections

These are optional sections for EDK II DSC Files.

#### Summary

Defines the `[PcdsFeatureFlag]` section tag that may be required for platform
DSC files that will use EDK II module INF files.

The `!include` statement is permitted in the `[PcdsFeatureFlag]` sections,
although not recommended.

The `PcdsFeatureFlag` entry is a formatted string with two fields, separated by
the pipe character, "|". PCDs listed in this section must not be listed in
`PcdsPatchableInModule`, `PcdsDynamic`* or `PcdsDynamicEx`* sections of the DSC
file.

#### Prototype

```c
<Pcds>         ::= "[PcdsFeatureFlag" [<attribs>] "]" <EOL> <FFStatments>*
<FFStatements> ::= {<MacroDefinition>} {<IncludeStatement>} {<PcdEntry>}
<attribs>      ::= <attrs> ["," <TS> "PcdsFeatureFlag" <attrs>]*
<attrs>        ::= "." <arch> ["." <SkuIds>]
<SkuIdS>       ::= <Keyword> [<FS> <Keyword>]*
<Keyword>      ::= <UiName>
<UiName>       ::= <Word>
<PcdEntry>     ::= <TS> <PcdName> <FS> <PcdValue> <EOL>
<PcdValue>     ::= {<BoolType>} {<MACROVAL>} {<Expression>}
```

#### Parameters

**_Expression_**

Refer to the EDK II Expression Syntax Specification for additional information.

**_SkuIds_**

SkuIds in the DSC file can be used in two different ways. They can be used to
as conditional modifiers to exclude some content from a build, or they can be
used to identify and group content during a build. If no `SkuId` option (`-x`)
is present on the command-line, or defined in the `[Defines]` section, then the
build system must group content by `SkuId`. If a `SkuId` option is present on
the command-line, or one or more `SkuId`s are listed in the `[Defines]`
section, then the `SkuId` values are used to include specific content, while
excluding content for sections where the `SkuId` is not present in the list of
SkuIds from either the command-line or from the `SKUID_IDENTIFIER` element in
the `[Defines]` section. Use of the `SkuId` modifier for the `[PcdsFeatureFlag]`
section tag can only be used as a conditional modifier.

#### Example

```ini
[PcdsFeatureFlag]
  gEfiMdePkgTokenSpaceGuid.PcdComponentNameDisable|FALSE
  gEfiMdeModulePkgTokenSpaceGuid.PcdDxePcdDatabaseTraverseEnabled|TRUE

[PcdsFeatureFlag.IA32]
  gEfiMdeModulePkgTokenSpaceGuid.PcdDxeIplSwitchToLongMode|FALSE

[PcdsFeatureFlag.X64]
  gEfiMdeModulePkgTokenSpaceGuid.PcdDxeIplSwitchToLongMode|TRUE

# SkuId 1 platform supports serial output
[PcdsFeatureFlag.common.P440FX_WithSerialConnectors]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeUseSerial|TRUE

# SkuId 2 platform has no serial output capability
[PcdsFeatureFlag.common.440FX_NoSerialConnectors]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeUseSerial|FALSE
```

**********
**Note:** If the DSC file contains `[PcdsFeatureFlag]` sections with
`SkuId` _modifiers in the section tags and neither command-line or
`SKUID_IDENTIFER` element exists in the `[Defines]` section, the build
must break, stating that this platform requires separate builds for individual
`SkuId`s.
**********

### 3.10.2 [PcdsFixedAtBuild] Section

These are optional sections for EDK II DSC Files.

#### Summary

This section defines the `[PcdsFixedAtBuild]` section tag that may be required
for EDK II DSC files that will use EDK II module INF files. The values listed
below must match the datum type specified in the DEC file that declares this
PCD. If a PCD's datum type is `VOID`* the `MaximumDatumSize` field is required.

The `!include` statement is permitted in `[PcdsFixedAtBuild]` sections,
although not recommended.

The PcdsFixedAtBuild entry is a formatted string consisting of two to four
fields that are separated by the pipe character, "|".

#### Prototype

```c
<Pcds>          ::= "[PcdsFixedAtBuild" [<attribs>] "]" <EOL>
                    <FabStatements>*
<attribs>       ::= <attrs> ["," <TS> "PcdsFixedAtBuild" <attrs>]*
<attrs>         ::= "." <arch> ["." <SkuIds>]
<SkuIdS>        ::= <Keyword> [<FS> <Keyword>]*
<Keyword>       ::= <UiName>
<UiName>        ::= <Word>
<FabStatements> ::= {<MacroDefinition>} {<IncludeStatement>} {<PcdEntry>} {<PcdFieldEntry>}
<PcdEntry>      ::= <TS> <PcdName> [<FS> <PcdValue>] <EOL>
<PcdValue>      ::= if (pcddatumtype == "BOOLEAN"):
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
<MaxSize>       ::= <FS> "VOID*" <FS> {<IntNum>} {<UINT16>}
<StringValue>   ::= {<StringVal>} {<MACROVAL>} {<Expression>}
```

#### Parameters

**_Expression_**

Refer to the EDK II Expression Syntax Specification for additional information.

**_SkuIds_**

`SkuId`s in the DSC file can be used in two different ways. They can be used to
as conditional modifiers to exclude some content from a build, or they can be
used to identify and group content during a build. If no `SkuId` option (`-x`)
is present on the command-line, or defined in the `[Defines]` section, then the
build system must group content by `SkuId`. If a `SkuId` option is present on
the command-line, or one or more `SkuId`s are listed in the `[Defines]`
section, then the `SkuId` values are used to include specific content, while
excluding content for sections where the `SkuId` is not present in the list of
`SkuId`s from either the command-line or from the `SKUID_IDENTIFIER` element in
the `[Defines]` section. Use of the `SkuId` modifier for the
`[PcdsFixedAtBuild]` section tag can only be used as a conditional modifier.

**_PcdValues_**

PCD values are optional for `[PcdsFixedAtBuild]` sections. If the value is not
listed, the tools must obtain the values from either the INF file or, if a
value is not listed in the INF file, then the default value from the DEC file
must be used.

#### Example

```ini
[PcdsFixedAtBuild.IA32] gEfiMdePkgTokenSpaceGuid.PcdMaximumUnicodeStringLength
  gEfiEdkNt32PkgTokenSpaceGuid.PcdWinNtMemorySizeForSecMain|L"64!64"|VOID*|0x10

[PcdsFixedAtBuild.ARM]
  gEmbeddedTokenSpaceGuid.PcdPrePiCpuMemorySize|32
  gEmbeddedTokenSpaceGuid.PcdPrePiCpuIoSize|0

[PcdsFixedAtBuild.IA32]
  gEmbeddedTokenSpaceGuid.PcdPrePiCpuMemorySize|36
  gEmbeddedTokenSpaceGuid.PcdPrePiCpuIoSize|16

[PcdsFixedAtBuild.common.P440FXS1]
  gEfiMdePkgTokenSpaceGuid.PcdFSBClock|100000000

[PcdsFixedAtBuild.common.P440FXS2]
  gEfiMdePkgTokenSpaceGuid.PcdFSBClock|200000000

[PcdsFixedAtBuild.X64]
  gEmbeddedTokenSpaceGuid.PcdPrePiCpuMemorySize|52
  gEmbeddedTokenSpaceGuid.PcdPrePiCpuIoSize|16
```

**********
**Note:** If the DSC file contains `[PcdsFixedAtBuild]` sections with
`SkuId` modifiers in the section tags and neither command-line or
`SKUID_IDENTIFER` element exists in the `[Defines]` section, the build
must break, stating that this platform requires separate builds for individual
`SkuId`s.
**********

### 3.10.3 [PcdsPatchableInModule] Sections

These are optional sections.

#### Summary

Defines the `[PcdsPatchableInModule]` section tag that may be required for EDK
II DSC files that will use EDK II module INF files. The values listed below
must match the datum type specified in the DEC file that declares this PCD. If
a PCD's datum type is `VOID`* the `MaximumDatumSize` field is required.

The `!include` statement is permitted in `[PcdsPatchableInModule]` sections,
although not recommended.

The `PcdsPatchableInModule` entry is a formatted string with two to four
fields, separated by the pipe "|"character. PCDs listed in this section must
not be listed in `PcdsFixedAtBuild`, `PcdsDynamic` or `PcdsDynamicEx` sections
of the DSC file.

#### Prototype

```c
<Pcds>          ::= "[PcdsPatchableInModule" [<attribs>] "]" <EOL>
                    <PimStatements>*
<attribs>       ::= <attrs> ["," <TS> "PcdsPatchableInModule"
                    <attrs>]*
<attrs>         ::= "." <arch> ["." <SkuIds>]
<SkuIdS>        ::= <Keyword> [<FS> <Keyword>]*
<Keyword>       ::= <UiName>
<UiName>        ::= <Word>
<PimStatements> ::= {<MacroDefinition>} {<IncludeStatement>} {<PcdEntry>} {<PcdFieldEntry>}
<PcdEntry>      ::= <TS> <PcdName> [<FS> <PcdValue>] <EOL>
<PcdValue>      ::= if (pcddatumtype == "BOOLEAN"):
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
<MaxSize>       ::= <FS> "VOID*" <FS> {<IntNum>} {<UINT16>}
<StringValue>   ::= {<StringVal>} {<MACROVAL>} {<Expression>}
```

#### Parameters

**_Expression_**

Refer to the EDK II Expression Syntax Specification for additional information.

**_SkuIds_**

`SkuId`s in the DSC file can be used in two different ways. They can be used to
as conditional modifiers to exclude some content from a build, or they can be
used to identify and group content during a build. If no `SkuId` option (`-x`)
is present on the command-line, or defined in the `[Defines]` section, then the
build system must group content by `SkuId`. If a `SkuId` option is present on
the command-line, or one or more `SkuId`s are listed in the `[Defines]`
section, then the `SkuId` values are used to include specific content, while
excluding content for sections where the `SkuId` is not present in the list of
`SkuId`s from either the command-line or from the `SKUID_IDENTIFIER` element in
the `[Defines]` section. Use of the `SkuId` modifier for the
`[PcdsPatchableInModule]` section tag can only be used as a conditional
modifier.

**_PcdValues_**

PCD values are optional for `[PcdsPatchableInModule]` sections. If the value is
not listed, the tools must obtain the values from either the INF file or, if a
value is not listed in the INF file, then the default value from the DEC file
must be used.

#### Example

```ini
[PcdsPatchableInModule]
  gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000000

[PcdsPatchableInModule.IA32]
  gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask|0x0f
  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x1f

[PcdsPatchableInModule.common.P440FXS1]
  gEfiUnixPkgTokenSpaceGuid.PcdUnixFirmwareFdSize|0x002a0000

[PcdsPatchableInModule.common.P440FXS2]
  gEfiUnixPkgTokenSpaceGuid.PcdUnixFirmwareFdSize|0x00400000

[PcdsPatchableInModule.X64]
  gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask|0x0f
  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x1f
```

**********
**Note:** If the DSC file contains `[PcdsPatchableInModule]` sections with
`SkuId` modifiers in the section tags and neither command-line or
`SKUID_IDENTIFER` element exists in the `[Defines]` section, the build
must break, stating that this platform requires separate builds for individual
`SkuId`s.
**********

### 3.10.4 [PcdsDynamic] Sections

These are optional sections.

#### Summary

Defines the `[PcdsDynamic]` section tag that may be required for platform DSC
files that will use EDK II module INF files. The values listed below must match
the datum type specified in the DEC file that declares this PCD. If a PCD's
datum type is `VOID`* the `MaximumDatumSize` field is required. Specifying
different `<DataStore>` values(`Default`, `HII`, or `VPD`) in one section
header is not permitted. It is permissible to specify different Architectures
using an identical `<DataStore>` value. The following line is permissible:

`[PcdsDynamicDefault.X64.Default, PcdsDynamicDefault.IPF.Default]`

The following line is incorrect, and must cause a build break during parsing of
the file:

`[PcdsDynamicDefault.X64.Default, PcdsDynamicVpd.IPF.Default]`

The `!include` statement is permitted in `[PcdsDynamic]` sections, although not
recommended.

The `Offset`, `MaximumDatumSize` and Value fields are optional for the VPD PCD
entries, and a wildcard character may be used for the `Offset`. The wildcard
character can only be used if a `VPD_TOOL_GUID` is specified in the `[Defines]`
section and the tool supports automatic calculation of the `Offset`. For VPD
PCDs of type `VOID`*, if the VPD's value is present, the `MaximumDatumSize`
must be given. For `VOID`* PCDs, the size is the larger of the length of the
specified value, the length of default values in INF files that use PCD and the
length of the default value in the DEC file. If there are no values specified
in any of the files, the build tools must break. If no value is specified in
the DSC file, and multiple INF files exist that define default values, then the
build must break, since only one value can be used.

The `PcdsDynamic` entry is a formatted string consisting of three to six fields
that are separated by the pipe character, "|". PCDs listed in this section must
not be listed in `PcdsFixedAtBuild`, `PcdsPatchableInModule` or `PcdsDynamicEx`
sections of the DSC file.

#### Prototype

```c
<Pcds>           ::= {<PcdsDefault>} {<PcdsVpd>} {<PcdsHii>}
<PcdsDefault>    ::= "[PcdsDynamicDefault" [<PddAttribs>] "]" <EOL>
                     <PddEntries>*
<PddEntries>     ::= {<MacroDefinition>} {<IncludeStatement>}
                     {<TS> <MinEntry>} {<TS> <PcdFieldEntry>}
<PddAttribs>     ::= <attrs> ["," <TS> "PcdsDynamicDefault" <attrs>]*
<PcdsVpd>        ::= "[PcdsDynamicVpd" [<PdvAttribs>] "]" <EOL>
                     <PdvEntries>*
<PdvAttribs>     ::= <attrs> [ "," <TS> "PcdsDynamicVpd" <attrs>]*
<PdvEntries>     ::= {<MacroDefinition>} {<IncludeStatement>} {<TS> <VpdEntry>} {<TS> <PcdFieldEntry>}
<PcdsHii>        ::= "[PcdsDynamicHii" [<PdhAttribs>] "]" <EOL>
                     <PcdHiiEntries>*
<PdhAttribs>     ::= <phattrs> ["," <TS> "PcdsDynamicHii <phattrs>]* <PdvEntries>*
<PcdHiiEntries>  ::= {<MacroDefinition>} {<IncludeStatement>} {<TS> <HiiEntry>} {<TS> <PcdFieldEntry>}
<attrs>          ::= "." <arch> ["." <SkuIds>]
<phattrs>        ::= "." <arch> ["." <SkuIds>]["." <DefaultStore>]
<SkuIdS>         ::= <Keyword> [<FS> <Keyword>]*
<DefaultStore>   ::= <Keyword>
<Keyword>        ::= <UiName>
<UiName>         ::= <Word>
<MinEntry>       ::= <PcdName> [<FS> <PcdValue>] <EOL>
<PcdValue>       ::= if (pcddatumtype == "BOOLEAN"):
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
<MaxSize>        ::= <FS> "VOID*" [<FS> <SizeValue>]
<SizeValue>      ::= {<IntNum>} {<UINT16>}
<StringValue>    ::= {<StringVal>} {<MACROVAL>} {<Expression>}
<VpdEntry>       ::= <PcdName> <FS> <VpdOffset> [<FS> <VpdData>] <EOL>
<VpdOffset>      ::= {<Number>} {"*"}
<VpdData>        ::= if (pcddatumtype == "BOOLEAN"):
                       {<BoolType>} {<Expression>}
                     elif (pcddatumtype == "UINT8"):
                       {<NumValUint8>} {<Expression>}
                     elif (pcddatumtype == "UINT16"):
                       {<NumValUint16>} {<Expression>}
                     elif (pcddatumtype == "UINT32"):
                       {<NumValUint32>} {<Expression>}
                     elif(pcddatumtype == "UINT64"):
                       {<NumValUint64>} {<Expression>}
                     else:
                       <VpdMaxSize>
<VpdMaxSize>     ::= {<IntNum>} {<UINT16>} [<FS> <StringValue>]
<HiiEntry>       ::= <PcdName> <FS> <HiiString> <Field2> <EOL>
<HiiString>      ::= {<CArray>} {<UnicodeString>}
<Field2>         ::= <FS> <VariableGuid> <FS> <VariableOffset> [<ValueField>]
<VariableGuid>   ::= <CName>
<ValueField>     ::= <FS> <DefaultValue> [<FS> <HiiAttrs>]
<VariableOffset> ::= <Number>
<DefaultValue>   ::= if (pcddatumtype == "BOOLEAN"):
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
                       <StringValue>
<HiiAttrs>       ::= <HiiAttr> [<CS> <HiiAttr>]*
<HiiAttr>        ::= {"NV"} {"BS"} {"RT"} {"RO"}
```

#### Parameters

**_Expression_**

Refer to the EDK II Expression Syntax Specification for additional information.

**_VpdOffset_**

A non-negative numeric value that is the number of bytes from the start of the
Vpd Region specified in the FDF file. The star "*" character may be used in
this field in order to permit an external tool to automatically calculate the
offset values for optimizing data size. This character can only be used if a
`VPD_TOOL_GUID` has been specified, and the tool supports automatic calculation
of the offset.

**_VariableOffset_**

A non-negative numeric value that is the number of bytes from the start to the
start of this variable. The offset value must not exceed the maximum value of
0xFFFF (`UINT16`).

**_HiiString_**
The HiiString field in the HII format PCD entry must not be an empty string.

**_SkuIds_**

`SkuId`s in the DSC file can be used in two different ways. They can be used to
as conditional modifiers to exclude some content from a build, or they can be
used to identify and group content during a build. If no `SkuId` option (`-x`)
is present on the command-line, or defined in the `[Defines]` section, then the
build system must group content by `SkuId`. If a `SkuId` option is present on
the command-line, or one or more `SkuId`s are listed in the `[Defines]`
section, then the `SkuId` values are used to include specific content, while
excluding content for sections where the `SkuId` is not present in the list of
`SkuId`s from either the command-line or from the `SKUID_IDENTIFIER` element in
the `[Defines]` section. Use of the `SkuId` modifier for the `[PcdsDynamic*]`
section tag can be used as a conditional modifier or to groups sets of PCDs
according to the `SkuId` identifier.

**_DefaultStore_**

`DefaultStore` in the DSC file is used to specify DynamicHii/DynamicExHii PCD 
value as the default EFI variable for which default store. It is only valid in 
DynamicHii/DynamicExHii section. If it is not specified, DynamicHii/DynamicExHii 
PCD value will be used as the standard default EFI variable. For the different 
combination of SKU and DefaultStore, their inheritance is described as the below.

SKU will inherit its parent SKU setting. DEFAULT SKU is the default parent SKU.
DefaultStore is the subsection of SKU. It will first inherit from the same 
DefaultStore in its parent SKU, then inherit other DefaultStore in the same SKU. 
DefaultStore with the big default store ID will inherit the setting from one with 
the small default store ID. If there are more than one small default store ID, 
it will use the biggest one to be inherit. Here is the example. Four PcdsDynamicHii
sections are defined.

# Four PcdsDynamicHii section
[PcdsDynamicHii.common.Default.Standard]
[PcdsDynamicHii.common.Default.Manufacturing]
[PcdsDynamicHii.common.Sku1.Standard]
[PcdsDynamicHii.common.Sku1.Manufacturing]

# DEFAULT Manufacturing setting based on two sections
[PcdsDynamicHii.common.Default.Standard]
[PcdsDynamicHii.common.Default.Manufacturing]

# SKU1 Standard setting based on two sections
[PcdsDynamicHii.common.Default.Standard]
[PcdsDynamicHii.common.Sku1.Standard]

# SKU1 Manufacturing setting based on four sections
[PcdsDynamicHii.common.Default.Standard]
[PcdsDynamicHii.common.Default.Manufacturing]
[PcdsDynamicHii.common.Sku1.Standard]
[PcdsDynamicHii.common.Sku1.Manufacturing]

**_PcdValues_**

PCD values are optional for `[PcdsDynamicDefault]` sections. The PCD values for
PCDs listed in `[PcdsDynamicVpd]` and `[PcdsDynamicHii]` sections are also
optional. For a PCD with a Datum Type of `VOID`* and an access method
implementation of `PcdsDynamicHii`, a default value should not be listed, as
the value is a pointer.

**_HiiAttr_**

Each HII attribute may only be present once in the list. Refer to _Table 9_ and
the _UEFI Specification_ for a description of these attributes.

#### Examples

```ini
[PcdsDynamicDefault.IA32.Default]
  gEfiEdkNt32PkgTokenSpaceGuid.PcdWinNtMemorySizeForSecMain|"L64!64"|10
  gPcAtChipsetPkgTokenSpaceGuid.Pcd8259LegacyModeEdgeLevel |0x0         # UINT16
  gPcAtChipsetPkgTokenSpaceGuid.PcdIsaAcpiPs2MouseEnable   |0           # BOOLEAN

[PcdsDynamicVpd.common.Sku_Two|SkuSeven]
  gUefiCpuPkgTokenSpaceGuid.PcdCpuLocalApicBaseAddress |0x0001ffff|0x1  # UINT32
  gNoSuchTokenSpaceGuid.PcdPciDevice0Name   |0x2282|64 |"None"          # VOID*
  gNoSuchTokenSpaceGuid.PcdPciDevice50Info  |0x22C2|18 |{0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF,0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF}  # VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionName|0x22D4|100|" "             # VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionPath|0x2338|100|" "             # VOID*
  gNoSuchTokenSpaceGuid.PcdEnableFastBoot   |0x239C|1  |FALSE           # BOOLEAN

[PcdsDynamicHii.IA32, PcdsDynamicHii.X64.Sku1.Standard]
  gEfiMyModulePkgTokenSpaceGuid.PcdChassisIntrution|0x0053 0x0065 0x0074 0x0075 0x0070|gSysConfigGuid|0x83|0x0
  gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdPlatformBootTimeOut|L"Timeout"|gEfiGlobalVariableGuid|0x0|10  # Variable: L"Timeout"
```

### 3.10.5 [PcdsDynamicEx] Sections

These are optional sections.

#### Summary

Defines the `[PcdsDynamicEx]` section tag that may be required for platform DSC
files that will use EDK II module INF files. The values listed below must match
the datum type specified in the DEC file that declares this PCD. If a PCD's
datum type is `VOID`* the `MaximumDatumSize` field is required. Specifying
different `<DataStore>` values(`Default`, `HII`, or `VPD`) in one section header
is not permitted. It is permissible to specify different Architectures using an
identical `<DataStore>` value. The following line is permissible:

`[PcdsDynamicExDefault.X64.Default, PcdsDynamicExDefault.IPF.Default]`

The following line is incorrect, and must cause a build break during parsing of
the file:

`[PcdsDynamicExDefault.X64.DEFAULT, PcdsDynamicExVpd.IPF.DEFAULT]`

The `!include` statement is permitted in `[PcdsDynamicEx]` sections, although
not recommended.

The `PcdsDynamicEx` entry is a formatted string with three to six fields,
separated by the pipe "|" character. PCDs listed in this section must not be
listed in the `PcdsFixedAtbuild`, `PcdsPatchableInModule` or `PcdsDynamic`
sections of the DSC file.

#### Prototype

```c
<Pcds>           ::= {<PcdsExDefault>} {<PcdsExVpd>} {<PcdsExHii>}
<PcdsExDefault>  ::= "[PcdsDynamicExDefault" [<PddAttribs>] "]" <EOL>
                     <PddEntries>*
<PddEntries>     ::= {<MacroDefinition>} {<IncludeStatement>} {<TS> <MinEntry>} {<TS> <PcdFieldEntry>}
<PddAttribs>     ::= <attrs> ["," <TS> "PcdsDynamicExDefault" <attrs>]*
<PcdsExVpd>      ::= "[PcdsDynamicExVpd" [<PdvAttribs>] "]" <EOL>
                     <PdvEntries>*
<PdvAttribs>     ::= <attrs> [ "," <TS> "PcdsDynamicExVpd" <attrs>]*
<PdvEntries>     ::= {<MacroDefinition>} {<IncludeStatement>}
                     {<TS> <VpdEntry>} {<TS> <PcdFieldEntry>}
<PcdsExHii>      ::= "[PcdsDynamicExHii" [<PdhAttribs>] "]" <EOL>
                     <PcdHiiEntries>*
<PdhAttribs>     ::= <phattrs> ["," <TS> "PcdsDynamicExHii <phattrs>]*
                     <PdvEntries>*
<PcdHiiEntries>  ::= {<MacroDefinition>} {<IncludeStatement>} {<TS> <HiiEntry>} {<TS> <PcdFieldEntry>}
<attrs>          ::= "." <arch> ["." <SkuIds>]
<phattrs>        ::= "." <arch> ["." <SkuIds>]["." <DefaultStore>]
<SkuIdS>         ::= <Keyword> [<FS> <Keyword>]*
<DefaultStore>   ::= <Keyword>
<Keyword>        ::= <UiName>
<UiName>         ::= <Word>
<MinEntry>       ::= <PcdName> [<FS> <PcdValue>] <EOL>
<PcdValue>       ::= if (pcddatumtype == "BOOLEAN"):
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
<MaxSize>        ::= <FS> "VOID*" [<FS> <SizeValue>]
<SizeValue>      ::= {<Number>} {<Expression>}
<StringValue>    ::= {<StringVal>} {<MACROVAL>} {<Expression>}
<VpdEntry>       ::= <PcdName> <FS> <VpdOffset> [<FS> <VpdData>] <EOL>
<VpdOffset>      ::= {<Number>} {"*"}
<VpdData>        ::= if (pcddatumtype == "BOOLEAN"):
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
                       <VpdMaxSize>
<VpdMaxSize>     ::= {<LongNum>} {<UINT32>} [<FS> <StringValue>]
<HiiEntry>       ::= <PcdName> <FS> <HiiString> <Field2> <EOL>
<HiiString>      ::= {<CArray>} {<UnicodeString>}
<Field2>         ::= <FS> <VariableGuid> <FS> <VariableOffset> [<ValueField>]
<VariableGuid>   ::= <CName>
<ValueField>     ::= <FS> <DefaultValue> [<FS> <HiiAttrs>]
<VariableOffset> ::= <Number>
<DefaultValue>   ::= if (pcddatumtype == "BOOLEAN"):
                       {<BoolType>} {<Expression>}
                     elif (pcddatumtype == "UINT8"):
                       {<NumValUint8>} {<Expression>}
                     elif (pcddatumtype == "UINT16"):
                       {<NumValUint16>} {<Expression>}
                     elif (pcddatumtype == "UINT32"):
                       {<NumValUint32>} {<Expression>}
                     elif(pcddatumtype == "UINT64"):
                       {<NumValUint64>} {<Expression>}
                     else:
                       <StringValue>
<HiiAttrs>       ::= <HiiAttr> [<CS> <HiiAttr>]*
<HiiAttr>        ::= {"NV"} {"BS"} {"RT"} {"RO"}
```

#### Parameters

**_Expression_**

Refer to the EDK II Expression Syntax Specification for additional information.

**_VpdOffset_**

A non-negative numeric value that is the number of bytes from the start of the
Vpd Region specified in the FDF file. The star "*" character may be used in
this field in order to permit an external tool to automatically calculate the
offset values for optimizing data size. This character can only be used if a
`VPD_TOOL_GUID` has been specified, and the tool supports automatic calculation
of the offset.

**_VariableOffset_**

A non-negative numeric value that is the number of bytes from the start to the
start of this variable. The offset value must not exceed the maximum value of
0xFFFF (`UINT16`).

**_HiiString_**
The HiiString field in the HII format PCD entry must not be an empty string.

**_SkuIds_**

`SkuId`s in the DSC file can be used in two different ways. They can be used to
as conditional modifiers to exclude some content from a build, or they can be
used to identify and group content during a build. If no `SkuId` option (`-x`)
is present on the command-line, or defined in the `[Defines]` section, then the
build system must group content by `SkuId`. If a `SkuId` option is present on
the command-line, or one or more `SkuId`s are listed in the `[Defines]`
section, then the `SkuId` values are used to include specific content, while
excluding content for sections where the `SkuId` is not present in the list of
`SkuId`s from either the command-line or from the `SKUID_IDENTIFIER` element in
the `[Defines]` section. Use of the `SkuId` modifier for the `[PcdsDynamic*]`
section tag can be used as a conditional modifier or to groups sets of PCDs
according to the `SkuId` identifier.

**_DefaultStore_**

`DefaultStore` in the DSC file is used to specify DynamicHii/DynamicExHii PCD 
value as the default EFI variable for which default store. It is only valid in 
DynamicHii/DynamicExHii section. If it is not specified, DynamicHii/DynamicExHii 
PCD value will be used as the standard default EFI variable. For the different 
combination of SKU and DefaultStore, their inheritance is described as the below.

SKU will inherit its parent SKU setting. DEFAULT SKU is the default parent SKU.
DefaultStore is the subsection of SKU. It will first inherit from the same 
DefaultStore in its parent SKU, then inherit other DefaultStore in the same SKU. 
DefaultStore with the big default store ID will inherit the setting from one with 
the small default store ID. If there are more than one small default store ID, 
it will use the biggest one to be inherit. Here is the example. Four PcdsDynamicExHii
sections are defined.

# Four PcdsDynamicExHii section
[PcdsDynamicExHii.common.Default.Standard]
[PcdsDynamicExHii.common.Default.Manufacturing]
[PcdsDynamicExHii.common.Sku1.Standard]
[PcdsDynamicExHii.common.Sku1.Manufacturing]

# DEFAULT Manufacturing setting based on two sections
[PcdsDynamicExHii.common.Default.Standard]
[PcdsDynamicExHii.common.Default.Manufacturing]

# SKU1 Standard setting based on two sections
[PcdsDynamicExHii.common.Default.Standard]
[PcdsDynamicExHii.common.Sku1.Standard]

# SKU1 Manufacturing setting based on four sections
[PcdsDynamicExHii.common.Default.Standard]
[PcdsDynamicExHii.common.Default.Manufacturing]
[PcdsDynamicExHii.common.Sku1.Standard]
[PcdsDynamicExHii.common.Sku1.Manufacturing]

**_PcdValues_**

PCD values are optional for `[PcdsDynamicExDefault]` sections. The PCD values
for PCDs listed in `[PcdsDynamicExVpd]` and `[PcdsDynamicExHii]` sections are
also optional. For a PCD with a Datum Type of `VOID`* and an access method
implementation of `PcdsDynamicExHii`, a default value should not be listed, as
the value is a pointer.

**_HiiAttr_**

Each HII attribute may only be present once in the list.Refer to _Table 9_ and
the _UEFI Specification_ for a description of these attributes.

#### Examples

```ini
[PcdsDynamicExDefault.IA32.Default]
  gEfiEdkNt32PkgTokenSpaceGuid.PcdWinNtMemorySizeForSecMain|L"64!64"|VOID*|10
  gPcAtChipsetPkgTokenSpaceGuid.Pcd8259LegacyModeEdgeLevel|0x0
  gPcAtChipsetPkgTokenSpaceGuid.PcdIsaAcpiPs2MouseEnable|0               # BOOLEAN

[PcdsDynamicExVpd.common.Sku_Two|SkuSeven]
  gUefiCpuPkgTokenSpaceGuid.PcdCpuLocalApicBaseAddress|0x0001ffff | 0x1  # UINT32
  gNoSuchTokenSpaceGuid.PcdPciDevice0Name   |0x2282|64 |"None"           # VOID*
  gNoSuchTokenSpaceGuid.PcdPciDevice0Info   |0x22C2|18 |{0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF,0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF}  # VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionName|0x22D4|100|" "              # VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionPath|0x2338|100|" "              # VOID*
  gNoSuchTokenSpaceGuid.PcdEnableFastBoot   |0x239C|FALSE                # BOOLEAN

[PcdsDynamicExHii.IA32, PcdsDynamicExHii.X64.Sku1.Standard]
  gEfiMyModulePkgTokenSpaceGuid.PcdChassisIntrution|0x0053 0x0065 0x0074 0x0075 0x0070|gSysConfigGuid|0x83|0x0
  gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdPlatformBootTimeOut|L"Timeout"|gEfiGlobalVariableGuid|0x0  # Variable: L"Timeout"
```
