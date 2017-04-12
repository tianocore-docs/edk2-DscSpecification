<!--- @file
  2.8 PCD Section Processing

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

## 2.8 PCD Section Processing

This section is for specifying global (or default) PCD values as well as the
access method each PCD will use for modules in the platform.

### 2.8.1 PCD Access Methods

There are five defined PCD access methods. The five access methods are:
`FeatureFlag`, `FixedAtBuild`, `PatchableInModule`, `Dynamic` and `DynamicEx`
PCDs.

#### 2.8.1.1 FeatureFlag and Dynamic PCD Types

The two recommended access methods that are commonly used in modules are
`FeatureFlag` and the generic, `Dynamic method`. The `Dynamic` form is used for
configuration when the PCD value is produced and consumed by drivers during
execution, the value may be user configurable from setup or the value is
produced by the platform in a specified area. It is associated with modules
that are released in source code. The dynamic form is the most flexible method,
as platform integrators may chose a to use a different access method for a
given platform without modifying the module's INF file or the code for the
module.

#### 2.8.1.2 DynamicEx, FixedAtBuild and PatchableInModule PCD Access Methods

Similar in function, the `DynamicEx` access method can be used with modules
that are released as binary. The `FixedAtBuild` and `PatchableInModule` PCDs
are static and only the `PatchableInModule` PCD can have the value changed in a
binary prior to including the module in a firmware image.

The platform integrator must check DEC file that declares the PCD to determine
the PCD's valid access methods. If a module defines a PCD as dynamic (not
`DynamicEx`), and the DEC file lists the PCD under all access methods, the
platform integrator can specify any access method (basically making a dynamic
PCD into a static, fixed PCD) overriding the INF module definition. However, if
a module declares a PCD is coded to use a specific access method , then the
platform integrator must select that PCD access method. For example, if a PCD
is listed as FixedAtBuild in a module file, then the platform integrator must
either list the PCD in a `[PcdsFixedAtBuild]` section of the DSC or let the
tools use the default value and automatically set the PCD access method to
FixedAtBuild. It is not necessary to modify an INF file in order to use this
feature - the tools will automatically "correct" the PCD access method for
platforms that use an alternate access method for Dynamic PCDs.

The content in these sections is used for generating the `AutoGen.c` and
`AutoGen.h` files for each of the EDK II modules that are coded for the PCD.

```ini
[Pcds(PcdType)]
[Pcds(PcdType).common]
[Pcds(PcdType).IA32]
[Pcds(PcdType).X64]
[Pcds(PcdType).IPF]
[Pcds(PcdType).EBC]
```

### 2.8.2 PCD Access Method Categories

Of the five access methods of PCDs that have been defined, they fall into one
of three categories:

* `FeatureFlag` - always has a Boolean value.

* `FixedAtBuild` and `PatchableInModule`, will have a value of one of three
  datum types, Boolean, numeric or pointer. The `FixedAtBuild` PCD will be a
  defined as a const, while the `PatchableInModule` will be defined as volatile.

* `Dynamic` and `DynamicEx`, will have a value of one of the three data types,
  Boolean, numeric or pointer.

**********
**Note:** For the dynamic types of PCDs, using an `$(Arch)` extension other
than `common` in the section header is not valid.
**********
**Warning:** A PCD can only use one type for all modules. It is not permissible
to list a PCD in a PcdsFixedAtBuild section and also list it in a
PcdsPatchableInModule section.
**********

Datum Types for PCD values are either Boolean (`BOOLEAN` - 1 byte), numeric
(`UINT8` - 1 byte, `UINT16` - 2 bytes, `UINT32` - 4 bytes or `UINT64` - 8
bytes) or variable length (`VOID`*, which indicates that the value is usually
accessed via a pointer). To put a limit on the number of bytes for a variable
length value (when the PCD Datum Type is `VOID`*) the PCD entry must include
the `MaximumDatumSize` parameter. The `MaximumDatumSize` parameter is optional
for all other PCD data types.

