<!--- @file
  Appendix A Variables

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

# Appendix A Variables

One of the core concepts of this utility is the notion of symbols. Use of
symbols follows the makefile convention of enclosing within $(), for example
`$(EDK_SOURCE)`. As the parsing utility processes files, it will often perform
parsing of variable assignments. These variables can then be referenced in
other sections of the DSC file. Variable assignments will be saved internally
in either a local or global symbol table. The local symbol table is purged
following processing of individual Platform (DSC) files. Global symbol values
persist throughout execution of the utility. Local symbol values take precedent
over global symbols. The following table describes the symbols generated
internally by the utility. They can be overridden either on the command line,
in the DSC file, or in individual INF files. The symbols in the table below are
typically global.

For a pure EDK II build, two environment variables are required to be set prior
to executing any build, `WORKSPACE` and EDK_TOOLS_PATH must be set. point to
the Location of the BaseTools directory tree. When EDK II Packages are in
directories that are not under a single directory, then the PACKAGES_PATH and
EDK_TOOLS_BIN (Windows builds only) directories must also be set.

For a build using EDK components in the EDK II DSC file, the `EDK_SOURCE`
directory must point to the root of the EDK (either a directory containing an
EDK tree or the location of the EdkCompatibilityPkg directory) tree.

Table 10 Standard Variables

| Variable Name    | Description                                                                                                                                                                                                                                                                                                                                              |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `WORKSPACE`      | Defines the root directory of the local development tree, for example C:\work. If not defined as an environmental variable when an EDK II tool is invoked, the utility will give an error message and exit. If the development tree contains sub-directories that contain EDK II Packages, then this variable must be set prior to running edksetup.bat. |
| `EDK_TOOLS_PATH` | Defines the root directory of the local EDK II BaseTools directory.                                                                                                                                                                                                                                                                                      |
| `PACKAGES_PATH`  | This variable is not permitted in EDK II meta-data files.                                                                                                                                                                                                                                                                                                |
|                  | This variable contains an ordered list of directories that contain EDK                                                                                                                                                                                                                                                                                   |
|                  | II Package directories. This is NOT required if the development tree (defined by the WORKSPACE) contains all EDK II Packages used for development. If the development tree contains sub-directories that contain EDK II Packages (i.e., C:\work\edk2\MdePkg,                                                                                             |
|                  | C:\work\edk2\MdeModulePkg, C:\work\myplatform\MyPlatformPkg) then this variable must list all directories that contain EDK II Packages (i.e., PACKAGES_PATH=C:\work\edk2;C:\work\myplatform); this variable must be set prior to running edksetup.bat                                                                                                    |
| `EDK_TOOLS_BIN`  | This variable is not permitted in EDK II meta-data files. This a Windows only variable that points to the EDK II BaseTools binary directory. If the WORKSPACE directory contains a                                                                                                                                                                       |
|                  | BaseTools\Bin\Win32 directory and the directory is populated with the build tools, then this variable is not required. If the binaries are located in a different directory, i.e., C:\work\edk2-BaseTools-win32, then this variable must be set prior to running edksetup.bat                                                                            |
| `EDK_SOURCE`     | Defines the directory of an original EDK directory tree, for example C:\EFI2.0\EdkCompatibilityPkg. If not defined as an environmental variable when an EDK II tool is invoked, the utility will give an error message and exit.                                                                                                                         |
| `EFI_SOURCE`     | Defines the root directory of a local original EDK source tree, for example C:\EFI2.0\MyEdkDriver.                                                                                                                                                                                                                                                       |
| `EFI_TOOLS_PATH` | Defines the root directory containing the original EDK Tools directory tree, for example C:\Tools.                                                                                                                                                                                                                                                       |
| `EFI_TOOLS_BIN`  | Defines the directory containing the original EDK Tools executable files. Default: `$(EFI_TOOLS_PATH)/Bin`                                                                                                                                                                                                                                               |
| `PROCESSOR`      | Defines the EDK target processor for which the code is to be built. This symbol will typically be used only in EDK INF files, and is ignored for pure EDK II builds.                                                                                                                                                                                     |
| `TARGET_ARCH`    | This defines which of the supported architectures will be built - this value is set in the file: `$(WORKSPACE)/Conf/target.txt` or it may be specified on a command-line.                                                                                                                                                                                |
| `BUILD_DIR`      | Defines the build tip directory for the current platform. For example, this may be `$(OUTPUT_DIR)\Platform\Nt32`.                                                                                                                                                                                                                                        |
| `SOURCE_DIR`     | For a component, defines the directory of the component source files.                                                                                                                                                                                                                                                                                    |
| `DEST_DIR`       | For a component, defines the directory (typically under BUILD_DIR) where the component object files are to be built.                                                                                                                                                                                                                                     |
| `LIB_DIR`        | Specifies the directory where EFI libraries are deposited after building. Default: `$(BUILD_DIR)\$(TARGET)_$(TAGNAME)\$(ARCH)\LIB`                                                                                                                                                                                                                       |
| `BIN_DIR`        | Specifies the directory where final component binaries (.efi) are deposited during build. Default: `$(BUILD_DIR)\$(TARGET)_$(TAGNAME)\$(ARCH)`                                                                                                                                                                                                           |
| `FV_DIR`         | Specifies a WORKSPACE relative or absolute directory where the final image files will be placed at the conclusion of a successful build. If the directory does not exist, the tools must create this directory. If not defined, tools must create an FV directory underneath the BIN_DIR.                                                                |
| `DSC_FILENAME`   | Name of the DSC file as specified on the command line. Can be used for dependencies in the makefiles.                                                                                                                                                                                                                                                    |
| `INF_FILENAME`   | Name of the INF file for a given EDK component or EDK II Module. Can be used for dependencies in the makefiles.                                                                                                                                                                                                                                          |
| `MAKEFILE_NAME`  | Name of the output makefile for the platform. Default is "makefile". The default value can be overridden to support keeping multiple variations of a makefile in the same DEST_DIR directory.                                                                                                                                                            |
