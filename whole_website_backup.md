To make a backup of the whole website, enter the following:

```
wget --recursive --level=inf --no-parent --page-requisites --adjust-extension --backup-converted --convert-links --restrict-file-names=windows --wait=1 --random-wait --execute robots=off --user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36" 'target-url-here'
```

where:

- `--recursive` or `-r` will recursively retrieve data, default depth is 5 times
- `--level=` or `-l` will specify the recursion depth, if it's 0 or inf, then recursion is infinite
- `--no-parent` or `-np` will assure that the parent directory is never retrieved twice during recursion
- `--page-requisites` or `-p` will download everything necessary to properly display HTML page
- `--adjust-extension` or `-E` will append .html extension in case a file of this type doesn't have it already
- `--convert-links` or `-k` will convert all the links in the local backup so that they can be used for local viewing
- `--backup-converted` or `-K` will backup the original files with .orig extension when converting
- `--restrict-file-names=` will adjust characters depending on the OS given. Since certain characters aren't allowed on Windows filesystems (NTFS, exFAT, FAT), but are allowed on Linux, it's better to specify Windows
- `--execute robots=off` or `-e robots=off` ignores the nofollow attribute on websites
- `--wait=` or `-w` adds delay in seconds so that the load on server is minimized, better stick with 0.5 sec. Also, you can use `--random-wait` in case website blocks you from using wget (it will randomly choose wait time between 0.5 and 1.5 multiplied by `--wait` time)
- `--user-agent=` or `-U` identifies as the input user agent on the server, useful in case website doesn't allow wget


Additional useful switches:

- `--no-clobber` or `-nc` will prevent the same file from downloading twice in a recursive download. Unfortunately, doesn't work with `--convert-links`
- `--directory-prefix=` or `-P` will specify the download directory, default is the current working directory
- `--span-hosts` or `-H` enables spanning across hosts when doing recursive retrieving
- `--domains=` specifies the domains in `--span-hosts`