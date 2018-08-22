<!--- @file
  3.3 Platform DSC Definition

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

## 3.3 Platform DSC Definition

The DSC definitions set the final properties (including final PCD values) for
building UEFI/PI compliant binaries. Only command-line values override values
in this file.

Values in this file override any values set in INF or DEC files.

Binary modules do not need to be listed in this file unless they reference
PatchableInModule or DynamicEx PCDs.

**********
**Note:** Some PCD values may be obtained from the Flash Description (FDF) file
specified in the `[Defines]` section or SET statements.
**********

The `[Defines]` section must appear before any other sections (except the
header comment blocks). The remaining sections may appear in any order, however
the EBNF, below, shows the recommended order. (The `[Libraries]` section is
required if building EDK libraries and components in the context of an EDK II
platform.)

#### Summary

The EDK II Platform Description (DSC) file has the following format (using the
EBNF).

```c
<EDK_II_DSC> ::= [<Header>]
                 <Defines>
                 [<SkuIds>]
                 <Libraries>*
                 <LibraryClasses>*
                 <Pcds>*
                 <Components>+
                 <BuildOptions>*
                 <UserExtensions>*
```

**********
**Note:** Assignments set as command-line arguments to the parsing tools take
precedence over all assignments defined in the DSC file. If a variable/value
assignment is specified on the build tool's command-line, that value will
override any variable/value assignment defined in the DSC file.
**********
**Note:** Conditional statements may be used anywhere within the DSC file, with
the ability to group any item within a section as well as entire sections.
**********

### 3.3.1 Common Definitions

#### Summary

The following are common definitions used by multiple section types.

#### Prototype

