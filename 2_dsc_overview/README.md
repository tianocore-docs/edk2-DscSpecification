<!--- @file
  2 DSC Overview

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

# 2 DSC Overview

This section of the document provides an overview to processing EDK II platform
description (DSC) file. Additional chapters describe different sections of the
EDK II DSC file in detail.

EDK II parsing tools contain the templates for processing files to create the
component binary images from source files. EDK II Binary Modules are not
required to be included in EDK II DSC files (they must be included if the
Binary Module uses PCDs that use PatchableInModule or DynamicEx PCD access
methods). EDK II DSC files are a list of:

* EDK II Module INF Files
* EDK Components
* EDK libraries (only used by EDK Components)
* EDK II Library Class Instance Mappings (only used by EDK II Modules)
* EDK II PCD Entries

There are no new features or format introduced in this specification.

DSC files that use any new features must use the new
`DSC_SPECIFICATION = 0x0001001A` in the `[Defines]` section. Older DSC files
that do not use any of these features do not need to update the
`DSC_SPECIFICATION` value.

This version of the specification reflects changes to the EDK II reference
build system that has been updated to support builds using EDK II Packages that
are located in directories outside of the directory specified by the system
environment variable, WORKSPACE.

An EDK II Package (directory) is a directory that contains an EDK II package
declaration (DEC) file.

The EDK II build system has been updated to allow the setting of multiple paths
that will be searched when attempting to resolve the location of EDK II
packages. This new feature allows for more flexibility when designing a tree
layout or combining sources from different sources. The new functionality is
enabled through the addition of a new environment variable (PACKAGES_PATH).

The PACKAGES_PATH variable is an ordered list of additional search paths using
the default path separator of the host OS between each entry ( ";" on Windows,
":" on Linux and OS/X). The path specified by the WORKSPACE variable always has
the highest search priority over any PACKAGE_PATH entries. The first path (left
to right) in the PACKAGES_PATH list has the highest priority and the last path
has the lowest priority.

For the remainder of this document, unless otherwise specified (using "system
environment variable, WORKSPACE"), references to the `WORKSPACE` and
`$(WORKSPACE)` refer to the ordered list of directories specified by the
combination of `WORKSPACE + PACKAGES_PATH`. The build system will automatically
join the directories and search these paths to locate content, with the first
match terminating the search. For example given the following set of
environment variables, and the MdeModulePkg is located in both the edk2 and
edk2Copy directories, the build system would use the C:\work\edk2\MdeModulePkg
when attempting to locate the MdeModulePkg.dec file.

```
set WORKSPACE=c:\work
set PACKAGES_PATH=c:\work\edk2;c:\work\edk2Copy
```

Build tools will stop searching when the first location is resolved.

Refer to the TianoCore.org web-site for more information on the EDK II build
system.

**********
**Note:** Path and Filename elements within the DSC are case-sensitive in order
to support building on UNIX style operating systems. Use of "/../" in a path
and, "./" or "../" at the start of a path is prohibited.
**********
**Note:** This document uses "\" to indicate that a line that cannot be
displayed in this document on a single line. Within the DSC specification, each
entry must appear on a single line.
**********
**Note:** The total path and file name length is limited by the operating
system and third party tools. It is recommended that for EDK II builds that the
WORKSPACE (or directories listed in the PACKAGESPATH system environment
variable) directory be either a directory under a subst drive in Windows
(s:/build as an example) or be located in either the /opt directory or in the
user's /home/username directory for Linux and OS/X.
**********
