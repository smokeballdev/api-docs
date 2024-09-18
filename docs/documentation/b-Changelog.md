---
tags: [Documentation]
---

# Changelog

We improve the Smokeball API all the time by releasing new features, fixing bugs and updating the documentation. This changelog accounts for all of the changes as they are released, in chronological order.

### September 2024
- **FirmUsers**: Fixed intermittent issue where staff was not being set to former when FirmUser mapping is removed.
- **ReferralTypes**: Added ReferralTypes API.

### August 2024
- **Matters**: Added ability to search for matters using the GET endpoint.
- **Matters**: Lead PATCH endpoint bug fixes.
- **Invoices**: Added ability to download finalized invoice pdfs.

### June 2024
- **Matters**: Matters v2 API is now standard and the Api-Version header is no longer required.
- **Invoices**: Added debtors to API payload.
- **Webhooks**: Added `invoice.updated` event type.

### May 2024
- **Contacts**: UTBMS (US only) and Citizenship details fields added.
- **Expenses**: Previously invoiced externally expenses can now be unfinalized.
- **Firm**: Added firm status
- **MatterTypes**: Categories are now filtered by account white label.

### April 2024
- **Events**: Added `MatterId` to GET endpoint.
- **Expenses**: Auto-assign callng staff if staff id is not provided.
- **Fees**: Auto-assign callng staff if staff id is not provided.
- **Firm**: Added AddOns to GET endpoint.
- **Matters**: Added missing account validation checks.

### February 2024

- **Fees**: Added `CreatedFromActivityId` to GET endpoint.
- **Fees**: Resolved issues where StaffId could cause PATCH failures
- **Webhooks**: Added `files.updated` event type which is triggered against a matter. Use the cursor returned to rereive the updated files via the files API.

### January 2024

- **Files**: Added `IsCancelled` and `IsUploaded` to GET endpoint.
- **Matters**: Resolved issues with incorrect matter access permissions.

### November 2023

- **Contacts**: Added `correctionsReferenceNumber` and `centrelinkReferenceNumber` to AU endpoints.
- **Expenses**: Added PATCH endpoint.
- **Fees**: Added PATCH endpoint.
- **Files**: Added `isFavorite` field.
- **Folders**: Added folder `name` to GET endpoint.
- **Webhooks**: Added `matter.converted` event type which is triggered once a lead is converted to a matter.

### October 2023

- **Fees**: Added `isInvoiceExternally` to GET.
- **Matters**: Added additional validation for `ClientIds` and `OtherSideIds`.
- **Matters**: Added `referralType`, `referrerId`, `closedDate`, `leadOpenedDate`, `leadClosedDate`, `leadClosedReason` fields.
- **Matters**: Fixed issue where matters could not be created with more than one client.
- **MatterTypes**: Added restrictions based on account white label.
- **Memos**: Fixed data issue using GET endpoint.
- **Tasks**: Add search by `matterId`.

### August 2023

- **Expenses**: Fix issue where the `Finalized` field was being ignored on POST and PUT endpoints.
- **Files**: Fix issue preventing newly added files from being indexed for search.
- **Files**: Fix issue where newly added files are being added to deleted folders. A new folder is now added if the file is being added to a deleted folder.
- **Firm**: Added `productId` field to GET endpoint.
- **Firm**: Added attorney/solicitor licence numbers to GET endpoint.
- **Matters**: Fix issue preventing matter creation when multiple clients are specified.
- **Matters**: Several improvements and fixes to the beta PATCH endpoint including:
  - Added additional validation to multiple fields.
  - Ensured changes made via the PATCH endpoint reflect in the matter history correctly.
- **MatterTypes**: Fix issue exposing deleted matter types from GET endpoint.
- **Staff**: Added attorney/solicitor licence numbers to GET endpoint.
- **Tasks**: Added missing validation to the `MatterId` and `StaffId` fields. These fields must now be valid GUID values, empty strings are no longer permitted. The `StaffId` is mandatory.

### July 2023

- **Expenses**: Added `AssignToFirmOwner` option when creating an expense.
- **Files**: Added `DateModified` field when creating a file.
- **Files**: Added `DateModified` field when bulk creating files.
- **Firm**: Deprecated the `Product` field and introduced the `ProductId` field.
- **Firm**: Added support for the Boost Product.
- **Matters**: Fix issue with combining search filters, e.g. `IsLead` and `ContactId`.

