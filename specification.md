# DICOM De-Identification Specification

## Definitions

Basic Profile - Basic Attribute Level Confidentiality Profile as defined in DICOM PS3.15 2021a.
SOP Class - Service Object Pair Class. Definition of an action (Operation) that can performed on an Object (IOD).
SOP Instance - Service Object Pair Instance. In practice the atomic unit of storage in DICOM.
IOD - Information Object Definition. List of attribute elements found in a type DICOM object. Attributes in an MRI for example.

## Preamble

De-identification will be based on on the Basic Attribute Level Confidentiality Profile (Basic Profile) as defined in DICOM PS3.15 2021a - Security and System Management Profiles. This document provides details and interpretation of the Basic Profile and associated options. This document is meant to be accompanied by the DICOM De-Identification Form (Form) which describes deviations from Basic Profile.

## De-Identification

Unless otherwise described by the Form, attributes specified in *Table E.1-1. Application Level Confidentiality Profile Attributes* SHALL be treated as described for the Basic Profile.

Unless otherwise described by the Form, the following attributes MUST be removed:

- Private attributes.
- Attributes not belonging to the IOD associated with the SOP Instance being de-identified.

All other attributes SHALL be retained.

The Retain UIDs Option MUST NOT be used.

### Notes on De-identification Action Codes (Table E.1-1a)

- *D* - Dummy values MUST be chosen in a manner that ensures they can not be confused with original data.
- *U* - Internal consistency of ALL UIDs in the de-identified dataset MUST be ensured, this specifically includes UIDs other than SOPInstanceUID, SeriesInstanceUID, and StudyInstanceUID.

All tags not addressed by the Basic Profile MUST be retained.

## Relinking