```c
<Word>                 ::= (a-zA-Z0-9_)(a-zA-Z0-9_-.)* Alphanumeric characters
                           with optional period ".", dash "-" and/or underscore
                           "_" characters. A period character may not be
                           followed by another period character.
                           No white space characters are permitted.
<SimpleWord>           ::= (a-zA-Z0-9)(a-zA-Z0-9_-)* A word that cannot contain
                           a period character.
<ToolWord>             ::= (A-Z)(a-zA-Z0-9)* Alphanumeric characters. white
                           space characters are not permitted.
<FileSep>              ::= "/"
<Extension>            ::= (a-zA-Z0-9_-)+ One or more alphanumeric characters.
<File>                 ::= <Word> ["." <Extension>]
<PATH>                 ::= [<MACROVAL> <FileSep>] <RelativePath>
<RelativePath>         ::= <DirName> [<FileSep> <DirName>]*
<DirName>              ::= {<Word>} {<MACROVAL>}
<FullFilename>         ::= {<PATH> <FileSep> <File>}
                           {<MACROVAL> <FileSep> <File>}
                           {<MACROVAL>}
<Filename>             ::= {[<PATH> <FileSep>] <File>}
                           {[<RelativePath> <FileSep>] <File>}
                           {<MACROVAL>}
<Chars>                ::= (a-zA-Z0-9_)
<Digit>                ::= (0-9)
<NonDigit>             ::= (a-zA-Z_)
<Identifier>           ::= <NonDigit> <Chars>*
<CName>                ::= <Identifier> # A valid C variable name.
<AsciiChars>           ::= (0x21 - 0x7E)
<CChars>               ::= [{0x21} {(0x23 - 0x26)} {(0x28 - 0x5B)}
                           {(0x5D - 0x7E)} {<EscapeSequence>}]*
<DblQuote>             ::= 0x22
<SglQuote>             ::= 0x27
<EscapeSequence>       ::= "\" {"n"} {"t"} {"f"} {"r"} {"b"} {"0"} {"\"}
                           {<DblQuote>} {<SglQuote>}
<TabSpace>             ::= {<Tab>} {<Space>}
<TS>                   ::= <TabSpace>*
<MTS>                  ::= <TabSpace>+
<Tab>                  ::= 0x09
<Space>                ::= 0x20
<CR>                   ::= 0x0D
<LF>                   ::= 0x0A
<CRLF>                 ::= <CR> <LF>
<WhiteSpace>           ::= {<TS>} {<CR>} {<LF>} {<CRLF>}
<WS>                   ::= <WhiteSpace>*
<Eq>                   ::= <TS> "=" <TS>
<FieldSeparator>       ::= "|"
<FS>                   ::= <TS> <FieldSeparator> <TS>
<Wildcard>             ::= "*"
<CommaSpace>           ::= "," <Space>*
<Cs>                   ::= "," <Space>*
<AsciiString>          ::= [ <TS>* <AsciiChars>* ]*
<EmptyString>          ::= <DblQuote><DblQuote>
<CFlags>               ::= <AsciiString>
<PrintChars>           ::= {<TS>} {<CChars>}
<QuotedString>         ::= <DblQuote> <PrintChars>* <DblQuote>
<SglQuotedString>      ::= <SglQuote> <PrintChars>* <SglQuote>
<CString>              ::= {<QuotedString>} {<SglQuotedString>}
<NormalizedString>     ::= <DblQuote> [{<Word>} {<Space>}]+ <DblQuote>
<GlobalComment>        ::= <WS> "#" [<AsciiString>] <EOL>+
<Comment>              ::= "#" <AsciiString> <EOL>+
<UnicodeString>        ::= "L" {<QuotedString>} {<SglQuotedString>}
<HexDigit>             ::= (a-fA-F0-9)
<HexByte>              ::= {"0x"} {"0X"} [<HexDigit>] <HexDigit>
<HexNumber>            ::= {"0x"} {"0X"} <HexDigit>+
<HexVersion>           ::= "0x" [0]* <Major> <Minor>
<Major>                ::= <HexDigit>? <HexDigit>? <HexDigit>?
                           <HexDigit>
<Minor>                ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<DecimalVersion>       ::= {"0"} {(1-9) [(0-9)]*} ["." (0-9)+]
<VersionVal>           ::= {<HexVersion>} {(0-9)+ "." (0-99)}
<GUID>                 ::= {<RegistryFormatGUID>} {<CFormatGUID>}
<RegistryFormatGUID>   ::= <RHex8> "-" <RHex4> "-" <RHex4> "-" <RHex4> "-"
                           <RHex12>
<RHex4>                ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<RHex8>                ::= <RHex4> <RHex4>
<RHex12>               ::= <RHex4> <RHex4> <RHex4>
<RawH2>                ::= <HexDigit>? <HexDigit>
<RawH4>                ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>
<OptRawH4>             ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>?
<Hex2>                 ::= {"0x"} {"0X"} <RawH2>
<Hex4>                 ::= {"0x"} {"0X"} <RawH4>
<Hex8>                 ::= {"0x"} {"0X"} <OptRawH4> <RawH4>
<Hex12>                ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4> <RawH4>
<Hex16>                ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4> <OptRawH4>
                           <RawH4>
<CFormatGUID>          ::= "{" <Hex8> <CommaSpace> <Hex4> <CommaSpace>
                           <Hex4> <CommaSpace> "{"
                           <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                           <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                           <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                           <Hex2> <CommaSpace> <Hex2> "}" "}"
<CArray>               ::= "{" {<NList>} {<CArray>} "}"
<NList>                ::= <HexByte> [<CommaSpace> <HexByte>]*
<RawData>              ::= <TS> <HexByte>
                           [ <Cs> <HexByte> [<EOL> <TS>] ]*
<Integer>              ::= {(0-9)} {(1-9)(0-9)+}
<Number>               ::= {<Integer>} {<HexNumber>}
<HexNz>                ::= (\x1 - \xFFFFFFFFFFFFFFFF)
<NumNz>                ::= (1-18446744073709551615)
<GZ>                   ::= {<NumNz>} {<HexNz>}
<TRUE>                 ::= {"TRUE"} {"true"} {"True"} {"0x1"} {"0x01"} {"1"}
<FALSE>                ::= {"FALSE"} {"false"} {"False"} {"0x0"} {"0x00"} {"0"}
<BoolVal>              ::= {<TRUE>} {<FALSE>}
<BoolType>             ::= {<BoolVal>} {"{"<BoolVal>"}"}
<MACRO>                ::= (A-Z)(A-Z0-9_)*
<MACROVAL>             ::= "$(" <MACRO> ")"
<PcdFieldName>         ::= <TokenSpaceGuidCName> "." <PcdCName> "." <Field>
<PcdName>              ::= <TokenSpaceGuidCName> "." <PcdCName>
<PcdCName>             ::= <CName>
<TokenSpaceGuidCName>  ::= <CName>
<Field>                ::= <CName>
<PcdFieldEntry>        ::= <PcdFieldName> <FS> <PcdFieldValue> <EOL>
<PcdFieldValue>        ::= {<BoolType>} {<NumValUint8>} {<NumValUint16>}
                           {<NumValUint32>} {<NumValUint64>} {<StringVal>}
                           {<MACROVAL>} {<Expression>}
<PCDVAL>               ::= "PCD(" <PcdName> ")"
<UINT8>                ::= {"0x"} {"0X"} (\x0 - \xFF)
<UINT16>               ::= {"0x"} {"0X"} (\x0 - \xFFFF)
<UINT32>               ::= {"0x"} {"0X"} (\x0 - \xFFFFFFFF)
<UINT64>               ::= {"0x"} {"0X"} (\x0 - \xFFFFFFFFFFFFFFFF)
<UINT8z>               ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
<UINT16z>              ::= {"0x"} {"0X"} <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit>
<UINT32z>              ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit>
<UINT64z>              ::= {"0x" <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit>
<ShortNum>             ::= (0-255)
<IntNum>               ::= (0-65535)
<LongNum>              ::= (0-4294967295)
<LongLongNum>          ::= (0-18446744073709551615)
<ValUint8>             ::= {<ShortNum>} {<UINT8>} {<BoolVal>}
                           {<CString>} {<UnicodeString>}
<ValUint16>            ::= {<IntNum>} {<UINT16>} {<BoolVal>}
                           {<CString>} {<UnicodeString>}
<ValUint32>            ::= {<LongNum>} {<UINT32>} {<BoolVal>}
                           {<CString>} {<UnicodeString>}
<ValUint64>            ::= {<LongLongNum>} {<UINT64>} {<BoolVal>}
                           {<CString>} {<UnicodeString>}
<NumValUint8>          ::= {<ValUint8>} {"{"<ValUint8>"}"}
<NumValUint16>         ::= {<ValUint16>}
                           {"{"<ValUint8> [<CommaSpace> <ValUint8>]*"}"}
<NumValUint32>         ::= {<ValUint32>}
                           {"{"<ValUint8> [<CommaSpace> <ValUint8>]*"}"}
<NumValUint64>         ::= {<ValUint64>}
                           {"{"<ValUint8> [<CommaSpace> <ValUint8>]*"}"}
<StringVal>            ::= {<UnicodeString>} {<CString>} {<Array>}
<Array>                ::= "{" {<Array>} {[<Lable>] <ArrayVal>
                           [<CommaSpace> [<Lable>] <ArrayVal>]* } "}"
<ArrayVal>             ::= {<Num8Array>} {<GuidStr>} {<DevicePath>}
<NonNumType>           ::= {<BoolVal>} {<UnicodeString>} {<CString>}
                           {<Offset>} {<UintMac>}
<GuidStr>              ::= "GUID(" <GuidVal> ")"
<GuidVal>              ::= {<DblQuote> <RegistryFormatGUID> <DblQuote>}
                           {<CFormatGUID>} {<CName>}
<DevicePath>           ::= "DEVICE_PATH(" <DevicePathStr> ")"
<DevicePathStr>        ::= A double quoted string that follow the device path
                           as string format defined in UEFI Specification 2.6
                           Section 9.6
<Num8Array>            ::= {<NonNumType>} {<ShortNum>} {<UINT8>}
<Num16Array>           ::= {<NonNumType>} {<IntNum>} {<UINT16>}
<Num32Array>           ::= {<NonNumType>} {<LongNum>} {<UINT32>}
<Num64Array>           ::= {<NonNumType>} {<LongLongNum>} {<UINT64>}
<UintMac>              ::= {<Uint8Mac>} {<Uint16Mac>} {<Uint32Mac>} {<Uint64Mac>}
<Uint8Mac>             ::= "UINT8(" <Num8Array> ")"
<Uint16Mac>            ::= "UINT16(" <Num16Array> ")"
<Uint32Mac>            ::= "UINT32(" <Num32Array> ")"
<Uint64Mac>            ::= "UINT64(" <Num64Array> ")"
<Lable>                ::= "LABEL(" <CName> ")"
<Offset>               ::= "OFFSET_OF(" <CName> ")"
<ModuleType>           ::= {"BASE"} {"SEC"} {"PEI_CORE"} {"PEIM"}
                           {"DXE_CORE"} {"DXE_DRIVER"} {"SMM_CORE"}
                           {"DXE_RUNTIME_DRIVER"} {"DXE_SAL_DRIVER"}
                           {"DXE_SMM_DRIVER"} {"UEFI_DRIVER"}
                           {"UEFI_APPLICATION"} {"USER_DEFINED"}
<ModuleTypeList>       ::= <ModuleType> [" " <ModuleType>]*
<Boolean>              ::= {<BoolType>} {<Expression>}
<EOL>                  ::= <TS> 0x0A 0x0D
<OA>                   ::= (a-zA-Z)(a-zA-Z0-9)*
<arch>                 ::= {"IA32"} {"X64"} {"EBC"} {<OA>} {"COMMON"}
```

