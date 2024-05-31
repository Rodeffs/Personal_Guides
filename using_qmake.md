### First you need to properly configure the project_name.pro file (look at examples in Year1_Programming folder or in the internet) in the source code folder. After that, make a build directory, then cd into it. Next step is to type:

```
qmake path_to_project_name.pro
``` 

### This will create the Makefile in the build directory. After that just type:

```
make 
```

### And this will build the project in the current build directory, which you can run by double clicking it or by typing in terminal:

```
./project_name
```
