# Combining Commands and Files

* In GNU/Linux OS it is possible to combine multiple commands together a to achieve a task.

## Executing Multiple Commands

* ` command1; command2; command3; `
	- Command execution is independent of the exit status.
		
		```terminal
		~$ ls -d D* ; date; wc -l /etc/profile;
		Desktop  Documents  Downloads
		Saturday 24 December 2022 09:09:25 AM EET
		27 /etc/profile
		```
		
	- If any command fails it will not affect execution of other command.
		+ here ` ls /blah ` fails.
		
		```terminal
		~$ ls /blah ; date; wc -l /etc/profile;
		ls: cannot access '/blah': No such file or directory
		Saturday 24 December 2022 09:11:04 AM EET
		27 /etc/profile
		```
				
* ` command1 && command2 && command3 `
	- ` && ` is logical AND operator. 
	- A command is executed if exit status of previous command is ` true `.
	- ` command2 ` will be executed only if ` command1 ` succeeds.
	- If ` command2 ` succeeds ` command3 ` will be executed.
		
		```terminal
		~$ ls -d D* && date
		Desktop  Documents  Downloads
		Saturday 24 December 2022 09:40:19 AM EET
		```
		
	- If a command fails subsequent commands will not execute.
		
		```terminal
		/$ ls /blah && date
		ls: cannot access '/blah': No such file or directory
		```
		
* ` command1 || command2 || command3 `
	- ` || ` is logical OR operator.
	- A command is executed if exit status of previous command is ` false `
	- If ` command1 ` succeeds, the execution will stop.
		
		```terminal
		~$ ls -d D* || date
		Desktop  Documents  Downloads
		```
		
	- ` command2 ` will be executed if ` command1 ` fails. 
	- ` command3 ` will be executed if ` command2 ` fails.
		
		```terminal
		/$ ls /blah || date
		ls: cannot access '/blah': No such file or directory
		Saturday 24 December 2022 09:46:55 AM EET
		```
	
* ` ( command1; command2; command3 ) ` 
	- Execution of multiple commands by spawning subshells.
		
		```terminal
		~$ ( date; echo $BASH_SUBSHELL )
		Saturday 24 December 2022 09:26:57 AM EET
		1
		```
		
	- Nesting of the subshell commands
		+ Nesting increases the subshell level.
		+ It is useful when environment which runs a program needs to be closed.
		+ It is not a good practice to launch subshells indefinitely, as it is expensive to prepare environment, computationally.
		
		```terminal
		~$ echo $BASH_SUBSHELL; ( echo $BASH_SUBSHELL; (echo $BASH_SUBSHELL))
		0
		1
		2
		```
		
* ` && ` and ` || ` operators can be combined together to achieve fairly complex task.		


# File Descriptors
File Descriptors can be used to redirect output to either a file or a command.
The data is stream of characters.
There are 3 standard file descriptors.
* ` stdin `
	- Abbreviation for standard input.
	- Reference Number : ` 0 `
	- By default is pointer to input stream coming from keyboard.
* ` stdout `
	- Abbreviation for standard output.
	- Reference Number : ` 1 `
	- By default refers to screen where the output is displayed.
* ` stderr `
	- Abbreviation for standard error.
	- Reference Number : ` 2 `
	- By default refers to screen the error is displayed.

## Using ` cat ` to Listen ` stdin ` and ` stdout `
* When no option is given, 	
	- ` cat ` reads from ` stdin ` (` 0 `) and writes to the ` stdout ` (` 1 `).
		
		```terminal
		~$ cat
		Hello, how are you?
		Hello, how are you?
		I am fine.
		I am fine.
		Press <ctrl+D> to send EOF and gracefully exit.
		Press <ctrl+D> to send EOF and gracefully exit.
		~$
		```
	

