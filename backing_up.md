# Restic

To open repository, use:

```
sudo restic -r repository_location -p password_file option
```

for me:

- repository_location is /backups/restic_backup
- password_file is /usr/local/bin/get-restic-password

You can also make auto-backups by configuring systemd timer and systemd service and a script to run with all needed options in /usr/local/bin/restic-backup. If you have done so, then you can make a backup simply by running:

```
sudo restic-backup
```

To list possible options run:

```
restic --help
```

or read manual for restic or a specific command:

```
man restic
man restic-command
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
