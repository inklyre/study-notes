# AWK

AWK is a language for processing fields and records.

## Introduction

* It is a programming language
* *AWK* is abbreviation of the three people who developed it:
  - *A*ho
  - *W*einberger
  - *K*ernighan

* It is a part of POSIX, IEEE 1003.1-2008
* Variants : nawk, gawk, mawk ...
* gawk contains features that extend POSIX

## Execution Model

* Input is a set of records
* Eg., using "\n" as separator, lines are records
* Each record is a sequence of fields
* Eg. using " " as fields separator, words are fields
* Splitting of records to fields is done automatically
* Each code block executes on one record at a time, as matched by the pattern of the block

## Usage

* Single line at the command line

```terminal
~$ cat /etc/passwd | awk -F":" '{print $1}'
```
 
* Script interpreted by awk

```terminal
~$ ./myscript.awk input_file
```

* Script reading from a file using ` -f `

```terminal
~$ cat /etc/passwd | awk -f myscript.awk
```

## Built-in Variables

| Variables   | Description    |
| :---------------: | :--------------- |
| ` ARGC `   | Number of arguments supplied on the command line (except those that came with -f & -v options) |
| ` ARGV `  | Array of command line arguments supplied; indexed from 0 to ARGC-1 |
| ` ENVIRON ` | Associative array of environment variables   |
| ` FILENAME `   | Current filename being processed   |
| ` FNR `   | Number of the current record, relative to the current file |
| ` FS `   |  Field separator, can use regex |
| ` NF `    | Number of fields in the current record  |
| ` NR `  | Number of the current record   |
| ` OFMT `| Output format for numbers |
| ` OFS ` | Output fields separator |
| ` ORS ` | Output record separator |
| ` RLENGTH ` | Record separator |
| ` RSTART ` | Length of string matched by match() function  |
| ` SUBSEP ` | First position in the string matched by match() function |
|  ` $0 ` | Entire input record |
| ` $n `   | *n*th field in the current record |

## AWK scripts

```syntax
[block] { procedure }
```

An awk script has to follow the syntax as shown above.

* There three types of *blocks* in awk.
  - BEGIN: ` BEGIN {procedure }`
  - General : ` [pattern] { procedure } `
    + expression 
    + regex
    + Relational expression
    + Pattern-matching expression
  - END: ` END { procedure } `

* A procedure is any of or all of the following:
  - Variable assignment
  - Array assignment
  - Input / Output commands
  - Built-in functions
  - Control loops.

* User defined functions needs to be defined before any block starts.
* Like python, one line comments in awk start with ` # `.

## Execution

* ` BEGIN { commands; } `
  - Executed once, before files are read
  - Can appear anywhere in the script
  - Can appear multiple times
  - Can contain program code


* ` END { commands } `
  - Executed once, after files are read
  - Can appear anywhere in the script 
  - Can appear multiple times
  - Can contain program code

* ` pattern { commands } `
  - Patterns can be combined with ` && `, ` || ` and ` ! `
  - Range of records can be specified using comma
  - Executed each record pattern evaluates to true
  - Script can have multiple blocks

* ` { commands } `
  - Executed for all records.
  - Can have multiple such blocks


## Operators

* Assignment
  - ` = ` ` += ` ` -= ` ` *= ` ` /= ` ` %= ` ` ^= ` ` **= `
* Logical
  - ` || ` ` && ` ` ! `
* Algebraic
  - ` + ` ` - ` ` * ` ` / ` ` % ` ` ^ ` ` ** `
* Relational
  - ` > ` ` <= ` ` > ` ` >= ` ` != ` ` == `

| Operators       | Description    |
| :---------:     | :--------------- |
| ` expr ? a : b `  | Conditional expression   |
| ` i in array `    |  Array index/key membership  |
| ` a ~ /regex/ `   | Regular expression match  |
| ` a !~ /regex/ `  |  Negation of regular expression match  |                          
| ` ++ `            |   Increment, both prefix and postfix     |
| ` -- `            |  Decrement, both prefix and postfix  |
| ` $ `             |  Field reference  |
| `   `             |  Blank is for concatenation |


