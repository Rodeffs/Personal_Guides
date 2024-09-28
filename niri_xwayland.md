Unfortunately, the niri compositor doesn't come with the X11 window manager or native Xwayland support

To launch X11 apps, first you need to launch Xwayland itself in a separate shell:

```
Xwayland
```

Then you'll need to launch any X11 window manager on a DISPLAY, again in a separate shell, e.g. for kwin_x11:

```
env DISPLAY=:0 kwin_x11
```

And finally, in a different shell you need to launch the app itself, e.g. for steam:

```
env DISPLAY:=0 steam
```

So in total you need to launch 3 instances of your terminal

You can also define DISPLAY variable in your shell config, so you don't have to enter it all the time, e.g. for .bash_profile:

```
export DISPLAY=:0
```
