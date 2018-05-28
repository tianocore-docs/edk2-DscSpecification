<!--- @file
  2.2 Build Description File Format

  Copyright (c) 2006-2018, Intel Corporation. All rights reserved.<BR>

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

## 2.2 Build Description File Format

EDK II build description files--DSC, FDF, DEC and INF files, along with other
files like build_rules.txt, target.txt and tools_def.txt, contain information
used by the parsing utility to create makefiles that process source files to
generate binary (PE32/PE32+/Coff) files. The binary files can be distributed as
EDK II binary packages or used to create a platform are defined in an FDF file,
rather than the EDK II DSC file unless the binary modules included in a platform
use PCDs.

### 2.2.1 Section Entries

To simplify parsing, the EDK II meta-data files continue using the INI format.
This style was introduced for EDK meta-data files, when only the Windows tool
chains were supported. It was decided that for compatibility purposes, that INI
format would continue to be used. EDK II formats differ from the de facto INI
format in that the semicolon ";" character cannot be used to indicate a
comment.

Leading and trailing space/tab characters must be ignored.

Duplicate section names must be merged by tools.

This description file consists of sections delineated by section tags enclosed
within square `[]` brackets. Section tag entries are case-insensitive. The
different sections and their usage are described below. The text of a given
section can be used for multiple section names by separating the section names
with a comma. For example:

`[LibraryClasses.X64, LibraryClasses.ipf]`

The content below each section heading is processed by the parsing utilities in
the order that they occur in the file. The precedence for processing these
architecture section tags is from right to left, with sections defining an
architecture having a higher precedence than a section which uses common (or no
architecture extension) as the architecture.

**********
**Note:** Content, such as filenames, directory names, macros and C variable
names, within a section IS case sensitive. IA32, Ia32 and ia32 within a section
are processed as separate items. (Refer to Naming Conventions below for more
information on directory and/or file naming.)
**********

Sections are terminated by the start of another section or the end of the file.

Comments are not permitted between square brackets of a section specifier.

Duplicate sections (two sections with identical section tags) will be merged by
tools, with the second section appended to the first.

If architectural modifiers are used in the section tag, the section is merged
by tools with content from common sections (if specified) with the
architectural section appended to the first, into an architectural section. For
example, given the following:

```ini
[BuildOptions]
  MSFT:*_*_*_CC_FLAGS = /nologo

[BuildOptions.IA32]
  MSFT:*_*_IA32_CC_FLAGS = /D MDEPKG_NDEBUG

[BuildOptions.X64]
  MSFT:*_*_X64_CC_FLAGS = /Gy
```

After the first pass of the tools, when building the module for IA32, the
source files will logically be:

```ini
[BuildOptions.IA32]
  MSFT:*_*_*_CC_FLAGS    = /nologo
  MSFT:*_*_IA32_CC_FLAGS = /D MDEPKG_NDEBUG
```

When building the module for X64, the source files will logically be:

```ini
[BuildOptions.X64]
  MSFT:*_*_*_CC_FLAGS   = /nologo
  MSFT:*_*_X64_CC_FLAGS = /Gy
```

The `[Defines]` section tag prohibits use of architectural modifiers. All other
sections can specify architectural modifiers.

### 2.2.2 Comments

The hash "#" character indicates comments in the Platform Description (DSC)
file. In line comments terminate the processing of a line. In line comments
must be placed at the end of the line, and may not be placed within the section
`[]` tags.

Only `FIX_LOAD_TOP_MEMORY_ADDRESS = 0xF0000000` in the following example is
processed by tools; the remainder of the line is ignored:

`FIX_LOAD_TOP_MEMORY_ADDRESS = 0xF0000000 # set top memory address`

**********
**Note:** Blank lines and lines that start with the hash "#" character
must be ignored by tools.
**********

Hash characters appearing within a quoted string are permitted, with the string
being processed as a single entity. The following example must handle the
quoted string as single element by tools.

`UI = "# Copyright 2007, NoSuch, ltd. All rights reserved."`

### 2.2.3 Valid Entries

Processing of the line is terminated if a comment is encountered.

Processing of a line is terminated by the end of the line.

Items in quotation marks are treated as a single token and have the highest
precedence. Items encapsulated in parenthesis are also treated as tokens, with
embedded tokens being processed first. All other processing occurs from left to
right.

