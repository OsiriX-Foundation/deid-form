# DICOM De-Identification Specification

## Definitions

Basic Profile - Basic Attribute Level Confidentiality Profile as defined in DICOM PS3.15 2021a.

SOP Class - Service Object Pair Class. Definition of an action (Operation) that can performed on an Object (IOD).

SOP Instance - Service Object Pair Instance. In practice the atomic unit of storage in DICOM that contains one instance of an IOD.

IOD - Information Object Definition. List of attribute elements found in a specific type of DICOM object. Attributes in an MRI for example.

## Preamble

De-identification will be based on on the Basic Attribute Level Confidentiality Profile (Basic Profile) as defined in DICOM PS3.15 2021a - Security and System Management Profiles. This document provides details and interpretation of the Basic Profile and associated options. This document is meant to be accompanied by the DICOM De-Identification Form (Form) which describes deviations from the Basic Profile.

## De-Identification

Unless otherwise described by the Form, attributes specified in *Table E.1-1. Application Level Confidentiality Profile Attributes* SHALL be treated as described for the Basic Profile.

Unless otherwise described by the Form, the following attributes MUST be removed:

- Private attributes.
- Attributes not belonging to the IOD associated with the SOP Instance being de-identified.
- Original Attributes Sequence `0400,0561`

The following attributes WILL be considered to be included in Table E.1-1.

```
FrameAcquisitionDateTime    (0018,9074)
FrameReferenceDateTime      (0018,9151)
```

Patient Identity Removed `0012,0062` MUST be set to the value of `YES`.

The De-identification Method Code Sequence `0012,0064` MUST contain the following in addition to the entries defined below:

- CodeValue `0008,0100` MUST be set to `113100`.
- Code Meaning `0008,0104` MUST be set to `Basic Application Confidentiality Profile`.
- Coding Scheme Designator `0008,0102` MUST be set to `DCM`.
- Mapping Resource `0008,0105` MUST be set to `DCMR`.
- Context Group Version `0008,0106` MUST be set to `20170914`.
- Context Identifier `0008,010F` MUST be set to `7050`.
- Context UID `0008,0117` MUST be set to `1.2.840.10008.6.1.925`.
- Mapping Resource UID `0008,0118` MUST be set to `1.2.840.10008.2.​16.​4`.
- Mapping Resource Name `0008,0122` MUST be set to `DCMR`.

The File Meta Information must be cleaned.

The following clinical trial attributes MUST be set or removed.

```
ClinicalTrialCoordinatingCenterName                (0012,0060)
ClinicalTrialSponsorName                           (0012,0010)
ClinicalTrialProtocolID                            (0012,0020)
ClinicalTrialProtocolName                          (0012,0021)
ClinicalTrialSiteID                                (0012,0030)
ClinicalTrialSiteName                              (0012,0031)
ClinicalTrialSubjectID                             (0012,0040)
ClinicalTrialSubjectReadingID                      (0012,0042)
ClinicalTrialTimePointID                           (0012,0050)
ClinicalTrialTimePointDescription                  (0012,0051)
ClinicalTrialProtocolEthicsCommitteeName           (0012,0081)
ClinicalTrialProtocolEthicsCommitteeApprovalNumber (0012,0082)
```

All tags not addressed by the Basic Profile MUST be retained.

The following UID Attributes must be considered to be part of  

The Retain UIDs Option MUST NOT be used.

### Notes on De-identification Action Codes (Table E.1-1a)

- *D* - Dummy values MUST be chosen in a manner that ensures they can not be confused with original data.
- *U* - Internal consistency of ALL UIDs in the de-identified dataset MUST be ensured, including but not limited to SOPInstanceUID, SeriesInstanceUID, StudyInstanceUID, Instance Creator UID, etc. UIDs beginning with `1.2.840.10008.` MUST NOT be modified.
- The proper action codes MUST be used for the SOP Instance being de-identified.

## Form Interpretation

### Applicable IODs

Only the IODs specified will be de-identified. SOP classes pertaining to other IODs will not be treated.

*Some text on how to interpret the PII Cleaning form inlcuding what to add to the De-identification Method Code Sequence*

### Retain Descriptors

The specified attributes MUST be retained.

If Retain Request Attributes is specified, the Request Attributes Sequence Attribute `0040,0275` MUST be retained, and the nested attributes corresponding to the selected items MUST be retained or cleaned as described in the PII Cleaning form. All other attributes present in the Request Attributes Sequence Attribute MUST be removed.

- Requested Procedure Description `0032,1060`.
  - Requested Procedure Code Sequence `0032,1064`.
- Scheduled Procedure Step Description `0040,0007`.
  - Scheduled Protocol Code Sequence `0040,0008`.
- Reason for the Requested Procedure `0040,1002`.
  - Reason for Requested Procedure Code Sequence `0040,100A`.

### Retain References

If Retain references is specified, Referenced Image Sequence `0008,1140`, Referenced Instance Sequence `0008,114A`, Source Image Sequence `0008,2112`, and Source Instance Sequence `0042,0013` MUST be retained. All UID references MUST be replaced with internally consistent UIDs.

Note that Referenced Image Sequence and Source Image Sequence appear in the Basic Profile. The others would in principle not be removed even if this choice is not selected. No de-identification code is added when this option is specified.

### Retain Longitudinal Temporal Information

If Retain Times is specified and Retain all times is specified

If Retain Dates is specified, and Randomly Shift dates is not:

- Retain all dates specified in the Rtn. Long. Full Dates Opt. column of Table E.1-1. Do not retain time attributes specified based on this option.

If Retain Dates is specified, and Randomly Shift dates is also specified:

- .......


## Relinking