### May 2023

- **Staff**: Added `ColorFill` and `ColorStroke` fields.
- **Webhooks**: Improvements to `Matter` webhooks, notifications are more accurate aswell as more deterministic.
- **Webhooks**: Added `Memo` webhooks. Use the `memo.created`, `memo.updated`, `memo.deleted`, `memo.restored` event types to listen to chages made to memos. 

### March 2023

- **Archive**: Added Archives API.
- **Events**: Added Events API.
- **Expenses**: Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  - `StaffId`, instead use `Staff`.
  - `MatterId`, instead use `Matter`.
- **Fees**: Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  - `StaffId`, instead use `Staff`.
  - `MatterId`, instead use `Matter`.
- **Webhooks**: Added `Task` webhooks. Use the `task.created`, `task.updated`, `task.deleted` event types to listen to chages made to tasks.
- **Webhooks**: Added `Event` webhooks. Use the `event.created`, `event.updated`, `task.deleted` event types to listen to changes made to events.
- **Webhooks**: Added `LayoutDesign` webhooks. Use the `layoutdesign.updated` event type to listen to changes made to layout designs.
- **Webhooks**: Added `Archive` webhooks. Use the `archive.updated` event type to listen to changes made on a matters archive details.

### January 2023

- **Activities**: Fixed issues with activity code(s) not being deleted.
- **BillingConfiguration**: Added `BillingConfiguration` [GET](22c98d537baca-get-matter-billing-configuration) and [PUT](0878ea1284fa5-update-matter-billing-configuration) endpoints. Billing configurations are set on the matter level.
- **Contacts**: Added `Locality` and `County` fields.
- **Contacts**: GroupOfPeople are no longer permitted in the UK.
- **Files**: Added `DateModified` field to GET responses.
- **Firm**: Fixed issues updating `Email` field.
- **Firm**: Fixed issues updating `DxAddress` field.
- **Matters**: Added `Supervisor` field. 
- **Matters**: Added ability to filter by multiple `status`. Multiple statuses can now be passed in the query parameters.
- **Roles**: Prevent adding roles that are not permitted for the specified matter.
- **Staff**: Added `UserId` field.
- **TaskDocuments**: Fixed task document(s) not being added to tasks.
- **Webhooks**: Added `BillingConfiguration` webhooks. Use the `billingconfiguration.updated` event type to listen to changes to billing configurations on a matter level.

### November 2022