In the following example, B - C is processed first, then result is added to A
followed by adding 2; finally 3 is added to the result.

`(A + (B - C) + 2) + 3`

In the next example, A + B is processed first, then C + D is processed and
finally the two results are added.

`(A + B) + (C + D)`

Space and tab characters are permitted around field separators.

### 2.2.4 Naming Conventions

The EDK II build infrastructure is supported under Microsoft* Windows*, Linux*
and MAC OS/X operating systems. All directory and file names must be treated as
case sensitive because of multiple environment support.

* The use of special characters in directory names and file names is restricted
  to the dash, underscore, and period characters, respectively "-", "_", and
  ".".

* Period characters may not be followed by another period character. File and
  Directory names must not start with "./", "." or "..".

* Directory names and file names must not contain space characters.

* Directory Names must only contain alphanumeric characters, the dash and
  underscore characters and start with an alpha character. A single period
  character is permitted in the name provided that alphanumeric characters
  appear before and after the period character (as in: _MdePkg.1_).

* Additionally, all EDK II directories that are architecturally dependent must
  use a name with only the first character capitalized. _Ia32_, _Ipf_, _X64_
  and _Ebc_ are valid architectural directory names. IA32, IPF and EBC are not
  acceptable directory names, and may cause build breaks. From a build tools
  perspective, IA32 is not equivalent to Ia32 or ia32.

The build tools must be able to process the tool definitions file:
`tools_def.txt` (describing the location and flags for compiler and user
defined tools), which may contain space characters in paths on Windows* systems.

**********
**Note:** The toolsdef.txt file and `[BuildOptions]` sections are the
places that permit the use of space characters in a directory path.
**********

The EDK II Coding Style specification covers naming conventions for use within
C Code files, and as well as specifying the rules for directory and file names.
This section is meant to highlight those rules as they apply to the content of
the INF files.

Architecture keywords (`IA32`, `IPF`, `X64` and `EBC`) are used by build tools
and in metadata files for describing alternate threads for processing of files.
These keywords must not be used for describing directory paths. Additionally,
directory names with architectural names (_Ia32_, _Ipf_, _X64_ and _Ebc_) do
not automatically cause the build tools or meta-data files to follow these
alternate paths. Directories and Architectural Keywords are similar in name
only.

All directory paths within EDK II INF files must use the "/" forward slash
character to separate directories as well as directories from filenames.
Example:

`C:/Work/Edk2/edksetup.bat`

File names must also follow the same naming convention required for
directories. No space characters are permitted. The special characters
permitted in directory names are the only special characters permitted in file
names.

### 2.2.5 !include Statement Processing

The `!include` statement may appear within any section of EDK II DSC file. The
included file content must match the content type of the current section
definition, contain complete sections, or combination of both.

The argument of this statement is a filename. The file is relative to the
directory that contains this DSC file, and if not found the tool must attempt
to find the file relative to the paths listed in the system environment
variables `$(WORKSPACE)`, `$(EFI_SOURCE)`, `$(EDK_SOURCE)`, and
`$(ECP_SOURCE)`. If the file is still not found, the parsing tools must
terminate with an error.

Macros, defined in this file, are permitted in the path or file name of the
!include statement, as these files are included prior to processing the file
for macros. The system environment variables `$(WORKSPACE)`, `$(EDK_SOURCE)`,
`$(EFI_SOURCE)`, and `$(ECP_SOURCE)` may also be used; only these system
environment variables are permitted to start the path of the included file.

Statements in `!include` files must not break the integrity of the DSC file,
the included file is read in by tools in the exact position of the file, and is
functionally equivalent of copying the contents of the included file and
inserting (paste) the content into the DSC file.

The following examples show the valid usage of the `!include` statement.

```ini
[LibraryClasses]
  BaseLib|MdePkg/Library/BaseLib/BaseLib.inf
  !include MyPlatformCommonLibs.txt

[LibraryClasses]
  DEFINE MDELIBUAEP = MdePkg/Library/UefiApplicationEntryPoint
  UefiApplicationEntryPoint|$(MDELIBUAEP)/UefiApplicationEntryPoint.inf
  !include Sample.txt

  ### Contents of Sample.txt
  DEFINE EMULATE = 1

  !if $(EMULATE) == 0
  [LibraryClasses.IA32]
    TimerLib|Some/Existing/TimerLib/Instance.inf

  [LibraryClasses.X64]
    TimerLib|Another/Existing/TimerLib/Instance.inf
  !else
    TimerLib|The/NULL/TimerLib/Instance.inf
  !endif
```

