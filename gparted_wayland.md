Running GUI applications with root will not work on Wayland by default. To circumvent this, e.g. to run gparted, use one of the following:

1. Execute (this will pass your environmental variables to the root):

```
sudo -E gparted
```

2. Execute:

```
su -c gparted
```

3. Install *xorg-xhost* and execute:

```
xhost si:localuser:root
```

before launching the application itself. After that, execute:

```
xhost -si:localuser:root
```

to remove access
