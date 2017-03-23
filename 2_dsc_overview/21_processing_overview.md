<!--- @file
  2.1 Processing Overview

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

## 2.1 Processing Overview

Each platform DSC file is broken out into sections in a manner similar to the
component description (INF) files. However, while the intent of a component's
INF file is to define the source files, libraries (or library classes), and
definitions relevant to building the component, the function of the platform
DSC file is to specify the library instances, components and output formats
used to generate binary files that will be processed by other tools to generate
an image that is either put into a flash device, made available for recovery
booting or updating existing firmware on a platform.

**********
**Note:** For users familiar with EDK, the EDK II DSC file is not used to
specify how compiled binary images get placed into UEFI/PI compliant binaries.
The EDK II Flash Description File (FDF) file specifies how to package the
binaries files into UEFI/PI compliant images.
**********

In general, the parsing utilities read each line from the sections of the
platform description (DSC) file, process the component, module, or library's
INF file on the line to generate a makefile, and then continue with the next
line.

Figure 1 illustrates the process flow, with the dark format indicating the
process for building PE/PE32+/Coff files.

![](../media/image1.png)

###### Figure 1 EDK II Parsing Data Flow
