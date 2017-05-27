<!--- @file
  README.md for EDK II Platform Description (DSC) File Specification

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

<img src="media/TianocoreTitlePageLogo.jpg" width="300" />

### {{ book.title }}

{% if book.draft %}
** DRAFT FOR REVIEW **
{% else %}
** {{ book.version }} **
{% endif %}

** {{ gitbook.time|date('MM/DD/YYYY hh:mm:ss') }} **

{% if book.udkrelease %}
** {{ book.udkrelease }} **
{% endif %}


### Acknowledgements

Redistribution and use in source (original document form) and 'compiled'
forms (converted to PDF, epub, HTML and other formats) with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code (original document form) must retain the
   above copyright notice, this list of conditions and the following
   disclaimer as the first lines of this file unmodified.

2. Redistributions in compiled form (transformed to other DTDs, converted to
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

Copyright (c) 2006-2017, Intel Corporation. All rights reserved.


### Revision History

| Revision   | Revision History   | Date        |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| 1.0        | Initial release.                                                                                                                                                                                                                                                                             | December 2007  |
| 1.1        | Updated based on errata                                                                                                                                                                                                                                                                      | August 2008    |
| 1.2        | Updated based on enhancement requests                                                                                                                                                                                                                                                        | June 2009      |
| 1.21       | Updated based on errata and enhancement requests                                                                                                                                                                                                                                             | March 2010     |
|            | Added language filters: RFC_LANGUAGES and ISO_LANGUAGES                                                                                                                                                                                                                                      |                |
|            | Added Note that a reserved macro name, MDEPKG_NDEBUG                                                                                                                                                                                                                                         |                |
|            | Definitions in DSC file are now global to both DSC and FDF files                                                                                                                                                                                                                             |                |
|            | PCD Values may be constructed using C-style expressions provided the result of the expression matches the datum type of the PCD                                                                                                                                                              |                |
|            | FeatureFlagExpression is now defined as a C-style expression using C relational, equality and logical numeric and bitwise operators and/or arithmetic and bitwise operators that evaluate to a value that matches the Datum Type of the PCD. Precedence and associativity follow C standards |                |
|            | Spec changed to match existing formats:                                                                                                                                                                                                                                                      |                |
|            | SUPPORTED_ARCHITECTURES and `BUILD_TARGETS` use the "&#124;" separator, not the comma character                                                                                                                                                                                              |                |
| 1.22       | Errata and grammatical changes                                                                                                                                                                                                                                                               | May 2010       |
| 1.22 w/    | Updates:                                                                                                                                                                                                                                                                                     | December 2011  |
| Errata A   | Updated to support UEFI version 2.3.1 and updated spec release dates in Introduction                                                                                                                                                                                                         |                |
|            | Clarify UEFI's PI Distribution Package Specification                                                                                                                                                                                                                                         |                |
|            | Standardize Common data definitions for all specifications                                                                                                                                                                                                                                   |                |
|            | Grammatical, formatting and spelling changes                                                                                                                                                                                                                                                 |                |
|            | Replaced "should" with wording saying that it is "recommended"                                                                                                                                                                                                                               |                |
|            | Remove "$(WORKSPACE)/" from the examples                                                                                                                                                                                                                                                     |                |
|            | Removed content that only applied to the EDK build system and EDK DSC format that is not used by the EDK II build system                                                                                                                                                                     |                |
|            | Rename $(TAGNAME) to $(TOOL_CHAIN_TAG) and remove the synonym $(TOOLCHAIN)                                                                                                                                                                                                                   |                |
|            | Added TIME_STAMP_FILE back into the [Defines] section                                                                                                                                                                                                                                        |                |
|            | Added the SOURCE_OVERRIDE_PATH for EDK components (not valid for EDK II modules)                                                                                                                                                                                                             |                |
|            | Added EDK_GLOBAL as a special type of MACRO statement only valid for EDK INF files                                                                                                                                                                                                           |                |
|            | Added VPD_TOOL_GUID in [Defines] section                                                                                                                                                                                                                                                     |                |
|            | Allow the OUTPUT_DIRECTORY path to be either absolute or relative to the WORKSPACE                                                                                                                                                                                                           |                |
|            | Revised EBNF for PCD sections to allow more precise definitions                                                                                                                                                                                                                              |                |
|            | Added EBNF for `<Extension>`                                                                                                                                                                                                                                                                 |                |
|            | Added EBNF for `<Keyword>` 3.9.x and renamed `<LibInstanceMap>` to `<ClassInstanceMap>` in 3.10.                                                                                                                                                                                             |                |
|            | Require PCDs to use the full name, removing ShortPcdName from definitions                                                                                                                                                                                                                    |                |
|            | Updated to clarify that a PCD cannot be used in multiple methods; a PCD can ONLY be used in one way, Also, the `<PcdsDyanmic>` sub section can only be used if the PCD is not listed in any of the common sections; All instances of the PCD must then use this method                       |                |
|            | Revised grammar and include additional clarifications for conditional directives and macro usage; clarify PCD usage in conditional directives and expressions - the $(PcdName) format is never used                                                                                          |                |
|            | Fixed VPD PCD format; allow the MAX size value to use a wildcard character & added info about where it comes from; added appendix for BPDG file formats; renamed some PCD example tokens and PCD names; cleaned up the sample DSC file                                                       |                |
|            | Removed invalid <code>[&#124;MaximumDatumSize]</code> for HII PCDs in section 2.2.13.3                                                                                                                                                                                                       |                |
|            | Document where VOID* lengths are derived from when not specified in the DSC file                                                                                                                                                                                                             |                |
|            | Clarify that C data arrays must be byte arrays for PCD value fields; both C format and Registry format GUIDs structures are not permitted in VOID* PCD value fields                                                                                                                          |                |
|            | Removed the SET statement, used to define values for PCDs from all sections of this document. The SET statement is only valid in FDF files                                                                                                                                                   |                |
|            | Specify how sections are merged during parsing of the EDK II meta-data files                                                                                                                                                                                                                 | December 2011  |
|            | Specifically state how [BuildOptions] content should be applied; define how [BuildOptions] sections are merged                                                                                                                                                                               |                |
|            | Clarify where macros are evaluated/expanded                                                                                                                                                                                                                                                  |                |
|            | Provide rules for how macros can be used in different BuildOptions sections                                                                                                                                                                                                                  |                |
|            | Clarify Macros in [Defines] section can automatically be used in FDF files.                                                                                                                                                                                                                  |                |
|            | Removed references to system environment variables in the Macros section                                                                                                                                                                                                                     |                |
|            | Updated conditional rules in 2.2.7 for how to use macro and PCDs; change #elif to #elseif in 3.2.3 to match implementation                                                                                                                                                                   |                |
|            | Added the "IN" operator as an Equality Operator - added description and restriction of it's usage using `<MemberExpression>`                                                                                                                                                                 |                |
|            | Corrected [Defines] section information, changed format of for paths, directories and files in common section                                                                                                                                                                                |                |
|            | Added table of valid environment variables that can be used in this file                                                                                                                                                                                                                     |                |
|            | Make sure that macros are not restricted to directory/path usage - also update EBNF to specifically show MACROVAL - Added `<Filename>` to macro values.                                                                                                                                      |                |
|            | Removed unused `<VALUE>` EBNF from [Defines] section                                                                                                                                                                                                                                         |                |
|            | Prohibit macros in the filename specified in !include statements; clarify the rules for finding the !include files                                                                                                                                                                           |                |
|            | Clarify how macros can be shared between sections                                                                                                                                                                                                                                            |                |
| 1..22 w/   | Updates:                                                                                                                                                                                                                                                                                     | June 2012      |
| Errata B   | Section 3.2.1 Fixed the DOS EOL character sequence                                                                                                                                                                                                                                           |                |
|            | Section 1.2 and 1.3, Updated specs to include current errata versions                                                                                                                                                                                                                        |                |
|            | Section 3.2.1 Removed reference to the LineExtension parameter description;1 line entries in the DSC file cannot be extended to multiple lines                                                                                                                                               |                |
|            | Section 3.2.2 Prohibit macros that replace or define tokens that are defined in this specification, as well as to prohibit `<EOL>` characters in a macro's value field                                                                                                                       |                |
|            | Section 3.4 Revised the value of the `BuildNumber` to be a `NumValUint16`, per the PI Specification                                                                                                                                                                                          |                |
|            | Section 2.8, fix precedence description for obtaining the length of a `VOID`* PCD: DSC, INF, DEC                                                                                                                                                                                             |                |
|            | Section 1.3 and 1.4, updated UEFI PI Distribution Package Specification Errata from A to B                                                                                                                                                                                                   |                |
|            | Section 2.2.8,and 3.2.3 Clarify PCD usage in conditional directive statements                                                                                                                                                                                                                |                |
|            | Section 2.3, and 3.4 Clarify location of FDF file                                                                                                                                                                                                                                            |                |
| 1.22 w/    | Updates:                                                                                                                                                                                                                                                                                     | August 2013    |
| Errata C   | Sections 1.3 and 1.4 updated UEFI specification versions                                                                                                                                                                                                                                     |                |
|            | Sections 2.2.8 and 3.2.3 Clarify PCD usage in conditional directive statements                                                                                                                                                                                                               |                |
|            | Sections 2.3 and 3.4, Clarify location of FDF file                                                                                                                                                                                                                                           |                |
|            | Section 2.8, fixed precedence description for obtaining the length of VOID* PCDs                                                                                                                                                                                                             |                |
|            | Section 3.2.1 Fixed DOS EOL Character Sequence                                                                                                                                                                                                                                               |                |
|            | Section 3.2.1 Removed reference to the LineExtension parameter description; 1 line entries in the DSC file cannot be extended to multiple lines.                                                                                                                                             |                |
|            | Section 3.2.1 Require C format for all arrays using curly braces around the byte elements in an array                                                                                                                                                                                        |                |
|            | Section 3.2.2 Prohibit macros that replace or define tokens that are defined in this specification, as well as to prohibit `<EOL>` characters in a macro's value field                                                                                                                       |                |
|            | Section 3.4, Revised the value of the BUILD_NUMBER to be NumValUint16, per the PI Specification                                                                                                                                                                                              |                |
|            | Section 3.8 Restrict the size of the HiiOffset to a UINT16                                                                                                                                                                                                                                   |                |
|            | Appendix D, Added VPD data file examples.                                                                                                                                                                                                                                                    |                |
|            | Various locations, replaced UCS-2 with UCS-2LE                                                                                                                                                                                                                                               |                |
| 1.24       | Updates:                                                                                                                                                                                                                                                                                     | December 2014  |
|            | Changed specification from 1.22 Errata C to 1.24.                                                                                                                                                                                                                                            |                |
|            | Allow specifying the DSC_SPECIFICATION as either 0x00010018 or 1.24.                                                                                                                                                                                                                         |                |
|            | Updates specification dates in 1.2 and added new specifications.                                                                                                                                                                                                                             |                |
|            | Removed Expression syntax with reference to external document.                                                                                                                                                                                                                               |                |
|            | Provide information on how to build multiple instances of a single module.                                                                                                                                                                                                                   |                |
|            | Updated the Terms in 1.3.                                                                                                                                                                                                                                                                    |                |
| 1.24 w/    | Updates:                                                                                                                                                                                                                                                                                     | March 2015     |
| Errata A   | Update link to the EDK II Specifications, fixed the name of the Multi-String .UNI File Format Specification                                                                                                                                                                                  |                |
| 1.25       | Updates:                                                                                                                                                                                                                                                                                     | June 2015      |
|            | Updated support for UEFI 2.5 and PI 1.4                                                                                                                                                                                                                                                      |                |
|            | Added clarification regarding the use of .. and . in path names in section 2                                                                                                                                                                                                                 |                |
|            | In section 2.8.3.7, added clarification regarding allocation of memory based on calculation of maximum size for VOID* PCD entries when the maximum size is not specified in the DSC file                                                                                                     |                |
|            | In section 3.6 added optional `<Edk2ModuleType>` element to the section header in order to support UEFI 2.5 Runtime Drivers that must be 4K page aligned                                                                                                                                     |                |
|            | Also in 3.6, updated the priority of [BuildOptions] to match current implementation that follows the Build Specification                                                                                                                                                                     |                |
|            | In sections 2.3 and 3.5, added new [Defines] boolean entry PCD_VAR_CHECK_GENERATION                                                                                                                                                                                                          |                |
|            | In sections 2.9.3.2, 2.9.3.5, 3.10.4 and 3.10.5 added HII Attribute entry.                                                                                                                                                                                                                   |                |
|            | Since the above changes are new features, update the minor revision of the DSC_SPECIFICATION to 0x00010019, older DSC files that do not use the new features do not need to modify the DSC_SPECIFICATION values                                                                              |                |
|            | Added clarification on when the DSC_SPECIFICATION value must be updated to 0x00010019                                                                                                                                                                                                        |                |
| 1.26       | Specification revision to 1.26                                                                                                                                                                                                                                                               | January 2016   |
|            | Update the DSC_SPECIFICATION version to 0x0001001A                                                                                                                                                                                                                                           |                |
|            | Revised WORKSPACE wording for updated build system that can handle packages located outside of the WORKSPACE directory tree (refer to the TianoCore.org/ EDKII website for additional instructions on setting up a development environment).                                                 |                |
|            | Added new system environment variables used by the build system.                                                                                                                                                                                                                             |                |
| 1.27       | Convert to GitBooks                                                                                                                                                                                                                                                                          | May 2017       |
|            | [#351](https://bugzilla.tianocore.org/show_bug.cgi?id=351) [DSC Spec] Extend macro usage in the !include statement                                                                                                                                                                           |                |
|            | [#484](https://bugzilla.tianocore.org/show_bug.cgi?id=484) DSC spec: support Prebuild and Postbuild in the [Defines] section                                                                                                                                                                 |                |
|            | [#353](https://bugzilla.tianocore.org/show_bug.cgi?id=353) Build spec: Allow nested includes in DSC and FDF files                                                                                                                                                                            |                |
|            | [#521](https://bugzilla.tianocore.org/show_bug.cgi?id=521) DSC spec: add clarification for mixed PCD usage in the DSC spec                                                                                                                                                                   |                |
