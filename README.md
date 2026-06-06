# aiauto

This repository contains an n8n workflow export for the `Spam Classifier` automation.

## BeAware Outlook inbox configuration

The workflow's first active node is `Outlook Trigger - BeAware Inbox`. It is configured to poll the Microsoft Outlook Inbox for unread incoming messages addressed to `BeAware@tpcentralodisha.com` in the `To` or `Cc` recipients.

## Where to pass the BeAware mailbox credentials

Do **not** hardcode the `BeAware@tpcentralodisha.com` mailbox password in `Spam Classifier.json`. n8n workflow exports only reference credentials by name/id; the actual login is created in the n8n UI and stored encrypted by n8n.

Use these steps after importing `Spam Classifier.json` into n8n:

1. Open n8n.
2. Go to **Credentials**.
3. Click **New**.
4. Select **Microsoft Outlook OAuth2 API**.
5. Name the credential **BeAware Outlook OAuth2 Credential**.
6. Complete the Microsoft login/consent flow using `BeAware@tpcentralodisha.com`, or a Microsoft 365 account that has delegated/shared-mailbox access to `BeAware@tpcentralodisha.com`.
7. Open the workflow node **Outlook Trigger - BeAware Inbox**.
8. In the node's **Credential for Microsoft Outlook OAuth2 API** field, select **BeAware Outlook OAuth2 Credential**.
9. Open the **Send Result** Outlook node and select the same credential if the workflow should send results from the BeAware mailbox.
10. Save and activate the workflow.

The workflow JSON now uses the credential display name **BeAware Outlook OAuth2 Credential** for the Outlook trigger and send nodes so that the correct credential is easy to select after import.

> If `BeAware@tpcentralodisha.com` is not hosted on Microsoft 365/Outlook and is only available through generic webmail/IMAP, this Outlook workflow cannot log in with that webmail password directly. In that case, replace the trigger with n8n's IMAP Email Trigger node and create an IMAP credential instead.
