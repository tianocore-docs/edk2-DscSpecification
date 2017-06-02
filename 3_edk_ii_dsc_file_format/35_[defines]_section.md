<!--- @file
  3.5 [Defines] Section

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

## 3.5 [Defines] Section

This is a required section.

#### Summary

Defines the content of the `[Defines]` section used in EDK II DSC files. This
section is created by the developer and is an input to the EDK II parsing
utilities. Elements may appear in any order. Some of the entries in this
section are emitted to the output makefiles.

Entries in this section are global to the both the DSC and FDF files and may be
used as MACRO statement using the form `$(MACRO_NAME)`.

The value of `OUTPUT_DIRECTORY` can be referenced in other meta-data and
makefiles as `$(OUTPUT_DIRECTORY)`. Likewise, `BUILD_NUMBER` can be referenced
in later on in this file (or may be used in files specified in the DSC file) as
`$(BUILD_NUMBER)`.

**********
**Note:** The `OUTPUT_DIRECTORY` value must be either a full path to the
output directory, or a `WORKSPACE` relative path.
**********

Some entries are used by build tools in conjunction with other meta-data files
and command-line options for context sensitive processing. The `BUILD_TARGETS`
value describes the possible build targets that can be built, with
`tools_def.txt`, `target.txt` and command-line options limiting the context of
the current build process. For example, if `BUILD_TARGETS` is set to
`DEBUG|RELEASE`, and the two tool configuration files, `target.txt` and
`tools_def.txt` permit building both of these targets, and no target is
specified on the command-line of the build command, the tool will perform two
builds of the platform, one `DEBUG` followed by a build for `RELEASE`. The two
entries that are scoped in this manner are `SUPPORTED_ARCHITECTURES` and
`BUILD_TARGETS`. These items may be referenced as `$(TARGET)` and `$(ARCH)` in
other meta-data files.

**********
**Note:** Assignments of variables in other sections take precedence over
global assignments.
**********

This revision of specification does not add new features. New EDK II DSC files
must include the statement: `DSC_SPECIFICATION = 0x0001001B` in this section.
Existing DSC files do not need to update the value.

Individual items must appear on a single line, they may not span multiple lines.

Of special note is the `FLASH_DEFINITION` file name. Unlike other file names in
the document, if the value does not include a full path, the file name is
relative to the location of the DSC file and NOT relative to the `WORKSPACE`.
If the `FLASH_DEFINITION` entry is not specified, the build tools assume that
the target output is one or more `UEFI_APPLICATION` modules.

The `VPD_TOOL_GUID` definition must use the registry format GUID of the tool
defined in the `Conf/tools_def.txt` file. If present, the build system will
generate a temporary file and call the tool identified by this GUID value. The
tool must provide two output files, an ordered list of VPD PCDs with offsets
and values, as well as a binary file containing the VPD data. Refer to the _EDK
II Build Specification_ for information about using "VPD PCD Data".

All "reserved" words in the `[Defines]` section can be used in `<MACROVAL>`
format in this file and in the FDF file. For example, $(PLATFORM_NAME) can be
used in processing both this file and the FDF file.

The statements may appear in any order; the order shown in the EBNF below is
only a recommendation.

Use of `<MACROVAL>`, $(MACRO), for values is permitted within this section,
however the macros must be defined before they can be used. It is recommended
to not use macros for the `DSC_SPECIFICATION`, `RFC_LANGUAGES, ISO_LANGUAGES`,
`BUILD_TARGETS` and `SUPPORTED_ARCHITECTURES`.

The `!include` statement may be used in a `[Defines]` section.

#### Prototype

