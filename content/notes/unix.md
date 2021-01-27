---
title: "Unix"
date: 2019-11-14T11:33:34+08:00
---

SSH: use UNIX remotely 
SFTP: Secure file transfer protocol
UNIX commands: Command + Flag + Argument

## UNIX file store, File Processing and I/O
**Suffix** suggests the type of content
Unix thinks everything is  a file.
File starts with a dot (.) is a hidden file
## Directories and pathnames
```bash
man #manual pages
whatis #show brief manual pages

ls / ls [pathname] #lists the contents of the named directories
ls -l #see permission lists
ls -g #see the long listing of the files (more about permission)
ls -a #show the hidden files (files start with a dot, do not show up in a straight ls command)
ls -F #show files with serial number

cd [pathname] changes current directory

.. #one level above
../.. #two level above
. #current directory
~ #user’s home directory
~user #a specific user’s home directory

* #wildcard matching any string
? #wildcard matching any single character

pwd #prints the pathname of the current directory

cp [pathname1 pathname2] #copies the contents of 1 to 2

mv [pathname1 pathname2] #changes the name of pathname1 to be pathname2 (move files or directory) or mv [filename1 filename2] to rename or [filename pathname]to move

rm [pathname] remove files but not directories

mkdir [pathname] #creates a new directory, fails if they already exits
rmdir [pathname] #removes directories, fails if not empty
```


## Displaying the contents of a file on the screen
```bash
cat [filename] #displays the contents of the named files
more / less [filename] #let you scroll through a file. This command writes the contents of a file onto the screen a page at a time, user [space-bar] to see the next page, use [q] to quit reading

head [filename] #displays the first 10 lines of a file
head -5 [filename] #displays first 5 lines, works on tail too
tail [filename] #displays the last 10 lines of a file

less test.txt #in less, type a forward slash [/] followed by the word to search, type n to search for the next occurrence

file #gives the content types of the specified files 
e.g. file /bin/*
wc #counts, reads from the keyboard
wc -l #counts for lines
wc -c #counts for characters
wc -w #counts for words

grep word [filename] #searches the specified files 
		e.g. grep “Eric” myfile
grep ‘spinning car’ test.txt #to search a phrase or pattern, enclose it in ‘’
grep -i eric myfile #ignore  upper/lower case distinctions
grep -v #display those lines that do NOT match
grep -n #precede each maching line with the line number
grep -c #print only the total count of matched lines

file #gives the content types of the specified files 
e.g. file /bin/*
wc #counts, reads from the keyboard
wc -l #counts for lines
wc -c #counts for characters
wc -w #counts for words
```

## Piping
```bash
who #to see who is on the system with you
who | sort #connect the output of the who command directly to the input of sort command

lpr -Pprinter [filename] #prints files
a2ps -Pprinter text #print text(my import using pipe)
```

## File system security
```bash
chmod #u, g, o; operation +, -; permission is r, w, x.
# u: user, g: group, o: other, a: all
		chmod o-r [filename]
		chmod u+rwx [filename]
		chmod 644 [filename] 110 100 100 (400: 100 000 000; 777: 111 111 111)
```

## Redirection
```bash
#STDIN: standard input, redirecting using [<]
#STDOUT: standard output, redirecting using [>] or [>>]
#STDRR: standard error, [2>]
#redirect STDOUT to a file, it will auto create or overwritten the file
> #redirect
>> #appends standard output to a file
2> #redirect error
2>&1 #redirect both outputs
cat list1 list2 > biglist #join list1 and list2 into a new file
sort < biglist #the sorted list will be output to the screen without changing the file
sort < biglist > sorted_list #output the sorted list to a file
```

