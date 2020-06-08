# Red Hat Certified System Administrator, 3rd edition notes

## Module 1: Performing Basic System Management Tasks

### Lesson 1: Installing Red Hat Enterprise Linux Server

#### 1.1. Understanding Server Requirements

RHEL can be installed or deployed in different ways
    * On physical hardware
    * In virtual machines
    * As a container
    * As an instance in cloud

We will install it in a virtual machine

We can use RHEL 8 or CentOS 8

Requirements depend on the type of installation
    * Physical on the type of installation
    * With or without GUI?
    * What are goint to do with the server

For installation of a virtual machine in this class
    * 2 GiB
    * 20 GiB of disk space
    * Wired Network Connection
    * Optical drive or access to DVD ISO

#### 1.2. Performing a Basic Installation

* choose workstation
* enable networking (automatic setup)
* automatic partitioning



#### 1.3. Installing with Custom partitioning

* 18 G for primary partition
* 2 G for swap partition
* use xfs file system
* both disk need to be standard partition (cannot be boot from LVM)


#### 1.4. Logging into the Server

* For security, don't log in as root by default
* use **su -** to open a root shell when required
* or use **sudo** to run commands as a root

* Everything is in Activity tab
* GNOME3 is used

* when using Terminal we can change settings 
  - changing color scheme (Edit -> Preferences -> Colors)

* Disable locking screen
  - Settings -> Power -> Power Saving -> Blank screen -> Never

Logging into a root user
* `su -` (password for root)
  * `-` is used to force loading all enviromental variables
* it changes $ (ordinary user) to # (root user) in the command line
* `exit` - leaving root user to the normal user



#### 1.5. Deploying RHEL in Cloud
- We need to use EC2 (AWS)
- when creating an EC2 we need to an image (AMI - Amazon Machine Image) we need to choose RHEL
- It can be run withing free tier (t2.micro)
- We can 

#### Lesson 1 Lab: Installing RHEL
- 

#### Lesson 1 Lab Solution: Installing RHEL

### Lesson 2: Using Essential Tools
- `pwd` - prints working directory
- `free -m` - free memory using megabytes
- `whoami ` - prints active user
- `df -h` - disk usage in human readable format 
- `cat` - prints a content of a file (configuration)
- `findmnt` - find mounted filesystems
- `ip addr show` - shows your current IP configuration
- `ps aux` - listing all processes
- `wc` - word count - lines/words/characters
- `less` - scrolling (vim command, Q for exit)
- `who` - who is currently logged in

#### Working in the Bash shell 

Bash is the default shell and provides several useful features
* Tab command completion (or double)
* history 
  - `!11` - run command from history (last command `11`)
  - `!f ` - runs last command that starts with `f`
  - `Ctrl+r` - reverse-i-search command
    - after find a command we need to present `Enter` to confirm command and `Enter` to confirm the command 
