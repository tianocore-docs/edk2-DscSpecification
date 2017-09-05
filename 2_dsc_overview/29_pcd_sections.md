<!--- @file
  2.9 PCD Sections

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

## 2.9 PCD Sections

### 2.9.1 [PcdsFeatureFlag] section

The required content for the FeatureFlag PCD is the PCD Token Space Guid C
name, the PCD's C name (these two entries are separated by the period
character), and a Boolean value of either TRUE, FALSE, 1 or 0 The PCD name
and value entries are separated by the pipe "|" character.

FeatureFlag PCDs can be used in conditional directive statements within the DSC
and FDF files. These PCDs may also be used to select execution paths in some
code routines. The build tools will generate a const variable for each
PcdsFeatureFlag used by a module.

The section modifier, `SkuIdentifier`, can be used by the build tools to create
images for one specific SKU. Unlike the `PcdsDynamic` and `PcdsDynamicEx`
entries, no access methods are allowed for having different values during
runtime for different SKUs. Do not use the `SkuIdentifier` when building all
SKUs.

The following are typical entries, with a supported module type qualifier
omitted in these examples:

```ini
[PcdsFeatureFlag]
[PcdsFeatureFlag.common]
[PcdsFeatureFlag.IA32]
[PcdsFeatureFlag.X64]
[PcdsFeatureFlag.IPF]
[PcdsFeatureFlag.EBC]
```

Format of an entry in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value`

#### Example

```ini
[PcdsFeatureFlag.common]
  gEfiMdeModulePkgTokenSpaceGuid.PcdDxePcdDatabaseTraverseEnabled|1
```

### 2.9.2 [PcdsFixedAtBuild] and [PcdsPatchableInModule] sections

The section modifier, `SkuIdentifier`, can be used by the build tools to create
images for one specific SKU. Unlike the `PcdsDynamic` and `PcdsDynamicEx`
entries, no access methods are allowed for having different values during
runtime for different SKUs. Do not use the `SkuIdentifier` when building all
SKUs.

#### 2.9.2.1 PcdsFixedAtBuild

The `FixedAtBuild` PCD access method cannot be used in a Binary Module.

The required content for the `FixedAtBuild` PCD are the PCD Token Space Guid
C name, the PCD's C name (these two entries are separated by the period
character) and the Value (any one of Boolean, numeric or pointer types).
The PCD name and value entries are separated by the pipe "|" character.

If the Datum Type for the PCD is `VOID`*, then a fourth field that specifies
the maximum datum size is required. This is the maximum size allocated by the
Platform Integrator. Module developers won't know how much size will be
allocated, and just use it. The platform integrator must figure out what the
maximum length will be, based on the usage from the modules included.

FixedAtBuild PCDs can be used in conditional directive statements in the DSC
and FDF files. The build tools will generate a `const` variable for each
`FixedAtBuild` PCD used by a module.

The following are typical examples of the `[PcdsFixedAtBuild]` section tag (the
`$(arch)` and `$(SkuIdentifier)` would be replaced with real values).

```ini
[PcdsFixedAtBuild]
[PcdsFixedAtBuild.common]
[PcdsFixedAtBuild.IA32]
[PcdsFixedAtBuild.X64]
[PcdsFixedAtBuild.IPF]
[PcdsFixedAtBuild.EBC]
[PcdsFixedAtBuild.$(arch).$(SkuIdentifier)]
```

Format of a point (VOID*) entry in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value[|DatumType|MaximumDatumSize]]`

Format for Boolean and numeric entries in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value`

#### Examples

```
[PcdsFixedAtBuild.common]
  gEfiMdePkgTokenSpaceGuid.PcdFSBClock|200000000
  gEfiMdeModulePkgTokenSpaceGuid.PcdVpdBaseAddress|0x0
  gEfiEdkNt32PkgTokenSpaceGuid.PcdWinNtPhysicalDisk|L"E:RW;245760;512"|VOID*|32
```

#### 2.9.2.2 PcdsPatchableInModule

The `PatchableInModule` PCD access method can be used with modules that are
distributed in binary form. The PCD's value can be patched by tools that know
the offset of the PCD into the binary file.

The required content for the `PatchableInModule` PCD are the PCD Token Space
Guid C name, the PCD's C name (these two entries are separated by the period
character) and Value. The PCD name and value entries are separated by the pipe
"|" character. If the Datum Type for the PCD is `VOID*`, then a fourth field
that specifies the maximum datum size is also required.

PatchableInModule PCDs cannot be used in conditional directive statements.
Build tools will generate a volatile variable for each `PatchableInModule` PCD
that is used by a module.

The following are typical examples of the `[PcdsPatchableInModule]` section tag
(the `$(arch)` and `$(SkuIdentifier)` would be replaced with real values).

```ini
[PcdsPatchableInModule]
[PcdsPatchableInModule.IA32]
[PcdsPatchableInModule.X64]
[PcdsPatchableInModule.IPF]
[PcdsPatchableInModule.EBC]
[PcdsPatchableInModule.$(arch).$(SkuIdentifier)]
```

Format of an entry in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value[|DatumType[|MaximumDatumSize]]`

