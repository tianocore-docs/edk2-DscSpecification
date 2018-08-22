<!--- @file
  2.6 [Libraries] Section Processing

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

## 2.6 [Libraries] Section Processing

This section specifies all the EDK INF files that must be processed to build
the libraries used to build the individual EDK components. This will include
all the libraries called out in the individual component INF files. A sample
section is listed below. Each line from the libraries section specifies a
library component's INF file (relative to `$(EDK_SOURCE)`, or absolute path).

This section is required for any EDK II DSC file that specifies one or more EDK
components. If only EDK II Modules are used, this section must not be
specified. If the section is specified, and only EDK II Modules are found, the
build and parsing tools will ignore this section. A warning message will be
emitted by the parsing tool if and only the parsing tool is executed in a
verbose mode.

The `!include` statements may be used within the `[Libraries]` section.

The file specified after the `!include` statement can only contain a list of
EDK Library INF files (with the path to the file). If the line starts with a
word, rather than a variable like `$(EDK_SOURCE)` the path is assumed to be
relative to `$(EDK_SOURCE)`. Again, only EDK Library INF files are permitted in
the file specified in the `!include` statement.

This section will typically use one of the following section definitions:

```ini
[Libraries.common]
[Libraries.IA32]
[Libraries.X64]
[Libraries.EBC]
```

The formats for entries in this section is:

```
$(EDK_SOURCE)/Path/to/LibraryName.inf
$(CUSTOM_DECOMPRESS_LIB_INF)
```
