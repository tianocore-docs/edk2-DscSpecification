<!--- @file
  2.10 PCD Database

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

## 2.10 PCD Database

Dynamic and DynamicEx PCDs can be modified during the boot/setup stages. In
order to support modifications, a PEIM and a DXE driver use databases of these
PCDs so that changes can persist across reboots. These databases are generated
prior to the final image assembly. The following rules determine when the build
system will add the PCDs into these databases.

1. If a PCD is listed in a `PcdsDynamicVpd` or `PcdsDynamicExVpd` section, and
   the PCD is not used by any module that is listed in the DSC file, the build
   MUST ADD the entry in the Platform's PCD Database, and the parser must not
   throw an error or warning message.

2. If PCD is listed in a `PcdsDynamicDefault` or `PcdsDynamicExDefault`
   section, and the PCD is not used by any module that is listed in the DSC and
   FDF file, the build must NOT add the entry in the Platform's PCD Database;
   the build may provide a warning message.

3. If PCD is listed in a `PcdsDynamicHii` or `PcdsDynamicExHii` section, and
   the PCD is not used by any module that is listed in the DSC and FDF file,
   the build must NOT add the entry in the Platform's PCD Database; the build
   may provide a warning message.

4. If a PCD is not listed in the DSC file but is listed under a `[PcdEx]`
   section in a Binary INF file listed in the FDF file, then the build must add
   the entry to the Platform's PCD Database as `PcdsDynamicExDefault`.

5. If a PCD is not listed in the DSC file, but binary INF files used by this
   platform all (that use this PCD) list the PCD in a `[PcdEx]` section, AND
   all source INF files used by this platform the build that use the PCD list
   the PCD in either a `[Pcd]` or `[PcdEx]` section, then the tools MUST ADD
   the PCD to the Platform's PCD Database; the build must assign the access
   method for this PCD as `PcdsDynamicExDefault`.

6. If a PCD is not listed in the DSC file, but binary INF files used by this
   platform all (that use this PCD) list the PCD in a `[PatchPcd]` section, AND
   all source INF files used by this platform the build that use the PCD list
   the PCD in either a `[Pcd]` or `[PatchPcd]` section, then the tools must NOT
   add the PCD to the Platform's PCD Database; the build must assign the access
   method for this PCD as `PcdsPatchableInModule`

7. If one of the Source built modules listed in the DSC is not listed in FDF
   modules, and the INF lists a PCD can only use the `PcdsDynamic` access
   method (it is only listed in the DEC file that declares the PCD as
   `PcdsDynamic`), then build tool will report warning message- notify the PI
   that they are attempting to build a module that must be included in a flash
   image in order to be functional. These Dynamic PCD will not be added into
   the Database unless it is used by other modules that are included in the FDF
   file.

8. If one of the Source built modules listed in the DSC is not listed in FDF
   modules, and the INF lists a PCD can only use the `PcdsDynamicEx` access
   method (it is only listed in the DEC file that declares the PCD as
   `PcdsDynamicEx`), then DO NOT break the build; DO NOT add the PCD to the
   Platform's PCD Database.

9. If a module is listed in FDF file and use a Dynamic or DynamicEx PCD, the
   PCD MUST be added into the PCD Database.