```c
<Defines>      ::= "[Defines]" <EOL>
                   <Statements>*
<Statements>   ::= <TS> "DSC_SPECIFICATION" <Eq> <SpecValue> <EOL>
                   <TS> "PLATFORM_NAME" <Eq> <Word> <EOL>
                   <TS> "PLATFORM_GUID" <Eq> <RegistryFormatGUID> <EOL> <TS>
                   "PLATFORM_VERSION" <Eq> <DecimalVersion> <EOL> [<TS>
                   "SKUID_IDENTIFIER" <Eq> <SkuUiName> <EOL>]
                   <TS> "SUPPORTED_ARCHITECTURES" <Eq> <ArchList> <EOL>
                   <TS> "BUILD_TARGETS" <Eq> <BuildTargets> <EOL> [<TS>
                   "OUTPUT_DIRECTORY" <Eq> <OUTPATH> <EOL>]
                   [<TS> "FLASH_DEFINITION" <Eq> <Filename> <EOL>] [<TS>
                   "BUILD_NUMBER" <Eq> <BuildNumber> <EOL>]
                   [<TS> "RFC_LANGUAGES" <Eq> <Rfc4646List> <EOL>]
                   [<TS> "ISO_LANGUAGES" <Eq> <Iso6392List> <EOL>] [<TS>
                   "TIME_STAMP_FILE" <Eq> <Filename> <EOL>]
                   [<TS> "VPD_TOOL_GUID" <Eq> <RegistryFormatGUID>
                   <EOL>]
                   [<TS> "PCD_VAR_CHECK_GENERATION" <Eq> <TF> <EOL>]
                   [<TS> "PREBUILD" <Eq> {<AsciiString>} {<QuotedString>} <EOL>]
                   [<TS> "POSTBUILD" <Eq> {<AsciiString>} {<QuotedString>} <EOL>]
                   [<TS> <AddressStmts>]
                   <IncludeStatement>*
                   <MacroDefinition>*
<SpecValue>    ::= {<HexVersion>} {(0-9)+ "." (0-9)+}
<SkuUiName>    ::= <Word> [<FS> <Word>]*
<ArchList>     ::= <arch> [<FS> <arch>]*
<AddressStmts> ::= "FIX_LOAD_TOP_MEMORY_ADDRESS" <Eq> <Address> <EOL>
<Address>      ::= <NumValUint64>
<BuildTargets> ::= _Target_ [<FS> _Target_]*
<OUTPATH>      ::= [<AbsolutePath>] <PATH>
<AbsolutePath> ::= [<DosPath>] <FileSep>
<DosPath>      ::= (a-zA-Z) ":"
<BuildNumber>  ::= <NumValUint16>
<Rfc4646List>  ::= <DblQuote> <Rfc4646Code> [<Ext4646>]* <DblQuote>
<Ext4646>      ::= ";" <Rfc4646Code>
<Iso6392List>  ::= <DblQuote> <Iso639-2Code> [<Ext639>]* <DblQuote>
<Ext639>       ::= <Iso639-2Code>
<Rfc4646Code>  ::= RFC4646 Format Language code
                   <Iso639-2
Code>          ::= ISO 639-2 Format Language code
<TF>           ::= {"TRUE"} {"FALSE"}
```

#### Parameters

**_SpecVal_**

New DSC files or DSC files that get updated to use any of the new features
defined in this specification must ensure that the 0x0001001B value is used.
The EDK II build system must maintain backward compatibility, therefore, there
is no requirement to change existing DSC files if no other content changes.
This value may also be specified as a decimal value of 1.27.

**_SkuUiName_**

If specified, the image created from the DSC/FDF file pair will only be valid
for the SkuUiNames listed. If not specified, and the `[SKUIDS]` section is
defined, the image is valid for all SkuIds listed in the `[SKUIDS]` section.

**_RFC4646 Language Code_**

One or more language codes, formatted per RFC4646, using a semi-colon list
separator (example: "en;en-US;es;es-419;fr;fr-FR;zh-Hans-CN".) This list can be
used to filter the output of tools that generate unicode strings. For example,
if the unicode strings file defines 50 different languages for each string, but
the platform integrator only wants to support only three languages, then
specifying the three language codes in this statement will limit the final
output of string parsing tools to strings for these three languages. Tools must
use a "Get Best Language" function when filtering the content. The
`RFC_LANGUAGES` statement must be used when processing EDK II Modules. Space
characters are not permitted within the list.

**_ISO 639-2 Language Code_**

One or more three character language codes, formatted per ISO 639-2, with no
separator character (for example: "engfraspa".) This list can be used to filter
the output of tools that generate unicode strings. Tools must use a "Get Best
Language" function when filtering the content. The `ISO_LANGUAGES` statement
must be used when processing EDK Components. Space characters are not permitted
within the list.

**_BuildNumber_**

This value, if present, will be used during the creation of
`EFI_SECTION_VERSION` sections. Values in this file override any values set in
the INF files. If not present, the EDK II build tools must use a value of `0`.

**_Target_**

All BUILD_TARGET values must be defined in the `Conf/tools_def.txt` file. Three
predefined targets, `NOOPT`, `DEBUG` and `RELEASE` exist, however users may
choose to add their own targets in the `Conf/tools_def.txt` file (e.g., PERF or
SHRINK).

**_FLASH_DEFINITION Filename_**

The FDF filename must be either relative to the directory that contains this
DSC file, or it can be absolute, as well as relative to the WORKSPACE .

#### Example

```ini
[Defines]
  PLATFORM_NAME           = NT32
  PLATFORM_GUID           = EB216561-961F-47EE-9EF9-CA426EF547C2
  PLATFORM_VERSION        = 0.3
  DSC_SPECIFICATION       = 0x0001001B
  OUTPUT_DIRECTORY        = Build/Nt32
  SUPPORTED_ARCHITECTURES = IA32
  BUILD_TARGETS           = DEBUG|RELEASE
  RFC_LANGUAGES           = "en-us;
  zh-hans;fr-fr"
  ISO_LANGUAGES           = "engchnfra"
  SKUID_IDENTIFIER        = SkuTwo|DEFAULT
```
