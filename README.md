# mail

## Configure domain

You must set the following DNS records at your public domain, replacing variables between `<>`:

### DKIM

Used to verify content of signed emails. Public key can be found inside `dkim-vol` volume (`dkim.public` file).

* Type: `TXT`
* Name: `mail._domainkey.<domain>`
* Value: `v=DKIM1; k=rsa; p=<public-key>`

### SPF

Used to prevent being considered as spam, validating which domains are allowed to send emails.

* Type: `TXT`
* Name: `<domain>`
* Value: `v=spf1 +a +include:_spf.google.com ~all`

### DMARC

Used to define what to do with incoming spam.

* Type: `TXT`
* Name: `_dmarc.<domain>`
* Value: `v=DMARC1; p=quarantine; rua=mailto:postmaster@<domain>`

## Test mail server

First, create a file `email.txt` with a email example (including required headers and content):

```
From: mail <mail@<domain>>
To: postmaster <postmaster@<domain>>
Subject: email test
Date: Thu, 23 May 2019 23:04:16

Test content.
```

Then, run this command to send a test email:

```
curl smtp://<domain> \
--mail-from mail@<domain> \
--mail-rcpt postmaster@<domain> \
--upload-file email.txt
```

Note that `mail@<domain>` does not exist, but `postmaster@<domain>` is defined as an alias by default.
All received emails will be resent to the external address, defined by the `EXTERNAL_EMAIL_ADDRESS` environment variable.

You can also send an email to `postmaster@<domain>` (or any defined alias account) directly from other email service provider.
