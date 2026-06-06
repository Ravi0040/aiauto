# aiauto

This repository contains an n8n workflow export for the `Spam Classifier` automation.

## BeAware on-premises Exchange mailbox configuration

The `BeAware@tpcentralodisha.com` mailbox is treated as an **on-premises Exchange mailbox**, not as Microsoft 365/Office 365 Outlook.

That means the workflow does **not** use Microsoft Graph or Microsoft Outlook OAuth to log in. Instead, the workflow uses standard Exchange mail protocols:

- **IMAP** to read incoming email from the BeAware inbox.
- **SMTP** to send the analysis result.

The workflow's first active node is `Exchange IMAP Trigger - BeAware Inbox`. It uses n8n's Email Trigger (IMAP) node to connect to the on-premises Exchange IMAP/CAS endpoint and read incoming messages from `INBOX`.

The trigger is configured to:

- connect with the n8n credential named **BeAware Exchange IMAP Credential**;
- read the `INBOX` mailbox;
- search for unseen/new messages;
- mark each processed message as read so the same email is not processed repeatedly;
- pass normalized email fields into the rest of the spam-classifier workflow.

## Where to pass the BeAware mailbox credentials

Do **not** hardcode the `BeAware@tpcentralodisha.com` mailbox password in `Spam Classifier.json`. n8n workflow exports only reference credentials by name/id; the actual login is created in the n8n UI and stored encrypted by n8n.

Use these steps after importing `Spam Classifier.json` into n8n:

1. Ask the Exchange administrator to confirm that IMAP is enabled for the on-premises Exchange server and for the BeAware mailbox.
2. Ask for the Exchange IMAP/CAS hostname, port, SSL/TLS requirement, and accepted username format.
3. Open n8n.
4. Go to **Credentials**.
5. Click **New**.
6. Select **IMAP**.
7. Name the credential **BeAware Exchange IMAP Credential**.
8. Set **User Name** to the format required by your Exchange server. Common formats are `BeAware@tpcentralodisha.com`, `DOMAIN\username`, or a service account that has access to the BeAware mailbox.
9. Enter the mailbox password, app password, or approved service-account password.
10. Enter the on-premises Exchange IMAP/CAS host, for example `mail.tpcentralodisha.com` or the hostname provided by your Exchange administrator.
11. Use port `993` with SSL/TLS unless your Exchange administrator provides a different value.
12. Save the credential.
13. Open the workflow node **Exchange IMAP Trigger - BeAware Inbox**.
14. In the node's **Credential to connect with** field, select **BeAware Exchange IMAP Credential**.
15. Save and activate the workflow.

## Sending results with on-premises Exchange SMTP

The `Send Result` node uses n8n's **Send Email** node. To send through on-premises Exchange, create a separate SMTP credential:

1. Go to **Credentials → New → SMTP**.
2. Name the credential **BeAware Exchange SMTP Credential**.
3. Enter the Exchange SMTP server or internal relay host provided by your Exchange administrator.
4. Use the correct port and security mode for your environment. Common examples are port `587` with STARTTLS for authenticated SMTP or port `25` for an internal relay.
5. Enter the approved username/password if your Exchange SMTP endpoint requires authentication.
6. Open the **Send Result** node and select **BeAware Exchange SMTP Credential**.

## Exchange administrator checklist

Before activating the workflow, confirm these details with the Exchange administrator:

- IMAP is enabled on the Exchange server/client access service.
- IMAP is enabled for `BeAware@tpcentralodisha.com` or for the service account used by n8n.
- The n8n server can reach the Exchange IMAP host and port, usually TCP `993`.
- SMTP relay or authenticated SMTP is allowed from the n8n server.
- The n8n server can reach the Exchange SMTP host and port, commonly TCP `587` or `25`.
- The chosen account has permission to read the BeAware mailbox and send the result email.
