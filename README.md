# blocking-milter

In F</etc/mail/sendmail.mc>:

    INPUT_MAIL_FILTER(`blocking-milter', `S=local:/run/blocking-milter.sock, T=S:30s;R:3m')dnl

In F</etc/rc.local>:
	rm -f /run/blocking-milter
	/usr/local/etc/blocking-milter local:/run/blocking-milter&

Or put this is in /etc/systemd/system/blocking-milter.service:

    [Unit]
    Description=Block resent spams
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