**********
**Note:** When using CString, UnicodeString or byte array format as
UINT8/UINT16/UINT32/UINT64 values, please make sure they fit in the
target type's size, otherwise tool would report failure.
**********
**Note:** LABEL() macro in byte arrays to tag the byte offset of a
location in a byte array. OFFSET_OF() macro in byte arrays that returns
the byte offset of a LABEL() declared in a byte array.
**********
**Note:** When using the characters "|" or "||" in an expression, the
expression must be encapsulated in open "(" and close ")" parenthesis.
**********
**Note:** Comments may appear anywhere within a DSC file, provided they follow
the rules that a comment may not be enclosed within Section headers, and that
in line comments must appear at the end of a statement.
**********

#### Parameters

**_Expression_**

There is also a membership expression, using the non-C "in" operator, which
allows for testing if an item is present in a list. The usage of this
membership expression is restricted to testing architectures, targets and tool
chain tag names that are being built. Refer to the EDK II Expression Syntax
Specification for additional information.

**_UnicodeString_**

When the `<UnicodeString>` element (these characters are string literals as
defined by the C99 specification: L"string"/L'string', not actual Unicode
characters) is included in a value, the build tools may be required to expand
the ASCII string between the quotation marks into a valid UCS-2 character string.
The build tools parser must treat all content between the field separators
(excluding white space characters around the field separators) as ASCII literal
content when generating the AutoGen.c and AutoGen.h files.

