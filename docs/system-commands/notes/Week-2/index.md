---
comments: true
---

# Simple Commands

In this section We cover the following topics:

- Introduction to packages and repositories. Using 'apt' commands to manage packages. 
- File types and related commands. Understanding file permissions and access modes. 
- Managing file permissions through symbolic and numeric mode. 
- Concept of environment variables. Important environment variables such as $HOME, $USER and $PATH

-------------------------------------------------------------------------------------------------

The root folder ` / ` is the parent of it's own.
```terminal
/$ cd ..
/$
```
* Multiple uses of ` / ` is as good as one.
```terminal
~$ cd ///usr///bin
/usr/bin$
```

## More on ` ls `

* Interpretation of directory as an argument
	- ` ls -l level1 ` will long list all the files of directory ` level1 `.
```
~$ ls -l level1
total 2
-rw-rw-r-- 1 sanr sanr 0 Dec 18 17:06 f1
-rw-rw-r-- 1 sanr sanr 0 Dec 18 17:06 f2
```

* Multiple options
	- ` ls -ld level1 ` will long list the directory itself (not it's content).
	- ` ls -ldi level1 ` will long list the directory itself along with inode number (not it's content).
```terminal
~$ ls -ld level1
drwxrwxr-x 2 sanr sanr 4 Dec 18 17:06 level1
~$ ls -ldi level1
79564 drwxrwxr-x 2 sanr sanr 4 Dec 18 17:06 level1
```

* Recursive listing
	- ` ls -R `
```
~$ ls -R level1
level1:
f1  f2
```
* Order of options for `ls` is immarterial.
	- But the standard practice is ` command options... argument... `
```terminal
~$ ls -ld level1 -i
79564 drwxrwxr-x 2 sanr sanr 4 Dec 18 17:06 level1
~$ ls -li level1 -d
79564 drwxrwxr-x 2 sanr sanr 4 Dec 18 17:06 level1
~$ ls -di level1 -l
79564 drwxrwxr-x 2 sanr sanr 4 Dec 18 17:06 level1
~$ ls level1 -ldi
79564 drwxrwxr-x 2 sanr sanr 4 Dec 18 17:06 level1
```

* Short and long forms of [options](#type-of-options "Types of Options")               
	- ` ls -a ` and ` ls --all ` are equivalent.
	- Not all options have long form, eg. ` -l ` for ` ls `
```
~$ ls -l --directory --inode level1
79564 drwxrwxr-x 2 sanr sanr 4 Dec 18 17:06 level1
```

## Utilities for Knowing Files Better

#### [` less `](/Week_1/Lecture2-3.md#less)

#### ` cat `
* ` cat <filename> ` : Dumps the file output on screen for reading.
* Abbr. for concatenate.
* Navigation is difficult.
* To see the ` profile ` file content
```terminal
/etc$ cat profile
```

#### ` more `
* ` more <filename> ` : Open file filename for reading page by page.
* It beautifully combines the features of [` less `](#less) and [` cat `](#cat).
* Can not scroll ` down ` to see content.
* Shows percentage of file read.
* To view the ` profile ` file content
```terminal
/etc$ more profile
```

#### ` head `
* ` head <filename> ` : Print the first 10 lines of the file.
* Can also specify number of lines using ` -n ` option.
* To print first 5 lines.
```terminal
/etc$ head -n 5 profile
```

#### ` tail `
* ` tail <filename> ` : Print the last 10 lines of the file.
* Can also specify number of lines using ` -n ` option.
* To print last 5 lines.
```terminal
/etc$ head -n 5 profile
```

#### ` wc `
* ` wc <filename> ` - Print number of newline, word and byte for each file
* To print newline (` -l `), word (` w `) an byte (` -c `) count of ` profile `
```terminal
/etc$ wc profile
 27  97 582 profile
```
* To print newline (` -l `) count of ` profile `
```terminal
/etc$ wc -l profile
 27 profile
```
---------------

## Knowing More Commands

#### [` man `](/Week_1/Lecture2-3.md#man)

#### ` which `
* ` which  <command> ` - Print the path of ` command ` or check if a package exists or not.
* To print path of commands ` less ` and ` more ` 
```terminal
/etc$ which less
/usr/bin/less
/etc$ which more
/usr/bin/more
```
* Actually the size of ` less ` is ` more `
```terminal
/etc$ ls -l /usr/bin/less
-rwxr-xr-x 1 root root 199048 Mar 24  2022 /usr/bin/less
/etc$ ls -l /usr/bin/more
-rwxr-xr-x 1 root root 43392 Feb 21  2022 /usr/bin/more
```

* To print the path of command ` which `!
```terminal
/etc$ which which
/usr/bin/which
```
* ` which ` is kind of reflexive!

#### ` whatis `
* ` whatis <command> ` - Print brief description of ` command ` as the first line in ` man ` page
* To print the description of ` which `
```terminal
/etc$ whatis which
which (1)            - locate a command
```

#### ` apropos `
* ` apropos <command | word> ` - Search the manual page names and descriptions.
```
~$ apropos who
w (1)                - Show who is logged on and what they are doing.
who (1)              - show who is logged on
whoami (1)           - print effective userid
```
* ` apropos ` is equivalent to ` man -k ` i.e ` man -k who ` will give the same result above as given by ` apropos who `
* As shown below, ` apropos ` is the symbolic link for ` whatis `, but why the outputs are different?
```terminal
~$ ls -l /usr/bin/apropos
lrwxrwxrwx 1 root root 6 Nov  3 19:18 /usr/bin/apropos -> whatis
~$ ls -l /usr/bin/whatis
-rwxr-xr-x 1 root root 48416 Mar 17  2022 /usr/bin/whatis
```
* In GNU-Linux, every executable will know by which name it is invoked which results in different behaviour.

#### ` help `
* Print the help for currently running shell. [More on shell](#Shells)
* It includes keywords, syntax for commands, loops and symbolic expressions.  

#### ` info `
* ` info ` - Prints documentation for commands.
* ` info <command> ` - Documentation of specific command ` command `.
* It is highly navigatable, just like a webpage.
* Links are marked in * and underline and can be navigated using arrow keys.

| ` keys ` | Description |
| :---:    | ---         |
| ` enter ` | Open a link |
| ` < ` ` shift + , ` | Go back or previous |
| ` > ` | Go forward or next | 
| ` M ` ` m ` | Search menu, similar to seach box |
| ` S ` ` s ` | Regex search |
| ` Q ` `q`| Quit |

#### ` type `
* ` type <command> ` - Prints the type of the command.
* Is it
	- offered by shell?
	- offered by operating system?
	- alias?
* To print the type of commands ` type ` and ` ls `
```terminal
~$ type type
type is a shell builtin
~$ type ls
ls is aliased to `ls --color=auto'
```
* Note : commands displayed by ` help ` are all shell builtins. 

#### More on [` alias `](/Week_1/Lecture2-3.md#alias)
* To create alias for ` ls -l `
```terminal
~$ alias ll="ls -l"
~$ ll
drwxrwxr-x 5 groot groot 21 Dec 12 18:52 Desktop  
drwxrwxr-x 2 groot groot  3 Nov 19 19:41 Downloads
~$ type ll
ll is aliased to `ls -l'
```

#### ` unalias `
* ` unalias <alias_command> ` - Remove the alias command.
* To remove alias ` ll `
```terminal
~$ unalias ll
~$ ll
ll: command not found
```

#### ` rmdir `
* ` rmdir <dirname> ` - Remove an empty directory.
* To remove empty directory ` level2 `
```terminal
~$ rmdir level2
```

### Multiple Arguments
* Let's create some files using ` touch ` and multiple arguments and a directory using ` mkdir `.
```terminal
~$ touch file1 file2 file3
~$ mkdir mydir
```
#### Second argument
* First argument is a file and second argument is directory.
* ` file1 ` is copied to ` mydir ` 
```terminal
~$ cp file1 mydir
```
* Both arguments are files.
* ` file2 ` is overridden by contents of ` file2 `
```terminal
~$ cp file1 file2
```
* Note
	- Here the alias is not set for ` cp `, hence the command is not interactive.
	- See the [` man `](/Week_1/Lecture2-3.md#man "Explore man") command to figure out interactive option for ` cp `. 

* ` rmdir ` is not meant to handle non empty directories.
```
~$ rmdir mydir
rmdir: failed to remove 'mydir': Directory not empty
```
* We can use ` rm ` and ` rmdir ` and delete directory by explicitly navigating into them.
* But, can we do something better? as this process might be cumbersome.
* To force the deletion ` rm ` has one option ` -r `
```terminal
~$ rm -r mydir
```
* As you can see the file deletion happens without any noise.
* Note 
	- Interactive mode is not set here.
	- Now you know what to do to enable it.


#### Interpretation of last argument

#### Recursion assumed for ` mv ` and not ` cp `
* Some commands assume recursion while some don't.
* Let's setup our directory structure.
```terminal 
~$ mkdir mydir
~$ cp file1 mydir
```
* Let's check for ` cp `
```terminal 
~$ cp mydir mydir2
cp: -r not specified; omitting directory 'mydir'
~$ cp -r mydir mydir2
```
* Note that most commands tell you what to do.
* To take home, recursion is not assumed for ` cp `.

* Let's check for ` mv `
```terminal
~$ mv mydir mydir3
```
* As you can see no error is generated.
* That means ` mydir ` is successfully renamed as ` mydir3 `

## Links
### ` ln `
* ` ln [-s] <source> <linkname> ` : Create link for ` source ` as ` linkname `

#### Symbolic Links
* Inode number of source file and linkname are different.
* ` l `, the first character in long format of file denotes the file is symbolic link.
* ` -s ` option is used to create symbolic link.
* To create a symbolic link (same as shortcut in Windows) for ` file1 ` as ` file0 `
```terminal
~$ ln -s file1 file0
```
* Long list the ` file0 ` to see the output.
```terminal
~$ ls -l file0
lrwxrwxrwx 1 sanr sanr 5 Dec 19 08:28 file0 -> file1
```
* Print the inode numbers of ` file1 ` and ` file0 `
```
~$ ls -i file1 file0
66488 file0  80500 file1
```

#### Hard Links
* ` ln <source> <linkname> ` - Create hard link
* We have already come across this in [Week 1](/Week_1/Lecture2-3.md#hard-links).
* Let's take an example.
* To create a hard link for ` file1 ` as ` file11 `
```terminal
~$ ln file1 file11
```
* See the long listing along with inode numbers.
``` terminal
~$ ls -li file1 file11
80500 -rw-rw-r-- 2 sanr sanr 0 Dec 18 20:20 file1
80500 -rw-rw-r-- 2 sanr sanr 0 Dec 18 20:20 file11
```
* As you can see here, number of hard links are two for ` file1 ` and ` file2 ` as both are same which contrasts with symbolic link.
* As long as number of hard links for a *file* > 1 you can delete any file which points to the same hard link as *file*.
* X-device/X-cross storage hard links are forbidden.
* It is typically forbidden to create hard link for a directory.

## File Sizes
### ` ls -s `
* ` ls -s <file> ` - Print the size of ` file `.
* Combine with ` -h ` to print in human readable format.

### ` stat `
* ` stat <file> ` - Shows statistics (file or file system status) on ` file `.
* Typical output of ` stat ` for file ` znew `.
```terminal
/usr/bin$ stat znew
  File: znew
  Size: 4577      	Blocks: 10         IO Block: 4608   regular file
Device: 1bh/27d	Inode: 225090      Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2022-12-18 15:12:06.475640237 +0200
Modify: 2022-09-05 15:33:59.000000000 +0200
Change: 2022-11-03 19:51:25.078671510 +0200
 Birth: 2022-11-03 19:51:25.014673255 +0200
```
* You can get the atomic output using ` -c ` option using ` FORMAT `.
* For example, get textual and octal permissions on file ` znew `
```
/usr/bin$ stat  -c "%a %A" znew
755 -rwxr-xr-x
```
* To know more visit the ` man ` pages.

### ` du `
* ` du <file> ` - Estimate file space usage.
* To see the disk usage of ` znew ` file.
```terminal
/usr/bin$ du znew
5       znew
/usr/bin$ du -h znew
5.0K    znew
```

* Try to figure out the difference between output by ` stat ` and ` du `. 

### Roll of block size

### ` df `
* ` df [-h]` - shows filesystem information in format [human readable] below.
> Filesystem   1K-blocks    Used Available Use% Mounted on

## In-memory Filesystems
* These are special read-only filesytems, available in memory and not on hard disk.
* You can view these filesystems.
* These are directory structures to know more about the system.

### ` /proc `
* Older filesystem, but still useful.
* Used to store different processes.
* These file are just representaions, so file sizes it contains are zero.
* Useful files
	- ` cpuinfo ` - stores cpu information.
	- ` version ` - stores system information, content similar to [`uname`](/Week_1/Lecture2-3.md#uname) -a ` command.
	- ` meminfo ` - Diagnostic information about memory. Check [` free `](/Week_1/Lecture2-3.md#free) command.
	- ` partitions ` - Disk partition information. Check [` df `](#df)  
	- ` kcore ` - The astronomical size ( 2 ^ 47 bits)  tells the maximum virtual memory (47 bits) the current Linux OS is going to handle.
* Directories named by ` number `
	- Each of these correspond to running processes. 
	- The ` number ` corresponds to the process id. 

### ` /sys `
* In use since Kernel 2.6+.
* It's a well organized filesystem.
* Explore usb devices used in the machine.
```
/sys/bus/usb/devices$ ls
1-0:1.0  2-0:1.0  2-1  2-1:1.0  2-2  2-2:1.0  usb1  usb2
```
* Each number corresponds to a device.
* Let's check the ` product ` and ` manufactures ` of the usb device ` 2-1 `
```terminal
/sys/bus/usb/devices/2-1$ cat product
VMware Virtual USB Mouse
/sys/bus/usb/devices/2-1$ cat manufacturer
```
* The VMWare is using its virtual USB mouse to connect the host machine mouse (touch pad) to the guest machine.
* Thus, these two commands help us to explore hardware attached to the machine.


#### Type of Options
1. UNIX options, which may be grouped and must be preceded by a dash.
2. BSD options, which may be grouped and must not be used with a dash.
3. GNU long options, which are preceded by two dashes.
## $HELL Variables
* The variables defined within shell in the command line environment.
* $hell variables are efficient medium of communication for two processes.
* They can also be made available to it's child processes.
* They are private to the shell they are defined in.
* $hell variables can also be used to store information in an intermediate form during processes.
* Commercial softwares can use shell variables to to know ports for which license is available.
* Hence, it is important to study them.
* Let's start.
* Before going ahead look at command below.

----------------------
#### ` echo `
* Display a line of text or value of a variable.
* Ignores multiple spaces when string is without quotes.
* Use quotes for string with multiple spaces.
* Use double quotes for variables.
* Accepts multi-line input.
* Print strings to screen.
```terminal
~$ echo hello, world
hello, world
```
* Print values of variables.
```terminal
~$ echo $HOME
/home/groot
~$ echo "$HOME"
/home/groot
```
* To suppress trailing newline, use ` -n ` option.
```terminal
~$ echo -n "Hello"
Hello~$
```

* To enable interpretation of [backslash escapes](#backslash-escapes) use ` -e ` option.

----------------------

* Conventionally shell variables are defined in uppercase.
* To refer to a shell variable use ` $ `.

### Frequently Used Shell Variables

* ` $USERNAME `  or ` $USER `
	- Stores username of the logged in user.

```terminal
~$ echo $USERNAME
groot
~$ echo "User logged into the shell is: $USER"
User logged into the shell is: groot
```

* ` $HOME `       
	- Stores the path of the home directory.
```terminal
~$ echo $HOME
/home/groot
```
* ` $HOSTNAME `
	- Stores host name or name of the machine given in the file ` /etc/hostname `.
	- This a static file, if configured the network setting the file can be made dynamic. 
```terminal
~$ echo $HOSTNAME
rich-linux
```
* ` $PWD ` 
	- Stores the path of current directory. Same as [` pwd `](/Week_1/Lecture2-3.md#pwd)
```terminal
~$ echo $PWD
/home/groot
```
* ` $PATH ` 
	- Stores the path of commands used.
	- Value is list of directories with ` : ` as delimiter.
```terminal
~$ echo $PATH
/home/sanr/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin
```
* Escaping ` $ ` 
	- make ` $ ` an ordinary character using ` \ `
```terminal
~$ echo  "hostname is \$HOSTNAME and user is $USERNAME"
hostname is $HOSTNAME and user is groot
```
 
### Commands to Inspect Shell Variables

#### ` printenv `
* Print all o part of environment.
```terminal
~$ printenv HOME
/home/groot
```
#### ` env `
* Print the variables defined in shell
#### ` set `
* Display the names and values of shell variables.
* set or unset values of set variables.


## Different Ways of Executing a Command
* Using command itself 
```terminal
~$ date
Wednesday 21 December 2022 06:20:23 PM EET
~$ date -R
Wed, 21 Dec 2022 18:21:43 +0200
```
* Using commads binary path.
```terminal
~$ /usr/bin/date
Wednesday 21 December 2022 06:23:34 PM EET
```
* Executing from ` /usr/bin `
```terminal
/usr/bin$ ./date
Wednesday 21 December 2022 06:25:50 PM EET
```
* Using alias
```
~$ alias date="date -R"
~$ date
Wed, 21 Dec 2022 18:22:10 +0200
```
* Escaping alias using ` \ `
```terminal
~$ \date
Wednesday 21 December 2022 06:26:10 PM EET
```

### Special Shell Variables
| VARIABLE    | description |
| :------:    | :---------  |
|  ` $0 `   |  name of the shell | 
|  ` $$ `   |  process ID of the shell |
|  ` $? `   |  exit/return code of previously run command/program |
|  ` $- `   |  flags set in the bash shell |

#### ` $0 `
```terminal
~$ echo $0
bash
```
#### ` $$ `
* see [ps](/Week_1/Lecture2-3.md#ps)
```terminal
~$ echo $$
3423
```

## [Process Control](/Week_2/Lecture5.md "Linux Process Management")
#### [` & `](/Week_2/Lecture5.md#&)
* Use to run a program in the background.
#### [` fg `](/Week_2/Lecture5.md#fg)
* Bring the program to the foreground.
#### [` coproc `](/Week_2/Lecture5.md#coproc)
* Run a program while also being able to use the shell.
#### [` jobs `](/Week_2/Lecture5.md#jobs)
* List all background jobs.
#### [` top `](/Week_2/Lecture5.md#top)
* Live view of all running processes.
#### [` kill `](/Week_2/Lecture5.md#kill)
* Kill running/stopped process or processes.

## [Program Exit Codes](/Week_2/Lecture5.md#more-on-program-exit-codes "More on Program Exit Codes")
* ` 0 ` - success
* ` 1 ` - failure
* ` 2 ` - misuse of shell builtins when the permissions are not adequate
* ` 126 ` - command can not be executed
* ` 127 ` - command not found
* ` 130 ` - processes killed using ` ctrl + C ` or ` ^C `
* ` 137 ` - processes killed using ` kill -9 <pid> `
* Exit codes have values between ` 0 ` and ` 255 `.
#### ` $? `
```
~$ true
~$ echo $?
0
~$ false
~$ echo $?
1
```

## [Flags set in bash](/Week_2/Lecture5.md#more-on-flags-set-in-bash)
* ` h ` - locate and hash commands
* ` B ` - brace expansion enabled
* ` i ` - interactive mode
* ` m ` - job control enabled (jobs can be taken foreground or background)
* ` H ` - ` ! ` style history substitution enabled
* ` s ` - commands are read from stdin
* ` c ` - commands are read from arguments
` echo $- `



## Shell Variables Part - 1
* Creation, inspection, modifictaion, lists...

### Creating a Variable
* ` myvar="value string" `
* Variable name can have mix of alpha-numeric chars and _. 
	- ` myvar `, ` MyVar `, ` My10Var ` and ` MyVar_1 ` are valid
* It does not start with a number.
	- ` 10myvar ` is invalid
* There are no spaces around ` = ` (assignment operator).
	- ` myvar = 10 ` is invalid
* A value can be number, string or a `` `command` `` (command substitution).
* Safer to enclose string value within double quotes.
* ` ${myvar} ` is more convenient for string concatenation than ` $myvar `
#### Examples
* Create and display variable ` myvar ` with value ` 10 `
```terminal
~$ myvar=10
~$ echo $myvar
10
```
* Change value of variable ` myvar `
```terminal
~$ myvar="hello world"
~$ echo $myvar
hello world
```
* Effect when value not within quotes.
	- variable is assigned a value null
```terminal
~$ myvar=hello world
Command 'world' not found.
~$ echo $?
127
```

* ` command ` as value of a variable. (` command ` is any valid command)
	- store value of ` date ` in variable ` myvar `
```terminal
~$ mydate=` date ` 
~$ echo $myvar
Thursday 22 December 2022 12:23:07 PM EET
~$ myvar=` echo Today is Thursday `
Today is Thursday
```


### Exporting a Variable
* Make variable available to the subshell.
* It can be done in following ways
	- ` export myvar="value string" `
	- ` myvar="value string" ;`
	  ` export myvar `
#### Examples
* By default variable is not available for child shell
```terminal
~$ myvar=3.14
~$ bash
~$ echo $myvar

```
* Make the variable available in child shell
	- Changing the value of variable in child shell does not affect it's value in parent shell
```terminal
~$ export myvar=3.14
~$ bash
~$ echo $myvar
3.14
```



### Using Variable Values
* Refering by variable name using ` $ `.
	- ` echo $myvar `
	- ` echo ${myvar}`
	- ` echo "${myvar}_something" `
#### Example
* Accessing variable as ` ${myvar} ` and without ` $myvar `
	- As stated above ` _ ` can be part of variable name.
```terminal
~$ myvar=FileName
~$ echo "$myvar.txt"
FileName.txt
~$ echo "$myvar_txt"

~$ echo ${myvar}_txt
FileName_txt
```


### Removing a Variable
* Delete the variable.
	- ` unset myvar `
* Example
```terminal
~$ unset myvar
~$ echo $myvar

```
### Removing Value of a Variable
* Set the value of variable as 	` null `
	- ` myvar= `
* Example
```terminal
~$ myvar=
~$ echo $myvar

```
.............................................................
### Test if a Variable is Set
* ` -v varname `
	- True if the shell variable ` varname ` is set and is name reference. 
```bash
[[ -v myvar ]];
echo $?
```
* Return codes:
	- ` 0 ` : success (variable ` myvar ` is set)
	- ` 1 ` : failure (variable ` myvar ` is not set) 
* Example
```terminal
~$ unset myvar
~$ [[ -v myvar ]];
~$ echo $?
1
~$ myvar=10
~$ [[ -v myvar ]];
~$ echo $?
0
```

### Test if a Variable is *Not* Set
* ` -z string ` 
	- True if the length of the string is **z**ero.

```bash
[[ -z ${myvar} ]];
echo $?
```

* Return codes:
	- ` 0 ` : success (If the length of the string ` ${myvar} ` is zero.)
	- ` 1 ` : failure (If the length of the string ` ${myvar} ` is not zero.) 
* Example
```terminal
~$ unset myvar
~$ [[ -z ${myvar} ]];
~$ echo $?
0
~$ myvar=10
~$ [[ -z ${myvar} ]];
~$ echo $?
1
```

### Substitute Default Value
* If the variable ` myvar ` is not set (` :- `), use ` "default" ` as temporary value.
```bash
echo ${myvar:-"default"}
```
* There are no spaces around ` :- `
* Pseudocode : 
> if ` myvar ` is set:
>
> >	display its value
>
> else:
>
> >	display "default"
#### Examples
```terminal
~$ myvar=
~$ echo ${myvar:-hello}
hello
~$ echo ${myvar:-"myvar is not set"}
myvar is not set
~$ echo ${myvar}

~$ myvar="HELLO"
~$  ${myvar:-hello}
HELLO
```

### Set Default Value

* If the variable ` myvar ` is *not* set, then set ` "default" ` as it's value.
```bash
echo ${myvar:="default"}
```
* There are no spaces around ` := `
* Pseudocode : 
> if ` myvar ` is set:
> >
> > display its value
>
> else:
>
> >	set "default" as its value
> >
> >	display its new value

#### Examples
```terminal
~$ myvar=
~$ echo ${myvar:=hello}
hello
~$ echo ${myvar:=HELLO}
hello
~$ echo ${myvar}
hello
```

### Reset Value if Variable is Set
* If the variable ` myvar ` is set, then set "default" as its temporary value.
```bash
echo ${myvar:+"default"}
```
* There are no spaces around ` :+ `
* Pseudocode : 
> if ` myvar ` is set:
>
> >	display "default"	
>
> else:
>
> >	display ""

#### Example
```
~$ myvar=apple
~$ echo ${myvar:+APPLE}
APPLE
~$ echo $myvar
apple
~$ unset myvar
echo ${myvar:+APPLE}

```


### User Defined Error Message (Alert)
* Display user defined error when variable is not set.
```bash
echo ${myvar:?"myvar is not set"}
```
#### Example
```
~$ unset myvar
~$ echo ${myvar:?"myvar is not set"}
bash: myvar: myvar is not set
```

### List of Variable Names
* Print the environment variable names matching *init_chars*.
* We can access these variables using the methods we have seen already.
```bash
echo ${!init_chars*}
```
#### Example 
- List of names of shell variables that start with ` H ` and ` HI `.
```bash
~$ echo ${!H*}
HISTCMD HISTCONTROL HISTFILE HISTFILESIZE HISTSIZE HOME HOSTNAME HOSTTYPE
~$ echo ${!HI*}
HISTCMD HISTCONTROL HISTFILE HISTFILESIZE HISTSIZE
~$ echo ${HISTFILE}
/home/groot/.bash_history
```

### Length of String Value
* Display length of the string value of the variable ` myvar `.
* If ` myvar ` is not set, display ` 0 `.
```bash
echo ${#myvar}
```
#### Example 
- Getting length of string returned by ` date ` command stored in a variable.
```terminal
~$ mydate=` date `
~$ echo ${mydate}
Thursday 22 December 2022 04:50:31 PM EET
~$ echo ${#mydate}
41
~$ myvar=
~$ echo ${#myvar}
0
```

### String Operations
#### Slice of String Value
* Provide *offset* and *slice_length* separated by ` : `.
* ` ${varname:offset:slice_length} `
* Display *4 chars* of the string value of the variable ` myvar ` skipping first *5 chars*.
* If *slice_length > ${#varname}*, *slice_length = ${#varname}*.
* Offset value can be negative.
```bash
echo ${myvar:5:4}
```
##### Examples
* Extract part of string by using offset from the beginning.
```terminal
~$ myvar=abcdefgh12345678
~$ echo ${myvar:3:3}
def
~$ echo ${mydate:0:6}
Sunday
```

* Extract part of string by using offset from the end.
	- notice ` <space> ` between operator ` : ` and ` - ` sign.
```terminal
~$ myvar=abcdefgh12345678
~$ echo ${myvar: -3:3}
678
~$ echo ${mydate: -3:2}
67
```

* Using command and variable to obtain the same result. 
```terminal
~$ date
Thursday 22 December 2022 05:10:53 PM EET
~$ date +"%d %B %Y"
22 December 2022
~$ mydate=` date `
$ echo ${mydate:9:16}
22 December 2022
```

#### Remove Prefix Matching a Pattern 
* Match the string from the beginning.
* Pattern is regex (later)
* ` * ` matches any number of characters.
* Match once using ` ## `
```bash
echo ${myvar#pattern}
```

```terminal
~$ myvar=MyFile.tar.gz
~$ echo ${myvar#*.}
tar.gz
~$ echo ${myvar#*.*.}
gz
```
* Match max possible using ` ### `
```bash
echo ${myvar##pattern}
```

```terminal
~$ myvar=MyFile.tar.gz
~$ echo ${myvar##*.}
gz
```

#### Remove Suffix Matching a Pattern
* Match the string from the end.
* Match once using ` % `
```bash
echo ${myvar%pattern}
```

```terminal
~$ myvar=MyFile.tar.gz
~$ echo ${myvar%.*}
MyFile.tar
~$ echo ${myvar%.*.*}
MyFile
```

* Match max possible using ` %% `
```bash
echo ${myvar%%pattern}
```
```terminal
~$ myvar=MyFile.tar.gz
~$ echo ${myvar%%.*}
MyFile
~$ echo ${myvar%%.*}.${myvar##*.}
MyFile.gz
~$ echo ${myvar%%.*}.zip
MyFile.zip
```

#### Replace Matching Pattern
* Replace matching pattern from anywhere in the string.
* *pattern* and **string** are separated with ` / `
* Match *pattern* once (` / `) and replace with **string**.
```bash
echo ${myvar/pattern/string}
```
* Example 1 - Change only first occurrence of *pattern* with **string**
```terminal
~$ myvar=MyFile.SomeThing.jpeg
~$ echo ${myvar/e/E}
MyFilE.SomeThing.jpeg
```

* Match *pattern* max possible (` // `) and replace with **string**.
```bash
echo ${myvar//pattern/string}
```

```terminal
~$ echo ${myvar//e/E}
MyFilE.SomEThing.jpEg
~$ myvar=MyjpegFile.Something.jpeg
~$ echo ${myvar//jpeg/jpg}
MyjpgFile.Something.jpg
~$ myfname=` echo ${myvar//jpeg/jpg} `
~$ echo $newfname
MyjpgFile.Something.jpg
```

#### Replace Matching Pattern by Location
* Match and replace the prefix (` /## `)
	- match at the *beginning*
```bash
echo ${myvar/#pattern/string}
```

```terminal
~$ echo ${myvar/#M/m}
myFile.SomeThing.jpeg
~$ mydate=`date`
~$ newdate=` echo ${mydate/#*day }`
~$ echo $newdate
22 December 2022 05:10:53 PM EET
```

* Match and replace the suffix (` /% `)
	- match at the *end*
```bash
echo ${myvar/%pattern/string}
```

```terminal
~$ echo ${myvar/%jpeg/jpg}
MyFile.SomeThing.jpg
```

#### Changing Case to Lower Case
* Uses ` , ` (comma) symbol.
* Changes only view and not the value.
* Change first character to lower case using ` , `
```bash
echo ${myvar,}
```

```terminal
~$ mymonth="MARGALI"
~$ echo ${mymonth,}
mARGALI
```


* Change all character to lower case using ` ,, `
```bash
echo ${myvar,,}
```
```terminal
~$ echo ${mymonth,,}
margali
```

#### Changing Case to Upper Case
* Uses ` ^ ` (caret) symbol.
* Changes only view and not the value.
* Change first character to Upper case using ` ^ `
```bash
echo ${myvar^}
```

```terminal
~$ mymonth="margali"
~$ echo ${mymonth^}
Margali
```

* Change all characters to UPPER case using ` ^^ `
```bash
echo ${myvar^^}
```

```terminal
~$ mymonth="margali"
~$ echo ${mymonth^^}
MARGALI
```

### Set or Unset Attributes on  Value Types
* Attributes are some restrictions.
* ` declare [option] varname `
* ` declare [option] varname=value... `
* ` - ` is used to set attribute and ` + ` is used to remove attribute.
#### Options which set and unset attributes: 
* ` -i ` : Can only assign ` integer ` values, strings converted to integer 0.
```bash
declare -i myvar
```

```terminal
~$ declare -i mynum=1729
~$ echo $mynum
1729
~$ mynum="hello world"
~$ echo $mynum
0
```

* ` +i ` : Remove integer attribute/restriction
```bash
declare +i myvar
```

```terminal
~$ declare +i mynum
~$ mynum="hello world"
~$ echo $mynum
hello world
```

* ` -l ` : Convert value to lower case on assignment.
```bash
declare -l myvar
```

```terminal
~$ declare -l myvar="HELLO WORLD"
~$ echo ${myvar}
hello world
```

* ` +l ` : Remove lower case restriction.
```bash
declare +l myvar
```

*  ` -u ` : Convert value to upper case on assignment.
```bash
declare -u myvar
```

```terminal
~$ declare -u MYVAR="hello world"
~$ echo ${MYVAR}
HELLO WORLD
```

*  ` +u ` : Remove upper case restriction.
```bash
declare +u myvar
```

* ` -r ` : Make the variable read only. ` + ` can not be used to remove this attribute.
```bash
declare -r myvar
```

```terminal
~$ declare -r PI=3.142
~$ echo $PI
3.142
~$ PI=2.142
bash: PI: readonly variable
~$ declare +r PI
bash: declare: PI: readonly variable
```

### Indexed Arrays
* Declare an indexed array ` arr ` using ` -a ` 
```bash
declare -a arr
```

```terminal
~$ declare -a arr
```

#### Indexed Array Operations
* Set value of element with some index in the array.
```bash
arr[0]="value"
```
```terminal
~$ arr[0]=Sunday
~$ arr[1]=Monday
```

* Access value of element with index 0 in the array.
```bash
echo ${arr[0]}
```

```terminal
~$ echo ${arr[0]}
Sunday
~$ echo $arr
Sunday
~$ 
```

* Number of elements in the array (` @ ` : all elements).
```bash
echo ${#arr[@]}
```
```terminal
~$ echo ${#arr[@]}
2
```

* Display all indices used. (Indices in bash array can be sparse.)
```bash
echo ${!arr[@]}
```

```terminal
~$ echo ${!arr[@]}
0 1
```

* Diplay values of all elements in the array.
```bash
echo ${arr[@]}
```

```terminal
~$ echo ${arr[@]}
Sunday Monday
```

* Array indices are sparse
	- indices are sorted in natural order.
```terminal
~$ arr[100]=NotToBeDay
~$ echo ${arr[@]}
Sunday Monday NotToBeDay
$ echo ${!arr[@]}
0 1 100
```

* Delete element with index 100 in the array
```bash
unset 'arr[100]'
```

```terminal
~$ unset 'arr[100]'
~$ echo ${arr[@]}
Sunday Monday
```

* Append an element with a value to the end of the array.
	- Multiple elements can be separated with space
```bash
arr+=("value")
```

```terminal
~$ arr+=(Tuesday)
~$ echo ${arr[@]}
Sunday Monday Tuesday
```

* Array declaration and initialization
	- ` <space> ` as separator.
	- indices start with 0
```terminal
~$ declare -a weekdays=(Sunday Monday Tuesday Wednesday Thursday Friday Saturday)
~$ echo ${!weekdays[@]}
0 1 2 3 4 5 6
~$ echo ${weekdays[@]}
Sunday Monday Tuesday Wednesday Thursday Friday Saturday
```

* ` ls ` command output into array
```terminal
~$ declare -a arr
~$ arr=(` ls `)
```

### Associative Arrays
* Declare an associative array or hash using ` -A `
```bash
declare -A hash
```

```terminal
~$ declare -A hash
```

#### Associative Arrays Operations
* set value of element with index ` "a" ` in the array.
```bash
hash["a"]="value"
```
```terminal
~$ hash[0]="Amul"
~$ hash[1]="Gokul"
~$ hash["city"]="Madras"
```

* Access value of element with index ` "a" ` in the array.
```bash
echo ${hash["a"]}
```
```terminal
~$ echo ${hash["city"]}
Madras
```
* Number of elements in the array (` @ ` : all elements).
```bash
echo ${#hash[@]}
```

```terminal
~$ echo ${#hash[@]}
3
```

* Display all indices used. (Indices in bash array can be sparse.)
```bash
echo ${!hash[@]}
```

```terminal
~$ echo ${!hash[@]}
0 1 city
```

* Diplay values of all elements in the array.
```bash
echo ${hash[@]}
```

```terminal
~$ echo ${hash[@]}
Amul Gokul Madras
```

* Delete element with index ` "a" ` in the array
```bash
unset 'hash["a"]'
```

```terminal
~$ unset 'hash["city"]'
~$ echo ${hash[@]}
Amul Gokul
```

Shell Variable Manipulations are FAST!	
## Linux Process Management
* Helps to switch between tasks while we are in the command line environment.
### ` sleep `
* Delay for a specified amount (` NUMBER `) of time.
```bash
sleep NUMBER
```
* To ` sleep ` for 10 seconds.
```terminal
~$ sleep 10

```

We will use ` sleep ` a simple process to demonstrate Linux process management.

### ` & `
*  A process is run in the background.
```bash
command &
```

```terminal
~$ sleep 30 &
[1] 5477
```
* ` [1] ` denotes the command number that is pushed to background. Not to be confused with ` 5477 ` which is ` PID `

### ` coproc `
* Bash builtin command to create interactive asynchronous process.
* The program runs asynchronously and can listen to standard input and give back standard output on need.

* Run command(s) in an executing shell. ` NAME ` is optional, default name is ` COPROC `
```bash
coproc [NAME] { command; }
```
* Write to a specific coprocess stdin
```bash
echo "input" >&"${NAME[1]}"
```
* Read from a specific coprocess stdout
```bash
read varname <&"${NAME[0]}"
```

* Create a coprocess running `bc`
```terminal
~$ coproc BC { bc --mathlib; };	## creates a coprocess
[1] 3820
~$ echo "22/7" >&"${BC[1]}" 	## write input to coprocess  
~$ read output <&"${BC[0]}"	## read output from coprocess
~$ echo $output	## echo the read output.
3.14285714285714285714
```
- run ` ps --forest ` to see the process tree.
	
* Run command `sleep 10` as a coprocess.
	- command run within the same shell.
	- process name can not be given.
	- default name is used.
```terminal
~$ coproc sleep 10
[1] 5499
~$ 
[1]+	Done				coproc COPROC sleep 10
```


### ` fg `
* Bring a recent process running in the background to the foreground.
```terminal
~$ sleep 30 &
[1]	5510
~$ fg
sleep 30

```

### ` jobs `
* List the processes running in the background by the user.
```terminal
~$ coproc sleep 30
[1] 5583
~$ jobs
[1]+  Running                 coproc COPROC sleep 30 &
```


### ` top `
* Prints summary and live snapshots of processes running onto the screen.
* It is in decreasing order of CPU utilization ` %CPU `.
* Use ` q ` key or ` ^C ` to quit, ` ^Z ` to suspend it (move to background).  
```bash
top
```

### ` kill `
* Kill a command using it's process id.
```bash
kill -9 <process id>
```
```terminal
~$ coproc sleep 10
[1] 5507
~$ kill -9 5530
~$
[1]+	Killed				coproc COPROC sleep 10		
```

### ` $! `
* It's an environment variable which stores the ` PID ` of the last process that is running/has run in the background.
* The value persits even if the process is finished.


## More on [Program Exit Codes](/Week_2/Lecture2.md#program-exit-codes)

*  If a command is run successfully then error code is ` 0 `.
```
~$ echo hello
hello
~$ echo $? 
0
```

* If a program/command has failed (like permission error) then error code is ` 1 `.
```terminal
/$ touch file1
touch: cannot touch 'file1': Permission denied
/$ echo $?
1

```

* If there is misuse of shell builtins, then error code is ` 2 `
```terminal
~$ ls -w
ls: option requires an argument -- 'w'
Try 'ls --help' for more information.
~$ echo $?
2
~$ help -h
bash: help: -h: invalid option
help: usage: help [-dms] [pattern ...]
~$ echo $?
2
```

* If the command can not be executed then error code is ` 126 `
	- ` file1 ` is not executable.
```terminal
~$ stat -c "%A" file1
-rw-rw-r--
~$ ./file1; echo $?
bash: ./file1: Permission denied
126
```

* If the command is not found then the error code is ` 127 `
```terminal
~$ daet
```
Very big output.
```terminal
~$ echo $?
127
```

* If the command is killed/inturrupted with ` ^C `, then the error code is `130`
```terminal
~$ sleep 30
^C
~$ echo $?
130
```

* If the process in some other shell is killed using ` kill -9 ` command, then the error code is ` 137 ` 

Terminal 1 : Iterative output of ` top ` command	
```terminal
~$ top
```
Terminal 2 : Kill command ` top ` running in Terminal 1.
```terminal
~$ kill -9 2267
~$
```

Terminal 1 : Response
```terminal
Killed
~$ echo $?
137
```

* Other exit codes
	- Returns exit code modulo 256 on exit code greater than 255.
```terminal
~$ bash -c "echo \$$; exit 3243;"
6669
~$ echo $?
171
```

Exit codes are useful when in a program you want to run a process based on exit code of other process.


## More on [Flags Set in Bash](/Week_2/Lecture2.md#flags-set-in-bash)

### ` $- `
* Special variable to print the flags set in bash.

```terminal
~$ echo $-
himBHs
```

* Let's run a command to check if there can be less number of flags.
	- ` bash ` : spawn a childshell
	- ` -c `  : consider first non-option argument as command
	- ` \ ` : escape character to stop early interpretation of ` $- `
```terminal
~$ bash -c "echo \$-"
hBc
```

* Check the pid of childshell and printing the forest view using ` ps `. 
	- Commands are separated with ` ; `
```terminal
~$ bash -c "echo \$$; echo \$-; ps --forest"
```

### ` history ` command

* Stores commands run so far as history.
* ` !NUMBER ` (bang) is used to refer `NUMBER`th command. 
	- This is enabled when ` H ` flag is set.
```bash
history
```
* To run ` echo $0 ` at position `2023` 
```terminal
~$ !2023
echo $0
bash
```

### Brace Expnasion
* Enabled using ` B ` flag.
* Expand ` {start..stop} ` into list of values between ` start ` and ` stop ` inclusive.
```terminal
~$ echo {a..d}
a b c d
~$ echo {5..1}
5 4 3 2 1
```

### Other Expansions
* A wildcard ` * ` matches all files.
```terminal
~$ echo *
Desktop Documents Music Pictures Public snap Templates Videos
~$ echo D*
Desktop Documents Downloads
```

## Multiple Commands on a Line
* Use semicolon ` ; ` to separate commands.
* Combining ` ls `, ` date ` and ` wc ` commands on same line.
* ` ; ` in the end is optional.
```terminal
~$ ls; date; wc -l /etc/profile;
Desktop Documents Music Pictures Public snap Templates Videos
Friday 23 December 2022 01:36:49 PM EET
27 /etc/profile
```


