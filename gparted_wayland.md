For some unknown reason, I can't run gparted on Wayland, because after I authorise, I get this:

```
(gpartedbin:7586): Gtk-WARNING **: 06:58:13.689: cannot open display: :0
```

This error occurs even when I run gparted with sudo, but strangely this works without errors:

```
su -c gparted
```

This even launches a window on Wayland without the need of an X server

Alternatively, I can install *xorg-xhost*, run Xwayland, open a X11 window manager and then gparted will run properly, but in X11, not on Wayland
