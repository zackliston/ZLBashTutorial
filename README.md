# ZLBashExample

##Creating Bash Scripts
First, create a file using your favorite text editor
```
vim testscript
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
chmod u+x testscript
```