# Redirection Operators
* ` command > file1 `
	- ` > ` is same as ` 1> `.
	- ` > ` is used to create an empty file or to empty the existing file.
		
		```terminal
		~$ > file.txt
		~$ ls -l file.txt
		-rw-rw-r-- 2 groot groot 0 Dec 24 10:14 file.txt
		```
		
	- ` > ` operator is used to redirect the ` stdout ` of ` command ` to ` file1 `.
	- New file1 will be created if it does not exist.
		
		```terminal
		~$ ls -1 /usr/bin > file1
		~$ ls -l file1
		-rw-rw-r-- 2 groot groot 13354 Dec 24 10:14 file1
		```
		
	- Contents of file1 will be overwritten.
	- Standard error is printed to the screen.
		
		```terminal
		~$ ls -1 /usr/blah > file1
		ls: cannot access '/usr/blah': No such file or directory
		~$ ls -l file1
		-rw-rw-r-- 2 groot groot 0 Dec 24 10:14 file1
		```
		
	- Since, you are creating a file write permission to the directory is required.
		
		```terminal
		/$ ls -1 /usr/bin > file1
		bash: file1: Permission denied
		```
		
	- Storing system hardware information in a file using [` hwinfo `]() command.
		
		```terminal
		~$  hwinfo > hwinfo.txt
		```
	
	- Using ` cat ` we can redirect the ` stdout ` (` 1 `) to write to (overwrite!) a file using ` > `.
		
		```terminal
		~$ cat > file1
		This is the first file I am creating on command line.
		We could use this feature to create text files on command line.
		You can exit using <ctrl+D>
		~$ ls -l file1
		-rw-rw-r-- 2 groot groot 146 Dec 24 11:50 file1
		```
		
	- Run ` cat file1 ` to see the contents of file1.
	
* ` command >> file1 `
	- ` >> ` is same as ` 1>> `.
	- ` >> ` is used to create file if does not exist. If the file exists, it does nothing.
		
		```terminal
		~$ >> file1.txt
		~$ ls -l file1.txt
		-rw-rw-r-- 2 groot groot 0 Dec 24 11:30 file1.txt
		~$ >> file.txt
		-rw-rw-r-- 2 groot groot 0 Dec 24 10:14 file.txt
		```
		
	- ` >> ` is used to *append* the ` stdout ` (` 1 `) of ` command ` to ` file1 `.
		
		```terminal
		~$ date >> file1
		~$ cat file1
		Saturday 24 December 2022 11:39:38 AM EET
		~$ date >> file1
		~$ cat file1
		Saturday 24 December 2022 11:39:38 AM EET
		Saturday 24 December 2022 11:40:45 AM EET
		```
		
	- You can put multiple redirections on same line separated with ` ; `
		
		```terminal
		~$ date >> file2; wc -l /etc/profile >> file2; file /usr/bin/znew >> file2;
		~$ cat file2
		Saturday 24 December 2022 11:44:57 AM EET
		27 /etc/profile
		/usr/bin/znew: POSIX shell script, ASCII text executable
		```

	- ` cat ` to *append* to the file using ` >> `.
		+ Exit using ` <ctrl+D> `
		
		```terminal
		~$ cat >> file3
		Attempt 1 : This is a way to create a new file and append text to it.
		~$ cat >> file3
		Attempt 2 : This is a way to append text to a file.
				We can inspect the content using cat or less command. 
		~$ cat file3
		Attempt 1 : This is a way to create a new file and append text to it.
		Attempt 2 : This is a way to append text to a file.
				We can inspect the content using cat or less command. 
		```
		
		
* ` command 2> file1 `
	- ` 2> ` is used to overwrite the  ` file1 ` with ` stderr ` of ` command `.
	- New ` file1 ` will be created if it does not exist.
	- ` 0 ` and ` 1 ` will have their default behaviour.
		+ ` /blah ` does not exist, hence ` ls ` will generate an error.
		```terminal
		~$ ls /blash 2> error.txt
		~$ cat error.txt
		ls: cannot access '/blash': No such file or directory
		```


* ` command > file1 2> file2 `
	- ` 1> ` or ` > ` redirects ` stdout ` of ` command ` to ` file1 `.
	- ` 2> ` redirects ` stderr ` of ` command ` to ` file2 `.
	- This prevents any output to be on the screen.
	- Contents of ` file1 ` and ` file2 ` will be overwritten.
		```terminal
		~$ ls $HOME /blah > output.txt 2> error.txt
		~$
		```
		+ see the output of both files using ` cat `.
	- Try the following command and see the output in two files created.
		```bash
		ls -R /etc > output.txt 2> error.txt
		```