## Functions and commands

| Type   | functions/commands    |
|--------------- | :--------------- |
| Arithmetic | ` atan2 ` ` cos ` ` exp ` ` int ` ` log ` ` rand ` ` sin ` ` sqrt ` ` srand ` |
| String   | ` asort ` ` asorti ` ` gsub ` ` index ` ` length ` ` match ` ` split ` ` sprintf ` ` strtonum ` ` sub ` ` substr ` ` tolower ` ` toupper `  |
| Control flow   | ` break ` ` continue ` ` do ` ` while ` ` exit ` ` for ` ` if ` ` else ` ` return `  |
| Input/Output   | ` close ` ` fflush ` ` getline ` ` next ` ` nextline ` ` print ` ` printf `  |
| Programming    | ` extension ` ` delete ` ` function ` ` system ` |
| bit-wise       | ` and ` ` compl ` ` lshift ` ` or ` ` rshift ` ` xor `|

## Arrays

* Associative arrays
* Sparse storage
* Index need not be integer
* ` arr[index]=value `
* ` for (index in arr ) `
* ` delete arr[index] `

## Conditionals and Loops

if:

```syntax
if (a > b)
{
  print a
}
```

for:

```awk
for (index in array)
{
  print array[index]
}
```

```awk
for (i=1;i<n;i++)
{
  print i
}
```

while:

```awk
while (a < n)
{
  print a
}
```

do ... while:

```awk
do
{
  print a
} while (a < n)
```

## Functions

* Invocation:

```terminal
~$ cat infile | awk -f mylib -f myscript.awk
```

Files : [mylib](awk-script-examples/mylib) [myscript.awk](awk-script-examples/myscript.awk)

## Pretty printing

```awk
printf "format", a, b, c 
```

* ` format ` has the following format
  - ` %[modifier]control-letter `

* Where ` modifier ` can be width, precision and -
* control-letter

| ctrl-letter | description |
| :----: | :---------  |
| ` c `  | ascii char |
| ` d ` | integer |
| ` i ` | integer |
| ` e ` | scientific notation |
| ` f ` | floating notation |
| ` g ` | shorter of scientific and float |
| ` o ` | octal value |
| ` s ` | string text |
| ` x ` | hexadecimal value |
| ` X ` | hexadecimal value in caps |

# bash + awk
* Including awk inside shell script
* heredoc feature
* Use with other shell scripts on command line using pipe

*awk is available everywhere !*

*awk is a programming language, quick to code and fast in execution*

*combine it on the command line with other scripts*
# AWK Demonstration

* ` awk ` points to ` gawk `
  - ` awk ` -> ` /usr/bin/awk ` -> ` /etc/alternatives/awk ` -> ` /usr/bin/gawk `

* The files used are there in folder [awk-script-examples](/Week-7/awk-script-examples/)
* All .awk files have the comments which can be used to understand the code in them.

### The basic block structure

* Files: 
  - script - [block-ex-1.awk](awk-script-examples/block-ex-1.awk)
  - input - [block-ex-1.input](awk-script-examples/block-ex-1.input)
* Run the script on input file and see the output.

```terminal
~$ ./block-ex-1.awk block-ex-1.input
```

or 

```terminal
~$ awk -f block-ex-1.awk block-ex-1.input
```

or

```terminal
~$ cat block-ex-1.input | awk -f block-ex-1.awk
```

###  Uses of ` NF ` ` FNR ` and ` $0 `

* Files: 
  - script - [block-ex-2.awk](awk-script-examples/block-ex-2.awk)
  - input - [block-ex-2.input](awk-script-examples/block-ex-2.input)

### Filtering using regex over general block

* Use of ` /regex/ { procedure } `
* Files: 
  - script - [block-ex-3.awk](awk-script-examples/block-ex-3.awk)
  - input - [block-ex-3.input](awk-script-examples/block-ex-3.input)

