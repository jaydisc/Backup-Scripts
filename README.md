# Backup-Scripts
Backup scripts for Linux and OS X servers

These are scripts I've developed over the years to assist in backing up OS X or Linux Servers and reporting data back to me.

I always store the scripts in /usr/local/bin. Each script usually performs one function. I then configure backup-report to include all of the individual scripts as required by any server, e.g.

    #!/bin/bash
    /usr/local/bin/backup-df
    /usr/local/bin/backup-postqueue
    /usr/local/bin/backup-backupd
    /usr/local/bin/backup-mysql
    /usr/local/bin/backup-rsync
    exit 0;

The backup-report script is usually trigger by the mail-backup-report script which emails the output of all of the other scripts:

    #!/bin/bash
    /usr/local/bin/backup-report | /usr/bin/mail -s 'Backup Report' 'support@example.com'

It is common to have one server's backup-report, collect the output of other server backup-reports. Usually, it just requires configuring the ssh host in ~/.ssh/config and adding a line to the backup-report:

    #!/bin/bash
    /usr/local/bin/backup-df
    /usr/local/bin/backup-postqueue
    /usr/local/bin/backup-backupd
    /usr/local/bin/backup-mysql
    /usr/local/bin/backup-rsync
    ssh host '/usr/local/bin/backup-report'
    exit 0;

Sometimes, there are a few scripts with similar names, e.g.

backup-opendirectory (the actual script)
backup-opendirectory-logger (the script that runs the above script and writes it to a log)
backup-opendirectory-report (the script that collects the output from the logger)

The purpose of these are for situations where a more elevated user is required to perform the actual script (the logger), so that when an external server connects via ssh (not as root), it can still collect the output.