**_Comments_**

Strings that appear in comments may be ignored by the build tools. An ASCII
string matching the format of the ASCII string defined by `<UnicodeString>`
(L"Foo" for example,) that appears in a comment must never be expanded by any
tool.

**_CFlags_**

CFlags refers to a string of valid arguments appended to the command line of
any third party or provided tool. It is not limited to just a compiler
executable tool. MACRO values that appear in quoted strings in CFlags content
must not be expanded by parsing tools.

**_OA_**

Other Architecture - One or more user defined target architectures, such as ARM
or PPC. The architectures listed here must have a corresponding entry in the
EDK II meta-data file, `Conf/tools_def.txt`. Only `IA32`, `X64`, `COMMON` and
`EBC` are routinely validated.

**_FileSep_**

FileSep refers to either the back slash "\" or forward slash "/" characters
that are used to separate directory names. All EDK II DSC files must use the
"/" forward slash character when specifying the directory portion of a
filename. Microsoft operating systems, that normally use a back slash character
for separating directory names, will interpret the forward slash character
correctly.

**_CArray_**

All C data arrays used in PCD value fields must be byte arrays. The C format
GUID style is a special case that is permitted in some fields that use the
`<CArray>` nomenclature.

**_EOL_**

The DOS End Of Line: "0x0D 0x0A" character must be used for all EDK II
meta-data files. All *Nix based tools can properly process the DOS EOL
characters. Microsoft based tools cannot process the *Nix style EOL characters.

### 3.3.2 MACRO Statements

Use of MACRO statements is optional.

#### Summary

Macro statements are characterize by a `DEFINE` token or may be defined on a
command line of a parsing tool.

Define statements are processed according to the following precedence.

Highest Priority

1. Command-line option `-D MACRO=Value`

2. Most recent in file

3. Macros defined in the DSC file's `[Defines]` section

Lowest Priority

Macros defined in the `[Defines]` section are considered global during the
processing of the FDF file and the DSC file. `EDK_GLOBAL` macros are considered
global during the processing of DSC, FDF and EDK INF files.

Macros are not allowed to redefine the reserved words specified in this file.
For example, it is not permitted to `DEFINE DEFINE = FOOBAR`, then use `FOOBAR`
as a the reserved word.

A macro that is not defined has a value of 0.

