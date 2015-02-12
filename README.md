# ZLBashExample

##Creating Bash Scripts
**Create a file using your favorite text editor**
```
vim testscript
```

**There are two conventions, that although not necessarily required, should be followed**
```bash
# You must tell the thing running the script where to find the bash interpreter 
# Usually /usr/bin/bash works fine, but in the case that bash is not in the /usr/bin directory
# we want to use /usr/bin/env bash. Where env is a program used for finding other programs, in our case - bash.

#!/usr/bin/env bash

# Lots of great code here

# Return 0 for success, or any other number up to 255 for failure
return 0

```
