# shutdown-backup
Systemd services to run a unison profile on shutdown.
When correctly setup will run the selected unison profile when the computer is shutdown or restarted every Wednesday and Friday

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
to enable automatic backups on shutdowns on Wednesdays and Fridays enanble and start the `shutdown-backup-enable@.timer` unit with the profile name as the instance variable
```
$ systemctl enable shutdown-backup-enable@profile-name.timer
$ systemctl start shutdown-backup-enable@profile-name.timer
```

to manually enable the backup to be run at the next backup just enable the `shutdown-backup@.service` service with the profile name as the instance variable
```
$ systemctl enable shutdown-backup@profile-name
```

to run the backup straight away just start the service instead of enabling it
```
$ systemctl start shutdown-backup@profile-name
```