**********
**Warning:** A `FixedAtBuild` or `PatchableInModule` PCD may have a
different datum type based on the architecture. For example, a PCD that is used
for address manipulation may have a datum type of `UINT32` for IA32 and
`UINT64` for X64 and IPF architectures. This will be declared in the EDK II
Package Declaration (DEC) File.
**********

### 2.8.3 PCD Section Usage

PCD sections are optional unless the EDK II modules specified in the
`[Components]` section use PCDs.

The PCD sections are used to define the access method for a PCD. Since each
module is built once for a given architecture, the PCD can be listed under
different PCD access methods provided they are listed under different
architectures.

#### 2.8.3.1 Access Methods

However, once a PCD access method is selected for a given architecture, the PCD
can only use that access method.

#### Example

A PCD that will use the `FixedAtBuild` access method for IA32 cannot use the
`PatchableInModule` access method for individual modules built for the IA32
architecture.

#### 2.8.3.2 Different Access Methods

It is permissible to have a PCD use different access methods for different
architectures.

#### Example

A PCD that will use the FixedAtBuild access method for IA32 can use the
Patchable in Module access method for X64.

#### 2.8.3.3 Item Access Methods

Multiple item access methods, `PcdsFeatureFlag`, `PcdsFixedAtBuild`,
`PcdsPatchableInModule`, `PcdsDynamic` and `PcdsDynamicEx` are not allowed to
be specified within a single [] section tag.

#### Incorrect example

```ini
[PcdsFixedAtBuild.IA32, PcdsPatchableInModule.IA32, PcdsDynamicDefault.IA32]
```

#### Correct example

```ini
[PcdsFixedAtBuild.IA32]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeMemorySize|1
  gEfiMdeModulePkgTokenSpaceGuid.PcdResetOnMemoryTypeInformationChange|FAL  SE

[PcdsPatchableInModule.IA32]
  gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000000

[PcdsDynamicDefault.IA32]
  gEfiMdeModulePkgTokenSpaceGuid.PcdEmuVariableNvStoreReserved|0
  gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableBase64|0
```

#### 2.8.3.4 Mixing PCD Dynamic item storage methods

It is not permissible to mix different PCD Dynamic item storage methods within
a single section, as the format for the PCD entries in PcdsDynamicDefault,
PcdsDynamicVpd, PcdsDynamicHii, and PcdsDynamicExDefault, PcdsDynamicExVpd and
PcdsDynamicExHii sections are different.

#### Incorrect Example

```ini
[PcdsDynamicExDefault.IA32, PcdsDynamicExVpd.IA32]
  gEfiMdeModulePkgTokenSpaceGuid.PcdEmuVariableNvStoreReserved|0
  gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableBase64|*|0
```

#### Correct Example

```ini
[PcdsDynamicExDefault.IA32]
  gEfiMdeModulePkgTokenSpaceGuid.PcdEmuVariableNvStoreReserved|0

[PcdsDynamicExVpd.IA32]
  gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableBase64|*|0
```

#### 2.8.3.5 Multiple Architectural Section Tags

It is permissible to specify multiple architectural section tags for the same
PCD item type in a single section.

#### Example

```ini
[PcdsFixedAtBuild.IA32, PcdsFixedAtBuild.X64]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeMemorySize|1
  gEfiMdeModulePkgTokenSpaceGuid.PcdResetOnMemoryTypeInformationChange|FALSE

[PcdsPatchableInModule.IA32, PatchableInModule.X64]
  gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000000

[PcdsDynamicDefault.IA32, PcdsDynamicDefault.X64]
  gEfiMdeModulePkgTokenSpaceGuid.PcdEmuVariableNvStoreReserved|0
  gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableBase64|0
```

#### 2.8.3.6 Dynamic and DynamicEx PCD Storage Methods

The PCDs that use Dynamic and DynamicEx access methods can have their values
stored in one of three different methods, Default, VPD or HII. A PCD using one
of these access methods can use one storage method. It is not permissible to
have a PCD try to store the data in the Default database and a VPD region at
the same time.

The Default methods (`[PcdsDynamicDefault]` and `[PcdsDynamicExDefault]`) rely
on internal databases generated by the build tools and stored as a data segment
in the PEI and DXE PCD drivers. The format for listing a boolean or numeric
datum type PCD in these sections is:

