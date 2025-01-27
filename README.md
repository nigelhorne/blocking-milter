# blocking-milter

In C<sendmail.mc>:

    INPUT_MAIL_FILTER(`blocking-milter', `S=unix:/var/run/blocking-milter.sock, T=S:30s;R:3m')dnl

Rebuild sendmail:

    m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf
    service sendmail restart
