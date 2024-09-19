In order to edit the single line in a file:

```
sed -i "Ns/.*/new text/" [filename]
 ```
 
 where *N* is the line number, *new text* and *[filename]* are self-explanatory
 
 In order to do that in multiple files at once in the current directory, use *for* cycle:
 
 ```
 for file in *; do sed -i "Ns/.*/new text/" $file; done
 ```
 