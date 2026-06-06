# aiauto

This repository contains an n8n workflow export for the `Spam Classifier` automation.

## BeAware Outlook inbox configuration

The workflow's first node is now `Outlook Trigger - BeAware Inbox`. It is configured to poll the Microsoft Outlook Inbox for unread incoming messages addressed to `BeAware@tpcentralodisha.com` in the `To` or `Cc` recipients.

After importing the workflow into n8n, open the trigger node and attach a Microsoft Outlook OAuth2 credential that signs in as `BeAware@tpcentralodisha.com`, or an account that has delegated/shared-mailbox access to that mailbox. The workflow JSON can store the intended credential name, but n8n still requires the actual OAuth credential to be connected in the n8n UI/runtime.
