# kwebs.cloud mail service

## Description

The service consists of a mailbox identified by an email address. The service provides the functionality to receive messages (as defined in [RFC5322](https://datatracker.ietf.org/doc/html/rfc5322)) to the mailbox, and to send messages from mailbox to other email addresses.

### Service parameters

The service (mailbox) is identified by

- email address (format defined in [RFC5322](https://datatracker.ietf.org/doc/html/rfc5322))
- password
- quota

## Accessing the service

The service can be accessed by any email client with the following parameters:

- IMAP (for accessing received mails)

  Host: mail.kwebs.cloud

  Port: 993 (IMAP over TLS)

  Username: < email address >

  Password: < password >

- SMTP (for sending mails)

  Host: mail.kwebs.cloud

  Port: 465 (SMTP/SUBMISSION over TLS)

  Username: < email address >

  Password: < password >

Furthermore, a web based service is povided at https://webmail.srv.kwebs.cloud.

## Additional features

### Inbound

Inbound mails are filtered using SpamAssasin, ClamAV, sending servers are verified by OpenDKIM and OpenDMARC software components.

### Outbound

Outbound mails are signed by OpenDKIM. Outbound mails are rate-limited. The recipient count is rate-limited, not the email messages. Current rate-limits are: 1 recipient/second, with a burst of 60 recipients.

That means, that instantly you can send a mail with a maximum of 60 recipients. Then, your remaining quota will be empty, and will recharge at a rate of 1 recipient/second until reaching 60.

### Admin site

- For domain administrators: [postfixadmin](https://postfixadmin.srv.kwebs.cloud/)
- For users: [postfixadmin user](https://postfixadmin.srv.kwebs.cloud/users/)

## Required DNS Settings

For `kwebs.cloud mail service` to be able to receive & send mails for a specific domain, the following entries must be present in the domain's zone:

```
; mx / for receiving mails
@               IN      MX      0 mx.kwebs.cloud.

; spf
@               IN      TXT     "v=spf1 redirect=_spf.kwebs.cloud"

; dkim
s01._domainkey	IN      CNAME   s01._domainkey.kwebs.cloud.
s02._domainkey	IN      CNAME   s02._domainkey.kwebs.cloud.
s03._domainkey	IN      CNAME   s03._domainkey.kwebs.cloud.
s04._domainkey	IN      CNAME   s04._domainkey.kwebs.cloud.

; dmarc
_dmarc          IN      CNAME   _dmarc.kwebs.cloud.
```

## Used software components

- https://github.com/kubernetize/postfix
- https://github.com/kubernetize/dovecot
- https://github.com/kubernetize/opendkim
- https://github.com/kubernetize/opendmarc
- https://github.com/kubernetize/roundcube
- https://github.com/kubernetize/postfixadmin
- https://github.com/rkojedzinszky/postfix-ratelimiter
