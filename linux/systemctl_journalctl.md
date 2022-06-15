## sytsemd
systemd deals with everything as 'unit'

systemd jounral
- kernel log
- system log messages
- system services that send output to standarad output or standard error
- audit records of SElinux messages

## journalctl

default location is in /run/log/journals  ,  **information would be lost on reboot**

to permanently keep the journal
```
mkidr -p /var/log/journal
systemd-tmpfiles --create --privfix /var/log/journal
```


system journal is a database file, not a regular flat text file
```
# log from old to new
journalctl

# latest on the top
journalctl -r 

# jump to the end of log file
journalctl -e

# latest 10 entries
journalctl -n 10

# -f follow the journal
journalctl -f

# -f -u follow with a specific unit
jounralctl -f -u httpd.service

# see the structure of jounral
journalctl -o verbose
journalctl -o jsonpretty

# input to the jounal
echo "hi there" | systemd-cat

-x show log lines with some text that comes from the message cata log
-k only shows the knernel messages
```
