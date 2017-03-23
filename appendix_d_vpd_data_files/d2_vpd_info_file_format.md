<!--- @file
  D.2 Vpd Info File Format

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

## D.2 Vpd Info File Format

The Vpd Info file contains an ordered list of the VPD PCDs, replacing any
wildcard character values that may appear in the DSC file, with the actual
computed (by the external tool - named by GUID in the `Conf/tools_def.txt`
file) values. The file is named by the GUID value of the tool named by GUID and
uses a ".map" extension. It is generated in the platform's FV directory.

#### Summary

This file is used by the AutoGen functions in the EDK II build system to
generate the PCD database used for Dynamic an DynamicEx PCDs. Blank lines and
lines that start with the hash "#" character (used for comments) are skipped
when processing this file. Each comment must appear on its own line or it can
be appended after an entry. Line extensions are not permitted.

#### Prototype

```c
<VpdInfoFile>   ::= <PcdStatements>
<PcdStatements> ::= <PcdName> <FS> <Offset> <FS> <SizeValue> <EOL>
<Offset>        ::= <HexNumber>
<SizeValue>     ::= if (pcddatumtype == "BOOLEAN"): "1" "|" {"0x0"} {"0x1"}
                    elif (pcddatumtype == "UINT8"):
                    "1" "|" <UINT8z> elif (pcddatumtype == "UINT16"):
                    "2" "|" {<UINT16z> elif (pcddatumtype == "UINT32"):
                    "4" "|" {<UINT32z> elif (pcddatumtype == "UINT64"):
                    "8" "|" <UINT64z> else:
                    <NumValUint32> "|" <CArray>
```

#### Example

```ini
## @file
#
# THIS IS AUTO-GENERATED FILE BY BPDG TOOLS AND PLEASE DO NOT MAKE MODIFICATION.
#
# This file lists all VPD informations for a platform fixed/adjusted by BPDG tool.
#
# Copyright (c) 2010, Intel Corporation. All rights reserved.<BR>
# This program and the accompanying materials
# are licensed and made available under the terms and conditions of the BSD License
# which accompanies this distribution. The full text of the license may be found at
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR IMPLIED.
#

gEfiMdeModulePkgTokenSpaceGuid.PcdVideoHorizontalResolution|0x0|4|800
gEfiMdeModulePkgTokenSpaceGuid.PcdVideoVerticalResolution|0x4|4|600
gEfiMdeModulePkgTokenSpaceGuid.PcdConOutRow|0x8|4|25
gEfiMdeModulePkgTokenSpaceGuid.PcdConOutColumn|0xc|4|80
```