### 2.2.6 Macro Statements

The use of MACRO statements, which assign a value to a variable. Macros defined
in the `[Defines]` section are considered global during the processing of the
DSC file and the FDF file. This means that a Macro can be used in the FDF file
without defining it in the FDF as long as it is defined in the DSC file.

Token names (words defined in the EDK II meta-data file specifications) cannot
be used as macro names. As an example, using PLATFORM_NAME as a macro name is
not permitted, as it is a token defined in the DSC file's `[Defines]` section.

Macros in the DSC file can be used to specify paths (and paths and filenames),
and build options. They can define other items, such as values for PCDs,
expressions or names that can be used in conditional directive statements,
which allows customization of the build, allowing the platform integrator to
select features from the command-line.

The format for the macro statements is:

`DEFINE MACRO = Path`

Any portion on a path or path and filename can be defined by a macro.

When assigning a string value to a macro, the string must follow the C format
for specifying a string, as shown below:

```
DEFINE MACRO1 = "SETUP"
DEFINE MACRO2 = L"SETUP"
```

When assigning a numeric value to a macro, the number may be a decimal, integer
or hex value, as shown below:

```
DEFINE MACRO1 = 0xFFFFFFFF
DEFINE MACRO2 = 2.3
DEFINE MACRO3 = 10
```

The format for usage of a Macro varies. When used as a value, the Macro name
must be encapsulated by "$(" and ")" as shown below:

`$(MACRO)/filename.foo`

When a macro is tested in a conditional directive statement, determining
whether it has been define or undefined uses the following format:

`!ifdef MACRO`

**********
**Note:** For backward compatibility, tools may allow `$(MACRO)` in the
`!ifdef` and `!ifndef` statements. This functionality may disappear in
future releases, therefore, it is recommended that platform integrators update
their DSC files if they also alter other content.
**********

When using string comparisons of Macro elements to string literals, the format
of the conditional directive must be:

`!if $(MACRO) == "Literal String"`

**********
**Note:** For backward compatibility, tools may allow testing literal strings
that are not encapsulated by double quotation marks. This functionality may
disappear in future releases, therefore, it is recommended that platform
integrators update their DSC files if they also alter other content.
**********

When testing Macro against another Macro:

`!if $(MACROALPHA) == $(MACROBETA)`

When testing a Macro against a value:

`!if $(MACRONUM) == 2`

or

`!if $(MACROBOOL) == TRUE`

When used with the `!if` or `!elseif` statements or in an expression used in a
value field, a macro that has not been defined has a value of `0`.

Any defined MACRO definitions will be expanded by tools when they encounter the
entry in the section except when the macro is within double quotation marks in
build options sections. The expectation is that macros in the quoted values
will be expanded by external build scripting tools, such as nmake or gmake;
they will not be expanded by the build tools. If a macro that is not defined is
used in locations that are not expressions (where the tools would just do macro
expansion as in C flags in a `[BuildOptions]` section), nothing will be
emitted. If the macro, MACRO1, has not been defined, then:

`MSFT:*_*_*_CC_FLAGS = /c /nologo $(MACRO1) /Od`

After macro expansion, the logical result would be equal to:

`MSFT:*_*_*_CC_FLAGS = /c /nologo /Od`

It is recommended that tools remove any excess space characters when processing
these types of lines.

Macro evaluation is done at the time the macro is used in an expression,
conditional directive or value field, not when a macro is defined. Macros in
quoted strings will not be expanded by parsing tools; all other macro values
will be expanded, without evaluation, as other elements of the build system
will perform any needed tests. Macro Definition statements that appear within a
section of the file (other than the `[Defines]` section) are scoped to the
section they are defined in. If the Macro statement is within the `[Defines]`
section, then the Macro is considered global to the entire DSC file, files
included using the !include statement and global to the FDF file, with local
definitions taking precedence (if the same MACRO name is redefined in
subsequent sections, then that MACRO value is local to only that section.)