* piping
  - `|` 
  - output from one command is input into another
  - `ps aux | wc` - lists process (can't see it) and then puts that into `wc` command to count processes
* redirection
  - `>` 
  - overwrites the existing file
  - `whoami > lsfiles` - writes output into another file (lsfiles in this case)
* append 
  - `>>` 
  - appends value to the end of the file
* error handling
  - `2>`
  - write a command and error into a file
* writing everythin else 
  - `*`
  - writes allways what exists in current directoty when used with `ls`  
* to see only valid results
  - we need to redirect output to a `null` device 
  - `ls lwiegh * 2> /dev/null`
* environment variables
  - `env` - list environmental variables
  - we can change it on the fly
    - `LANG=fr_FR.UTF-8` -> manual is in the french language
  - setup automatically during the installation, but we can set it later
* aliases
  - `alias` - all current aliases
  - `alias alias_name=alias definition` - how to define an alias
    - `alias h=history` - `history` can be called by using alias  `h` 
  - aliases are usually set in a configuration file
* scripts
  - bash shell

#### Using Redirection and Piping
    
Redirection    
    * STDIN (Standard Input)
        - `<`
    * STDOUT (Standard Output) 
        - `>` - overwrite the file
        - `>>`  - append to a file
    * STDERR (Standar Error) 
        - `2>` redirect standard error to a file
        - `2> /dev/null`

    In piping the STDOUT of the first command is used as STDIN of the second command
    -> It is easy to analyze the results when we read the file with the results

    - `grep -R student /etc` may contain many errors (noise on the screen)
      -> we redirect errors to /dev/null
      => `grep -R student /etc 2>/dev/null` 
        - we get only the useful output
        - all permission denied access are dropped out

Piping
  - `ls -l /etc` - lists all files in /etc/
  - `ls -l /etc | wc` - counts number of files in `/etc`
  - `ls -l /etc | less` - view it line by line 
  - `ls -l /etc | grep host` - lists all files in /etc/ that contain `host` in their name

#### 2.5. Understanding the Linux Filesystem Hierarchy  Standard

Directory usage on Linux is highly standardized
Standard directories are defined in the FHS, which is maintained by Linux Foundation
Starting point is the root directLory
Different devices may be integrated in the FHS by using mounts
Mount is crucial in FHS (everything can be mounted and it is really flexible)

`ls -l` (long listing) 
 Different colours mean that some files are regular files, directories or simbolic links

 **/boot**
 - everything what is needed to boot a linux system
 - `vmlinux-4.18.0-147.el8.x86_64` is a linux kernel (just 7.8 MB)
   - used for interaction with HW on the computer

**/dev**
- directory for devices
- talking to device files is directly talking to the kernel
- root permission is needed
- we don't get size of the files but address of devices with the kernel (major and minor)

**/etc**
- directory for storing configuration files
- `redhat-release` and `os-release` store information about linux releases

**/home** 
- stores personal information about users
- `useradd` 
    - creates a directory for a user automatically
    - adding a user

**/usr**
- stores all programs (like program files)
- `sbin` - system binary (root privileges)
- `bin` - ordirnary binary

**/var** 
- writes dynamic data
- `log` files directory
  -> `messages` = the most important log file
  -> `README` - informs how to use logs (systemd guide or traditional) 

`man hier`
-> description of the filesystem hierarchy in detail level

#### 2.6. Using `man`

`man` is the best source to get extensive usage information
* sections define command types
  - section numbers are always listed

Types of sections:
    1. Executable programs or shell commands
    2. System calls (functions provided by the kernel)
    3. Library calls (functions within program libraries)
    4. Special files (usuall foind in /dev)
    5. File formats and conventions e.g. /etc/passwd
    6. Games
    7. Miscellaneoous
    8. System administration commands (usually only for root)
    9. Kernel routins [Non standar]

More complicated commands have examples in the manual
  - `mvn lvcreate` 

Search for text like you do in Vim
  - `/` - forward search
  - `?` - backward search

  Synopsis shows us how to use directory

Options:
    - can be long or short notations
      - long for learning
      - short for the normal usage
 
complicated `man page`
  * `man lvcreate`

#### 2.7 Finding the right man page

* `man -k ... `
* `apropos ...`
* `mandb` - adds additional definitions 
* it is useful to combine output of find with some another tools
  - `man -k user | grep user`


#### 2.8. Understanding Vim

* `vim` the default editor, and is used as embedded editor by many commands 
* enhance version of `vi`
* to work with `vim`, you need to manage _command_ mode and _input_ mode

#### 2.9. Using vim
* not needed since I know how to use Vim

#### 2.10. Using globbing and wildcards

* Globbing is a shell feature that helps matching file names
* Not to be confused with regular expressions, which helps finding text patterns
  -> they are very alike regular expressions
* for documentation: see `man 7 glob`

Examples: 
  - `pwd` -> /etc
  - `ls host*`
  - `ls ?ost`  
  - `ls [hm]ost`
  - `ls [!hm]ost`
  - `ls script [0-9][0-9]`

#### 2.11. Using cockpit (not in RHCSA)

* `systemctl enable --now cockpit.socket`
* `systemctl status cockpit.socket` 
 -> listening at `[::]:9090` -> `localhost:9090`
 => requires a login (use `root`)

* very useful tool, but not a command line :-)

#### 2.12. Lab assignment


### Lesson 3: Essential File Management Tools

#### 3.1 Essential file management tasks

Commands:
    * `ls`
    * `mkdir`
      -> `mkdir -p parent/child` - longer path
    * `cp`
      - globbing can be use
      - `cp -r ...` - copying directories including content of the directories
    * `mv`
    * `rmdir`
      - removes a directory
      - only an empty directory
    * `rm`
      - generic remove (can remove anything)
      - `rm -rf` - remove recursively directory
      - `rm -rf /` 
        -> very dangerous since it removes from the root directory (it gives a warning)
        => `rm -rf / --no-preserve-root` 

#### 3.2 Finding Files

Commands: 
    * `ls` - is useds to list files, not to find files
    * `which` - looks for binaries in `$PATH`
    * `locate` uses a database, built by `updatedb` to find files in a database
      - `updatedb` requires root privileges
      - schedule `updatedb` using `cron` job
    * `find` is the most flexible tool that allows you to find files based on many criteria
      - looks for the exact filename
      - globbing can be used
      - `find --help` for the extra information (quite a complex command)
      - we can search in files using `find` command
        -> `find /etc -exec grep -l student {} \; 2>/dev/null`
           - searches for files that contain 'student' in them, and ommits errors
           - we can use `grep` only for this task, but in find we can combine more commands
      - every `-exec` need to be terminated with `\;`
      - complex example:
        -> `find /etc -size +100c -exec grep -l student {} \; -exec cp {} /tmp \; 2>/dev/null`
    

