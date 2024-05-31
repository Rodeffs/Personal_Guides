To clean cache, run:

```
pacman -Sc
```

To remove all orphans (unused packages), run:

```
pacman -Qdtq | pacman -Rns -
```

To detect all unneeded packages, run:

```
pacman -Qqd | pacman -Rsu --print -
```

Note that this will also list optional packages that you might want to keep, so keep in mind when removing them.

To remove them, run:

```
pacman -Qqd | pacman -Rsu -
```
