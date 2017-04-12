<!--- @file
  3.2 General Rules

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

## 3.2 General Rules

The general rules for all EDK II INI style documents follow.

**********
**Note:** Path and Filename elements within the DSC are case-sensitive in order
to support building on UNIX style operating systems. Additionally, names that
are C variables or used as a macro are case sensitive. Other elements such as
section tags or hex digits, in the DSC file are not casesensitive. The use of
 "..", "../" and "./" in paths and filenames is strictly prohibited.
**********
**Note:** This document uses "\" to indicate that a line that cannot be
displayed in this document on a single line. Within the DSC specification, each
entry must appear on a single line.
**********

* Multiple DSC files may exist in a directory, however it is recommended that
  either the `PLATFORM_GUID` or the `PLATFORM_VERSION` be unique to the DSC
  file. It is recommended that the `PLATFORM_NAME` and `OUTPUT_DIRECTORY` also
  be unique.

* Text in section tags is case in-sensitive.

* A section terminates with either another section definition or the end of the
  file.

* To append comment information to any item, the comment must start with a hash
  "#" character.

* All comments terminate with the end of line character.

* Field separators for lines that contain more than one field are pipe "|"
  characters. This character was selected to reduce the possibility of having
  the field separator character appear in a string, such as a filename or text
  string.

**********
**Note:** The only notable exception is the PcdName which is a combination of
the PcdTokenSpaceGuidCName and the PcdCName that are separated by the period "."
character. This notation for a PCD name is used to uniquely identify the PCD.
**********

* A line terminates with either an end of line character or a comment.

* When processing numeric values, either integer or hex, leading zeros
  specified in the entry may be ignored. For example, 0x00000000000000000000001
  can be a valid value for a `UINT8` data type, as the actual value is 1.

* All words in quotation marks in the EBNF in this file must be considered
  reserved. Redefining a reserved word is not permitted.

### 3.2.1 Backslash

Use of the back slash character "\" in this document is only for lines that
cannot be displayed within the margins of this document. The backslash
character must not be used to extend a line over multiple lines in the DSC file.

### 3.2.2 Whitespace characters

Whitespace (space and tab) characters are permitted between token and field
separator elements for all entries.

Whitespace characters are not permitted between the PcdTokenSpaceGuidCName and
the dot, nor are they permitted between the dot and the PcdCName.

### 3.2.3 Paths for filenames

Note that for specifying the path for a file name, if the path value starts
with a dollar sign "$" character, either a local `MACRO` or system
environment variable is being specified. If the path value starts with one of
"letter:\", "/", "\" or "\\" the path must be a fully qualified URI location.
If it does not, the specified path is relative to EDK II Packages
(sub-directories in directories pointed to by WORKSPACE or PACKAGES_PATH system
 environment variables).

**********
**Caution:** The use of "..", "./" and "../" in a path element is prohibited.
For all DSC files, the specified directory path must use the forward slash
character for separating directories. For example, `MdePkg/Include/` is Valid.
**********
**Note:** If the platform integrator is working on a Microsoft Windows*
environment and will not be working on a non-windows platform, then the
DOS-style directory separator can be used. The forward slash Unix-style
directory separator is mandatory for distributions where the build environment
is unknown.
**********

Unless otherwise noted, all file names and paths are relative EDK II Packages
(subdirectories in directories pointed to by WORKSPACE or PACKAGES_PATH system
environment variables). A directory name that starts with a word is assumed by
the build tools to be an EDK II Package directory name.

Each module may have one or more INF files that can be used by tools to
generate images. Specifically, the EDK Compatibility Package may contain two
INF files for any module that contains assembly code. Since the ECP can be used
with existing EDK tools (which is only supported by Microsoft and Intel Windows
based tools,) a separate INF file to support the multiple tool chain capability
of the EDK II build system must be provided for the modules that contain
assembly code. The EDK II ECP will use the _basename_edk2.inf_ for the filename
of the EDK II build system compatible INF files for non-Windows based tool
chains, and use just the _basename.inf_ for the filename of EDK only INF files
used by the EDK build system.
