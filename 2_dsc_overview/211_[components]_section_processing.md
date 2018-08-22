<!--- @file
  2.11 [Components] Section Processing

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

## 2.11 [Components] Section Processing

One or more `[Components]` sections contain lists of EDK components and EDK II
Modules. The format for specifying the INF file for EDK II modules incorporates
new scoping capabilities.

This section uses one or more of the following section definitions:

* `[Components]`
* `[Components.IA32]`
* `[Components.X64]`
* `[Components.EBC]`
* `[Components.common]`

EDK components are specified using a fully qualified path to the EDK INF file.

`$(EDK_SOURCE)/Path/and/Filename.inf`

Because EDK II modules have different requirements than EDK I components,
specifying the INF filename in the extended DSC file may require more than just
the INF filename and options. A scoping structure, that binds library class
(with an optional override instance,) PCD settings (also overriding the values
specified in the `[PcdsPatchableInModule]` or `[PcdsFixedAtBuild]` sections)
and build options for an EDK II module may be required. This scoping structure,
containing sub-elements, is enclosed within curly braces: "{}". The opening
curly brace, "{", must appear at the end of the inf filename line, before any
comments.

Scoping is needed only if specifying a non-default library class (one specified
in the `[LibraryClasses]` section), changing a PCD value from defaults specified
in either the `[PcdsPatchableInModule]` or `[PcdsFixedAtBuild]` sections, or
overriding tool flags set either in the `[BuildOptions]` section of the DSC file
or if an entry in the `tools_def.txt` file. Scoping can also be used to support
building multiple versions of a module where the different versions are built
using a different FILE_GUID value declared in the scoping section.

**********
**Note:** Section 3.11 defines the sub-element content of an INF file specified
in a component section. This code paragraph section shows the format of a
"scoping structure".
**********

```ini
Path/and/Filename.inf {
# Sub-elements - See EDK II INF file statement structure
}
```

There are four valid, optional sub-elements for EDK II modules. These
sub-element are enclosed within angle brackets: `<Defines>, <LibraryClasses>`,
`<Pcds*>` and `<BuildOptions>`.

For EDK component INF files, an optional sub-element of
`<SOURCE_OVERRIDE_PATH>` has been defined. If this element is specified, files
listed in the directory are used instead of the "same-named" files in the
component's directory. If an EDK component directory lists files, A.c, B.c and
C.h, and the directory specified in this sub-element contains the file B.c,
then the component will be built using files from the component directory: A.c
and C.h, and the file B.c from the override directory. Any other files listed
in the override directory will NOT be included in the build (no new or
additional files are permitted).

An INF file line may also have one argument, EXEC = Filename, that specifies
an executable file that takes the INF filename as a parameter. The Filename
must be executable, and must take the INF filename. No other arguments are
permitted to the Filename.

The parsing tools will call the executable specified by the Filename, as
follows:

`Filename Path/and/Filename.inf`

If the `EXEC` argument is included on the component or module INF line, EDK II
Tools will ignore processing of the module. Additionally, EDK II build tools
will not perform any dependency checking for files listed in the INF file nor
on the output object file. EXEC example follows:

`Path/and/Filename.inf EXEC = exe2bin.exe`

The structure for specifying an EDK II INF filename is as follows:

```c
$(EDK_SOURCE)/Path/and/ComponentName.inf [options] {
  # Library Class listing - Required for EDK II components. One or more
  # Library mapping lines are permitted.
  <LibraryClasses>
    LibraryClassName|Path/and/LibraryInstanceName.inf
  # Followed by zero (no pcds are used by the module) or more of the
  # following individual component override statements.
  # Pcd settings are applicable to both Library Instances that
  # are linked to the component and the component itself.
  # If a PCD entry is not specified, however the INF files for either
  # the library classes that are linked to the component, or the
  # component's INF file require Pcd settings, a globally defined
  # value for the Pcd ([Pcd{AccessMethod}] section or the default
  # value from the DEC file that declares the PCD) will be used. If
  # none of these values are specified, build tools processing this file
  # should fail with an error message (indicating the missing PCD # entry.)
  # NOTE: MaximumDataSize is required for VOID* datum type PCDs, and # should NOT be used for either the boolean or numeric datum types.
  # PcdsDynamic and PcdsDynamicEx cannot be specified in a component # scoped section as these values are platform scoped, not module
  # scoped.
  <PcdsFixedAtBuild>
    TokenSpaceGuidCName.PcdTokenName|Data[|MaximumDataSize]
  <PcdsFeatureFlag>
    TokenSpaceGuidCName.PcdTokenName|{TRUE}{FALSE}
  <PcdsPatchableInModule>
    TokenSpaceGuidCName.PcdTokenName|Data[|MaximumDataSize]
  # BuildOption Format is identical to the format used for the
  #tools_def.txt file. Options specified in this section take
  # precedence over the platform build options specified in the
  #[Defines] section. (The [Defines] section options take precedence #over options specified in the tools_def.txt file)
  <BuildOptions>
    TARGET_TOOLCHAIN_ARCH_COMMANDTYPE_FLAGS = string of flags
}
```

In order to build a module multiple times, the `<Defines>` tag is used. In the
following example, the S3Resume2Pei module is built twice, using different
library instances and PCD values (one for use with IA32 architecture and one
for the X64 architecture). The first module instance uses the standard
`FILE_GUID` value from the INF file when creating the FFS filename, while the
second module instances will use the `FILE_GUID` value specified in the DSC
file for creating the FFS filename.

```c
  UefiCpuPkg/Universal/Acpi/S3Resume2Pei/S3Resume2Pei.inf {
    <PcdsFeatureFlag>
      gEfiMdeModulePkgTokenSpaceGuid.PcdDxeIplSwitchToLongMode|FALSE
    <LibraryClasses>
      NULL|BinaryDistributionModulePkg/Library/SwitchToLongMode/SwitchToLongModeDisabledLib.inf
  }
  UefiCpuPkg/Universal/Acpi/S3Resume2Pei/S3Resume2Pei.inf {
    <Defines>
      FILE_GUID = 35B57EA0-4A41-4a12-B1F5-5F7B79095301
    <PcdsFeatureFlag>
      gEfiMdeModulePkgTokenSpaceGuid.PcdDxeIplSwitchToLongMode|TRUE
    <LibraryClasses>
      NULL|BinaryDistributionModulePkg/Library/SwitchToLongMode/SwitchToLongModeEnabledLib.inf
  }
```

If the values for the PcdTokenName are specified in the global
`[Pcds{AccessMethod}]` section and no values are specified in the INF filename
section, then the global values will be used. It should be noted that only
`LibraryClassName|library/instance` definitions are required. Adding just the
library class name without the library instances is for informational purposes
only, and they will not be processed.

#### Example

```ini
MdeModulePkg/Universal/Disk/DiskIo/Dxe/DiskIo.inf {
  #EDK II Module
  <LibraryClasses>
    DebugLib|MdePkg/Library/PeiDxeDebugLibReportStatusCode/PeiDxeDebugLibReportStatusCode.inf
    BaseMemoryLib|MdePkg/Library/DxeMemoryLib/DxeMemoryLib.inf
    MemoryAllocationLib|MdePkg/Library/DxeMemoryAllocationLib/DxeMemoryAllocationLib.inf
}
```
