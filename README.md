# ZLBashExample

###Getting started
####Creating bash scripts
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

###Basic Syntax
####Variables
#####Setting/Getting
```bash
#!/usr/bin/env bash
# Variables can be set like this
myVar="Hello world"

# And expanded like this 
echo $myVar

# prints "Hello world"

exit 0
```

#####Expansion
Variables not inside of quotes will be expanded 
```bash
$ echo This is my variable $myVar
# prints This is my variable Hello world
```

Variables inside single quotes will not be expanded
```bash
$ echo 'This is my variable $myVar
# prints This is my variable $myVar
```

Variables inside double quotes will be expanded
```bash
$ echo "This is my variable $myVar"
# prints This is my variable Hello World
```

####Space Characters
Spaces are very important in bash - my act as delimiters. For example, if you try to set a variable like this:
```
$ aVar = "Hello"
# prints -bash: myvar: command not found
```
Bash assumed that `aVar` was a script you wanted to run, with `=` and `"Hello"` as arguments. This is because it uses space charaters to separate commands. Instead, use:
```
# aVar="Hello"
```

######Parameter Separation
We have a [function called print](https://github.com/zackliston/ZLBashExample/tree/master/Examples) that prints each parameter on it's own line. Assume we want to print the word "Hello"

```bash
$ print Hello
Hello
```
Good, that works. Now let's print "Hello World"

```bash
$ print Hello World
Hello
World
```
That doesn't seem right, Hello World should be one the same line - or should it. This program prints each parameter on it's own line, and since space characters are the delimiters between paramters it thinks that "Hello" and "World" are two separate paramters. So how do we fix this - we simply use double quotes

```bash
$ print "Hello World"
Hello World
```

**On space charaters and double quotes**
It is generally advised to use double quotes very liberally. They will help us avoid common problems with parameter separation and expansion.

###Conditionals
#####Conditional Blocks 
```bash
#!/usr/bin/env bash
myName="Zack Liston"
yourName="John Doe"

# Note that the spacing here is important
if [[ $myName = $yourName ]]
  then echo "You're the same!"
elif [[ "Zack Liston" = $myName ]]
  then echo "Hello $myName"
else 
  echo "Something else"
fi

exit 0
```

#####Conditional Loops
######While
```bash
#!/usr/bin/env bash
while true
do echo "Infinite loop
done
```

######For
```bash
#!/usr/bin/env bash

for (( i=10; i > 0; i-- ))
  do echo "Loop one number is $i"
done

for i in 10 9 8 7 6 5 4 3 2 1
  do echo "Loop two number is $i"
done

for i in {10..1}
  do echo "Loop three number is $i"
done
exit 0
```

###Parameters
Parameters can be accessed individually like so 
```
$ script param1 param2 param3 param4
# $1 is "param1"
# $2 is "param2"
# etc, if there are more than 9 parameters the 9+n parameters must be accessed like so ${10} 
# Otherwise if accessed like this $10 it will subsitite $1 for the first parameter and append 0
```

Parameters can be accessed as a group like so 
```
$ script param1 param2 param3 param4
# $@ is a collection of all the parameters. We usually want to surrond it in double quotes to 
# promote space safety. 
```

We can also get the number of parameters
```
$ script param1 param2 param3 param4
# $# is the number of parameters, in this case 4
```

###File descriptors - Input/Output
####File Descriptors
Bash has three open file descriptors by default
* Standard Input - File Descriptor 0
* Standard Output - File Descriptor 1
* Standard Error - File Descriptor 2

######Standard Input - stdin
By default is connected to the keyboard.

######Standard Output - stdout
By default is connected to the screen. This is the actual result of a program

######Standard Error - stderr
By default is connected to the screen. This includes all messages, not just errors. 

####Redirection
Redirection is one of the most useful tools in bash. It allows us to get/put information from anywhere to anywhere. For example if I wanted to see all the files in the current directory I could do this:

```
$ ls
```

And since stdin is by default set to the screen, then we see the result. However, if we want to write the result to a file we can do it as simple as this

```
$ ls > files.txt
```

If you open `files.txt` you'll see the all the files in the directory.<br>
Now if we want to also send `stderr` to `files.txt` we can easily do that

```
$ ls > files.txt 2>&1
```

We can reference the file descriptors by their numbers. Remember that `stdout` is 1 and `stderr` is 2. The syntax above is saying `send stdout to the same place stdin is going`. 
<br>
Note that the `>` creates a new file or overwrites the existing one. If we want to append instead we can use this

```
$ ls >> files.txt
# appends the output to files.txt instead of replacing it. 
```

####Pipes
One of the most important capabilities of bash is being able to send the output of one script to the input of another. We can do this using pipes. For example, if we want to get any file/directory in the current directory that matches "Desktop":

```
$ ls -l | grep "Desktop"
```

This feature is the glue of bash. It allows us to string together many small programs to get the result we want, which is what bash is in a nutshell. 
<br> 
Sometimes we may want to send the output to another program, but also save it. We can do that using the `tee` command. 

```
$ ls -l | tee files.txt | grep "Desktop" > result.txt
# This first gets all the files in the directory, then saves it it files.txt, then sends
# it to grep, which finds which ones have "Desktop" and save those to result.txt
```

###Basic Tools
####Grep
Grep is a very powerful tool for searching. This is its most basic format, it returns the filename and the line that matches the expression
```
$ grep <expression> <files to search>

# to find all C files in the current directory that have the term "print"

$ grep "print" *.c
```

To get just the names of the files matching the expression provide the `-l` option

```
$ grep -l "print" *.c
```

To ignore the case provide the `-i` option
```
$ grep -i "print" *.c
```

To filter out results you don't want to see provide the `-v` option. For example, we want to see what files contain `print` but not `printable`. Not that we must pipe this to another grep. 

```
$ grep -i "print" *.c | grep -v "printable"
```

######Regular Expressions
We can grep either by string, or a regular expression. 