Macros may be used in conditional statements located within the DSC (and FDF)
file. Conditional MACROs may be defined on a command line of a parsing tool. It
is highly recommended that a macro defined in this manner have a `DEFINE`
statement to set a default value in the `[Defines]` section. (Macro values
specified on the command-line override all definitions of that Macro.) In the
reference build (Nt32Pkg/Nt32Pkg.dsc), macros set on a command line override
any macro value defined in the DSC (or FDF) file.

MACROs may also be used as values in PCD statements. See _Section 3.10_ for
more information on PCD statements.

In order to support EDK components and libraries, the word `DEFINE` is replaced
with `EDK_GLOBAL`. The `EDK_GLOBAL` macros are considered global during the
processing of the DSC, FDF and EDK INF files.

Macros that appear within double quotation marks in build options sections are
not expanded. It is assumed that they will be expanded by the OS or external
scripting tools.

Global variables that may be used in EDK II DSC and FDF meta-data files are
listed in the _Well-known Macro Statements_ table, while the format of the
System Environment variables that may be used in EDK II DSC and FDF files are
in the next table.

###### Table 2 Well-known Macro Statements

| Exact Notation        | Derivation                                                                                                                                                                                                                                                                                                                   |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$(OUTPUT_DIRECTORY)` | Used in FDF `[FV]` and `[Capsule]` sections; the value comes from parsing either the DSC file or via a command line option. This is commonly the Build/Platform name directory created by the build system in the EDK II WORKSPACE, however, it is possible to specify the output directory outside of the EDK II WORKSPACE. |
| `$(TARGET)`           | Used in various locations; valid values are derived from INF, DSC, target.txt, tool options and tools_def.txt. FDF parsing tools may obtain these values from command-line options.                                                                                                                                          |
| `$(TOOL_CHAIN_TAG)`   | Used in various locations; valid values are derived from INF, DSC, target.txt, tool options and tools_def.txt. FDF parsing tools may obtain these values from command-line options.                                                                                                                                          |
| `$(TOOLCHAIN)`        | A synonym for $(TOOL_CHAIN_TAG), with the value derived from INF, DSC, target.txt and tools_def.txt.                                                                                                                                                                                                                         |
|                       | This item has been deprecated, and must not be used.                                                                                                                                                                                                                                                                         |
| `$(ARCH)`             | Used in various locations, valid values are derived from INF, DSC, target.txt, tool options and tools_def.txt. FDF parsing tools may obtain these values from command-line options.                                                                                                                                          |

System environment variables may be referenced, however their values must not
be altered.

###### Table 3 Using System Environment Variable

| Macro Style Used in Meta-Data Files | Matches Windows Environment Variable | Matches Linux & OS/X Environment Variable |
| ----------------------------------- | ------------------------------------ | ----------------------------------------- |
| $(WORKSPACE)                        | %WORKSPACE%                          | $WORKSPACE                                |
| $(EFI_SOURCE)                       | %EFI_SOURCE%                         | $EFI_SOURCE                               |
| $(EDK_SOURCE)                       | %EDK_SOURCE%                         | $EDK_SOURCE                               |
| $(ECP_SOURCE)                       | %ECP_SOURCE%                         | $ECP_SOURCE                               |
| $(EDK_TOOLS_PATH)                   | %EDK_TOOLS_PATH%                     | $EDK_TOOLS_PATH                           |

The system environment variables, PACKAGES_PATH and EDK_TOOLS_BIN, are not
permitted in EDK II meta-data files.

Macros defined in the DSC file's `[Defines]` section may be used within an FDF
file. They are also positional in nature, with later definitions overriding
previous definitions for the remainder of the file.

Additionally, the macros defined in the DSC file may be used in conditional
directive statements located within the DSC and FDF files. Macros in the DSC
file can be used for file names, paths, PCD values, in the `[BuildOptions]`
section, on the right (value) side of the statements and in conditional
directives. Macros can also be defined or used in the `[Defines]`,
`[LibraryClasses]`, `[Libraries]`, `[Components]` and all PCD sections.

Macros defined by the user cannot be used in the `!include` statements in
either the DSC or FDF file.

Macros defined in common sections may be used in the architecturally modified
sections of the same section type. Macros defined in architectural sections
cannot be used in other architectural sections, nor can they be used in the
common section. Section modifiers in addition to the architectural modifier
follow the same rules as architectural modifiers.

#### Example

```ini
[LibraryClasses.common]
  DEFINE MDE = MdePkg/Library
  BaseLib|$(MDE)/BaseLib.inf

