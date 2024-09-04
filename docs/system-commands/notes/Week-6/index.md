# Knowing your Hardware

## Hardware Items

* CPU
* Storage and Partitions
* Graphics Card
* Memory Modules
* Battery and Status
* Network devices and configuration

## Required Packages

The following packages are required to explore the hardware


| | | | |
| :----: | :----: | :----: | :----: |
| clinfo | coreutils | dmidecode | fdisk |
| hardinfo | hdparm | hwinfo | lshw |
| memtester | net-tools | pciutils | procps |
| sysstat | upower | util-linux | |
| | | | |


### ` hwinfo ` 

* It is used to probe for the hardware present in the system.
* You can probe for a specific hardware and get information about it.
* For this `--<HARDWARE_ITEM>` option is used, i.e. probe for a particular **HARDWARE_ITEM**.
* Available hardware items list can be found using `man hwinfo` command.
* A few are

>	all, arch, bluetooth, memory, scanner, printer, mouse 

```bash
hwinfo
```

* Output is roughly equivalent to ` hwinfo --all --log=- `.

* Task :
	- Run the ` hwinfo ` command and store output to a file. Read the using `less`.

### ` lshw `

* It is used to get detailed information on the hardware configuration of the machine.

* It can report exact memory configuration, firmware  version,  mainboard configuration,  CPU  version  and  speed,  cache configuration, bus speed, etc

```bash
lshw
```

* To get information about specific hardware.
	- ` lshw -c <HARDWARE> `

```bash
lshw -c display
```

* Task:
	- Print information about ` memory `.

### ` /proc/cpuinfo `

* This filesystem contains information about the CPU.
* in file
    - *flags*: Capabilities of the CPU
    - *cache*: How well the machine performs the computation?

```bash
cat /proc/cpuinfo
```

### `/proc/partitions`

* This filesystem has the partition information.
* loop devices usually are there for snap packages.

```bash
cat /proc/partitions
```

### ` lsblk `

* It lists information about all available or the specified block devices. 
* The ` lsblk ` command reads the ` sysfs ` filesystem and ` udev db ` to gather information.
* If ` udev db ` is absent, it gathers information from LABELs, UUIDs and filesystem types from the block device (`su` required).

```bash
lsblk -o NAME,SIZE
```

* ` -o ` or ` --output ` is used to gather  only ` NAME ` and ` SIZE ` of the block device.

### ` lspci `

* It lists information about **PCI** (Peripheral Component Interconnect, a type of expansion slot on motherboard) buses and devices connected to them.

```bash
lspci
```

### ` free `

* Display amount of free and used memory in the system.

```bash
free -h
```

* ` -h ` : human-readable

### ` dmidecode `

* low level command, ` su ` required.
* The tool is used for dumping   a  computer's DMI (some  say  SMBIOS) table contents in a human-readable    format.

```bash
dmidecode -type memory
```

* Task:
	- Explore the ` man ` page to know more about ` --type ` keyword arguments

### ` hardinfo `

* A GUI based utility to display hardware information.

```bash
hardinfo
```

### ` clinfo `

* Prints the information about graphics card.

```bash
clinfo
```


### ` upower `

* It's a tool used to display battery status.
* It is based on UPower daemon.

```bash
upower -e 	# lists all the battery devices.
```

* To explore the **ITEM** in the above list use the syntax below.

```bash
upower -i <ITEM>
```

### ` hdparm `

* `hdparm` utility is used to check hard disk parameters.
* This command shows the speed of the disk.

```bash
hdparm -Tt /dev/sda
```

<!--
df
df - report file system disk space usage
-->

### ` iostat `

* Prints the information about input output speed.
* The output of this command can be used to compare speed at different times with speed at ideal timing.

```bash
iostat -dx /dev/sdb
```

### ` ifconfig `

* Prints the network configuration of the system like wi-fi, ethernet and loopback connection.

```bash
ifconfig
```
# Promp Strings
Fields and Customization

## Context of Prompt String

