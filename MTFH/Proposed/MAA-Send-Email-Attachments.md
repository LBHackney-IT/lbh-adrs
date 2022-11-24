# Send File by Gov Notify Email

### **Date:** 11th March 2022

### **Status:** PROPOSED

## **Context**

Income officers want to send letters they have generated from MAA applications via email.

Income officers already have the ability to send email from MAA but no documents can be associated with the message to the customer.

Income officers also send letters such as notices they have generated from MAA applications using the postal service via Gov Notify. The documents are accessible to hackney officers with permission to the Gov Notify portal. Gov Notify retains these documents for up to 7 days.

Documents sent with email will not be accessible for Hackney officers via gov notify portal to view or cancel sending (a feature they have for letters sent by post).

## **Further Details**

According to the [pricing documentation](https://www.notifications.service.gov.uk/pricing) for Gov Notify, there is no additional cost to using their email service.
Architecture and Hackney process for sending file by Gov Notify

The architecture for sending an email by Gov Notify is described below (For Gov Notify documentation, [click here](https://docs.notifications.service.gov.uk/ruby.html#architecture-for-sending-an-email)):

![Architecture](./doc-images/notify-diagram.png)

In the case of sending a file by email, it will be using the same process for a template email. The file will be uploaded to Gov Notify when the email is submitted by the user. A user editable template will be used in MAA to create the email body. This will merge into the Gov Notify email template. The uploaded document will become a linked document in the email.

Gov notify will manage the uploaded file including its 18 months retention for the customer. In their [documentation they state](https://docs.notifications.service.gov.uk/ruby.html#send-a-file-by-email):

![Send File](./doc-images/send-file.png)

The uploaded documentation linked in the email will not be accessible to users of Gov Notify portal.

_MMA will retain the emailed record and documents sent to Gov Notify_

The send file by Gov Notify email will use the existing document retention process used by the application.

When a document is created in MAA, as part of the process it is uploaded into AWS S3. A unique uuid is associated with the filename and a record of the meta data is saved in the MySQL documents table. Users are able to view some of the meta data in the tenancy profile view documents. They are also able to download the document via MAA if they need to access it later.

The content of the email when sent, including some of the metadata for the document created will be saved in the action diary notes (e.g. date period for the rent statement in the document).

Here is a high level overview of the process. The steps where documents are stored are highlighted in green and red.

![Process Overview](./doc-images/process-overview.png)

## **Decision**

MAA already has a feature to send email via Gov Notify. This proposes to expand that use case to send a file by Gov Notify email The documents are linked for the customer up to 18 months and are not attached to the email. These documents will not be accessible for Hackney officers via Gov Notify portal to view or cancel sending.

## **Consequences**

There will be an additional way for income officers to send letters from MAA, instead of copying the document to their mailbox or sending it via post which has a greater cost to Hackney.