#### Example

```ini
[PcdsPatchableInModule.common]
  gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000000|UINT32|4
```

### 2.9.3 [PcdsDynamic*] and [PcdsDynamicEx*] sections

PCDs listed in these sections cannot be used in conditional directive
statements.

The Dynamic PCD access method cannot be used for modules that are distributed
in binary form.

For Dynamic PCD settings, the section labels must include one of Default, Vpd
or Hii with optional architecture and an optional `SKUID_IDENTIFIER` name. The
Dynamic entry fields are separated by the pipe "|" character. If the Datum Type
for the PCD is `VOID*`, then a field that specifies the maximum datum size is
also required.

The use of the `SkuIdentifier` in the `PcdsDynamic` and `PcdsDynamicEx`
sections may be needed for creating the PCD database when a single platform
binary image supports multiple SKUs. The SKU selection based on things like a
hardware jumper, or some other method that is outside the scope of this
document.

For using the standard PCD Get/Set PPI or Protocol.

#### 2.9.3.1 PcdsDynamicDefault

The Dynamic Default PCD access method will generate a volatile variable that
can be accessed at runtime using PCD a Get PPI or Protocol.

```ini
[PcdsDynamic.$(arch).DEFAULT]
[PcdsDynamicDefault.$(arch).$(SkuIdentifier)]
[PcdsDynamicHii.$(arch).$(SkuIdentifier)]
[PcdsDynamicVpd.$(arch).$(SkuIdentifier)]
```

The format for a boolean or numeric datum type PCD entry in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value`

The format for a VOID* PCD entry in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value[|DatumType[|MaximumDatumSize]]`

#### Examples

```ini
[PcdsDynamicDefault]
gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableBase|0x0
gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableSize|0x0
```

#### 2.9.3.2 PcdsDynamicHII

The Dynamic Hii PCD access method will generate HII data content that can be
accessed at runtime.

For using the HII for PCD data, the section name is as follows:

`[PcdsDynamicHii.$(arch).DEFAULT]`

Specifying a SKUID_IDENTIFIER name for an Hii Pcd selection is optional, for
example:

`[PcdsDynamicHii.common.Sku1]`

While the format for content of this section is as follows, note that the
backslash character is used here to indicate the continuation of the line:

`PcdTokenSpaceGuidCName.PcdCName|VariableName|VariableGuid|VariableOffset[|HiiDefaultValue[|HiiAttrubte]]`

For VOID* PCDs, the HiiDefaultValue will be a pointer; specifying the optional
HiiDefaultValue has no meaning.

The optional HII Attribute entry is a comma separated list of attributes as
described in the following table.

###### Table 9 HII Attributes

| Keyword | C Flag                                  | Value        |
| ------- | --------------------------------------- | ------------ |
| `NV`    | `EFI_VARIABLE_NON_VOLATILE`             | `0x00000001` |
| `BS`    | `EFI_VARIABLE_BOOTSERVICE_ACCESS`       | `0x00000002` |
| `RT`    | `EFI_VARIABLE_RUNTIME_ACCESS`           | `0x00000004` |
| `RO`    | `VAR_CHECK_VARIABLE_PROPERTY_READ_ONLY` | `BIT0`       |

**Note:** The VariableName field in the HII format PCD entry must not be an empty string.

#### Examples

```ini
[PcdsDynamicHii.common.Sku_Two]
  NoSuchTokenSpaceGuid.PcdPreAllocatedMem| 0x0053 0x0065 0x0074 0x0075 0x0070|gSysconfigGuid|0x000000A9|0x3
[PcdsDynamicHii.common.DEFAULT]
  gEfiMdeModulePkgTokenSpaceGuid.PcdValidRange|L"PcdValidRange"|gEfiGlobalVariableGuid|0x07|0|BS,RT,NV
```

#### 2.9.3.3 PcdsDynamicVpd

The Dynamic Vpd PCD access method will generate macros that allow the data
content (stored in read-only memory) to be accessed at runtime. Note that the
PCD drivers may use a copy of the VPD data to allow runtime changes to these
variables.

For using the VPD for PCD data, the section name is:

`[PcdsDynamicVpd.$(arch).DEFAULT]`

Specifying a `SKUID_IDENTIFIER` for a VPD PCD selection is optional, for
example:

`[PcdsDynamicVpd.common.Vpd.SkuSeven]`

