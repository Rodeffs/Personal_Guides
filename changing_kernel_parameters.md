UKI allows you to just add a configuration file with desired parameters to /etc/cmdline.d

Don't forget to regenerate the initramfs:

```
mkinitcpio -P
```