---
## Searching and regular expressions
Shell metacharacter: characters that have some meaning to UNIX
Regex Metacharacters:
Used in quotes
Double quotes “ ”:
everything between “ and “ is taken literally, except for:
```bash
$ # variable substitution will occur
` # command substitution will occur
“ # marks the end of  the double quote 
```


echo “You have `ls | wc –l` files in `pwd`”
returns `You have files in /Users/lihang/SAN`

Single quotes ‘ ’:
everything between ‘ and ‘ is taken literally except for another ‘. You cannot embed another ‘ within such a quoted string

echo ‘You have `ls | wc –l` files in `pwd`’
returns You have `ls | wc –l` files in `pwd`


Escaping a Character: \\
`echo \$HOME`

Regex Metacharacter
```bash
grep “^Eric” myfile #to insist that Eric matches only at the start of the line
grep “Eric$” myfile #matches only at the end
grep “c.t” myfile #matches cat, etc.
grep “[Ee]ric” myfile #allow alternatives
grep “^a*$” myfile
grep “^aa*$” myfile
grep “Eric.*Foxley”
- #Indicates a range, a-z matches all characters form a to z
```

## Processes and Signals
### Processes
Processes: every time you run a program it creates a process

When a process runs:
- It works through its set of instructions
- To execute an instruction, the instruction has to be brought into a register
- Any data values needed also need to be brought into registers
- When the instruction is executed it is stored back into main memory along with the data

## Memory allocation (very simplified)
Kernel space: only kernel can use this memory
Stack: allocated to processes for storing simple (static) variables
Heap: allocated to processes for storing complex (dynamic) variables
Process P2: process instructions 

## Some definitions
Address space: the amount of memory allocated to something e.g. a program
Program counter: the address (location) in memory of the program instruction currently being executed
Registers: Fast storage available to CPU, instructions are loaded into registers before execution, separate form main memory
Stack, Heap: An area of memory that is allocated to programs on demand, stack for variables of a fixed size like integers, heap for variables of unknown size

A process need to keep track of :
Address space, Program counter, Open files, Registers, Stack, State(whether it is running or waiting), Global variables (ones that are visible to the whole running program).

### Parent and child processes - Forking
To create a new process (i.e. to run another program) a process has to fork (copy itself) to create another process
The new (child) process then runs the program (exec)
This involves copying a lot of things: memory allocations, register state, open files

A parent process can for to create a child process
Children run separately and simultaneously with each other and with their parents

### Process States
Process can have one of a number of states:
0 - running on a processor
S - sleeping (waiting for an event to complete)
R - runnable (process is on run queue)
Z - zombile

Processes are created and managed by the kernel
BUT this is expensive (fork) and switching between them is expensive
- Copying data
Threads share some things with their process so less expensive and faster, are like a “lightweight” process.

### Threads
A component of a process
Processes have different: address space, open files, global variables, etc.
Threads share these but have different: program counter, registers, stack, state

### Daemons
Daemon are processes which lie dormant until there are needed for a particular service
Commonly, their names end with a ‘d’
e.g. printer deamons, mail deamons.

### Listing Processes
Windows: Task manager
Unix: ps
Unix job control: top
A program shows you information about the top CPU processes
Updates this information at regular intervals
Type `q` to quit, `k` to kill a process, `u` (return) followed by a username (return) to see the 			processes belonging to that user
`nice` will run a program at a lower priority so that it doesn’t clog up the CPU
Priorities range form 20 to -20, -20 is the highest priority, but you are not allowed to set priority 		below 0 unless you are root
`severn$ nice -10 perl myProg.pl`

### Killing processes
Unix:  Use `top`  or `kill` 
`kill <signal number> <PID>`   E.g. `kill -15 25718`
“-15” is the signal number - means “stop the process cleanly” (i.e. exit any files it is using) 
“-9” means “kill the process whatever”  (Useful if all else fails!)

### Scheduling Jobs
Windows Task Scheduler 
VisualCron (Windows) 
cron:
Unix built in scheduler, run a process at a periodic interval
at (Unix): 
Schedule commands to be executed once at a particular time
batch (Unix) :
Like at, can be used to run scheduled jobs only when system load is below a certain threshold

### UNIX Signals
Signals are a UNIX mechanism for controlling processes 
A signal is a message to a process that requires immediate attention
Process uses a signal handler to deal with it
Each signal has a default action associated with it
Signals are generated by exceptions, e.g.
Attempts to use illegal instructions
The user pressing an interrupt key
A child process calling exit or terminating abnormally

Some control sequences affect processes: 
Control C - kill a process (2 SIGINT) 
Control S - suspend or pause the display of output (19 SIGSTOP)
Control Q - resume or continue output from Control S (18 SIGCONT)
Control Z – suspend a process (20 SIGTSTP)

If some signals are not caught, or if they are ignored, they generate a core image dump
i.e. Segmentation violation core dumped. e.g. caused when a process tries to read outside its memory map
Many signals can be caught from within a program. A programmer can then:
Ignore signal
Perform the default action
Execute a program specified function
Debuggers are special tools which allow signals to be caught so that the programmer can see why a program might be going wrong