* ` command < file1 `
	- ` < ` (` 0< `) refers to ` stdin `.
	- Any command expecting input from keyboard can also read input from a file.
	- Using ` < `, ` command ` reads input from ` file1 ` instead of reading the default input stream.
	- All other file descriptors have their default behaviour.
		+ ` wc ` reads from ` file1 `
		+ understand the difference between ` wc <file> ` and ` wc < <file>`
		```terminal
		~$ ls -l file1
		-rw-rw-r-- 2 sanr sanr 236 Dec 25 07:58 file1
		~$ wc file1
			6  43 236 file1
		~$ wc < file1
			6  43 236
		```
	- Using ` < `, read (` cat `) and count lines, words and characters (` wc `) of the files ` output.txt ` and ` error.txt `. 


* ` command > file1 2>&1 `
	- ` stdout ` of  ` command ` is redirected to the file ` file1 `.
	- Then we have another redirection.
		+ ` stderr `, specified by file descriptor ` 2 `, of ` command ` is redirected to ` stdout ` using file descriptor ` 1 `.
	- as a result ` file1 ` contains output and error from ` command `.
	- Contents if any in ` file1 ` will be overwritten. 
	- Note that there should be no space in string ` 2>&1 `
	- Run the commands below and see the output.
	
		```bash
		ls $HOME /blah > file1 2> file1
		cat file1
		```
		
		+ For commands above, the standard error is first written to file1 then it is overwritten by standard output.
	- Now run the commands given below and see the output.
		```terminal
		ls $HOME /blah > file1 2>&1
		cat file1
		```
		
		+ file1 constains both standard error and standard output.
		
<!-- write about custom file descriptors 	-->


## The pipe operator

* ` command1 | command2 `
	- ` | ` is called the pipe operator.
	- This operator redirects the standard output of ` command1 ` to the standard input of ` command2 `
	- If ` command2 ` is expecting any input from keyboard or a file, ` | ` makes possible to take it also from ` command1 `
	- Thus, ` command2 ` processes output from ` command1 ` by taking it as input.
	- By default, standard error from both commands go to the screen.
	- As an example let's count the number of files in ` /usr/bin ` directory.
		+ The following code creates ` file1 ` and then counts.
			```terminal
			~$ ls /usr/bin > file1; wc -l file1
			1353 file1
			```
		+ We can do it more efficiently by using ` | `
			```terminal
			~$ ls /usr/bin | wc -l file1
			1353
			```

	- There is no limit on using pipe operator in terms of numbers.
	- Try the command below and get familiarize with this operator
		+ Read the content of ` /usr/bin ` directly without writing to a file.
			```bash
			ls /usr/bin | less
			```
We can combine the pipe operator with file descriptor redirectors.

* ` command1 | command2 > file1 `
	- The standard output from ` command1 ` is mapped to standard input of ` command2 `. 
	- The standard output of ` command2 ` is written to the file ` file1 `.
	- However, standard error from both commands is written to the screen.
	- Example : Count the number of commands in ` /usr/bin ` directory and store it to a file in one go.
		```terminal
		~$ ls /usr/bin | wc -l > file1
		~$ cat < file1
		1353
		```

What if we don't want to store the error to a file, but instead discard it?
* ` /dev/null ` 
	- It is a character special file.
	- It acts as a sink when any output is redirected to it.
	- Use: silent and clean scripts.
	
* ` command1 > file1 2> /dev/null `
	- The standard output of ` command ` is written to the ` file1 `.
	- The standard error is redirected to ` /dev/null ` using file descriptor ` 2 `, and it just disappears.

	- Let's see an example
		+ The code below prints standard error to the screen.
		```terminal
		~$ ls $HOME /blah > file1
		ls: cannot access '/blah': No such file or directory
		```	
		
		+ We can redirect the standard error to ` /dev/null `.
		```terminal
		~$ ls $HOME /blah > file1 2> /dev/null
		~$ 
		```
		+ Inspect the contents of file1 using ` cat ` command.
		+ Try code below and see the contents of file1.
		
		```
		ls -R /etc > file1 2> /dev/null
		```
		
What if we want to redirect the standard output to file and also display to the screen?

