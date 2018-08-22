<!--- @file
  3.6 [BuildOptions] Sections

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

## 3.6 [BuildOptions] Sections

The `[BuildOptions]` sections are optional in all EDK II DSC Files.

#### Summary

Use of the `!include` statement in `[BuildOptions]` sections is permitted,
though not recommended.

Macro Values that have been previously defined in other sections of this
document may be used in `[BuildOptions]` sections. Of special note, a
`<MACROVAL>` element within a quoted string is not expanded by the tools. The
`<MACRO>` in the `<MACROVAL>` element within a quoted string that is a system
environment variable or a pre-defined "Wellknown Macro" (see _Macro
Statements_, _Section 2.2.6_) does not need to be defined in this file, as the
expectation is that other tools will be responsible for expanding the macro.

This section is used to replace flags or append flags to the end of the tool
code flags defined in the `tools_def.txt` file. The `Family` tag can be used
for elements that are shared between different architectures, and different
tool chain tag names.

The `[BuildOptions]` section modifier, CodeBase, (value of EDK or EDKII,
default is EDKII) allows for platform integrators to override default build
options set in the `tools_def.txt` file scoped according to the type of INF
file being processed. EDK II INF files all contain an `INF_VERSION` element in
their `[Defines]` section, while EDK libraries and components do not have the
element. The `[BuildOptions]` section of an INF file override both the
`tools_def.txt` options and the options set in the `[BuildOptions]` section. In
order to override options set in the INF file, the options must be overridden
using the INF scoped `<BuildOptions>` tag after an INF file specified in the
`[Components]` section.

Items in these sections are either appended or replace options specified
earlier.

Build options priority (appended from lowest to highest and/or highest
replacement) is:

* Highest, DSC file's component scoped `<BuildOptions>` for individual INF
  files.
* `[BuildOptions.$(arch).CodeBase.ModuleType]`
* `[BuildOptions.$(arch).CodeBase]`
* `[BuildOptions.common.CodeBase]`
* `[BuildOptions.$(arch)]`
* `[BuildOptions.common]`
* `[BuildOptions]`
* INF File's `[BuildOptions]` section
* Lowest - `tools_def.txt` entry

An example using the Family tag follows:

`MSFT:*_*_*_CC_FLAGS = /D MDEPKG_NDEBUG`

An optional, special Family tag can be used at the start of the command line,
using a colon ":" character after the `Family` tag. If not specified,
specific tool chain tags must be specified (the use of the asterisk "*" wild
card character is not permitted.)

**********
**Note:** The following is an example which does not use the Family tag, and
specific to a specific tool chain tag name:
**********

`RELEASE_MYTOOLS_IA32_CC_FLAGS = /nologo /c /WX /GS- /W4 /Gs8192 /Gy`

Two types of assignment operators are permitted, the single equal "=" sign is
used to append the string to the existing definition, while the double equal
"==" sign is used to override any previous definition, replacing the content
with just the string. For example, given:

```ini
[BuildOptions]
  RELEASE_MYTOOLS_IA32_CC_FLAGS = /nologo /c /WX /GS- /W4 /Gs8192 /Gy
```

Then the following was found in a higher priority section, as in:

```ini
[BuildOptions.common.EDKII]
  RELEASE_MYTOOLS_IA32_CC_FLAGS == /nologo /c /WX /GS- /W4
```

The logical results "clear" the /Gs8192 and /Gy flags - no other flags from the
tools_def.txt or other `[BuildOptions]` sections will apply - as in:

`RELEASE_MYTOOLS_IA32_CC_FLAGS == /nologo /c /WX /GS- /W4`

However, if a module as a `<BuildOptions>` sub-section defined, as in:

```ini
MyModule.inf {
  <BuildOptions>
    RELEASE_MYTOOLS_IA32_CC_FLAGS = /D EFI_DEBUG
}
```

Then, the logical result for building just my module would be:

`RELEASE_MYTOOLS_IA32_CC_FLAGS == /nologo /c /WX /GS- /W4 /D EFI_DEBUG`

