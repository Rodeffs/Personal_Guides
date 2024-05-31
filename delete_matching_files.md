### Write the following command to delete all matching files in /path folder and subfolders (use . to delete them in current folder and subfolders)

```
find /path -name 'name' -print0 | xargs -0 rm -f 
```