If the Macro statement is within the `[Defines]` section, then the Macro is
common to the entire file as well as common to the FDF file, with later
definitions overriding previous values (if the same MACRO name is used in
subsequent sections, then the MACRO value overrides all remaining instances
that following the definition.)

Macro statements referenced before they are defined are "undefined" (for the
`!ifndef` and `!ifdef` conditional directive statements).

If the tools encounter a macroval used in a directive, an expression or a value
field, as in `$(MACRO)`, that is not defined, the macro will have a value of 0
and, as in C programming, the build may break.

While it is recommended that tools catch exceptions for incorrect content, they
may report the error on the line that uses the macro, `$(MACRO)`, rather than
where the macro was defined.

#### Prototype

```c
<MacroDefinition> ::= {<NormalMacro>} {<EdkMacro>}
<NormalMacro>     ::= <TS> "DEFINE" <MTS> <MACRO> <Eq> [<Value>] <EOL>
<EdkMacro>        ::= <TS> "EDK_GLOBAL" <MTS> <MACRO> <Eq> [<Value>] <EOL>
<Value>           ::= {<Number>} {<BoolType>} {<GUID>}
                      {<CString>} {<UnicodeString>} {<CArray>}
                      {<PATH>} {<Expression>} {<CFlags>}
                      {<RelativePath>} {<Filename>}
```

#### Restrictions

**_System Environment Variables_**

System environment variable may not be re-defined in this file. System
environment variables cannot be overridden by the build system tools.

**_Token and Statements_**

It is not permissible to use `$(MACRO)` to replace a token or a complete token
statement. Tokens are the keywords defined in this specification.

**_Values_**

The macro's value must not include any `<EOL>` character sequences. (Using \r
and \n within a quoted string is permitted.

#### Parameters

**_Expression_**

Refer to the EDK II Expression Syntax Specification for additional information.

**********
**Note:** MACRO values defined in the `[Defines]` _section and PCD values
defined in this file may be used in the Flash FDF file.
**********

#### Examples:

```
DEFINE GEN_SKU = MyPlatformPkg/GenPei
DEFINE SKU1 = MyPlatformPkg/Sku1/Pei
DEFINE HACK = DEBUG
EDK_GLOBAL EFI_ACPI_TABLE_STORAGE_GUID = 7E374E25-8E01-4FEE-87F2390C23C606CD
```

### 3.3.3 Conditional Directive Blocks

Use of conditional directive blocks is optional.

#### Summary

The conditional statements may appear anywhere within the file. Conditional
directive blocks can be nested.

* All conditional directives can use MACRO, FixedAtBuild or FeatureFlag PCD
  values, which must evaluate to either "True" or "False".

* Directives must be the only statement on a line.

* String evaluations are permitted, and are case sensitive; the two string
  values must be an exact match to evaluate to "True".

* The expression immediately following the "!if" statement controls whether
  the content after the line is processed or not. `TRUE` is any non-zero and/or
  non-null value other than zero.

* Each "!if" within the source code must be matched with a closing "!endif".

* Zero or more "!elseif" statements are permitted; only one "!else"
  statement is permitted.

* Conditional directive blocks may be nested.

* Directives can be used to encapsulate entire sections or elements within a
  single section, such that they do not break the integrity of the section
  definitions.

* Directives are in-fix expressions that are evaluated left to right; content
  within parenthesis is evaluated before the outer statements are evaluated.
  Use of parenthesis is recommended to remove ambiguity.

* The values of the FixedAtBuild and FeatureFlag PCDs used in the conditional
  statements must be set in the `[PcdsFixedAtBuild]` or `[PcdsFeatureFlag]`
  section(s) of this DSC file. Other forms of PCDs cannot be used in
  conditional directive statements.

* Default PCD values from the DEC files cannot be used in conditional
  directives within the DSC file; all PCD values used in directive statements
  must be defined in the DSC file.

Conditional directives may appear before a Macro, FixedAtBuild or FeatureFlag
PCD has been defined. Therefore, the reference build tools must perform two
passes on this file:

1. Obtain the values of the MACROs, FixedAtBuild and FeatureFlag PCDs which are
   used for the conditional directives (these values must not be located within
   a conditional directive).

2. Evaluate the conditional statements for inclusion in the build.