Other modules without the `<BuildOptions>` sub-section would have the logical:

`RELEASE_MYTOOLS_IA32_CC_FLAGS == /nologo /c /WX /GS- /W4`

When one or more architectural modifiers are used in the section tag, as in
`[BuildOptions.IA32, BuildOptions.X64]`, the option lines will be sorted such
that the common lines appear in both IA32 sub-directory and X64 sub-directory
makefiles. Furthermore, using the architectural modifiers in the `<TargetArch>`
field of the `<ToolSpec>` listed below, additional sorting will be done.

Sections that have identical names are appended to each other in the order that
they appear in the file. Sections with modifiers are logically appended,
starting with the common, then (for each architecture) the architectural
section, followed by sections that use a code-base modifier.

Assume that `tools_def.txt` defines the following:

`*_*_*_TEST_FLAGS = /a`

Then,

```ini
[BuildOptions.common]
  # entries in this section are append to or replace items that
  # may have been defined in the tools_def.txt file.
  *_*_*_TEST_FLAGS = /b
```

The result would logically be: `*_*_*_TEST_FLAGS = /a /b`

```ini
[BuildOptions.common.EDKII]
  # Entries in this field are appended for EDK II modules only
  *_*_*_TEST_FLAGS = /c
```

The result for EDK II modules would be: `*_*_*_TEST_FLAGS = /a /b /c`

```ini
[BuildOptions.common.EDK]
  # Entries are for EDK components and libraries
  *_*_*_TEST_FLAGS = /d
```

The result for EDK components and libraries would be: `*_*_*_TEST_FLAGS = /a /b /d`

```ini
[BuildOptions.IA32]
  # Architectural options for IA32
  *_*_*_TEST_FLAGS = /e
```

The logical result is: `*_*_IA32_TEST_FLAGS = /a /b /c /e`

```ini
[BuildOptions.X64.EDK]
  # Architectural options for X64
  *_*_*_TEST_FLAGS       = /f
  DEBUG_*_*_TEST_FLAGS   = /g
  RELEASE_*_*_TEST_FLAGS = /h
```

The logical result is two sets of flags: `DEBUG_*_X64_TEST_FLAGS = /a /b /d /f /g`
and `RELEASE_*_X64_TEST_FLAGS = /a /b /d /f /h`

The wildcard character "*" is permitted in the Target, Tagname and TargetArch
fields of the `<ToolSpec>`. Specifying the wildcard character means that any
value can be substituted. The values of the well-known macros, `$(TARGET)`,
`$(TOOL_CHAIN_TAG)` and `$(ARCH)` will be used by the tool in place of the
wildcard character by the tools. If the platform build uses multiple
architectures, the build tools will automatically sort the statements, putting
statements into the appropriate makefiles.

Entries in `[BuildOptions]` sections are cumulative (as opposed to last line
takes precedence in other section of the DSC file) provided the "=="
character sequence is not used in statement later in the section. For example,
given the two following lines in a section:

```
*_*_*_TEST_FLAGS = /e
*_*_*_TEST_FLAGS = /f
```

The logical result is:

`*_*_*_TEST_FLAGS = /e /f`

#### Prototype

