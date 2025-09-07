# Google Workspace

Google Workspace is used to manage e-mail.

In order to send e-mails from the backend, we use SMTP.

We use the following two Google Support articles.

https://web.archive.org/web/20250517200708/https://support.google.com/a/answer/176600?hl=en
https://web.archive.org/web/20250508111359/https://support.google.com/a/answer/2956491

We choose an SMPT relay, currently called "SMTP voor e-mail versturen via code", with TLS required, only allowed IP addresses and SMTP authentication disabled. SMTP authentication is disabled because IP addresses are used to authenticate, which means we don't have to worry about app passwords or other complexity.

Currently, allowed IP addresses should only be home networks of .ComCom members that need to test it as well as the backend Hetzner server.

To send an email, all you need is to connect to smtp-relay.gmail.com:587 using SMPT (with TLS enabled) from an allowed IP address. Then, you can just send an email with SMTP.

In Python, using smtplib works perfectly. However, in some languages (like Go), it's important to make sure the local address name is set to your actual IP address. This sometimes requires sending an EHLO request first.