If the value of a FixedAtBuild or FeatureFlag PCD used in a conditional
directive cannot be determined during the first pass, the build must break. It
is permissible to have a Macro that is undefined after the first pass. Macros
must be defined before they can be used in an expression. Macros, FixedAtBuild
and FeatureFlag PCDs used in conditional statements in the first pass must not
be located within conditional directives.

The build system must break if a PCD in the directive is listed in

`[PcdsPatchableInModule], [PcdsDynamic] or [PcdsDynamicEx] section.`

**********
**Note:** PCDs, used in conditional directives, must be defined and the value
set in either the FDF or DSC file in order to be used in a conditional
statement; values from INF or DEC files are not permitted.
**********

#### Prototype

```c
<Conditional>       ::= <IfStatement> <EOL>
                        <ElseIfConditional>*
                        [<ElseConditional>]
                        <TS> "!endif" <EOL>
<IfStatement>       ::= {<TS> "!if" <MTS> <Expression> <EOL>}
                        {<TS> "!ifdef" <MTS> <MACRO> <EOL>}
                        {<TS> "!ifndef" <MTS> <MACRO> <EOL>}
                        <Statements>*
<Statements>        ::= {<Sections>} {<Conditonal>} {<SectionStatements>}
<Sections>          ::= _ValidStatements_
<SectionStatements> ::= _ValidStatements_
<ElseIfConditional> ::= <TS> "!elseif" <MTS> <Expression>
                        <EOL>
                        <Statements>*
<ElseConditional>   ::= <TS> "!else" <EOL>
                        <Statements>*
```

#### Restrictions

**_MACRO and PCD Values_**

When a MACRO is used in conditional directives `!if` or `!elseif`, the
`<MACROVAL>` - $(MACRO) - format is used. When a PCD is used in a conditional
directive (or in an expression) just the PcdName format is used.

**_Number values_**

For Numeric expressions, numbers must not be encapsulated by double quotation
marks

**_Strings_**

Strings in `PCD` elements must be NULL terminated. The `NULL` character is not
part of the string that is tested. All other string comparisons do not include
the double quotation marks encapsulating the string. If the string is
"myapple", the only characters that would be tested are myapple. When using
strings in the expression statements, there must be a comparison operator.

#### Parameters

**_ValidStatements_**

Any valid section, multiple sections, section statement or set of section
statements defined in this specification may be within the scope of the
conditional statements.

**_MACRO Usage in Expression Statements_**

A macro is said to be defined if and only if it has been set to a non-NULL
value. When used in the `!ifdef` and `!ifndef` statement, the `<MACROVAL>`
format is being deprecated.

**_PcdFeatureFlag_**

The FeatureFlag PCD is a boolean PCD that is set to either `True` (`1`) or
`False` (`0`). The PCD datum type for a Feature PCD is always `BOOLEAN`. It may
be used in a logical expression.

**_FixedPcdName_**

A FixedAtBuild PCD will have a set value at build time, and the value cannot be
modified in the binary image, nor modified at runtime. For directives, the PCD
datum type is limited to `UINT8`, `UINT16`, `UINT32`, `UINT64`, `UINTN` and
`BOOLEAN`. Using a FixedAtBuild PCD that has a datum type of `VOID`* is limited
to text-based comparisons in directives. Using a PCD that has a value of a byte
array is not permitted. FixedAtBuild PCDs may be used in a logical expression.

**_Numeric Expressions_**

This is a test of numbers, using relational or equality operators, that
evaluates to `TRUE` or `FALSE`

**_Logical Expressions_**

This is a test where the expression, MACRO value or PCD value (include
`<MACROVAL>` or `<PCDVAL>` used in an expression) must evaluate to either
`TRUE` (`1`) or `FALSE` (`0`), no operators are required, however logical
operators, as well full expressions can be used. (expressions that do not
evaluate to `TRUE` or `FALSE` must break the build).

**_String Expressions_**

The strings must be exactly identical in order to match. Literal strings must
be encapsulated by double quotation marks. There must be a comparison operator
between two strings (using a string without an operator is not permitted). Also
permitted are the membership expressions, for architectures, targets and tool
chain tag names.

**_All Expressions_**

Refer to the EDK II Expression Syntax Specification for additional information.

#### Example

