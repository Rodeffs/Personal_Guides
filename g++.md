### To compile program via g++ (or gcc) you have to write:

```
g++ file1.cpp file2.cpp ... fileN.cpp -o name_of_executable
```

### Shorter command:

```
g++ *.cpp -o name_of_executable
```

### In VIM it would look like:

```
:!g++ *.cpp -o name_of_executable
```

### Then you can run it via terminal by:

```
./name_of_executable
```

### Or in VIM:

```
:!./name_of_executable
```
