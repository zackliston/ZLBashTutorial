# ZLBashExample

###Creating Bash Scripts
First, create a file using your favorite text editor
```
$ vim testscript
```

######Skeleton Script
There are two conventions, that although not necessarily required, should be followed<br>
```bash
# You must tell the thing running the script where to find the bash interpreter 
# Usually /usr/bin/bash works fine, but in the case that bash is not in the /usr/bin directory
# we want to use /usr/bin/env bash. Where env is a program used for finding other programs, in our case - bash.

#!/usr/bin/env bash

# Lots of great code here

# Exit with 0 for success, or any other number up to 255 for failure
exit 0

```

######Permissions
Now we have a bash file, but we can't run it yet (you can try if you want using ./testscript but you'll get a permission denied error). This is because the file does not have the execute permission set. We can allow execution of this file by the user in this way:

```bash
$ chmod u+x testscript
```

####Running
If you try to run your script now by simply entering the name of the script it won't work. 
```
$ testscript
```
When you try to run a script, bash refers to a value ($PATH) that points to one or more directories that may contain the bash script. If the script is found in none of the directories then it won't be executed. The current directory is not (and should never) be part of the $PATH. <br>

There are a couple ways to address this. We can either run the script from the current directory using one of two methods, or we can move it to a directory that $PATH points for easier usage. 

######Running in current directory
We can run the script by simply telling bash to run it explicity
```bash
$ bash testscript
```

Or we can tell it to search in the current directory instead of using the $PATH value
```bash
$ ./testscript
```

######Moving to a PATH directory
If we're planning on using the script often or from places other than the current directory, we can put the script in one of the directories PATH points to for ease of use. 
```bash
# This will probably be a place your $PATH points to, but examine it first (using echo $PATH) to be sure
$ mv testscript /usr/local/bin
```
Now when you type 
```
$ testscript
```
into your terminal, it will search through all the directories in $PATH, find your script, and run it.