Path related issues:
    * `name_of_the_command` -> the path
    * `./name_of_the_command` -> current directory 

#### 3.3. Understanding Mounts

Understanding mounts:
    * to access a device, it must be connected to a directory
    * this is know as _mounting_ the device
    * the linux filesystem tipically uses multiple mounts
    * different types of data typically are on different devices for multiple reasons
      * security
      * manageability
      * specific mount options

#### 3.4. Understanding Links

   * Links are pointers to files in a different location
   * Compare to shortcuts on other operating system
   * Link can be useful to make the same file available on multiple locations
   * Linux uses _hard links_ and _symbolic links_
   * Create hard links with `ln` and symbolic link with `ln -s` 

Symbolic links
  * cross-device
  * directories
  * can point to a dead hard links
    -> becomes invalid

#### 3.5 Working with Links



`ls -il /etc/hosts` - display the `inode` number

create a hard link
  * `ls -il source destination`
  * both files grow in size simultaneously
  
create a symbolic link
  * `ls -s source destination`
  * symbolic links require absolute file
  * permissions are completely open (just gives direction)
  * much smaller file size
  * different colour and there is mark where symbolic links point to
  * the purpose of symbolic link that the information is available where it needs to be available
    -> bin in RHEL 7 points to usr/sbin

Symbolic links are more useful and used in Linux film system

#### 3.6 Working with `tar`

* `tar` is the Tape Archiver and was created long time ago
* by default it doesn't compress data
* basic use is to compress, extract or list
  * `tar -cvf my_archive.tar /home /etc`
    -> creates and archive that contains `/home` and `/etc` directories
  * `-` is optional
  * `tar -tvf` will show contents of an archive
  * `tar -xvf my_archive` 
    - it keeps the original archive intact
    - extracts to the current directoy
    - use `-C` switch the output path
      -> `tar xvf my_archive -C destination`
  * stores relative path directory

* to add compression, use the additional key
  - `-z` - gzip
  - `-j` - bzip2
  - `-J` - xz

`tar tvf | less` - inspect the content of the archive
-> every file has magicode at the beginning of file
-> `file filename` - shows what kind of file it is

extension `.tar` is not needed for linux, but it is good for us

#### 3.7 Working with compressed files

A wide range of compression solutions is available for linux
  * `gzip` is still the most common compression utility
    - compress and delete original file
    - `gunzip` - uncompress the archive
    - `gzip -k` - keeps the original file when compressing it
  * `bzip2` is an alternative utility
    - quite similiar to `gzip`
    - offers level of compression
    - slower and more efficient than `gzip`
  * `zip`  is also available and has Windows-compatible syntax
    -> nightmare for linux administrators
  * `xz` is showing up more often as well
    - a lot alike other tools
    - slowest and most eficient

All these tools can be directly used with tar utility

#### Lesson 3 Lab: Essential file management tools

* Create a directory structure /tmp/files/pictures, /tmp/files/photoos and /tmp/files/videos
* Copy all files that have a name starting with and a,b, or C from /etc/ to /tmp/files
* from /tmp/files, move all files that have a name starting with a or b to /tmp/files/photos,
  and files with a name starting with a c to /tmp/files/videos
* Find all files in /etc that have a size smaller than 1000 bytes and copy thos to /tmp/files/pictures
* in /tmp/files, create a symbolic link to /var
* Create a compressed archive file of the /home directory
* Extract this compressed archive file with relative file names in /tmp/archive

**Write a solution for the exercise**

#### Lesson 4: Working with Text files

#### 4.1 Common text tools
  
`more` and `less`
  * `more` was the original file pager
  * `less` was developed to offer some more advance features
  * As reaction to that `more` was developed a bit more
  * But still, to do more, use `less`
  * `more` cannot go upwards -> use `less`

`head` and `tail`
  * `head` to show the 10 first lines of a text file
  * `tail` to show the last 10 lines
  * `-n nn` to specifiy another number of lines
  * we can combine `head` and `tail` commands
    -> `head -n 10 /etc/passwd | tail -n 1`
  * `tail -f /var/log/messages`
    -> real-time refreshing of the log file (monitoring)
    -> to exit `Ctrl-C`

Displaying file content with `cat` and `tac`
  * `cat` dumps text file content on the screen
    - `-A` shows all not printable characters
    - `-b` numbers lines
    - `-s` supresses repeated empty lines
  * `tac` is doing the same, but in reversed order

