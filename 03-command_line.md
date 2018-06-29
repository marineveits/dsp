# Learn command line

Please follow and complete the free online [Bash Scripting Tutorial](https://ryanstutorials.net/bash-scripting-tutorial/) or [Codecademy's Learn the Command Line](https://www.codecademy.com/learn/learn-the-command-line). These are helpful tutorials. You should be able to go through these in a couple of hours.

**Note:** Bash is not installed on a PC. Rather, PC users must install Cygwin. Once Cygwin has been installed, the command line interface witll _emulate_ bash. You can find all information regarding Cygwin [here](https://www.cygwin.com/).

---

### Q1.  Cheat Sheet of Commands  

Here's a list of items with which you should be familiar:  
* show current working directory path
* creating a directory
* deleting a directory
* creating a file using `touch` command
* deleting a file
* renaming a file
* listing hidden files
* copying a file from one directory to another

Make a cheat sheet for yourself: a list of at least **ten** commands and what they do.  (Use the 8 items above and add a couple of your own.)  

> > 
1. `pwd` -- show current working directory 
2. `mkdir` -- creating a directory
3. `rm -r` -- deleting a directory
4. `touch` -- creating a file using `touch` command
5. `rm` -- deleting a file
6. `mv` (old file name as the first argument and new file name as the second) -- renaming a file
7. `ls -a` -- listing hidden files
8. `cp` (with the source file as the first argument and the destination directory as the second)-- copying a file from one directory to another
9. `cd` -- changing a directory
10. `ls -alt` -- lists all files and folders ordered in current directory including their properties 

---

### Q2.  List Files in Unix   

What do the following commands do:  
`ls` 
`ls -a` 
`ls -l`  
`ls -lh` 
`ls -lah` 
`ls -t` 
`ls -Glp`  

> > 
`ls` -- lists files and folders in current directory 

`ls -a`  -- lists hidden files

`ls -l`  -- long format listing of files

`ls -lh`  -- long format listing of files with sizes in human readable format

`ls -lah` -- like 'ls -lh', including hidden files

`ls -t` -- lists files sorted by time modified (latest first)

`ls -glp`  -- long format listing, exlude the owner name, displays directories with /


---

### Q3.  More List Files in Unix  

Explore these other [ls options](http://www.techonthenet.com/unix/basic/ls.php) and pick 5 of your favorites:

> > 
1. `ls -R` -- displays subdirectories as well
2. `ls -x` -- displays files as rows across the screen
3. `ls -q` -- displays all nonprinting characters
4. `ls -o` -- long format listing, excludes group name
5. `ls -m` -- displays the names as comma separated list

---

### Q4.  Xargs   

What does `xargs` do? Give an example of how to use it.

> > 
`xargs` converts input from standard input into arguments to a command. It reads data from stdin and executes the command supplied to it as an argument. 

Example:
$ xargs

Hi, wellcome to Metis. (xargs would echo the string back to us)

Hi, wellcome to Metis.



 

