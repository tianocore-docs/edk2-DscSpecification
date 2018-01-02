<!--- @file
  2.3 [Defines] Section Processing

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

## 2.3 [Defines] Section Processing

The defines section of an EDK II DSC file is used to define variable
assignments that can be used in later build steps.

This section is required in all EDK II DSC files.

The DSC_SPECIFICATION of existing DSC files does not need to be updated unless
content in the file has been updated to match new content specified by this
revision of the specification.

This section must be the first section in the file (following the header
comments) in order to simplify definition of macro statement processing.
Ordering statements within the section is not required, with the exception that
a Macro must be defined before it is used.

The defines section uses the following section definition:

`[Defines]`

The format for entries in this section is:

`Name = Value`

If the `PREBUILD` and/or `POSTBUILD` entries are specified, value must be a
tool that can be executed.  If the value contains space characters, then the
value must be a quoted string. The `PREBUILD` and `POSTBUILD` entry support
multiple arguments, and tool will convert the arguments that are WORKSPACE or
PACKAGES_PATH relative paths to absolute paths. Quotes may be used for arguments
that have spaces or special characters. The `build` tool suspends processing of
the DSC file if the `PREBUILD` entry is present, calls the script, and either
terminates or continues processing the DSC file depending on the exit code from
the script. If the `POSTBUILD` entry is present, prior to the successful `build`
exit, the script is called. If the script fails (non-zero exit code from the
script) `build` terminates immediately using the exit code returned from the
script, otherwise, `build` terminates normally. The author of the script is
responsible for ensuring that the script terminates with a non-zero exit code
when it fails.

All defined elements of the DSC file's `[Defines]` section are valid when
parsing the FDF file. The these elements must be treated as Macros when using
them in other sections of the DSC and FDF file, as in $(PLATFORM_NAME).

The use of the `DEFINE MACRO = Value` statements in this section globally
define the MACRO name during the processing of this file, files included by the
`!include` statement and the FDF file.

**********
**Warning:** The DEFINE MACRO = Value statements in other sections are local to
the section, and override the global definition for entries in the section that
follow the macro definition: `DEFINE MACRO = Value.`
**********

The `EDK_GLOBAL MACRO = Value` definitions in this section globally define the
MACRO name when parsing the DSC, files included by the !include statement, FDF
and EDK INF files.

The EDK II tools will locate the FDF file specified in the `FLASH_DEFINITION`
entry in the same directory as the DSC file. When the PCD_VAR_CHECK_GENERATION
entry is present and set to TRUE, tools will generate a binary file for
DynamicHii and DynamicExHii PCD variable checking.

The following table lists the valid content of this section and whether the
item is required.

###### Table 6 EDK II [Defines] Section Elements