The format for boolean and numeric datum type content of this section is as
follows:

`PcdTokenSpaceGuidCName.PcdCName|VpdOffset [|Value]`

The format for VOID* datum type content in this section is:

`PcdTokenSpaceGuidCName.PcdCName|VpdOffset [|MaximumDatumSize [|Value]]`

#### Examples

```ini
[PcdsDynamicVpd.IA32.DEFAULT, PcdsDynamicVpd.x64.DEFAULT]
  gEfiPhonyTokenSpaceGuidCName.PcdVpdCopyrightLine|0x000000A0
  gNoSuchTokenSpaceGuid.PcdPciDevice0Name    | 0x2282 | 64  | "None" # VOID*
  gNoSuchTokenSpaceGuid.PcdPciDevice50Info   | 0x22C2 | 18  | {0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF}  # |VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionName | 0x22D4 | 100 | " "    # VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionPath | 0x2338 | 100 | " "    # VOID*
  gNoSuchTokenSpaceGuid.PcdEnableFastBoot    | 0x239C | 1   | FALSE  # BOOLEAN
```

#### 2.9.3.4 PcdsDynamicExDefault

The DynamicEx access method of PCD is recommended for modules that are
distributed in binary form.

Entries for `DynamicEx` are identical to the `Dynamic` entries. The `DynamicEx`
entry fields are separated by the pipe "|" character. If the Datum Type for the
PCD is VOID*, then `MaximumDatumSize` field that specifies the maximum datum
size is required.

```ini
[PcdsDynamicExDefault.$(arch).Default]
[PcdsDynamicExDefault.$(arch).$(SkuIdentifier)]
```

The format for a boolean or numeric datum type PCD entry in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value`

The format for a VOID* PCD entry in this section is:

`PcdTokenSpaceGuidCName.PcdCName|Value[|DatumType[|MaximumDatumSize]]`

#### Examples

```ini
[PcdsDynamicExDefault.common.DEFAULT]
gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableBase|0x0
gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableSize|0x0
```

#### 2.9.3.5 PcdsDynamicEx Hii

For using the HII for PCD data, the section name is as follows:

`[PcdsDynamicExHii.$(arch).$(SKUID_IDENTIFIER)]`

Specifying a `SKUID` for an HII PCD selection is optional, for example:

`[PcdsDynamicExHii.common.Sku1]`

While the format for content of this section is as follows, note that the
backslash character is used here to indicate the continuation of the line:

`PcdTokenSpaceGuidCName.PcdCName|VariableName|VariableGuid|VariableOffset[|HiiDefaultValue]`

The optional HII Attribute entry is a comma separated list of attributes as
described in Table 9 HII Attributes.

**Note:** The VariableName field in the HII format PCD entry must not be an empty string.

#### Examples

```ini
[PcdsDynamicExHii.IA32.Sku_Two]
  gNoSuchTokenSpaceGuid.PcdPreAllocatedMem|0x0053 0x0065 0x0074 0x0075 0x0070|gSysconfigGuid|0x000000A9|0x11

[PcdsDynamicExHii.common.DEFAULT]
  gEfiMdeModulePkgTokenSpaceGuid.PcdValidRange|L"PcdValidRange"|gEfiGlobalVariableGuid|0x07|0|BS,RT,NV
```

#### 2.9.3.6 PcdsDynamicExVpd

For using the VPD for PCD data, the section name is:

`[PcdsDynamicExVpd.$(arch).$(SKUID_IDENTIFIER)]`

Specifying a `SKUID` for a VPD PCD selection is optional, for example:

`[PcdsDynamicExVpd.common.SkuTwo]`

The format for boolean and numeric datum type content of this section is as
follows:

Method

`PcdTokenSpaceGuidCName.PcdCName|VpdOffset[|Value]`

The format for VOID* datum type content in this section is:

`PcdTokenSpaceGuidCName.PcdCName|VpdOffset[|MaximumDatumSize[|Value]]`

####  Examples

```ini
[PcdsDynamicExVpd.common.DEFAULT]
  gEfiPhonyTokenSpaceGuidCName.PcdVpdCopyrightLine|0x000000A0
  gNoSuchTokenSpaceGuid.PcdPciDevice0Name   |0x2282|64 | "None"  # VOID*
  gNoSuchTokenSpaceGuid.PcdPciDevice50Info  |0x22C2|18 | {0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF} #|VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionName|0x22D4|100| " "     # VOID*
  gNoSuchTokenSpaceGuid.PcdOemBootOptionPath|0x2338|100| " "     # VOID*
  gNoSuchTokenSpaceGuid.PcdEnableFastBoot   |0x239C|1  | FALSE   # BOOLEAN
```
