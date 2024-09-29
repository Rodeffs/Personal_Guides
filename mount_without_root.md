To mount a removable drive without root permissions, just use:

```
udisksctl mount -b /dev/sda1
```

where /dev/sda1 is the device you want to mount

This will mount it to /run/media/your-user-account/device-label where you will also be able to access the files without the need of root

To unmount, simply type:

```
udisksctl unmount -b /dev/sda1
```

