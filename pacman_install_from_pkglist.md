If you have previously set up a pacman hook like this (in */etc/pacman.d/hooks*):

```
[Trigger]
Operation = Install
Operation = Remove
Type = Package
Target = *

[Action]
Description = Updating package lists
When = PostTransaction
Exec = /bin/sh -c '/usr/bin/pacman -Qqen > /etc/pacman-pkglist.txt ; /usr/bin/pacman -Qqem > /etc/pacman-pkglist-foreign.txt'
```

Then there will be two text files located in /etc/, one containing packages from official repositories, like *Core*, *Extra* and *Multilib* (in my exaple this file is */etc/pacman-pkglist.txt*) and the other containing packages installed from AUR (*/etc/pacman-pkglist-foreign.txt*). The reason why they are separated is because pacman won't install foreign packages by itself, you need something like an AUR helper, e.g. *yay*

To install from the former you just need to run:

```
pacman -S --needed - < /etc/pacman-pkglist.txt
```

and to remove packages not mentioned in the list, run:

```
pacman -Rsu $(comm -23 <(pacman -Qq | sort) <(sort /etc/pacman-pkglist.txt))
```

To install from the latter, as mentioned before, *yay* is recommended, since it uses the same syntax, so you can just run:

```
yay -S --needed - < /etc/pacman-pkglist-foreign.txt
```