`TokenSpaceGuid.PcdCname|<Value>`

The format for listing a VOID* datum type PCD in these sections is:

`TokenSpaceGuid.PcdCname|<Value>|VOID*|<MaxSize>`

The VPD methods ([PcdsDynamicVpd] and [PcdsDynamicExVpd]) rely on data stored
in read-only memory. The format for listing a boolean or numeric datum type PCD
in these sections is:

`TokenSpaceGuid.PcdCname|<Offset>|<Value>`

The format for listing a VOID* datum type PCD in these sections is:

`TokenSpaceGuid.PcdCname|<Offset>|<MaxSize>|<Value>`

The HII methods ([PcdsDynamicHii] and [PcdsDynamicExHii]) rely on an HII Data
Store. The format for listing a PCD in these sections is:

```
TokenSpaceGuid.PcdCname|<HiiString>|<VariableGuid>|<VariableOffset>|<Value>|<Attribute>
```

**********
**Note:** Some of the above fields are optional; refer to "PCD Sections" in the
next chapter for the exact syntax.
**********

#### 2.8.3.7 Unique PCDs

Unique PCDs are identified using the format to identify the named PCD:

`TokenSpaceGuidCName.PcdCName`

The content for a PCD in this section is the PCD's Name (PCD Token Space Guid
C name, the PCD C name - separated by a period character,) and Default value.
Field entries on a line are separated by the pipe "|" character.

This specification prohibits setting different PCD access methods for a single
PCD in different modules. The access methods here are `PcdsFixedAtBuild`,
`PcdsPatchableInModule`, `PcdsDynamic` and `PcdsDynamicEx`.

**********
**Note:** A future version of this specification and the tool may allow listing
a PCD in both `PcdsFixedAtBuild` and `PcdsPatchableInModule` sections. For
example, the platform integrator may want to use one module with a given PCD
as `FixedAtBuild`, and have a different module with the same PCD use it as
`PatchableInModule`.
**********
**Note:** A PCD that is used as FixedAtBuild for one module, while a different
module may want to use the PCD as PatchableInModule and a third module might
use the PCD as DynamicEx. Under normal circumstances, only two of these might
be used - `PcdsFixedAtBuild` for modules with wellknown values for a PCD,
then either `PcdsPatchableInModule` or `PcdsDynamicEx` - the first
being for testing a module, the second giving the ability for doing individual
driver performance tuning "on-the-fly".
**********

#### 2.8.3.8 Precedence

Tools must assume that the first method found for a PCD in the PCDs sections
will used for all instances of a PCD. Tools must not allow for different
modules using a PCD differently, using the `<Pcds*>` statements under the INF
file definitions in the `[Components]` section.

Tools must process VOID* PCD entries that do not include the maximum length
field by determining the maximum length of the PCD values in the DSC, DSC and
INF files. Size is allocated for "string" entries to be the length of the
string plus 1 byte for the null terminator, for L"string" entries to be the
length of the UCS-2 character string plus 2 bytes for the null terminator and
the exact length of a byte array.

Precedence for determining PCD values (high to low, last in position) is as
follows:

* A PCD value defined by a MACRO, ("MacroName" in this example), and the Macro
  is defined on the command-line using -D MacroName=Value

* A PCD value defined in the FDF file SET statements

* A PCD value defined positionally in the FDF file

* A FeatureFlag, PatchableInModule or FixedAtBuild PCD value defined in the
  `[Components]` INF scoping section

* A FeatureFlag, PatchableInModule or FixedAtBuild PCD value defined in a PCD
  access method section with an architectural modifier (the access method for a
  PCD in a section with an architectural modifier takes precedence over any
  other access method)

In this example, modules built for IA32 architecture, the PCD will use
PatchableInModule access, while modules built for all other architectures, the
PCD will use the FixedAtBuild access method:

```ini
[PcdsFixedAtBuild.common]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeMemorySize|1
  gEfiMdeModulePkgTokenSpaceGuid.PcdResetOnMemoryTypeInformationChange|FALSE

[PcdsPatchableInModule.IA32]
  gEfiMdeModulePkgTokenSpaceGuid.PcdStatusCodeMemorySize|1
  gEfiMdeModulePkgTokenSpaceGuid.PcdResetOnMemoryTypeInformationChange|FALSE
```