### Conditional filtering using field matching regex over general block

* Use of ` $n ~ /regex/ { procedure } ` where ` $n ` is nth field
* Files: 
  - script - [block-ex-4.awk](awk-script-examples/block-ex-4.awk)
  - input - [block-ex-4.input](awk-script-examples/block-ex-4.input)


### Conditional filtering using ` NF `, uses of ` FS `

* Use of ` NF < NUM { procedure } `
* Use of ` FS `, specifying multiple field separators
* Files:
  - script - [block-ex-5.awk](awk-script-examples/block-ex-5.awk)
  - input - [block-ex-5.input](awk-script-examples/block-ex-5.input)

### array, ` if ` Conditional filtering and using ` for ` loop for aggregation

* Aggregation if any is done in ` END ` block
* Getting Report
* Files: 
  - script - [block-ex-6.awk](awk-script-examples/block-ex-6.awk)
  - input - [block-ex-6.input](awk-script-examples/block-ex-6.input)

### Functions

* Definition of mathematical and user-defined functions, function call, pretty printing
* Files: 
  - library - [func-lib.awk](awk-script-examples/func-lib.awk)
  - script - [func-example.awk](awk-script-examples/func-example.awk)

```terminal
~$ echo "12.5" | awk -f func-lib.awk -f func-example.awk
```

  - Replace 12.5 with ""
  - Replace echo "12.5" with ` cat xaa `, where xaa is empty file
  - Notice the difference in output
  - What do you observe?

### The power of awk in terms of speed

* Files: 
  - script-1 - [rsheet-create.awk](awk-script-examples/rsheet-create.awk)
  - script-2 - [rsheet-process.awk](awk-script-examples/rsheet-process.awk)

* The script-1 creates random data sheet.
* Can be invoked by passing empty string as input. (use ` time ` command to time the script)
* Store the output in file *rsheet-data.txt*
* Run the script-2 on *rsheet-data.txt*, note the time.

```terminal
~$ time ./rsheet-process.awk rsheet-data.txt > rsheet-pdata.txt
```

* You can see that awk processes data very quickly. Try to load the data in LibreOffice and see the result.

## Processing log

* File:
  - script-1 - [apache-log-example-1.awk](awk-script-examples/apache-log-example-1.awk)
  - script-1a - [apache-log-example-1a.awk](awk-script-examples/apache-log-example-1a.awk)
  - script-2 - [apache-log-example-2.awk](awk-script-examples/apache-log-example-2.awk)
  - script-3 - [apache-log-example-3.awk](awk-script-examples/apache-log-example-3.awk)
  - input - [access-full.log](awk-script-examples/access-full.log) contains log from web server, the requests.
  - Create files *access-head.log* and *access-tail.log* using ` head ` and ` tail ` commands respectively. 

* awk on command line

```terminal
~$ awk 'BEGIN {FS=" "} {print $1}' access-head.log # prints the ip address
```

```terminal
~$ awk 'BEGIN {FS=" "} {d=substr($4,2,11); print d}' access-head.log #print date field which is 4th field
```

```terminal
~$ awk 'BEGIN {FS=" "} {d=substr($4,2,11); print d, $1}' access-head.log #print date and ip address
```

  - Run the same commands on *access-tail.log* and see the output
  - Run the same commands on input file and see the time.


* See [date](#date) command to follow further

* Using ` getline `, ` sprintf ` and getting log summary for 5 days
  - We will use script-1 which has code written to get the date and ip address along with the log summary ip and it's count.
  - Run the script on input file and see the time
  - To only get only the ip stats run the script-1a on input file
  - The sorted output can be obtained using ` sort -n ` on output of previous action.

* Using [` dig `](/Week-8/Lecture2.md/#dig) to find the DNS of the ip address along with the count stats

  - Time run script-2 on input file.
  - See the output. 

## ` date `

* Getting date of 5 days ago from today.

```terminal
~$ date --date="5 days ago" +%d/%m/%Y
```