[LibraryClasses.X64, LibraryClasses.IA32]
  # Can use $(MDE), cannot use $(MDEMEM)
  DEFINE PERF = PerformancePkg/Library
  TimerLib|$(PERF)/DxeTscTimerLib/DxeTscTimerLib.inf

[LibraryClasses.X64.PEIM]
  # Can use $(MDE) and $(PERF)
  DEFINE MDEMEM = $(MDE)/PeiMemoryAllocationLib
  MemoryAllocationLib|$(MDEMEM)/PeiMemoryAllocationLib.inf

[LibraryClasses.IPF]
  # Cannot use $(PERF) or $(MDEMEM)
  # Can use $(MDE) from the common section
  PalLib|$(MDE)/UefiPalLib/UefiPalLib.inf
  TimerLib|$(MDE)/BaseTimerLibNullTemplate/BaseTimerLibNullTemplate.inf
```

### 2.2.7 EDK_GLOBAL

The `EDK_GLOBAL` statements defined in the DSC file can be used during the
processing of the DSC, FDF and EDK INF files. The definition of the
`EDK_GLOBAL` name must only be done in the DSC `[Defines]` section. These
special macros can be used in path statements, DSC file `[BuildOptions]` and
FDF file `[Rule]` sections. These statements are used to replace the
environment variables that were defined for the EDK build tools. They must
never be used in a conditional directive statement in the DSC and FDF files,
nor can they be used by EDK II INF files.

### 2.2.8 Conditional Directive Statements (!if...)

Conditional directive statements are used by the build tools preprocessor
function to include or exclude statements in the DSC file. A limited number of
statements are supported, and nesting of conditionals is also supported.
Statements are prefixed by the exclamation "!" character. Conditional
statements may appear anywhere within the DSC file.

**********
**Note:** A limited number of statements are supported. This specification does
not support every conditional statement that C programmers are familiar with.
**********

Supported statements are:

`!ifdef, !ifndef, !if, !elseif, !else and !endif`

Refer to the Macro Statement section for information on using Macros in
conditional directives.

When using the `!ifdef` or `!ifndef`, the macro name can be used; the macro
name must not be encapsulated between `$(` and `)`. It is the name of the macro
that is used for testing, not the value of the macro.

PCDs must not be used in the `!ifdef` or `!ifndef` statements.

**********
**Note:** For backward compatibility, the EDK II build system will also process
the `!ifdef` or `!ifndef` statements with the macro encapsulated between
"$(" and ")".
**********

When using a marco in the `!if` or `!elseif` conditionals, the macro name must
be encapsulated between `$(` and `)`.

A macro that is not defined has a default value of `0` (`FALSE`) when used in a
conditional comparison statement.

When using a PCD in the `!if` or `!elseif` conditionals, the PCD name
(`TokenSpaceGuidCName.PcdCname`) can be used; the PCD name must not be
encapsulated between "$(" and ")" nor between "PCD(" and ")". Do not
encapsulate the PCD name in the "$(" and ")" required for macro values as shown
in the example below.

```
!if ( gTokenSpaceGuid.PcdCname == 1 ) AND ( $(MY_MACRO) == TRUE )
DEFINE FOO=TRUE
!endif
```

If the PCD is a string, only the string needs to be encapsulated by double
quotation marks, while a Unicode string can have the double quoted string
prefixed by "L", as in the following example:

```
!if gTokenSpaceGuid.PcdCname == L"Setup"
DEFINE FOO=TRUE
!endif
```

If a PCD is used in a conditional statement, the value must first come from the
FDF file, then from the DSC file. If the value cannot be determined from these
two locations, the build system should break with an error message.

**********
**Note:** PCDs, used in conditional directives, must be defined and the value
set in either the FDF or DSC file in order to be used in a conditional
statement; values from INF or DEC files are not permitted.
**********

When used in `!if` and `!elseif` conditional comparison statements, it is the
value of the Macro or the PCD that is used for testing, not the name of the
macro or PCD.

Strings can only be compared to strings of a like type (testing an ASCII string
against a Unicode format string must fail), numbers can only be compared
against numbers and boolean objects can only evaluate to `TRUE` or `FALSE`. See
the Operator Precedence table, in the Expressions section below for a list of
restrictions on comparisons.

Using macros in conditional directives that contain flags for use in the
`[BuildOptions]` sections is not recommended.

The following external macro names can be used in conditional directives
without defining them in DSC or FDF files.

###### Table 4 Well-known Macro Statements

| Tag                 | Value             | Notes                                                                                                                     |
| ------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `$(FAMILY)`         | Tool Chain Family | The value must be a string comparison against a FAMILY value that is defined in the Conf/ tools_def.txt file              |
| `$(TARGET)`         | Build Target      | The value must be a string comparison against a TARGET value that is defined in the Conf/ tools_def.txt file              |
| `$(TOOL_CHAIN_TAG)` | Tool Chain Name   | The value must be a string comparison against a tool chain tag name value that is defined in the Conf/tools_def.txt file. |
| `$(ARCH)`           | Architecture      | The value must be a string comparison against an architecture that is defined in the Conf/ tools_def.txt file.            |

The macro values listed above are derived from the build command-line options
(highest priority) or the `Conf/target.txt` file. They may be combined in
directive statements using logical expressions.

Most section definitions in the EDK II meta-data files have architecture
modifiers. Use of architectural modifiers in the section tag is the recommended
method for specifying architectural differences. Some sections do not have
architectural modifiers and there are some unique cases where having a method
for specifying architectural specific items would be valuable, hence the
ability to use these values.

The following are examples of conditional directives.

```ini
!if ("MSFT" IN $(FAMILY)) or ("INTEL" IN $(FAMILY))
... statements
!elseif $(FAMILY) == "GCC"
... statements
!endif