| Typical Tag Names             | Required / Optional | Value                    | Notes                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------------- | ----------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `DSC_SPECIFICATION`           | Required    | 0x0001001B or 1.27               | This entry is required for all EDK II DSC files. The value, 0x0001001B matches the 1.27 version of this specification. Build tools must continue to support DSC files that correspond to earlier versions of the document until such time as earlier versions are no longer in use. In order to maintain backward compatibility, this value must only be updated in existing DSC files if other content in the file is updated. |
|                               |             |                                  | This value may also be specified as decimal value, i.e., 1.27.                                                                                                                                                                                                                                                                                                                                                                  |
| `PLATFORM_GUID`               | Required    | Registry Format GUID(8-4-4-4-12) | The GUID value, along the PLATFORM_VERSION, is used to uniquely identify a platform file. It is recommended that minor changes to the file increment the PLATFORM_VERSION value, and that the GUID value change for completely new platforms.                                                                                                                                                                                   |
| `PLATFORM_VERSION`            | Required    | Integer or Decimal Number        | The Version value, along the PLATFORM_GUID, is used to uniquely identify a platform file. It is recommended that minor changes to the file increment the PLATFORM_VERSION value, and that the GUID value change for completely new platforms.                                                                                                                                                                                   |
| `PLATFORM_NAME`               | Required    | Single Word                      | Only alphanumeric, dash and underscore character are permitted                                                                                                                                                                                                                                                                                                                                                                  |
| `SKUID_IDENTIFIER`            | Required    | Formatted text                   | This value may be passed on the command line and must match one of the defined names in the [SkuIds] section. If it is passed on the command line, the command line value takes precedence.                                                                                                                                                                                                                                     |
| `SUPPORTED_ARCHITECTURES`     | Required    | List                             | Pipe ("<code>&#124;</code>") separated list of architectures that the platform supports                                                                                                                                                                                                                                                                                                                                                           |
| `BUILD_TARGETS`               | Required    | List                             | Pipe ("<code>&#124;</code>") separated list of build targets (that are defined in the tools_def.txt file)                                                                                                                                                                                                                                                                                                                                         |
| `OUTPUT_DIRECTORY`            | Optional    | Directory                        | Either a `WORKSPACE` relative or absolute directory location. The default location is: `$(WORKSPACE)/Build/PlatformName`                                                                                                                                                                                                                                                                                                        |
| `FLASH_DEFINITION`            | Optional    | Filename                         | The Filename (FDF) that contains the Flash Part Definition information. It is recommended that the file name be relative to the directory containing the DSC file, however, it is possible to use an absolute path, a path relative to the directory containing the DSC file.                                                                                                                                                   |
| `BUILD_NUMBER`                | Optional    | Up to four digit numbers         | Set this value in the generated Makefile.                                                                                                                                                                                                                                                                                                                                                                                       |
| `FIX_LOAD_TOP_MEMORY_ADDRESS` | Optional    | Address                          | The top memory address - the starting location in memory for all drivers, application loading. When it is not set, or set to 0, the load fixed address feature will be disabled. When it is set to 0xFFFFFFFFFFFFFFFF, enable the feature as fixed offset to TOLM. When it is set to the positive address, enable the feature as fixed address.                                                                                 |
| `TIME_STAMP_FILE`             | Optional    | Filename                         | The timestamp file contains a timestamp that will be used to set the creation timestamp on all created files. If this file is specified, it will be used to adjust the timestamp of created files, if it does not exist at the start of a build, the file will be created, using the current date and time.                                                                                                                     |
|                               |             |                                  | If this variable is not specified, the time and date of the start of the build are used by the EDK II tools that modify the time/date portion of a PE32/PE32+/Coff header. This file's path is either relative to the directory containing the DSC file or a `WORKSPACE`[^1] relative path followed by the file name.                                                                                                           |
| `DEFINE`                      | Optional    | MACRO = PATH or Value            | A name that is assigned to either a path or a value. This statement can be used to make the DSC file more readable, as in: `DEFINE MDE = MdePkg/Library` Then, later, `$(MDE)/BaseLib/ BaseLib.inf`                                                                                                                                                                                                                             |
| `EDK_GLOBAL`                  | Optional    | MACRO = PATH or Value            | Similar to the DEFINE statement, macros defined using this keyword are only valid when processing EDK ibraries and components. These values are ignored during processing of EDK II modules.                                                                                                                                                                                                                                    |
| `RFC_LANGUAGES`               | Optional    | RFC4646 Language code list       | A semi-colon ";" separated list of RFC4646 Language codes (EDK II Modules) used during the generation of only a set, rather than all, UNICODE languages during the StrGather AutoGen phase. The list must be encapsulated in double quotes.                                                                                                                                                                                     |
| `ISO_LANGUAGES`               | Optional    | ISO-639-2 Language code list     | A non-separated list of three character ISO 639-2 Language codes (EDK Components) used during the generation of only a set, rather than all, UNICODE languages during the StrGather AutoGen phase. The list must be encapsulated in double quotes.                                                                                                                                                                              |
| `VPD_TOOL_GUID`               | Optional    | Registry Format GUID             | When this element is present, the build process will be interrupted during the AutoGen stage in order to call an external program, named by GUID that must also be defined in the Conf/tools_def.txt file using a tool code name of VPDTOOL. Refer to the EDK II Build specification for additional information.                                                                                                                |
| `PCD_INFO_GENERATION`         | Optional    | TRUE or FALSE                    | If present, and set to TRUE, this flag will generate PCD information in the Pcd Database.                                                                                                                                                                                                                                                                                                                                       |
| `PCD_VAR_CHECK_GENERATION`    | Optional    | TRUE or FALSE                    | If present and set to TRUE, this flag will generate the variable validation table binary file in the build output FV floder. If not present ro set to FALSE, then the binary file will not be generated.                                                                                                                                                                                                                        |
| `PREBUILD`                    | Optional    | String of script with arguments  | If present, build tool will call the script of this statement to execute.                                                                                                                                                                                                                                                                                                                                                       |
| `POSTBUILD`                   | Optional    | String of script with arguments  | If present, build tool will call the script of this statement to execute.                                                                                                                                                                                                                                                                                                                                                       |

[^1]: WORKSPACE refers to the combination of the directories specified in the
WORKSPACE system environment variable and the PACKAGES_PATH system environment
variable.

**********
**Note:** EDK II Modules can have unicode string files that contain RFC4646
language codes. EDK II modules cannot have unicode string files that contain
ISO-629-2 language codes. USI-629-2 language codes are only valid for EDK
components. The format of the statement is specific to processing RFC4646
language code lists.
**********
