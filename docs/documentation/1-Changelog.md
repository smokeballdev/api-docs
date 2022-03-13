---
tags: [Documentation]
---

# Changelog

## 2022-03-13 (v8.2)

#### Firms
* Added `Logo` field to GET responses.

#### Contacts
* Added `MaritalStatus` field.
* Added `SpecialNeeds` field.

#### Tasks
* Added `CompletedDate` field.
* Added `LastUpdated` field to GET responses.


## 2022-02-07 (v8.1)

##### Firms
* Added validation when creating a firm to ensure the user is not associated to another firm.
* Added Product when creating a firm with 3 product types `(BillingOnly, SmokeballMid and Smokeball)`.
* Added `IsInternal` field when creating a firm so that a firm can be flagged as an internal firm and skip payment subscription.
* Added `ABN`, `ACN` and postal details, `CareOf`, `PoBoxType` and `PoBoxNumber` to firm MailingAddress.

##### Users
* Added functionality to create users with no password, and email is sent out with a temporary password.

##### Matters
* Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  * `MatterTypeId`, instead use `MatterType`.
  * `ClientIds`, instead use `Clients`.
  * `OtherSideIds`, instead use `OtherSides`.
  * `PersonResponsibleStaffId`, instead use `PersonResponsible`.
  * `PersonAssistingStaffId`, instead use `PersonAssisting`.
  * `OriginatingStaffId`, instead use `OriginatingStaff`.
* Added OriginatingStaff to matter.

##### Contacts
* Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  * `StaffIds`, instead use `Staff`.
  * `PersonIds`, instead use `People`.

##### Roles
* Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  * `ContactId`, instead use `Contact`.
  * `RepresentativeIds`, instead use `Representatives`.
  * `RelationshipIds`, instead use `Relationships`.
* Added DELETE role endpoint for removing roles from a matter.
* Added `IsOtherSide` field to determine if the role is the OtherSide of a matter.
* Fixed issues with Layout subitems not being created.
 
##### Relationships
* Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  * `ContactId`, instead use `Contact`.
  * `RepresentativeIds`, instead use `Representatives`.

##### Files
* Added `UserId` to files and folders.

##### Webhooks
* Add `LayoutMatterItem` webhooks.