!ifdef FOO
  !ifndef BAR
    # FOO defined, BAR not defined
  !else
    # FOO defined, BAR is defined
  !endif
!elseif $(BARFOO)
  # FOO is not defined, BARFOO evaluates to TRUE
!elseif $(BARFOO) == $(FOOBAR)
  # FOO is not defined, BARFOO equals the value of FOOBAR
  # (in this case, FALSE)
!else
  # FOO is not defined while BARFOO evaluates to FALSE and FOOBAR
  # evaluates to TRUE
!endif

!if $(FOO) == 2
  # The numeric value of FOO was defined as 2, as in DEFINE FOO = 2
!endif

!if MyTspGUID.MyPcd == 2
  # The value of PCD, MyTspGUID.MyPcd, is 2
!endif

!if $(FOO) == "MyPlatformName"
  # This is a string comparison, where the MACRO FOO was set using:
  # DEFINE FOO = "MyPlatformName"
!endif

!if MyTspGUID.MyTspGUID == "MyPlatform"
  # This is a string comparison where PCD VOID* value is "MyPlatform",
  # and must be a null terminated string.
!else
  # The strings do not match exactly
!endif
```

### 2.2.9 !error Statement

The `!error` statement may appear within any section of EDK II DSC file. The
argument of this statement is an error message, it causes build tool to stop
at the location where the statement is encountered and error message following
the `!error` statement is output as a message.

The following example show the valid usage of the `!error` statement.

```ini
!if $(FEATURE_ENABLE) == TRUE
  !error "unsupported feature!"
