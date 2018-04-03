<!--- @file
  Summary

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

# Summary

* [EDK II Platform Description (DSC) File Specification](README.md#edk-ii-platform-description-dsc-file-specification)
* [1 Introduction](1_introduction/README.md#1-introduction)
  * [1.1 Overview](1_introduction/11_overview.md#11-overview)
  * [1.2 Terms](1_introduction/12_terms.md#12-terms)
  * [1.3 Related Information](1_introduction/13_related_information.md#13-related-information)
  * [1.4 Conventions Used in this Document](1_introduction/14_conventions_used_in_this_document.md#14-conventions-used-in-this-document)
* [2 DSC Overview](2_dsc_overview/README.md#2-dsc-overview)
  * [2.1 Processing Overview](2_dsc_overview/21_processing_overview.md#21-processing-overview)
  * [2.2 Build Description File Format](2_dsc_overview/22_build_description_file_format.md#22-build-description-file-format)
  * [2.3 [Defines] Section Processing](2_dsc_overview/23_[defines]_section_processing.md#23-defines-section-processing)
  * [2.4 [BuildOptions] Section](2_dsc_overview/24_[buildoptions]_section.md#24-buildoptions-section)
  * [2.5 [SkuIds] Section Processing](2_dsc_overview/25_[skuids]_section_processing.md#25-skuids-section-processing)
  * [2.6 [Libraries] Section Processing](2_dsc_overview/26_[libraries]_section_processing.md#26-libraries-section-processing)
  * [2.7 [LibraryClasses] Section Processing](2_dsc_overview/27_[libraryclasses]_section_processing.md#27-libraryclasses-section-processing)
  * [2.8 PCD Section Processing](2_dsc_overview/28_pcd_section_processing.md#28-pcd-section-processing)
  * [2.9 PCD Sections](2_dsc_overview/29_pcd_sections.md#29-pcd-sections)
  * [2.10 PCD Database](2_dsc_overview/210_pcd_database.md#210-pcd-database)
  * [2.11 [Components] Section Processing](2_dsc_overview/211_[components]_section_processing.md#211-components-section-processing)
  * [2.12 [UserExtensions] Section](2_dsc_overview/212_[userextensions]_section.md#212-userextensions-section)
  * [2.13 [DefaultStores] Section](2_dsc_overview/213_[defaultstores]_section_processing.md#213-defaultstores-section-processing)
* [3 EDK II DSC File Format](3_edk_ii_dsc_file_format/README.md#3-edk-ii-dsc-file-format)
  * [3.1 Building multiple architectures](3_edk_ii_dsc_file_format/31_building_multiple_architectures.md#31-building-multiple-architectures)
  * [3.2 General Rules](3_edk_ii_dsc_file_format/32_general_rules.md#32-general-rules)
  * [3.3 Platform DSC Definition](3_edk_ii_dsc_file_format/33_platform_dsc_definition.md#33-platform-dsc-definition)
  * [3.4 Header Section](3_edk_ii_dsc_file_format/34_header_section.md#34-header-section)
  * [3.5 [Defines] Section](3_edk_ii_dsc_file_format/35_[defines]_section.md#35-defines-section)
  * [3.6 [BuildOptions] Sections](3_edk_ii_dsc_file_format/36_[buildoptions]_sections.md#36-buildoptions-sections)
  * [3.7 [SkuIds] Section](3_edk_ii_dsc_file_format/37_[skuids]_section.md#37-skuids-section)
  * [3.8 [Libraries] Sections](3_edk_ii_dsc_file_format/38_[libraries]_sections.md#38-libraries-sections)
  * [3.9 [LibraryClasses] Sections](3_edk_ii_dsc_file_format/39_[libraryclasses]_sections.md#39-libraryclasses-sections)
  * [3.10 PCD Sections](3_edk_ii_dsc_file_format/310_pcd_sections.md#310-pcd-sections)
  * [3.11 [Components] Sections](3_edk_ii_dsc_file_format/311_[components]_sections.md#311-components-sections)
  * [3.12 [UserExtensions] Sections](3_edk_ii_dsc_file_format/312_[userextensions]_sections.md#312-userextensions-sections)
  * [3.13 [DefaultStores] Sections](3_edk_ii_dsc_file_format/313_[defaultstores]_section.md#313-defaultstores-section)
* [Appendix A Variables](appendix_a_variables.md#appendix-a-variables)
* [Appendix B Sample EDK II DSC File](appendix_b_sample_edk_ii_dsc_file.md#appendix-b-sample-edk-ii-dsc-file)
* [Appendix C Module Types](appendix_c_module_types.md#appendix-c-module-types)
* [Appendix D Vpd Data Files](appendix_d_vpd_data_files/README.md#appendix-d-vpd-data-files)
  * [D.1 EDK II Build System Output File Format](appendix_d_vpd_data_files/d1_edk_ii_build_system_output_file_format.md#d1-edk-ii-build-system-output-file-format)
  * [D.2 Vpd Info File Format](appendix_d_vpd_data_files/d2_vpd_info_file_format.md#d2-vpd-info-file-format)
---
* Tables
  * [Table 1 EDK Build Infrastructure Support Matrix](1_introduction/11_overview.md#table-1-edk-build-infrastructure-support-matrix)
  * [Table 2 Well-known Macro Statements](2_dsc_overview/22_build_description_file_format.md#table-2-well-known-macro-statements)
  * [Table 3 Using System Environment Variable](2_dsc_overview/22_build_description_file_format.md#table-3-using-system-environment-variable)
  * [Table 4 Well-known Macro Statements](2_dsc_overview/22_build_description_file_format.md#table-4-well-known-macro-statements)
  * [Table 5 Operator Precedence and Supported Operands](2_dsc_overview/22_build_description_file_format.md#table-5-operator-precedence-and-supported-operands)
  * [Table 6 EDK II [Defines] Section Elements](2_dsc_overview/23_[defines]_section_processing.md#table-6-edk-ii-defines-section-elements)
  * [Table 7 EDK II [BuildOptions] Section Elements: Optional Tags](2_dsc_overview/24_[buildoptions]_section.md#table-7-edk-ii-buildoptions-section-elements-optional-tags)
  * [Table 8 EDK II [BuildOptions] Variable Descriptions](2_dsc_overview/24_[buildoptions]_section.md#table-8-edk-ii-buildoptions-variable-descriptions)
  * [Table 9 HII Attributes](2_dsc_overview/29_pcd_sections.md#table-9-hii-attributes)
  * [Table 10 Standard Variables](appendix_a_variables.md#table-10-standard-variables)
  * [Table 11 EDK II Module Types](appendix_c_module_types.md#table-11-edk-ii-module-types)
---
* Figures
  * [Figure 1 EDK II Parsing Data Flow](2_dsc_overview/21_processing_overview.md#figure-1-edk-ii-parsing-data-flow)
