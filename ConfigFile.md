# Introduction #

/etc/ganglia-alert.conf controls the operation of the alert daemon.  It contains a list of config variables, and a list of alerts.

## Variables ##

`variable: value(s)`

| email\_to | comma-delimited list of email addresses to send alerts to report (none) |
|:----------|:------------------------------------------------------------------------|
| email\_from | from-address for email (userid@hostname)                                |
| group\_by | either 'host' or 'alert' ... for reporting (host)                       |
| log\_file |  logfile used to write first-alerts to (stdout)                         |
| pid\_file |  pidfile to write pid to for daemon-mode (/var/run/ganglia-alert.pid)   |
| gmetad\_server | gmetad server to connect to (localhost:8651)                            |
| digest\_secs |  number of seconds to spool alerts before sending a                     |
| sleep\_secs | number of seconds to sleep before polling gmetad (5)                    |
| include   |  include another config file                                            |
| subject   |  subject template for email ($id: $cnt alerts)                          |

## Alerts ##

`alert[/thresh]: expression`

or

`!name[/thresh]: expression`

Theshold is options and defaults to 1.  It's the number of consecutive times the expression must be nonzero to be considered an "alert".

Alert expressions are "Safe" container PERL expressions, and can take advantage of arithmetic logic.  IE:  $load\_one > $cpu\_num\*2.

If an alert has no name, the expression itself is used in the report.

Everything's processed in-order.  Alerts with the same name override others.

Host-specific alerts can be set up with:
{{host: name}}

Any alerts following that section are host-specific.  Hosts probably should be globs... but right now they are literal strings that must match the beginning of the hostname.  In the future, expressions that contain a '**' will be treated as a "glob" match.**

## Example ##

```

# This is a basic ruleset, demonstrating various capabilities

# digest_secs: 10
# sleep_secs: 5
# email_from: username@host
# email_to: -

email_to: earonesty@example.com
pid_file: /tmp/ganglia-alert.pid
group_by: host

log_file: /opt/log/ganglia-alert.log

alert: $disk_free < .05 * $disk_total
alert: $load_one > $cpu_num*2
alert/5: $load_one_tn > 5000
alert/2: $pkts_in > 6 * $pkts_in_stdev
alert/2: $pkts_out > 6 * $pkts_out_stdev

```