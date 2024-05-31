A better alternative for cp command would be rsync, especially across different file systems.

E.g.:

```
rsync -P source dest/
```

will copy file source to directory dest/, while also keeping partial files in case connection drops. It will also show a progress bar with copy speed and estimated time left

To recursively copy one directory to another:

```
rsync -P -r source/ dest/
```
