To mount an existing Samba shared folder, use the following:

```
sudo mount --mkdir -t cifs //HOSTNAME/SHAREDFOLDER /where/to/mount -o username=User,password=Password,uid=LocalUser,gid=LocalGroup
```

where

* 'HOSTNAME' is either the name of the host (if you have local DNS) or its IP
* 'SHAREDFOLDER' is the shared folder or path to it on the host
* 'username' is the user on the host that has access to the shared folder
* 'password' is the password for the aforementioned user
* 'uid' is the local user on the client system who will be given access to this folder
* 'gid' is the local group on the client system which will be given access to this folder