## ` tee ` command
* ` tee [-a] file1 `
	- It reads the standard input and writes it to standard output as well as to the file ` file1 `.
	- ` -a ` option is used to append to ` file1 `.
	- It can be used to write to multiple files at a time.
	- Multiple file names can be separated by space.
	- It does not write the error.
* ` command1 | tee file1 ... `
	- It is used to redirect output from ` command1 ` to the screen as well as write it to a file ` file1 `.
	
		```terminal
		~$ ls $HOME | tee file1
		Desktop
		Documents
		Downloads
		file1
		Music
		Pictures
		Public
		snap
		Templates
		Videos
		```
		+ use ` cat ` to see the contents of ` file1 `.
	
	- Writing to multiple files.
		
		```terminal
		~$ ls $HOME /blah | tee file1 file2 | wc -l
		ls: cannot access '/blah': No such file or directory
		10
		```
		+ Try the above command without ` wc -l `
		+ The content of ` file1 ` and ` file2 ` is identical.
		+ This can be inspected using [` diff `](#diff-command) command.
		```
		diff file1 file2
		```
	
	- Handling errors - redirect the output to "sink" for a command that raises error.
		+ here ` ls ` generates error as ` /blah ` file/directory is not found.
		```terminal
		~$ ls $HOME /blah 2> /dev/null | tee file1 file2 | wc -l
		10
		```

## ` diff ` command
* ` diff files `
* It is used to compare files line by line.
	- Counting number of files in home directory and writing the directory contents to ` file1 ` and ` file2 `
		```terminal 
		~$ ls $HOME | tee file1 file2 | wc -l
		10
		~$ diff file1 file2
		~$
		```
		


# Software Management

Using package management systems.

## Need for a Package Manager
* Tools for installing, updating, removing, managing software.
* Install new / updated software across network.
* Package - File look up, both ways.
* Database of packages on the system including versions.
* Dependency checking.
* Signature verification tools.
* Tools for building packages.

## Package Types

* Package
	- RPM
		Used to manage packages of:                                             
		+ Red Hat Enterprise                                         
			Derivatives:                                                
			* CentOS                                                       
			* Fedora                                                                      
			* Oracle Linux                                                         
			
		+ SUSE Enterprise Linux                                                     
			Derivatives:                                                              
			* openSUSE                                                             
	- DEB                                                                                   
		Used to manage packages of:                                                          
		+ Debian                                                                    
			Derivatives:                                                       
			* Ubuntu                                                              
				Derivatives:                                                 
				-Mint                               
			* Knoppix                                                      
				Run from live CD.                                                  

Since, we are using Ubuntu OS, we should get familiar with *Debian* package type.
 

## ` lsb_release ` command
* To check the operating system, use the command below.
	- Codename is helpful while searching version specific packages.
		```terminal
		~$ lsb_release -a
		No LSB modules are available.
		Distributor ID:	Ubuntu
		Description:	Ubuntu 22.04.1 LTS
		Release:	22.04
		Codename:	jammy
		```


## Architectures
* Linux OS is available for a wide variety of architectures, and hence the packages.
* Package
	- amd64 | x86_64
	- i386 | x86
	- arm : mobile devices.
	- ppc64el | OpenPower : RISC-IV (Shakti Processor)
	- all | noarch | src : source codes/ pieces of softwares.
* It is important to install system compatible package to avoid any risk.
* But, thankfully package manager takes care of this.
* Use ` uname -a ` to check system architecture.

## Package Management Tools
* Package Type
	- RPM : Yellowdog Updater Modifier (` yum `) 
		+ Red Hat Package Manager (` rpm `) : ` yum ` internally calls this package to update software.
		+ Dandified YUM (` dnf `) : modified version of ` yum ` 
	- DEB 
		+ ` synaptic ` : GUI based
		+ ` aptitude ` : command line based
			* Advanced Package Tool (` apt `) is used by both ` synaptic ` and ` aptitude ` at the backend.
				- ` dpkg ` : used by ` apt ` at the backend.
					+ ` dpkg-deb ` : used by ` dpkg ` at the backend.


## Package Management in Ubuntu Using ` apt `

### Inquiring Package DB
* ` apt-cache` ` search ` `keyword `
	- Search packages for a ` keyword `
	- Run the commands below in terminal to see the output.
	
		```bash
		apt-cache search fortune
		```
		In the output we are interested in line that starts with ` fortunes `
		Click to see sample [output](/Week-2/examples/fortune_search.txt)
		More on the command [` fortune `](#fortune).
		
		```bash
		apt-cache search nmap		
		```
		In the output we are interested in *nmap - The Network Mapper* line.
		```bash
		apt-cache search wget		
		```
		In the output we are interested in *wget - retrieves files from the web* line.
		

* ` apt-cache` `pkgnames `
	- List of all packages installed
	- ` pkgnames ` is a keyword to list all packages in the system.
		```bash
		apt-cache pkgnames | less
		```
	- get sorted output using ` sort ` command.
		```bash
		apt-cache pkgnames | sort | less
		```
	- passing search key (*nm*) to ` pkgnames ` to search only for packages which start with *nm*.
		```bash
		apt-cache pkgnames nm
		```	
	- Try commands above in terminal.

* ` apt-cache` ` show [-a] ` `package `
	- Display package records of a ` package `
	- To see the details of package ` nmap `
		
		```bash
		apt-cache show nmap
		```
		
		+ [Sample Output](/Week-2/examples/nmap_show.txt)
		+ Filename: pool/universe/n/nmap/*nmap_7.91+dfsg1+really7.80+dfsg1-2build1_amd64.deb*
		+ The text in italic has foramt that signifies special information, that can be seen [here](#package-names)
		+ package name: *nmap*
		+ varsion: *7.91+dfsg1+really7.80+dfsg1*
		+ revision: *2build1*
		+ architecture: *amd64*
		+ The priority of ` nmap ` is extra. See [Priorities](#package-priorities "Package Priorities")
		+ *Section: universe/net* means that the package comes uder network utility. See [Sections](#package-sections)
	- Try commands below in terminal
	
		```bash
		apt-cache show wget
		```
		```bash
		apt-cache show fortunes
		```
		
## Package Names
The filenames follow specific format, it includes package name, version, release/revision and architecture separated by some special characters.
* Package
	- RPM
		+ Format : ` package-version-release.architecture.rpm `
	- DEB
		+ Format : ` package_version-revision_architecture.deb `

## Package Priorities
The ` apt-cache show pkgname ` command also lists the priority of the package. A user can decide
about installing a package based on priority.
* *required*
	- essential to proper functioning of the system.
* *important*
	- provides functionality that enables the system to run well.
* *standard*
	- inluded in the standard system installation.
* *optional*
	- can omit if you do not have enough storage.
* *extra*
	- could conflict with packages with higher priority, has specialized requirements, install only if needed.  
* e.g. for ` wget `, the priority is *standard*, for ` fortunes ` the priority is ` optional `


## Package Sections
* focal : https://packages.ubuntu.com/focal
* jammy : https://packages.ubuntu.com/jammy
* Package section signifies the use case of the package.
* There are a large number of package sections.
* e.g. 
	+ The package ` fortunes ` belongs to section ` universe/games `. 
	+ package ` wget ` belongs to section ` web `.

<!-- copy paste the package section content week 3 lecture 3-->

## Checksums
* It is important to know the packages we are installing are authentic.
* Checksums help in knowing if the package is authentic or not.
* This is important because while downloading the package (either from official/unofficial source) may get tampered.
* Even with small change in code, the checksum strings changes drastically.
* The following commands used to check for diffrent checksums.
### ` md5sum `
```bash
md5sum [filename]
```
* It gives *128 bit* checksum string irrespective of size of file.
		
	```terminal
	~$ md5sum nmap_show.txt
	2717ed2e8f6dd4a63a20c0317d695161  nmap_show.txt
	```
		
* When no input file is given, it reads the standard input (` - ` is the default filename for ` stdin `).
	+ Write text and hit <ctrl+D>.
		
	```terminal
	~$ md5sum
	Overflow on /dev/null, please empty the bit bucket.
	e67ef45f18ba7f8a6c339c08af30f72b  -
	~$ md5sum
	Overglow on /dev/null, please empty the bit bucket.
	ea16952d936440cdddcb1a68cdbb7295  -
	```
		
### ` sha1sum `
```bash
sha1sum [filename]
```
* It gives *160 bit* SHA1 checksum string.
* We will read the standard input, on same string given in [` md5sum `](#md5sum).
		
	```terminal
	~$ sha1sum
	Overflow on /dev/null, please empty the bit bucket.
	61d7ab21e9961d11640172e3183b86296c01874d  -
	~$ sha1sum
	Overglow on /dev/null, please empty the bit bucket.
	b52111e02999a26af3ebb2cdef7f0893dbbb92b1  -
	```
		
	- Try this command with filename on any file you wish.
### ` sha256sum `
```bash
sha256sum [filename]
```
* It gives *256 bit* SHA256 checksum string.
* We will read the standard input, on same string given in [` md5sum `](#md5sum).
		
	```terminal
	~$ sha256sum
	Overflow on /dev/null, please empty the bit bucket.
	27983a23a8319160adc6b4e2cadfac29f4e445e50998e0a5b4888c0becf51ffe  -
	~$ sha256sum
	Overglow on /dev/null, please empty the bit bucket.
	e2919763cc71cd324d29f96483c7f46eb3d6b64ab17ce3448aaca4da9b2e5b39  -
	```
		
	- Try this command with filename on any file you wish.


## Who can install packages?
Only administrators called sudoers can install / upgrade / remove packages.
* ` /etc/sudoers `
	- ` su ` : super user
	- ` do ` : can execute
	- This file constains list of sudoers.
	- Only sudoers can read this file.
		```terminal
		~$ whoami
		guest
		~$ sudo cat /etc/sudoers
		[sudo] password for guest:
		Sorry, try again.
		[sudo] password for guest:
		guest is not in the sudoers file. This incident will be reported.
		```
		+ When we run the command ` sudo cat /etc/sudoers ` it asks for password.
		+ For wrong password, the password is asked again.
		+ If the passwrod is correct, but the user is not listed in the file, it will notify in and report the login attempt to administrator.
		+ If ` $USER ` is listed in ` /etc/sudoers `, the file content will be dumped on screen for right password.
	- The failure if any is recorded in `/var/log/auth.log ` file.
	- To open this file you should have super user privilege.
	- The file lists timestamps and corresponding session details.

## Which website is used to download packages?
The details of websites are given in the following hierarchy.
* ` /etc/apt ` : directory
	- ` sources.list ` : regular file
		```terminal
		/etc/apt$ cat sources.list
		```
		+ The output is urls for repositories used for installing updates, upgrades and source code.
		+ It also has comments starting with ` # ` symbol.
		+ The comments describe the url, and it gives information about for what purpose the url is being used.
		
	- ` sources.list.d ` : directory / folder
		+ This folder contains files which has information about the repositories of third-party softwares.


## ` apt-get ` and ` apt `

### Updating and Installing Packages

* This command is used to fetch updates and upgrades from repositories listed in ` sources.list ` and files in ` sources.list.d `.
* This command should be run as ` sudo `.
* ` update ` : This option is used to fetch the update details.
	- The details fetched is saved as cache.
		```bash
		sudo apt-get update
		```
		or
		```bash
		sudo apt update
		```
* ` upgrade ` : This option is used to upgrade all installed packages.
	- This option reads the details fetched by ` apt-get update ` command.
		
		```bash
		sudo apt-get upgrade
		```
		or
		```bash
		sudo apt upgrade
		```
		
	- This lists how many updates are available and how much data needs to be downloaded.
	- This command asks for comformation to proceed the download.

* ` install ` : It is used to install a package.
	- To install *fortunes* package. See [fortune](#fortune) command.
		
		```bash
		sudo apt-get install fortunes 
		```
		
* ` reinstall ` : It is used to download and install the new version of the package.
	- To reinstall *fortunes* package.
		
		```bash
		sudo apt-get reinstall fortunes 
		```
		
### Removing or Cleaning up Packages
* ` autoremove ` : Remove packages that were automatically installed to satisfy a dependency and not needed.

	```bash
	sudo apt autoremove
	``` 
	- By removing these packages you can free some space.

* ` clean ` : Clean local repository of retrieved packages.
	- ` apt-get clean `
	- Usually, we may want to revert back to a particular version of a package, it may be ok to keep the retrieved package files.

* ` remove ` : This option is used to remove a particular package
		
	```bash
	sudo apt-get remove fortunes fortune-mod
	```
		
	- This command removes the fortunes package.
	- fortune-mod contains fortune cookies, hence it also needs to be removed.

* ` purge ` : Purge package files from the system.
	- ` apt-get purge pkgname `
	- Removes package files and configuration files.
	- Be careful while using this command.
	

# dpkg suite

## ` dpkg `
Package management in Ubuntu using ` dpkg `.
The following filesystem contains text information about packages.
* ` /var/lib/dpkg `
	- ` arch ` : regular file
		+ Architectures for which packages are installed on the system.
	- ` available` : regular file
		+ Contains all packages and it's full information.
		+ ` apt-cache show nmap ` 
	- ` status ` : regular file
		+ Information about the packages whether they are installed or not.
	- ` info ` : directory / folder 
		+ Set of files for each package.
		+ files for *wget* package.
		```terminal
		~$ ls wget*
		wget.conffiles  wget.list  wget.md5sums
		```
		+ ` wget.conffiles ` : contains path of configuration file, which is ` /etc/wgetrc `
		+ ` wget.list ` : contains list of files that are copied on the system during installation.
		+ ` wget.md5sums ` : contains md5sum of files used by *wget* package.
		+ This md5sum data can be used to check if the file is authentic or not.

* ` dpkg -l  pattern `
	- List all packages whose names match the pattern.
		
		```bash
		dpkg -l nmap
		```
		
	- prints name, version, architecture and description of package *nmap*.
* ` dpkg -L package `
	- List installed files that came from *package*.
		
		```bash
		dpkg -L nmap
		```
		
	- Prints list of directories and files added to the system and used by *nmap* package.
* ` dpkg -s package `
	- Report the status of *package*.
		
		```bash
		dpkg -s nmap
		```
		
	- Prints the status of whether the package *nmap* is installed or not and other info.
	- The output is similar to ` apt-cache show ` command.
	 
* ` dpkg -S pattern `
	- Search installed packages for a file using *pattern*.
	- Prints the name of the package that made the particular executable available.
		
		```bash
		dpkg -S /usr/bin/perl
		```
		
	- Prints the name of the package that the executable ` /usr/bin/perl ` belongs to.
	- Perl is scripting language used specifically for text processing using regex.

* ` dpkg -i package_version-revision_architecture.deb `
	- You can directly install deb package using the command.

* By default use package management pointing to a reliable repository.
* This will take care of compatibility and dependency if any for the package. 
* Uninstalling packages using ` dpkg ` is not recommended!
* Other package management tools: apt, dpkg, snap, docker

## ` dpkg-query `
It is a tool to query the *dpkg* database.
Prints package names and section name to which the package belongs.
* ` dpkg-query -W [package-name-pattern...]`
	- Show the list of packages

* ` dpkg-query -f=format `
	- Specify ` format ` for the output.
	- Some formats : ` Section `, ` binary:Package `
	- Introduce escapes using ` \ `.
		
		' \n ' : newline,
		' \r ' : carriage return,
		' \t ' : tab.

* Examples
	- Prints name of the section and package of each package.
		
		```bash
		dpkg-query -W -f="${Section} ${binary:Package}\n" | less
		```
		
	- Prints name of the section and package of each package in alphabetically sorted order.
	
		```bash
		dpkg-query -W -f="${Section} ${binary:Package}\n" | sort | less
		```
	- Using ` grep ` utility we are printing only those lines which contain the word *shells*.
	- More on [` grep `](/Week-4/Lecture1-2.md) in next week (Week-4). 

		```bash
		dpkg-query -W -f="${Section} ${binary:Package}\n" | grep shells
		```

# Games

## ` fortune `
* ` fortune `: print funny quotes, thoughts (Some quotes may be harsh, but it's just for fun.).
* package ` fortunes ` has data files containing fortune cookies.
```terminal
~$ fortune
Any circuit design must contain at least one part which is obsolete, two parts
which are unobtainable, and three parts which are still under development.
```


