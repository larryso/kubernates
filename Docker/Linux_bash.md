# Linux Bash

## set 

We use the set command to change the values of shell options and display variables in Bash scripts. We can also use it to debug Bash scripts, export values from shell scripts, terminate programs when they fail, and handle exceptions.

`$ set [option] [argument]`

The -e Option 
When a query returns a non-zero status, the -e flag stops the script. It also detects errors in the currently executing script.

```bash
#!/bin/bash 
set -e 
mkdir newfolder 
cat filenotindirectory 
echo 'The file is not in our directory!'
```

if we do not use set -e, the output will be:

```
root@822d0b664576:/tmp# ./test.sh
cat: filenotindirectory: No such file or directory
The file is not in our directory!
```

if we set -e, output will be:
```
root@822d0b664576:/tmp# ./test.sh
mkdir: cannot create directory 'newfolder': File exists
root@822d0b664576:/tmp#
```

## Reference: 
[https://www.gnu.org/software/bash/manual/bash.html](https://www.gnu.org/software/bash/manual/bash.html)