* Shells: bash, dash, zsh, ksh csh
* python: An elegant programming language
* octave: GNU octave language for numeric computations
* gnuplot: Command-line driven interactive plotting program.
* sage: Open source mathematical software

## Bash Prompts

* ` PS1 ` : primary prompt string : ` $ `
* ` PS2 ` : secondary prompt for multi-line input : ` > `
* ` PS3 ` : prompt string in select loops : ` #? `
* ` PS4 ` : prompt string for execution trace : ` + `

## Escape Sequences

| escape | Description |
| ------ | ----------- |
| ` \A `   | Current time in 24-hour as *hh:mm* |
| ` \d `   | Date in *weakday month day* format |
| ` \h `  | *Hostname* upto first period |
| ` \H ` | Complete *hostname* |
| ` \s ` | Name of the *shell*  |
| ` \t ` | Current time in 24-hour as *hh:mm:ss* |
| ` \T ` | Current time in 12-hour as *hh:mm:ss* |
| ` \u ` | Current user's *username* |
| ` \w ` | Current directory |
| ` \W ` | Basename of current directory |
| ` \# ` | Current command number |
| ` \$ ` | If uid is 0, # else $ |
| ` \@ ` | Current time in 12-hour am/pm |
| ` \\ ` | A literal \ character |

* Default prompt

```bash
\u@\h:\w\$
```

- username (` \u `), @ literal, machine name (` \h `), : literal, current directory (` \w `) followed by $ literal if the user is not superuser. 


### Examples

#### ` PS1 `

* The default prompt without colors

```terminal
~$ PS1="\u@\h:\w\$ "
a@a:~$ 
```

* Display time along with current directory

	- A typical output is given below.

```terminal
a@a:~$ PS1="\t:\w\$ "
21:47:07:~$ 
```

* Display date along with current directory

```terminal
21:48:05:~$ PS1="\d:\w\$ "
Fri Feb 17:~$ 
```

* Date and time together with current directory

```terminal
Fri Feb 17:~$ PS1="\d \t:\w\$ "
Fri Feb 17 21:50:01:~$ 
```

* Number of the command.

```terminal
Fri Feb 17 21:50:01:~$ PS1="\#:\$ "
45:$
```

* Doesn't look good? Source the default prompt
	- bash prompt is defined in `~/.bashrc` filesystem. 

```bash
source .bashrc
```


#### ` PS2 `

* This prompt is made available when there are unmatching quotes or newline escapes.
* Change the ` PS2 ` prompt to *~>*

```bash
PS2="~>"
```

### ` PS3 `

* This prompt can not be printed but can be seen within ` [select]() ` loop.

* ` select ` gives a menu, and on choosing right menu option (choose a menu item number), it can perform operations which are specified within the loop.

```bash
select x in alpha beta gamma; do echo $x; done
```

* ` #? ` is the prompt you will see, when you execute above expression.

* Change the prompt to 'choose your option as above: '

```bash
PS3="choose your option as above: "
```

* Execute the ` select ` code above again to see the prompt change.

### ` PS4 `

* It is activated when ` set -x `, used to debug, command is run.
* The typical output is shown below.

```terminal
~$ set -x
~$ pwd
+ pwd
/home/a
```

* Change ` PS4 ` to 'Now Running Command: '

```terminal
~$ PS4='Now Running Command: '
+ PS4='Now Running Command: '
~$ date
Now Running Command: date
Fri Feb 17 05:52:23 PM +06 2023
```

## Python Command Line

* ` ps1 ` and ` ps2 ` are defined in the module ` sys `
* Change ` sys.ps1 ` and ` sys.ps2 ` is needed
* Override ` __str__ ` method to have dynamic prompt.
* Default prompt:

```console
>>>
```

## octave prompt

* See prompt changing in octave, as we plot
	- x and y are array from 1 to 100, with hops of 10.

```terminal
~$ octave
octave:1> x=[1:10:100]
octave:2> y=[1:10:100]
octave:3> plot(x,y)
```

## gnuplot prompt

```terminal
~$ gnuplot
gnuplot> 
```

## sage prompt

