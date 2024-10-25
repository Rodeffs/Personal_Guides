# Reverting to a previous commit

To revert to a previous commit, first show the log of them:

```
git log --oneline
```

Then copy the ID of the commit to revert to and use:

```
git revert ID
```

Now all of the commits above the specified ID will be unstaged. To restore files to the state of that commit, run:

```
git restore file_name
```

or for everything in the current directory:

```
git restore .
```

Since now you're behind the commits, in order to to commit these changes, you will have to force push:

```
git push -f
```
