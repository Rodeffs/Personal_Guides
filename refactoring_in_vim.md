To search and replace in vim and neovim, do this:

```
X,Ys/old/new
```

where X,Y stand for range in strings where the replacement will take place (range includes both strings X and Y)

To set current string as X or Y, use .

To set last string as Y, use $

To replace all instances and not just the first entry in every string, do:

```
X,Ys/old/new/g
```

If old or new have the symbol / in them, then use | as the deliminator