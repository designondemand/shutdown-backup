# shutdown-backup
Systemd services to run a unison profile on shutdown.

When correctly setup will run the selected unison profile whenever the computer is shutdown or restarted but only once a week.

## requirements
- [systemd](http://www.freedesktop.org/wiki/Software/systemd/)
- [unison](http://www.cis.upenn.edu/~bcpierce/unison/)

## installation
Download the services
```
$ git clone https://github.com/designondemand/shutdown-backup.git
```
Install the service files into systemd
```
$ cd shutdown-backup
$ ./install.sh
```

## usage
to enable automatic weekly backups on shutdowns enanble and start the `shutdown-backup-enable@.timer` unit with the profile name as the instance variable
```
$ systemctl enable shutdown-backup-enable@profile-name.timer
$ systemctl start shutdown-backup-enable@profile-name.timer
```

to manually enable the backup to be run at the next shutdown just enable the `shutdown-backup@.service` service with the profile name as the instance variable
```
$ systemctl enable shutdown-backup@profile-name
```

to run the backup straight away just start the service instead of enabling it
```
$ systemctl start shutdown-backup@profile-name
```

## unison setup
the above examples assume you have a unison profile called profile-name at /root/.unison/profile-name.prf

the easiest way to set this up is to create and test the profile in your home directory using something like unison-gtk and then copy it (and it's corresponding archive file) it to the /root/.unison/ directory.

## set when the backup is run
by default the backup is set to be run on a weekly basis. The easiest way to do this is to add a [drop-in snippet](https://wiki.archlinux.org/index.php/systemd#Drop-in_snippets) for the `shutdown-backup-enable@.timer` unit and set the OnCalendar parameter to the desired value
```
$ systemctl edit shutdown-backup-enable@profile-name.timer
```

to set the backup to run on Wednesdays and Fridays add the following to the drop-in snippet
```
[Timer]
OnCalendar=Wed,Fri
```