```terminal
~$ sage
sage: plot(sin(x),x,0,2*pi)
```
# Utilities in GNU/Linux

Tools to augment your productivity.

## ` find `

* locating files and processing them

```bash
find [pathnames] [conditions]
```

| condition | Description |
| :-------: | :------- |
RrR -name `   | *pattern* to match filenames |
| ` -type `   | File type code eg., ` c ` for character file, ` d ` for direcotry, ` l ` for symbolic link 
| ` -atime `  | File accessed ` +n ` (more than n), ` -n ` (less than n) days ago |
| ` -ctime `  | File changed ` +n ` (more than n), ` -n ` (less than n) days ago  |
| ` -regex `  | Regular expression for *pattern* of filenames. Combine with -regextype posix-basic, posix-egrep etc. |
| ` -exec `   | Command to run using ` { } ` as place holder for filename. |
| ` -print `  | Print the full path name of matching files |

Examples

* Print all the file paths in home directory.

```bash
find $HOME -print 
```

* Count the number of files in home directory.

```bash
find $HOME -print | wc -l
```

* List the file names in home directory that are modified in last two days.

```bash
find $HOME -mtime -2 -print
```

* List the file names in home directory that are modified more than a month ago in home directory.

```bash
find $HOME -mtime +30 -print
```

* List the man page direcotries in ` /usr ` directory.
    - ` ? ` matches one character after *man*

```bash
find /usr -type d -name 'man?' -print
```

* List the files which are more than 10 MB.

```bash
find . -size +10M -print
```

* Using ` -exec ` long list with ` -h ` option the files which are more than 10 MB.
    - ` {} ` is the placeholder for file names

```bash
find . -size +10M -exec ls -lsh {} \;
```

* List all the jpeg files along with it's size in human readable form.

```bash
find . -name '*.jpg' -exec ls -sh {} \;
```

## File Packaging

* Deep file hierarchies
* Large number of tiny files
* ` tar ` : collect a file hierarchy into a single file
* ` gzip ` : compress a file
* Applications : backup, file sharing, reduce disc utilization

### Possibilities

* ` tar `, ` zip `
* ` compress ` (ncompress), ` gzip ` (ncompress), ` bzip2 ` (bzip2), ` xz `(xz-utils), ` 7z ` (p7zip-full)
* Tarballs like bundle.tgz for package + compress
* Time and memory required to shrink / expand versus size ratio
* Portability
* Unique names using timestamp, process ID etc., for backup tarballs

## ` tar `

* packaging (archiving) collection of files.

```text
tar -[cvx] [-f ARCHIVE] [files]
```

Examples

* Getting all log files in a directory ` logfiles `

```bash
cp -r /var/log logfiles
```

* Storage space used by logfiles directory.

```bash
du -sh logfiles/
```

* Creating a bundle of all log files in directory ` logfiles `.

```bash
tar -cvf logfiles.tar logfiles/
```

* Extracting files from tarball

```bash
tar -xvf logfiles.tar logfiles/
```

## ` gzip `

* Compress or expand files
* Extension: **.gz**.

```text
gzip [OPTIONS] [file]
```

Examples

* Compressing ` logfiles.tar `

```bash
gzip logfiles.tar
```

* Decompressing the ` logfiles.tar.gz ` file

```bash
gunzip logfiles.tar
```

## ` bzip2 `

* Compress or expand files
* More efficient than ` gzip ` in terms of compression ratio.
* Extension: **.bz2**.

```text
bzip2 [OPTIONS] [file]
```

Examples

* Compressing ` logfiles.tar `

```bash
bzip2 logfiles.tar
```

* Decompressing the ` logfiles.tar.bz2 ` file

```bash
bzip2 -d logfiles.tar.bz2
```

## ` compress `

* Compress or expand files
* Less efficient than both ` gzip ` and ` gzip2 ` in terms of compression ratio.
* Extension: **.Z**.

```text
compress [OPTIONS] [file]
```

Examples

* Compressing ` logfiles.tar `

```bash
compress logfiles.tar
```

* Decompressing the ` logfiles.tar.Z ` file

```bash
uncompress logfiles.tar.Z
```

## ` make `

Used to

* compile source code
* Conditional actions
* Conditional running of the scripts.
* Maintainance activity.
* Only performs action when the target files have changed.

```bash
make -f make.file # make.file is make script file.
```

* See [make.file](files/make.file)

Examples

* Running a section of the make file
  - The command below runs the ` backup ` section from the file ` make.file `.
```bash
make -f make.file backup 
```

# Automating Scripts

* Scheduled, recurring, automatic execution of scripts.

## ` cron ` 

* Service called ` cron.d ` (a daemon) to run scripts automatically at scheduled times.
* Tools : ` at `, ` crontab `, ` anacron ` and ` logrotate `.
* Script locations : 
  - ` /etc/crontab `
  - ` /etc/cron.d `
  - ` /etc/cron.hourly`
  - ` /etc/cron.daily `
  - ` /etc/cron.weekly `
  - ` /etc/cron.monthly `

* ` cron ` feature can be used for
  - Rotating log 
  - Taking backup
  - Cleaning up temporary files
  - Getting alerts through email on system failures.

### Job Definition

* Run a mkbackup.sh as root every working day at 02:05 AM.

```terminal
# ----------------------------- minute (0 - 59)
# |  --------------------------- hour (0 - 23)
# |  |  ------------------------- day of month (1 - 31)
# |  |  |   -------------------- month (1 - 12) OR jan,feb,... 
# |  |  |   |   ------------- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,...
# |  |  |   |   |   --------- user-name
# |  |  |   |   |   |    ---- command to execute
# |  |  |   |   |   |    |
# m  h dom mon dow user cmd
  5  2  *   *  1-5 root cd /home/a/scripts/backup && ./mkbackup.sh 
