<!--- @file
  3.13 [DefaultStores] Section

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

## 3.12 [DefaultStores] Section

The `[DefaultStores]` section is optional in all EDK II DSC files.

#### Summary

Entries may appear in any order. This section lists numeric mappings to the 
Default Store User Interface Name, only valid values from this list can be 
specified in the PcdsDynamicHii/PcdsDynamicExHii section.

If this section is not specified, the parsing tools will assume standard default
be applied in PcdsDynamicHii/PcdsDynamicExHii section.

#### Prototype

```c
<DefaultStore>     ::= "[DefaultStores]" <EOL>
                {<Statement>*}
<Statement> ::= <TS> <Number> <FS> <CName> <EOL>
```

#### Example

```ini
[DefaultStores]
  0 | Standard        # UEFI Standard default
  1 | Manufacturing   # UEFI Manufacturing default
```
