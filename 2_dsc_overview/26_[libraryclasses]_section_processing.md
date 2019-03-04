<!--- @file
  2.7 [LibraryClasses] Section Processing

  Copyright (c) 2006-2019, Intel Corporation. All rights reserved.<BR>

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

## 2.6 [LibraryClasses] Section Processing

The `[LibraryClasses]` section is used to provide a mapping between the library
class names used by an EDK II module and the Library Instances that are
selected by the platform integrator. Library Classes allow modules to be coded
for a library class, and then allow platform integrator then chooses a Library
Instance based on a priori knowledge of the instances. Library Instances are
classified using the architecture types they have been coded for as well as the
supported EDK II module types. As an example, within EDK II, the library class,
DebugLib has seven potential instances, only one of which may be linked to a
single component. To support a given module type selection, the
`[LibraryClasses]` section header can optionally specify the EDK II module type
(following the supported architecture field). This is permitted as some library
instances can be used by any or all module types.

This is a an optional section for EDK II DSC files only if there are no EDK II
modules used by the DSC file.

The following is the generic format for the specifying a section

```ini
[LibraryClasses]
[LibraryClasses.IA32]
[LibraryClasses.X64]
[LibraryClasses.EBC]
[LibraryClasses.common]
```

Format for entries in this section is as follows:

```
LibraryClassName|Path/To/LibInstanceName.inf
LibraryClassName1|Path/To/LibInstanceName1.inf
```

**********
**Note:** "LibraryClassName" is a keyword in the first field of the above
example format can not be `NULL`. The "LibraryClassName" name must be unique
to an instance specified in the second field. All INF files that require a
LibraryClassName will use this instance when linked to the other libraries
or modules.
**********

The first globally defined library instance, defined in a DSC file, that
satisfies a module's requirement for a Library Class, unless specifically
overridden by the module in the `[Components]` section, will be used.

The Library Instances will be selected using the following rules to satisfy a
library class for each module listed in the `[Components]` section (in order of
highest precedence):

1. `<LibraryClasses>` associated with the INF file in the `[Components]` section
2. `[LibraryClasses.$(Arch).$(MODULE_TYPE), LibraryClasses.$(Arch).$(MODULE_TYPE)]`
3. `[LibraryClasses.$(Arch).$(MODULE_TYPE)]`
4. `[LibraryClasses.common.$(MODULE_TYPE)]`
5. `[LibraryClasses.$(Arch)]`
6. `[LibraryClasses.common]` or `[LibraryClasses]`

If the Library instance is specified in the context of the INF file (see
`[Components]` section), then that library instance will be used. If only a
library class is specified in the context of the INF file, then the first
matching the `library class | library instance` following the above precedence
rules will be used. If no instance is found for the library class, the build
tools must fail with an error similar to the following.

```
ERROR: Library Class [$(LibClassName)] specified by the Module [$(InfFileName)] does not have a Library Class Instance Defined.
```

Build tools can propose fixes, as shown in the following:

* Check for spelling of the Library Class Name for the module in the components
  section, or

* Check the EDK II Packages (sub‐directories in directories pointed to by
  WORKSPACE or PACKAGES_PATH system environment variables) for a library
  instance that satisfies the Library Class, then add that instance to the DSC
  file in the correct Library Class section.

The selected library instance is added to the LIBS definition in the output
makefile:

`LIBS = $(LIBS) $(LIB_DIR)/$(LibInstanceName)`

PCDs that are used by a library instance are resolved using the PCD settings of
the driver linking the library instance. Note that if a module's PCD section is
used, and multiple modules specify different values for the same PCD setting,
there may be multiple instances of the library instance that will be compiled,
matching the PCD settings for each module.