```ini
!if $(EBC_VERSION) == 1
  [VTF.EBC.MyBsf]
    !ifdef IA32RESET
      # EBC_VERSION is 1 and IA32RESET defined
      IA32_RST_BIN           = IA32_RST.BIN
    !endif
    COMP_NAME              = PAL_A
    COMP_LOC               = F
    COMP_TYPE              = 0xF
    COMP_VER               = 7.01
    COMP_CS                = 1
    !if $(PROCESSOR_NAME) == "M1"
      COMP_BIN               = M1PalCode/PAL_A_M1.BIN
      COMP_SYM               = M1PalCode/PAL_A_M1.SYM
    !elseif PROCESSOR_NAME == "M2"
      COMP_BIN               = M2PalCode/PAL_A_M2.BIN
      COMP_SYM               = M2PalCode/PAL_A_M2.SYM
    !else
      COMP_BIN               = GenPal/PAL_A_GEN.bin
      COMP_SYM               = GenPal/PAL_A_GEN.sym
    !endif
    COMP_SIZE              = -
!elseif $(EBC_VERSION) == 2
  [VTF.EBC.MyBsf]
    !ifdef IA32RESET
      # EBC_VERSION is 2 and IA32RESET defined
      IA32_RST_BIN = IA32_RST.BIN
    !endif
    COMP_NAME    = PAL_A
    COMP_LOC     = F
    COMP_TYPE    = 0xF
    COMP_VER     = 7.01
    COMP_CS      = 1
    COMP_BIN     = GenPal/PAL_A_GEN.bin
    COMP_SYM     = GenPal/PAL_A_GEN.sym
    COMP_SIZE    = -
    COMP_NAME    = PAL_B
    COMP_LOC     = F
    COMP_TYPE    = 0x01
    COMP_VER     = -
    COMP_CS      = 1
    COMP_BIN     = GenPal/PAL_B_GEN.bin
    COMP_SYM     = GenPal/PAL_B_GEN.sym
    COMP_SIZE    =
!else
  [VTF.X64.MyVtf]
    IA32_RST_BIN = IA32_RST.BIN
!endif

[LibraryClasses]
!if $(TARGET) == "DEBUG"
  # List Debug Library Class Instances here
!else
  # List Release Library Class Instances here
!endif
```

### 3.3.4 !include Statements

Use of this statement is optional.

#### Summary

This section defines the `!include` statement in EDK II Platform (DSC) files.
This statement is used to include, at the statement's line, a file which is
processed at that point, as though the text of the included file was actually
in the DSC file. The included file's content must match the content of the
section that the `!include` statement resides, or it may contain completely new
sections. If the included file starts with a section header, then the section
being processed in the Platform DSC file is considered to have been terminated.

If the `<Filename>` contains "$" characters, then macros defined in the DSC
file and the system environment variables, `$(WORKSPACE)`, `$(EDK_SOURCE)`,
`$(EFI_SOURCE)`, and `$(ECP_SOURCE)` are substituted into `<Filename>`.

The tools look for `<Filename>` relative to the directory the DSC file resides.
If the file is not found, and a directory separator is in `<Filename>`, the
tools attempt to find the file in a WORKSPACE (or a directory listed in the
PACKAGES_PATH) relative path. If the file cannot be found, the build system
must exit with an appropriate error message.

Statements in the include file are permitted to override previous definitions
as well as to define new entries.

#### Prototype

`<IncludeStatement> ::= <TS> "!include" <MTS> <Filename> <EOL>`

#### Example (EDK II DSC)

```
!include MyPlatformPkg/feature_pcds.mak
!include MyFeatures.mak
!include $(WORKSPACE)/PackageDir/Features.dsc
!include $(MACRO1)/AnotherDir/$(MACRO2)/Features.dsc
```

**********
**Note:** The extension used in the example, "mak", is just a three
character extension, and would not processed by `make` or `nmake` commands. It
might just as well have been "foo".
**********

### 3.3.5 !error Statements

Use of this statement is optional.

#### Summary
This section defines the `!error` statement in EDK II Platform (DSC) files.
This statement is used to cause build tool to stop at the location where the
statement is encountered and error message following the `!error` statement
is output as a message.

#### Prototype

`<ErrorStatement> ::= <TS> "!error" <MTS> <ErrorMessage> <EOL>`
`<ErrorMessage>   ::= <AsciiString>`

#### Parameters

**_ErrorMessage_**

It should in the same line with `!error` statement, and it can consist of several
words not necessarily in quotes.

#### Example (EDK II DSC)

```
!error "unsupported feature!"
```
