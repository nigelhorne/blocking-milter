# blocking-milter

## Overview

Sendmail filter designed to detect and reject spam emails based on specific blacklisted phrases in the email subject and body.
It integrates with Sendmail or Postfix using `Sendmail::PMilter` and logs rejected messages via syslog.
The filter allows connections from private IPs while scanning incoming emails for spam-related keywords,
rejecting those that match,
and returning a 554 5.7.1 error.

## Installation

In `/etc/mail/sendmail.mc`:

    INPUT_MAIL_FILTER(`blocking-milter', `S=local:/run/blocking-milter.sock, T=S:30s;R:3m')dnl

In `/etc/rc.local`:

	rm -f /run/blocking-milter
	/usr/local/etc/blocking-milter local:/run/blocking-milter&

Or put this is in `/etc/systemd/system/blocking-milter.service`:

    [Unit]
    Description=Block SEO, and similar, spams
    After=network.target
 
    [Service]
    ExecStart=/usr/local/etc/blocking-milter local:/run/blocking-milter
    KillMode=process
    Restart=on-failure

    [Install]
    WantedBy=multi-user.target

Rebuild sendmail:

    m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf
    service sendmail restart
