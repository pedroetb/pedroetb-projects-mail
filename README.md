# mail

## Configure domain

You must set the following DNS records, replacing variables between `<>`:

### DKIM

Used to sign emails. Private key can be found inside `dkim-vol` volume.

* Type: `TXT`
* Name: `mail._domainkey.<domain>`
* Value: `v=DKIM1; k=rsa; p=<private-key>`

### SPF

Used to prevent being considered as spam.

* Type: `TXT`
* Name: `<domain>`
* Value: `v=spf1 +a ~all`

### DMARC

Used to define what to do with spam.

* Type: `TXT`
* Name: `_dmarc.<domain>`
* Value: `v=DMARC1; p=quarantine; rua=mailto:postmaster@<domain>`

## Test mail server

First, create a file `email.txt` with a email example:

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
