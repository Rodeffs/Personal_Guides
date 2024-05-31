To create an archive with *tar* do:

```
tar -cvf archive.tar path/to/files
```

To remove the unnecessary paths to folders from the archive do:

```
tar -cvf archive.tar -C path/to/files .
```

To compress using xz (in my opinion the best option in terms of compression and compatibility, however quite reliant on RAM) using the default compression rate -6:

```
tar -cvJf archive.tar.xz path/to/files
```

To use increase the compression rate, do:

```
tar -cvf archive.tar path/to/files ; xz -9 archive.tar
```