When installing Arch or anything else, don't forget to add:

```
fmask=0077,dmask=0077,umask=0077
``` 

to the *< options >* section in fstab for *boot* partition, or else things like UEFI random seed can be accessed, which is a security loophole