# aiauto

This repository contains an n8n workflow export for the `Spam Classifier` automation.

## BeAware incoming mailbox configuration

The workflow's first active node is now `IMAP Trigger - BeAware Inbox`. It uses n8n's Email Trigger (IMAP) node to log in to the `BeAware@tpcentralodisha.com` mailbox and read incoming messages from `INBOX`.

The trigger is configured to:

- connect with the n8n credential named **BeAware IMAP Credential**;
- read the `INBOX` mailbox;
- search for unseen/new messages;
- mark each processed message as read so the same email is not processed repeatedly;
- pass normalized email fields into the rest of the spam-classifier workflow.

## Where to pass the BeAware mailbox credentials

Do **not** hardcode the `BeAware@tpcentralodisha.com` mailbox password in `Spam Classifier.json`. n8n workflow exports only reference credentials by name/id; the actual login is created in the n8n UI and stored encrypted by n8n.

Use these steps after importing `Spam Classifier.json` into n8n:

1. Open n8n.
2. Go to **Credentials**.
3. Click **New**.
4. Select **IMAP**.
5. Name the credential **BeAware IMAP Credential**.
6. Set **User Name** to `BeAware@tpcentralodisha.com`.
7. Enter the mailbox password or app password.
8. Enter the IMAP host supplied by your mail administrator, for example `imap.your-mail-provider.com`.
9. Use port `993` with SSL/TLS unless your mail administrator provides a different value.
10. Save the credential.
11. Open the workflow node **IMAP Trigger - BeAware Inbox**.
12. In the node's **Credential to connect with** field, select **BeAware IMAP Credential**.
13. Save and activate the workflow.

## Sending results with SMTP

The `Send Result` node now uses n8n's **Send Email** node, so it works with webmail providers that expose SMTP. If you want the workflow to email analysis results, create **Credentials → New → SMTP**, name it **BeAware SMTP Credential**, and enter the SMTP host, port, username, password/app-password, and SSL/TLS settings supplied by your mail administrator.