```c
<BuildOptions> ::= "[BuildOptions" [<attribs>] "]" <EOL> <Statements>*
<attribs>      ::= "." <arch> [<CodeBase> ["." <ModuleType>]]
<CodeBase>     ::= "." {"Common"} {"EDK"} {"EDKII"}
<Statements>   ::= {<MacroDefinition>} {<IncludeStatement>}
                   {<TS> <BStatement>}
<BStatement>   ::= {<ToolFlag>} {<ToolPath>} {<ToolCmd>} {<Other>}
<ToolFlag>     ::= [<Family> ":"] <FlagSpec> <Equal> <Flags> <EOL>
<ToolPath>     ::= [<Family> ":"] <PathSpec> <Equal> <PATH> <EOL>
<ToolCmd>      ::= [<Family> ":"] <CmdSpec> <ReplaceEq>
                   <ExecCmd> <EOL>
<Other>        ::= [<Family> ":"] <OtherSpec> <Equal> <String> <EOL>
<Equal>        ::= {<AppendEq>} {<ReplaceEq>}
<AppendEq>     ::= <Eq>
<ReplaceEq>    ::= <TS> "==" <TS>
<Family>       ::= _Family_
<ToolSpec>     ::= <Target> "_" <TagName> "_" <TargetArch> "_" <ToolCode>
<FlagSpec>     ::= <ToolSpec> "_FLAGS"
<PathSpec>     ::= <ToolSpec> "_DPATH"
<CmdSpec>      ::= <ToolSpec> "_PATH"
<OtherSpec>    ::= <ToolSpec> "_" <Attribute>
<TargetArch>   ::= _TargetArch_
<Target>       ::= _Target_
<Attribute>    ::= _AttributeName_
<TagName>      ::= _TagName_
<ToolCode>     ::= _ToolCode_
<Flags>        ::= _FlagString_
<ExecCmd>      ::= [<PATH>] <Filename>
```

#### Parameters

**_Family_**

Must match a `FAMILY` name defined in the EDK II `tools_def.txt` file. If not
present, then the entry is valid for all tool chain families.

**_Target_**

Must match a target identifier in the EDK II `tools_def.txt` file - the first
field, where fields are separated by the underscore character. The "*"
character is a valid wildcard.

**_TagName_**

Must match a tag name field in the EDK II `tools_def.txt` file - second field.
The "*" character is a valid wild-card.

**_TargetArch_**

Must match the architecture field in the EDK II `tools_def.txt` file - third
field. The "*" character is a valid wild-card.

**_ToolCode_**

Must match a tool code field in the EDK II `tools_def.txt` file - fourth field.
Use of a wild-card character is not permitted.

**_AttributeName_**

Must match a tool attribute field in the EDK II `tools_def.txt` file - fifth
field. Use of a wild-card character is not permitted. The attributes, `_Flag`,
`_PATH` and `_DPATH` are defined elsewhere and cannot be used with the
`<OtherSpec>` definition.

**_FlagString_**

Must be a valid string for the tool specified. The string will be appended to
the end of the tool's flags (from the `tools_def.txt`). Both Microsoft and GCC
evaluate options from left to right on the command line. This allows disabling
some flags that may have been specified in the `tools_def.txt` by providing an
alternate flag, i.e., if the `tools_def.txt` file's `CC_FLAGS` defines /O2 and
an /O1 options is specified for this module, the module will compile with `/O1`
(size) not with /O2 (speed). Use of the quote characters around options is
required when specifying string values with spaces, path names with spaces or
values containing the hash "#" character not within a string. Note that a
macro named MDEPKG_NDEBUG is reserved for size reduction purposes. The user
must not use this keyword to define a new macro.

**_ExecCmd_**

The filename of an executable. If the executable can be located under a
directory specified in the system environment `PATH` variable, only the
filename is required. Otherwise, the filename must be relative to a directory
in either the WORKSPACE or PACKAGES_PATH environment variable or an absolute
path must be given. If an absolute path is used, the build system will fail the
build if the executable cannot be found.

#### Example

```ini
[BuildOptions]
  *_WINDDK3790x1830_*_CC_FLAGS = /Qwd1418,810
  *_MYTOOLS_*_CC_FLAGS         = /Qwd1418,810
  *_VS2003_*_CC_FLAGS          = /wd4244
  *_WINDDK3790x1830_*_CC_FLAGS = /wd4244
  *_MYTOOLS_*_CC_FLAGS         = /wd4244
  # Flags used by Dynamic linker
  DEBUG_WINDDK3790x1830_IA32_DLINK_FLAGS = /EXPORT:InitializeDriver=_ModuleEntryPoint /ALIGN:4096 /SUBSYSTEM:CONSOLE
```
