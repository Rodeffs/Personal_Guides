# Useful bash tips

#### *for* can be used in many ways, such as:

1. Creating numbered files:

```
for i in 1..20; do touch file_{$i}.txt; done
```

2. Going over all files in a directory and editing them all together:

```
for i in *; do mv $i new_$i; done
```

3. Going over all lines in a file:

```
for i in $(cat text.txt); do touch $i; done
```