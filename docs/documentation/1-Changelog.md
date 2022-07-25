---
tags: [Documentation]
---

# Changelog

We improve the Smokeball API all the time by releasing new features, fixing bugs and updating the documentation. This changelog accounts for all of the changes as they are released. 

## July 2022

#### Activities
* Fix `TaxInclusive` field returning incorrect value.

#### Files
* Added ability to add additional data on creation of a file (see [Files](https://smokeball.stoplight.io/docs/api-docs/4b58c83cc952a-add-file-to-a-matter)).

#### Roles
* Fix some [DELETE](https://smokeball.stoplight.io/docs/api-docs/64a5101608c79-remove-role-from-a-matter) endpoint issues.

#### Relationships
* Added [DELETE](https://smokeball.stoplight.io/docs/api-docs/36c4ce83b182f-remove-relationship-from-a-role-in-a-matter) relationship endpoint.

#### Tasks
* Added ability to link files and memos to tasks (see [TaskDocuments](https://smokeball.stoplight.io/docs/api-docs/3f91dd0d2662f-get-task-documents)).

#### Webhooks
* Added `MatterItems` webhooks.
* Fixed `MatterTypeId` field missing in some `matter.updated` event payloads.


## June 2022

#### Contacts
* Fix webhook events not triggering for deleted contacts from Smokeball native application.

#### Matters
* Added `Branch` field.
* Added `OtherSideIds` and `OtherSideRole` to POST request so matters and leads can be created with clients as well as othersides.
* Fix to paging url losing status on consecutive pages.

#### Memos
* Added `UserId` to POST and PUT requests to create or update a memo on behalf of a user.

#### SubTasks
* Added API.

## May 2022

#### Stages & StageSets
* Added API.

#### Contacts
* Added ability to restore contacts in PUT endpoint.
* Added ability to relate a person to a group of people.
* Added ability to add and update trusts.

#### Fees
* Removed check for existing `Fee` when performing PUT, requests are accepted and queued asynchonously like a POST.

#### Expenses
* Removed check for existing `Expense` when performing PUT, requests are accepted and queued asynchonously like a POST.

##### Webhooks
* Added `MatterStages` webhooks.
* Fixed `RequestId` not propogating from some Matter API calls.
* Fixed null fields on `Firm` update webhook events.


## March 2022

#### Firm
* Added `Logo` field to GET responses.

#### Contacts
* Added `MaritalStatus` field.
* Added `SpecialNeeds` field.

#### Tasks
* Added `CompletedDate` field.
* Added `LastUpdated` field to GET responses.


## February 2022

##### Firm
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