Other Common text processing utilities
  * `cut` filter output
    -> needs field to separate certain 
    -> `cut -f 3 -d : /etc/passwd`
       => field number 3 with delimiter :
  * `sort` sort output
    -> sorts each character separately (numbers are not sorted properly)
    -> for numeric sort add `-n`
      -> `sort -n`
  * `tr`   translates
    -> put text in a certain format
    -> `tr [:lower:] [:upper:]`
      -> convert from lower to upper
      -> multilingual support and working with special characters

#### 4.2. Using `grep`

Excellent utility to find text in files or in output
  - `ps aux | grep ssh`
    - searches the output of the file
  - `grep linda *`
    - searches for `linda` in every file in the current directory
  - `grep -i linda`
    - case insensitive search
  - `grep -A5 linda /etc/passwd`
    - print 5 lines after finding linda
    -> Useful for log messages 
  - `grep -B5 linda /etc/passwd`
    - print 5 lines lines before line linda (linda is included)
    -> Useful for log messages 
  - `grep -R root /etc`
    -> recursive search (some use `r`, some `R` for recursive calls)
  - `grep 

#### 4.3. Understanding Regular Expressions

* Regular expressions are text patterns that are used by tools like `grep` and others
* Don't confuse regular expressions with globbing
  -> globbing is related to file names, regex to searching in files
  - `grep 'a*' a*` 
    -> `'a*'` - regex
    -> `a*`  - globbing
* always put regex in ' '
  -> shell can recognize it
* Regular expressions are for use with specific tools only: `grep`, `vim`, `awk`, `sed`
* Extended regular expressions enhance basic regex features
  -> you need to put and extra switch to a command 
* see `man 7 regex` for details

Elements of regular expressions
  * Regular expressions are build around _atoms_; an atom specifies what text is to be matched
    - Atoms can be single characters, a range of characters, or a dot
    - Atoms can also be  a class, such as `[[:alpha:]]`, `[[:upper:]]`, `[[:digit:]]`, `[[:alnum]]`
  * Second is the repetition operator, specifying how often a character occurs
  * The third element is indicating where to find the character
p
`egrep` is used when want to apply extended RegEx 
  -> it corresponds to the modern RegEx and we should always use it

Common Regular Expressions:
  * `^`   beginnig of the line
  * `$`   end of the line
  * `\<`  beginning of the word
  * `\>`  end of the word
  * `*`   zero or more time
  * `+`   one or more times
  * `?`   zero or more times
  * `{n}` exactly _n_ times

#### 4.4. Using `awk`

Understanding `awk`
  * `awk` is a powerful text processing utility that is specialized in data extraction and reporting
  * It can perform action based on selector
  * very old tool (from Unix time)

Examples
  * `awk -F : '/linda/ { print $4 }' /etc/passwd`
    -> print 4th column in passwd file matching linda  
  * `awk -F : '{ print $NF }' /etc/passwd`
    -> prints the last field in the line
    -> useful when you don't have the same number of columns in the line
  * `ls -l /etc/ | awk '/pass/ { print }' | less`
    -> the same result can be done with grep 
      => `ls -l /etc | grep pass`

#### 4.5. Using `sed` (stream editor)

Understanding `sed`
  * `sed` is the Stream EDitor, used to search and transform text
  * It can be used to search for text, perform an operation on matching text

Examples:
  * `sed -n 4p sedfile`
    -> printing fourth line in a sedfile
  * `sed -i s/four/FOUR/g sedfile`
    -> interactive => it does changes to a file
    -> `-i` writes to the file, otherwise it would write only to the screen
  * -> substituition works the same as in `Vim` 
  * `sed -i -e '2d' sedfile`
    -> it deletes second line in the sedfile
    -> we don't need to open files when we delete something

#### Lesson 4 Lab: Working with text files

Assignment:
  1. use `head` and `tail` to display the fifth line of the file /etc/passwd
  2. use `sed` to display the fifth one of the file /etc/passwd
  3. use `awk` in a pipe to filter the last column out of the results of the command `ps aux`
  4. Use `grep` to show the names of all files in /etc that have lines that contain the text 'root' as a word
  5. use `grep` to show all lines from all files in /etc that contain exactly 3 characters
  6. use `grep` to find all files that contain the string 'alex', but make sure that 'alexander' is not included in the result

#### Lesson 4 Lab Solution: Working with text files

  **TODO**


### Lesson 5: Connecting to a RHEL Server

#### 5.1 Understanding the Root User

#### 5.2. Logging in to the GUI

#### 5.3. Logging in to the Console

#### 5.4. Understanding Virtual Terminals

#### 5.5. Switching between virtual terminal

#### 5.6. Using su to work as another user

#### 5.7. Using sudo to perform administrator tasks

#### 5.8. Using ssh to log in remotely

#### Lesson 5 Lab: Connecting to a RHEL server

#### Lesson 5 Lab Solution: Connecting to a RHEL server





### Managing Permissions

### Configure Networking