```

### Startup Scripts

* Some scripts are run every time you boot the machine.
* Startup scripts
  - ` /etc/init/ `
  - ` /etc/init.d/ `

* Runlevel Scripts
  - Scripts run to perform certain actions and to run the machine in a specific mode.


|       |          |              |
| :---: | -------- | ------------ |
| 0 | ` /etc/rc0.d/ ` | Shutdown and power off |
| 1 | ` /etc/rc1.d/ ` | Single user mode |
| 2 | ` /etc/rc2.d/ ` | Non GUI multi-user mode w/o networking |
| 3 | ` /etc/rc3.d/ ` | Non GUI multi-user mode with networking |
| 4 | ` /etc/rc4.d/ ` | Non GUI multi-user mode for special purposes |
| 5 | ` /etc/rc5.d/ ` | GUI multi-user mode with networking (default) |
| 6 | ` /etc/rc6.d/ ` | Shutdown and reboot |

### Examples

* See file [mkbackup.sh](files/mkbackup.sh)

* Opening ` crontab `
  - After the command, select your favourite editor from options given.

```bash
crontab -e
```

* To run the ` mkbackup.sh ` script insert the line below in ` crontab ` file. (Pick time suitable to your needs)

 - ` 27 * * * * cd /home/a/scripts/backup && ./mkbackup.sh `
  - The script will run automatically at choosen time.
# ` sed ` 

* A language for processing text streams.

## Introduction

* It is a programming language
* ` sed ` is an abbreviation for `s`tream `ed`itor
* It is a part of POSIX
* sed precedes awk

## Execution model

* Input stream is a set of lines
* Each line is sequence of characters
* Two data buffers are maintained: active *pattern* space and auxiliary *hold* space
* Matched patterns are found in the *hold* space if parenthesis is used and the *pattern* space will contain the line that is read.
* For each line of input, an **execution cycle** is performed loading the line into *pattern* space
* During each cycle, all the statements in the script are executed in the sequence for matching **address pattern** for **actions** specified with the **options** provided

## Usage

* Single line at the command line

```bash
sed -e 's/hello/world/g' input.txt 
```

* Script interpreted by ` sed `

```bash
sed -f ./myscript.sed input.txt 
```

  - [myscript.sed](files/myscript.sed)

## sed statements
 
*  <!-- 4:13 week 6 lecture 5  add svg -->

```text
:label address-pattern action options
```

* Grouping commands

```text
{ cmd ; cmd ; }
```

### address

* Selecting by numbers : 
  - ` 5 ` - 5th line
  - ` $ ` - the last line 
  - ` % ` - all the lines
  - ` 1~3 ` - every 3rd line starting from 1st line

* Selection by text matching : 
  - ` /regexp/ ` - lines which match regexp

* range addresses : 
  - ` /regexp1/,/regexp2/` - from first line which matches regexp1 till the line that matches regexp2
  - ` /regexp/, +4 ` - 4 lines from first line which matches regexp
  - ` /regexp/, ~2 ` - every second line from first line which matches regexp 
  - ` 5,15 ` - from line 5 to 15
  - ` 5,/regexp/` - from line 5 to line which first matches regexp 

* In GNU extended version of ` sed ` the starting and ending part of the address both can be *regexp*.

| action | description    |
|:---------------: | --------------- |
| ` p `   | Print the pattern space   |
|  ` d `  | Delete the pattern space   |
| ` s `   | Substitute using regex match ` [address]s/search/replace/[flags] ` |
| ` = ` | Print current input line number, \n |
| ` # ` | comment | 
| ` i `   |  Insert above the current line  |
|  ` a `  | Append below the current line |
| ` c `   | Change current line |

