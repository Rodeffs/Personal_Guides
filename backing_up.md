# Restic

[Archwiki page on Restic](https://wiki.archlinux.org/title/Restic)

You can make auto-backups by configuring systemd timer and systemd service and a script to run with all needed options in /usr/local/bin

Example for the script:

```
#!/bin/bash

if [[ -n $(pgrep 'restic' | grep 'restic backup') ]]; then
  echo 'restic is already running...' 1>&2
  exit 0
fi

set -e
set -v

export RESTIC_REPOSITORY='/where/is/your/repo'
export RESTIC_PASSWORD_FILE='/file/which/contains/password'
export RESTIC_EXCLUDE_FILE='/file/which/contains/excluded/files'
export RESTIC_COMPRESSION='auto'
export RESTIC_CACHE_DIR='/where/to/store/cache'

mkdir -p "${RESTIC_CACHE_DIR}"

restic unlock -r ${RESTIC_REPOSITORY}
restic backup / -r ${RESTIC_REPOSITORY} --password-file=${RESTIC_PASSWORD_FILE} --exclude-file=${RESTIC_EXCLUDE_FILE} --tag scheduled 
restic check -r ${RESTIC_REPOSITORY} --with-cache --cache-dir=${RESTIC_CACHE_DIR} --read-data-subset=5G
restic forget --prune --keep-daily 7
```

Example for systemd service:

```
[Unit]
Description=Backup system using restic

[Service]
ExecStart=systemd-inhibit /usr/local/bin/restic-backup
```

Example for systemd timer:

```
[Unit]
Description=Timer for full system backups using restic

[Timer]
OnCalendar=daily
AccuracySec=1us
RandomizedDelaySec=1hour
Persistent=true
Unit=restic-backup.service

[Install]
WantedBy=timers.target
```

To browse the repo, mount it to somewhere, like:

```
restic -r repository_location -p password_file mount /mnt/
```

# Rsync

To use rsync for backup purposes, enter:

```
rsync -aAXHhv --filter="merge /path/to/filter.filter" /what/to/backup /where/to/store/backup --delete
```

where:

* `-v` means verbose
* `-a` means archive mode (look up man rsync)
* `-A` preserves permissions
* `-h` means that the numbers are human readable
* `-X` preserves extended attributes
* `-H` preserves hard links, e.g. made with the ln command
* `--filter` points to the filter file, aka to the list of excluded files and directories
* `--delete` tag is needed, so that the deleted files from source are also deleted from destination

Then you can compress the backup and encrypt it with GPG, or use an archiving extension that allows for encryption (but not zip, since it doesn't encrypt file names)

It is also a good idea to create a systemd timer for auto backup, akin to restic backup

### Important note

If your backup directory is located on the same filesystem as the directory you're backing up then in order to preserve space and minimize disk wear out, it is preferrable to create hard links for files instead of copying them, like:

```
rsync -aAXHhv --filter="merge /path/to/filter.filter" --delete --link-dest=/what/to/backup /what/to/backup /where/to/store/backup 
```

where `--link-dest` is the path to directory we're backing up *relative* to the destination

### Example:

Backup:

```
sudo rsync -aAXHhv --filter="merge /etc/rsync/excludes.filter" --delete --link-dest=/ / /backups/rsync_backup/
```

Archive and encrypt using tar and gpg:

```
sudo tar -cvzf - -C /backups/rsync_backup . | gpg -c --cipher-algo AES256 --no-symkey-cache -o laptop_backup.tar.gz.gpg
```

(optionally you can also change compression from gzip to xz by replacing switch -z with -J)

Or the same but with zip:

```
sudo zip -vr - /backups/rsync_backup | gpg -c --cipher-algo AES256 --no-symkey-cache -o laptop_backup.zip.gpg
```

(to control compression add an appropriate switch: -0 for no compression to -9 for maximum compression or anything in between, default is -6)

By using - as the archive name it will not be stored on the disk and instead immediately passed to gpg, so no double copies are created

To decrypt:

```
gpg -o laptop_backup.tar -d laptop_backup.tar.gpg
```

Or:

```
gpg -o laptop_backup.zip -d laptop_backup.zip.gpg
```

You can also do all of this separately:

Archive (using tar):

```
sudo tar -cvf laptop_rsync_backup.tar -C /backups/rsync_backup .
```

Archive (using zip):

```
sudo zip -vr laptop_rsync_backup.zip /backups/rsync_backup
```

Encrypt:

```
gpg -c --cipher-algo=AES256 --no-symkey-cache laptop_rsync_backup.tar
```
