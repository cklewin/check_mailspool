# check_mailspool

This Nagios/NRPE plugin reports the number of emails in the mailspool for a user from a specified sender address or subject.

## Installing

This should be located on the host with access to your mailspool.  I recommend copying it into the same folder with your other NRPE plugins.

example:
```
cp check_mailspool /usr/local/libexec/nagios/check_mailspool
```

### Usage

```
check_mailspool -S <senderaddress> -s <spoolfile> -w <warn> -c <crit>

-S, --subject=STRING
   Subject of email
-F, --from=EMAILADDRESS
   Sender's address (ex. foo@bar.com)
-s, --spool=PATH
   Full path to the spool file (ex. /var/mail/foo)
-w, --warning=INTEGER
   Percentage strength below which a WARNING status will result
-c, --critical=INTEGER
   Percentage strength below which a CRITICAL status will result
```

### Add the command definition to your NRPE configuration

example:
```
command[check_spam_2]=/usr/local/libexec/nagios/check_mailspool -w10 -c20 -FMAILER-DAEMON -s/var/mail/cklewin
```

### Add the command definition to your Nagios configuration

example definition for Nagios checkcommands.cfg:
```
define command{
        command_name    check_spam_1
        command_line    $USER1$/check_nrpe2 -H $HOSTADDRESS$ -c check_spam_1
}
```

example definition for Nagios services.cfg:
```
define service {
        use                  generic-service-1
        host_name            Mail
        service_description  Spam Bounces
        check_command        check_spam_1
}
```
