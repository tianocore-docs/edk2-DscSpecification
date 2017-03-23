<!--- @file
  3.1 Building multiple architectures

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

## 3.1 Building multiple architectures

The build tools use EDK II meta-data files (INF, DEC, DSC and, optionally, the
FDF) to create Makefiles that are then processed by third party tools. The
third party utility that is called after all pre-work has been completed is
define in the `Conf/tools_def.txt` file using the tool code of MAKE. On Windows
systems, this will typically point to the (with the fully qualified path)
utility "_`nmake.exe`_", while on *NIX systems, the utility is typically just
"_`make`_". The build system will also generate a single master makefile, one
Makefile per architecture specified on the command-line or listed in the
`Conf/target.txt` file and one Makefile for each for each module or library
instance within the architectural tree. Therefore, the build system must be
capable of processing and keeping track of common items and architecturally
specific content. Common content (not architecturally dependent) will be
processed for each architecture. For example, if -a IA32 -a X64 appear on the
command line, all modules that do not specify an architecture modifier in the
`[Components]`, `[Libraries]` or `[LibraryClasses]` sections will be processed
so that makefiles for the module entries appear under _IA32_ and under _X64_
sub-directories in the build output tree. Where architectural modifiers are
used, the build tools will process content such that the modules (and their
makefiles) are only under the corresponding architecture trees.