### programming

|  syntax  |  description   |
|:--------------:| --------------- |
| ` b label ` | Branch unconditionally to *label* |
|  ` :label `  | Specify location of *label* for branch command  |
| ` N `   | Add a new line to the pattern space and append line of input into it  |
| ` q `   | Exit sed without processing further commands or input lines  |
| ` t label `   | Branch to label only if there was a successful substitution was made   |
|  ` T label `   |  Branch to label only if there was **no** successful substitution was made |
| ` w filename `   | Write pattern space to filename   |
| ` x `   | Exchange the contents of hold and pattern spaces |


## bash + sed

* Including sed inside shell script
* heredoc feature
* Use with other shell scripts on command line using pipe

*sed is available everywhere !*

*sed is a meant for text processing, fast in execution*

*use sed to pre-process input for further processing*

## Demonstration

* [sample.txt](files/sample.txt)

* The default action of the stream editor is to print the line that is sent to it.
  - output same as ` cat sample.txt `
```bash
sed -e "" sample.txt
```

* Suppressing the default action of printing using ` -n ` option

```bash
sed -n -e "" sample.txt
```

* Print the line number and ` \n ` and then the line itself

```bash
sed -e "=" sample.txt
```

### Selection by numbers

* Print only a line specified by line number as address
  - print 5th line

```bash
sed -n -e "5p" sample.txt 
```
  - if ` -n ` option is omitted it will print all lines with 5th line printed twice

* To print special characters as it is without any interpretation single quote are useful.

* To print all lines except a particular line (using !)
  - print all lines except 5th line

```bash
sed -e '5!p' sample.txt
```

* Print the last line using ` $ `

```bash
sed -n -e '$p' sample.txt
```

  - Replacing ` '$p' ` with ` "$p" ` will match the value in shell variable ` p `
  - Hence, ` "$VARNAME" ` can be used to match for a shell variable value.


* Print every 5th line starting from line 1

```bash
sed -n -e '1~5p' sample.txt
```

* A single line can be deleted using the line number.
  - delete the 5th line

```bash
sed -e '5d' sample.txt
```

### Selection by address

* Print lines 5 to 8

```bash
sed -n -e '5,8p' sample.txt
```

* Separating commands with ` ; `
  - Print all line numbers and contents of lines 5 to 8.

```bash
sed -n -e '=; 5,8p' sample.txt
```

* Print line numbers and the content for only lines 5 to 8

```bash
sed -n -e '5,8{=;p}' sample.txt
```

* Print lines where the address matches the word ` microsoft ` in regexp.

```bash
sed -n -e '/microsoft/p' sample.txt
```

