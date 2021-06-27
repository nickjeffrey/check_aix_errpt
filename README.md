# check_aix_errpt
nagios check for messages in AIX error report

# Requirements
ksh, ssh  on nagios server

# Configuration

This script is executed remotely on a monitored system by the NRPE or check_by_ssh  methods available in nagios.

If you hare using the check_by_ssh method, you will need a section in the services.cfg file on the nagios server that looks similar to the following.
This assumes that you already have ssh key pairs configured.
```
    define service {
       use                             generic-24x7-service
       hostgroup_name                  all_aix
       service_description             AIX errpt
       check_command                   check_by_ssh!/usr/local/nagios/libexec/check_aix_errpt
       }
```

Alternatively, if you are using the NRPE method, you should have a section similar to the following in the services.cfg file:
```
    define service{
       use                             generic-24x7-service
       hostgroup_name                  all_aix
       service_description             AIX errpt
       check_command                   check_nrpe!check_aix_errpt -t 30
       notification_options            w,c,r                    ; Send notifications about warn, critical, and recovery events
       }
```

If you are using the NRPE method, you will also need a command definition similar to the following on each monitored host in the /usr/local/nagios/nrpe/nrpe.cfg file:
```
    command[check_aix_cpu]=/usr/local/nagios/libexec/check_aix_errpt
```