- **Activities**: Added additional validation for activity codes. Codes can no longer be more than 20 characters long.
- **Contacts**: Added `Name` field to GET response for `GroupOfPeople` contacts. This returns the combined names of the contacts in the group, saving you needing to retrieve the individual contacts.
- **Files**: Added [PATCH](89218c51253ef-patch-version-or-metadata-of-a-file) endpoint
- **Firm**: Added `Owner` field to GET responses. This can be used to determine the whitelabel product linked to the firm.
- **MatterTypes**: Added `Type` field to GET responses. This can be used to determine if the type is for a Lead or Matter.
- **Tasks**: Added `Duration` field. This follows the `ISO 8601` duration format (e.g. `PT4H33M`)
- **Tasks**: Fixed categories not saving when adding or updating tasks.
- **Webhooks**: Added `error` event type. This can be used to listen to errors when your API requests fail, see [Error Handling](3d812ecec78d9-webhooks#34-error-handling).


### September 2022

- **Contacts**: Added ability to exclude deleted contacts from GET response (see `ExcludeDeletedContacts` [query parameter](0d6f4aba1a214-get-contacts)).
- **Contacts**: Added `BirthFirstName`, `BirthMiddleName`, `BirthLastName` fields.
- **Contacts**: Added `PreviousNames` field.
- **Contacts**: Added `LanguageOfInterpreter` field.
- **Contacts**: Added free text option for `Gender` field.
- **Contacts**: Fixed validation issues when creating new contacts.
- **Matters**: Added ability to filter matters by contact id (see `ContactId` [query parameter](40aa708f63ce3-get-matters)).
- **Matters**: Matter access can now be restricted by `UserId` when using Client Credentials grant (see [Acting on behalf of a user](91f30a1eaee08-making-requests#31-acting-on-behalf-of-a-user)).
- **Leads**: Restricted leads API to Grow and Prosper tiered clients.
- **Files**: File access can now be restricted by `UserId` when using Client Credentials grant (see [Acting on behalf of a user](91f30a1eaee08-making-requests#31-acting-on-behalf-of-a-user)).

### July 2022

- **Activities**: Fix `TaxInclusive` field returning incorrect value.
- **Files**: Added ability to add additional data on creation of a file (see [Files](4b58c83cc952a-add-file-to-a-matter)).
- **Roles**: Fix some [DELETE](64a5101608c79-remove-role-from-a-matter) endpoint issues.
- **Relationships**: Added [DELETE](36c4ce83b182f-remove-relationship-from-a-role-in-a-matter) relationship endpoint.
- **Tasks**: Added ability to link files and memos to tasks (see [TaskDocuments](3f91dd0d2662f-get-task-documents)).
- **Webhooks**: Added `MatterItems` webhooks.
- **Webhooks**: Fixed `MatterTypeId` field missing in some `matter.updated` event payloads.

### June 2022

- **Contacts**: Fix webhook events not triggering for deleted contacts from Smokeball native application.
- **Matters**: Added `Branch` field.
- **Matters**: Added `OtherSideIds` and `OtherSideRole` to POST request so matters and leads can be created with clients as well as othersides.
- **Matters**: Fix to paging url losing status on consecutive pages.
- **Memos**: Added `UserId` to POST and PUT requests to create or update a memo on behalf of a user.
- **SubTasks**: Added SubTasks API.

### May 2022

- **Stages & StageSets**: Added Stages and StagesSets API.
- **Contacts**: Added ability to restore contacts in PUT endpoint.
- **Contacts**: Added ability to relate a person to a group of people.
- **Contacts**: Added ability to add and update trusts.
- **Fees**: Removed check for existing `Fee` when performing PUT, requests are accepted and queued asynchonously like a POST.
- **Expenses**: Removed check for existing `Expense` when performing PUT, requests are accepted and queued asynchonously like a POST.
- **Webhooks**: Added `MatterStages` webhooks.
- **Webhooks**: Fixed `RequestId` not propogating from some Matter API calls.
- **Webhooks**: Fixed null fields on `Firm` update webhook events.

### March 2022

- **Firm**: Added `Logo` field to GET responses.
- **Contacts**: Added `MaritalStatus` field.
- **Contacts**: Added `SpecialNeeds` field.
- **Tasks**: Added `CompletedDate` field.
- **Tasks**: Added `LastUpdated` field to GET responses.

### February 2022

- **Firm**: Added validation when creating a firm to ensure the user is not associated to another firm.
- **FIrm**: Added Product when creating a firm with 3 product types `(BillingOnly, SmokeballMid and Smokeball)`.
- **Firm**: Added `IsInternal` field when creating a firm so that a firm can be flagged as an internal firm and skip payment subscription.
- **Firm**: Added `ABN`, `ACN` and postal details, `CareOf`, `PoBoxType` and `PoBoxNumber` to firm MailingAddress.
- **Users**: Added functionality to create users with no password, and email is sent out with a temporary password.
- **Matters**: Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  - `MatterTypeId`, instead use `MatterType`.
  - `ClientIds`, instead use `Clients`.
  - `OtherSideIds`, instead use `OtherSides`.
  - `PersonResponsibleStaffId`, instead use `PersonResponsible`.
  - `PersonAssistingStaffId`, instead use `PersonAssisting`.
  - `OriginatingStaffId`, instead use `OriginatingStaff`.
- **Matters**: Added OriginatingStaff to matter.
- **Contacts**: Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  - `StaffIds`, instead use `Staff`.
  - `PersonIds`, instead use `People`.
- **Roles**: Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  - `ContactId`, instead use `Contact`.
  - `RepresentativeIds`, instead use `Representatives`.
  - `RelationshipIds`, instead use `Relationships`.
- **Roles**: Added DELETE role endpoint for removing roles from a matter.
- **Roles**: Added `IsOtherSide` field to determine if the role is the OtherSide of a matter.
- **Roles**: Fixed issues with Layout subitems not being created.
- **Relationships**: Deprecated the following fields in the response payloads, this includes the fields in the Webhooks payload:
  - `ContactId`, instead use `Contact`.
  - `RepresentativeIds`, instead use `Representatives`.
- **Files**: Added `UserId` to files and folders.
- **Webhooks**: Add `LayoutMatterItem` webhooks.