* A PCD value defined in a PCD access method (item type) section for common
  architectures

* A PCD value defined in an INF (provided all INF files have defined the same
  value)

* A PCD default value defined in the DEC file that declares the PCD.

**********
**Note:** Dynamic or DynamicEx PCD sections with architectural modifiers is not
allowed unless the platform can only be built using a single architecture, even
if there is more than one architecture listed in the SUPPORTED_ARCHITECTURES
element in the `[Defines]` section. When building more than one architecture
for a given platform (the platform supports multiple architectures in firmware)
only a single value can be used for either a Dynamic or DynamicEx PCD. Therefore,
listing PcdsDynamic or PcdsDynamicEx sections with architectural modifiers is
prohibited in this type of platform description file.
**********
**Note:** PCD values within a section are positional, (last wins) if a PCD is
listed more than one time within a section. List a PCD in one of the other
access methods is allowed, provided a single access method must be used for all
instances of the PCD.
**********

#### 2.8.3.9 Library Instances

Library Instances that use PCDs that the module is linked with must use the
same PCD setting as the module using the Library Instance. So if a module uses
a PCD as `PcdsFixedAtBuild`, then all library instances that use that PCD must
also use the PCD as `PcdsFixedAtBuild` with the same value.

Build Tools must detect missing PCD entries (PCD specified in an INF file, but
not in the DSC file) and search the DEC files in the EDK II Packages
(sub‐directories in directories pointed to by WORKSPACE or PACKAGES_PATH system
environment variables), in order to use the default value from the DEC file.PCD
Values may be absolute (a number, string, etc.) a MACRO name or an expression.
The expression is a C-style expression using C relational, equality and logical
numeric and bitwise operators or numeric and bitwise operators that evaluate to
a value that matches the PCD's Datum Type (specified in the DEC package
declaration file.) Precedence and associativity follow C standards. Using PCDs
in expressions is also permitted.

#### 2.8.3.10 Maximum Size of a VOID* PCD

If the maximum size of a VOID* PCD is not specified in the DSC file, then the
maximum size will be calculated based on the largest size of the following:

* the string or array in the DSC file

* the string or array in the INF file

* the string or array in the DEC file

#### Scenario A

If for a given PCD and architecture:

1. The PCD is not listed anywhere in the DSC file,

2. If the PCD is listed in the INF file of at least one of the modules listed
   in the `[Components]` section,

3. All of the modules in the `[Components]` section that use the PCD, list the
   PCD using the Dynamic access method in their INF files,

4. The DEC file has the PCD listed in the sections for Dynamic, Patchable in
   Module and FixedAtBuild,

The build tools must use the FixedAtBuild access method for this PCD in this
scenario.

#### Scenario B

If for a given PCD and architecture:

1. The PCD is not listed anywhere in the DSC file,

2. If the PCD is listed in the INF file of at least one of the modules listed
   in the [Components] section,

3. All of the modules listed in the `[Components]` section that use the PCD,
   list the PCD using the Dynamic access method in their INF files,

4. The DEC file has the PCD listed in the sections for Dynamic and
   FixedAtBuild.

The build tools must use the FixedAtBuild access method for this PCD in this
scenario.

#### Scenario C

If for a given PCD and architecture:

1. The PCD is not listed anywhere in the DSC file,

2. If the PCD is listed in the INF file of at least one of the modules listed
   in the `[Components]` section,

3. One or more module uses the Patchable in Module access method for the PCD in
   the INF files,

4. All of the other modules listed in the `[Components]` section that use the
   PCD, list the PCD using the Dynamic access method in their INF files,

The build tools must use the Patchable in Module access method for the PCD in
all of the modules that use this PCD in this scenario. Since number 3 shows
that there are modules that are coded for only patchable in module access, and
the EDK II build system requires that for a single architecture a single access
method must be selected for each PCD, no other methods of access, such as
FixedAtBuild, can be used for modules that may not be coded specifically for
patchable in module access.
