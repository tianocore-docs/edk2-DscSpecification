<!--- @file
  3.12 [UserExtensions] Sections

  Copyright (c) 2006-2019, Intel Corporation. All rights reserved.<BR>

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

## 3.11 [UserExtensions] Sections

The `[UserExtensions]` sections are optional.

#### Summary

There can be multiple `[UserExtension]` sections, depending on the `UserID` and
`Identifier` attributes of the DSC file. An example section is listed below.
`[UserExtensions]` sections are for processing by tools outside of the standard
tools provided by EDK II.

The EDK II build tools will ignore these sections.

Each UserExtensions section must have a unique set of `UserId`, `IdString` and
`Arch` values.

The "common" architecture modifier in a section tag must not be combined with
other architecture type; doing so will result in a build break.

This means that the same `UserId` can be used in more than one section,
provided the `IdString` or `Arch` values are different. The same `IdString`
values can be used if the `UserId` or `Arch` values are different. The same
`UserId` and the same `IdString` can be used if the `Arch` values are different.

#### Prototype

```c
<UserExtensions>   ::= "[UserExtensions" <attrs> "]"" <EOL> [<statements>]
<attrs>            ::= <UserId> <IdentifierString> [<attr>]
<attr>             ::= "." <arch>
<UserId>           ::= "." <Word>
<IdentifierString> ::= "." {<Word>} {<QuotedString>}
<statements>       ::= Content is build tool chain specific.
```

#### Parameters

**_UserId_**

Words that contain period "." must be encapsulated in double quotation marks.

**_IdString_**

Normalized strings that contain period "." or space characters must be
encapsulated in double quotation marks. The `IdString` must start with a letter.

#### Example

```
[UserExtensions.MyOrgDotCom."1.0"]
This content is processed by my NoSuch applications.
```