!endif
```

### 2.2.10 Expressions

Expressions can be used in conditional directive comparison statements and in
value fields for Macros and PCDs in the DSC and FDF files.

Refer to the EDK II Expression Syntax Specification for additional information.

**********
**Note:** Note that the data types are not required to be literal numbers, but
rather they can be a Macro or a PCD whose value is a number or a boolean. The
same rule applies for strings, where the value of the Macro or a VOID* PCD can
be tested as a string.
**********

###### Table 5 Operator Precedence and Supported Operands

| Operator                                     | Use with Data Types   | Notes                                                                                                                                                                                                                         | Priority |
| -------------------------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `or`, `OR`, <code>&#124;&#124;</code>        | Number, Boolean       |                                                                                                                                                                                                                               | Lowest   |
| `and`, `AND`, `&&`                           | Number, Boolean       |                                                                                                                                                                                                                               |          |
| <code>&#124;</code>                          | Number, Boolean       | Bitwise OR                                                                                                                                                                                                                    |          |
| `^`, `xor`, `XOR`                            | Number, Boolean       | Exclusive OR                                                                                                                                                                                                                  |          |
| `&`                                          | Number, Boolean       | Bitwise AND                                                                                                                                                                                                                   |          |
| `==`, `!=`, `EQ`, `NE`, `IN`                 | All types             | The IN operator can only be used to test a quoted unary literal string for membership in a list.                                                                                                                              |          |
|                                              |                       | Space characters must be used before and after the letter operators Strings compared to boolean or numeric values using "==" or "EQ" will always return FALSE, while using the "!=" or "NE" operators will always return TRUE |          |
| `<=`, `>=`, `<`, `>`, `LE`, `GE`, `LT`, `GT` | All                   | Space characters must be used before and after the letter operators.                                                                                                                                                          |          |
| `+`, `-`                                     | Number, Boolean       | Cannot be used with strings - the system does not automatically do concatenation. Tools should report a warning message if these operators are used with both a boolean and number value                                      |          |
| `!`, `not`, `NOT`                            | Number, Boolean       |                                                                                                                                                                                                                               | Highest  |

The "IN" operator can only be used to test a literal string against elements
in the following global variables:

**_$(FAMILY)_**

`$(FAMILY)` is considered a list of families that different `TOOL_CHAIN_TAG`
values belong to. The `TOOL_CHAIN_TAG` is defined in the `Conf/target.txt` or
on the command-line. The `FAMILY` is associated with the `TOOL_CHAIN_TAG` in
the `Conf/tools_def.txt` file (or the `TOOLS_DEF_CONF` file specified in the
`Conf/target.txt` file) file. While different family names can be defined,
ARMGCC, GCC, INTEL, MSFT, RVCT, RVCTCYGWIN and XCODE have been predefined in
the `tools_def.txt` file.

**_$(ARCH)_**

`$(ARCH)` is considered the list of architectures that are to be built, that
were specified on the command line or come from the `Conf/target.txt` file.

**_$(TOOL_CHAIN_TAG)_**

`$(TOOL_CHAIN_TAG)` is considered the list of tool chain tag names specified on
the command line

**_$(TARGET)_**

`$(TARGET)` is considered the list of target (such as DEBUG, RELEASE and NOOPT)
names specified on the command line or come from the `Conf/target.txt` file.

For logical expressions, any non-zero value must be considered `TRUE`.

Invalid expressions must cause a build break with an appropriate error message.

### 2.2.11 Section Handling

The DSC file parsing routines must process the sections so that common
architecture sections are logically merged with the architecturally specific
sections. The architectural sections need to be processed so that they are
logically after the common section. It is recommended that EDK II developers
use a logical ordering of the sections.

Other section modifiers must also be logically appended to the merged sections
(for DSC files that have architectural and common architecture sections) after
the merge.

For `[BuildOptions]` sections in the DSC file, the entries with a common left
side (of the "=") will be either appended or replace previous entries based
on the "==" replace or "=" append assignment character sequence.

```
  Common Section + Architectural Section + Common Section w/extra Modifier
  + Architectural Section w/extra Modifier
```

#### Example:

```ini
[BuildOptions.Common]
  MSFT:*_*_*_CC_FLAGS = /nologo

[BuildOptions.Common.EDK]
  MSFT:*_*_*_CC_FLAGS = /Od

[BuildOptions.IA32]
  MSFT:*_*_IA32_CC_FLAGS = /D EFI32
```

For IA32 architecture builds of an EDK II INF file would logically be:

`MSFT:*_*_IA32_CC_FLAGS = /nologo /D EFI32`

For non-IA32 architecture EDK INF files, tool parsing would logically be:

`MSFT:*_*_*_CC_FLAGS = /nologo /Od`

For IA32 architecture builds of an EDK INF file, tool parsing would logically
be:

`MSFT:*_*_IA32_CC_FLAGS = /nologo /D EFI32 /Od`