* Try for yourself:
  - print the line which matches address ` /in place of/ `.
  - print lines which does not match the address ` /text/ `


* Printing a specific number of lines after the address match.
  - print 2 lines after including the matching address.

```bash
sec -n -e '/adobe/,+2p' sample.txt
```

* A range of lines can be deleted specified by address
  - to delete lines 1 to 5

```bash
sed -e '1,5d' sample.txt
```

* Try for yourself:
  - delete all the lines.
  - delete lines except lines 4 to 6
  - delete every 2nd line after matching address ` /microsoft/ `

### The ` s ` command

* The most used command in ` sed `.

* Search for the word `microsoft` and replace it with `MICROSOFT`

```bash
sed -e 's/microsoft/MICROSOFT/g' sample.txt
```

  - Before ` s ` comes the address space. Since it is not specified the command is applicable for all lines.
  - ` g ` flag is used to match *search* regexp and replace all the occurrences of it with ` replace ` string on a line.

* On first line, replace the word ` linux ` with ` LINUX `

```bash
sed -e '1s/linux/LINUX/g'
```

* Try for yourself:
  - replace all occurrences of *in place of* with *in lieu of*

* By default sed uses BRE, with ` -E ` we can force to use ERE.

* Use ERE to search for ` L ` and one or more digits followed by space and replace it with empty string.

  - for line 3 to 6

```bash
sed -E -e '3,6s/^[[:digit:]]+ //g' sample.txt
```

  - from line 3 upto the occurrence of phrase *symbolic*.

```bash
sed -E -e '3,/symbolic/s/^[[:digit:]]+ //g' sample.txt
```

* Try for yourself:
  - do the same for all lines.
  - do the same starting from line 1, every 3rd line.
  - do the same for even numbered line (with and without using ` ! `).
  - do the same for the address range ` /text/,/video/ `

### Adding a line before and after a line.

* The sed ` i ` (`i`nsert) command can be used to add a line before a line specified by address.
* The sed ` a ` (`a`ppend) command can be used to add a line after a line specified by address.

* To insert a header line before the first line.

```bash
sed -e '1i -----------header-----------' sample.txt
```

* To append footer after the last line.

```bash
sed -e '$a -----------footer-----------' sample.txt
```

* Both the operations can be performed with a single command with ` -e ` spanning the same command.

```bash
sed -e '1i -----------header-----------' -e '$a -----------footer-----------' sample.txt
```

* Add a break after before every 5th line starting with line 1

```bash
sed -e '1~5i ------- break --------' sample.txt
```
* The two operations can be combined together to perform meaningful insertion and append operations on different address ranges.

### Replacing or changing a line with some text

* The sed ` c ` (`c`hange line) command can be used to change che contents of a line with new content.

* To change the lines which match the address ` /miscosoft/ `

```bash
sed -e '/microsoft/c ------censored------' sample.txt
```

## Sed Scripts 

A sed script is a file which contains the sed commands (the part in quotes after ` -e `).
The commands given in the script are executed line by line on input file.

A sample script file can be found as [hf.sed](files/hf.sed).

* Executing the [hf.sed](files/hf.sed) on the file [sample.txt](files/sample.txt)

```bash
sed -f hf.sed sample.txt 
```

* We will now process the file [block-ex-6.input](/Week-7/files/block-ex-6.input) with script [clean.sed](files/clean.sed) which produces only roll number and amount separated by space.

```bash
sed -E -f clean.sed block-ex-6.input
```

## Joining Lines using

* Joining lines requires sed to read one more line into the buffer.

* Consider file [sample-split.txt](files/sample-split.txt). It has line breaks after `\` on some lines. 

* Join the lines which are ending with `\` using the sed script [join.sed](files/join.sed).
  - The script will produce the similar result as in file [sample.txt](files/sample.txt)

```bash
sed -f join.sed sample-split.txt
```

## Sed debugging

* Option ` --degug ` to sed can be used to enable debugging.
* For every line that is being precessed, it prints the line which is read, pattern that matched and command that is executed.
