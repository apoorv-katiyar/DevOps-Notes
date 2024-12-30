Linux: 
  - Open Source Operating System based on unix.
  - It is known for its security, flexibility.
History:
  - Created by Linus Torvalds in 1991.
  - The name "Linux" comes from Linus+Unix.

Unix is licensed while linux is open-source. Linux is inspired by unix.

Linux is not an OS. We cannot directly use it. It is an open source Kernel. 

A kernel is the core part of an operating system, managing system resources and communication between hardware and software.

Linux Distributions ~ Distros
  - An operating system made from a software collection, which includes the linux kernel and often a package management system.
  - Operating system - Ubuntu, centos, redhat, linux-kali.

Key Features:
  - Opensource
  - Multiuser
  - Multitasking
  - Security
  - Portability


Virtualization
  - Hypervisor: It is a software that creates and runs virtual machine. 

How Hypervisor works:
  - Virtual box share hardware resources from Host OS.
  - Separate set of virtual CPU, RAM, Storage etc.
  - VMs are fully isolated(independent of HostOS)

Types of HyperVisors:
  - Type1 (Bare Metal) - These types of HyperVisors directly setup/install on the Hardware. It works as a mini operating system with basic requirements. Used by Cloud Providers and Enterprise Servers.
  - Type2 (Hosted) - This is having Host Operating System.

Virtualization Use Case:
  - Cheap
  - Reduces:
    - Workload
    - Space
    - Energy
  - Easy Backup using Snapshot
  - Easy Recovery.

Putty:
  - Free and Open-Source Terminal.
  - Its widely used for remote access to servers via ssh.
  

for checking IP addresses: ifconfig

SCP (Secure Copy Protocol):
  - It is a command-line tool used for securely transferring files between a local and a remote host, or between two remote hosts, using ssh for encryption and authentication.
  - scp /local/file
  - username@remote_host:/path/


pwd : present working directory
whoami : current user
date : current date 
(date + %T) for time and (date + %D) for date :  if you want to see only date and time then we can use.
ls : show files nd folders
ls -lt : this will do reverse sorting and provide more information of files/directories
ls -lh : means it will provide the information in human readable
ctrl+l : to clear terminal
creating deleting and editing files:
  - cat <filename>
  - less <filename> and then  do /the char that you want to search. and if you want to go to next press N. and if you want to go to bottom press shift+G and if you want to come to first line press P. adn if you are at bottom of the file and you want to search something then you can use ? and if you want to come out then press Q for quit.
  - more command if used for page by page view.
  - touch <filename>: for creating own file
  - rm <filename>: for deleting file name
  - nano <filename> or vi <filename> - for editing file
  - mkdir <newfolder> : making new folder
  - rmdir <dir_name>: deleting existing directory
  - rm -rf <dir_name> - same as above
  - cd <path> : change directory
  - cd .. : coming one folder back
  - cd / : forward / means root directory.
  - cp <filename> /dest/path : copy and paste a file from one folder to another in linux.
  - head -<no of lines> <filename> : starting lines.
  - tail -<no of lines> <filename> : last lines.
  - sort <filename> : sort content
  - sort -r <filename> : reverse sort
  - sort <filename> | uniq : find unique items in file
  - split -l 3 <filename> : if file has 9 lines, splitting the file into 3 diff files.
  - grep "word" <filename> : search a word and display matching content from a file.
  - wc -l <filename> : to get no of lines in file.
  - diff -u <fileA> <fileB> : compare and display diff between two files.
  - find /path/ -name <filename> : how to find a file.
  - updatedb & locate <filename> : fast and before using locate we have to use updatedb.
  - alias l="ls -ltr" : used for short aliases
  - alias -p : existing alias
  - gzip -k <filename> : to compress the files.
  - gunzip <filename> / gzip -d <filename> : for unzipping the file.
  - tar -czf <filename that we want to give> <folder name> : same way we can create an archive version of the folder.
  - tar -xgf <folder that is compressed> : decompress the file that is already compressed.
  - zip <files name> <file1> <file2> : to compress multiple files in one zipped file inn linux.
  - unzip <filename> : to unzip
  - unzip -l <filename> : to list all the files that are on zip
  - wget <URL_of the file> : downloading a file from internet
  - wget -O opt_file.txt <URL_of the file> : same as above
  - curl <api address> : to all an api in linux.
  - apt or yum / dnf : install an application on linux.
  - systemctl start/stop service_name : start/stop a service on linux.
  - systemctl list-units --type=service --all : list all services on linux.
  - awk -F , '{print $2}' file.csv : print a specific column from a csv file.
  - cut -c1-2 <filename ex: file.txt> : display starting two characters of all line.
  - sed -n '5p' <filename ex: file.txt> display specific line in a file

  - convert a content to uppercase and lowercase within a file:
    - tr [:lower:] [:upper:] <file.txt> , here <tr> means translate
    - tr [:punct:] Z <<file.txt>
    - tr [:digit:] Z <<file.txt>
  - to delete some content from a file:
    - tr -d % <test.txt>   (% is the sign that we want to delete)
  - extend or shrink size of a file:
    - truncate -s 100M file.txt
  - display following lines in vertical line: ABCDE
    - echo "ABCDE" | fold -w1
  

  ## Access Remote Servers:
    - ssh user@hostname
  - how to copy a file to a remote access server:
    - scp file user@hostname:/tmp/  </tmp/ is location of the file>

  ## Working with permissions:
  - ls -ltr : to check permissions of files
  - rwx : user   rw- : group  r-- : other
  - modify the permissions of files:   chmod a+rwx  file.txt   u: user, g: group, o: other, a: all
  
  ## Memory information:
  - Free
  - free -h
  - top : % of memory and task utilized
  - du  : disk utilization
  - dh : to check filesystem available and disk space allocated.
  - hostname : check hostname
  - lscpu  : information available for cpu nd all
  
  
## linux commands starts from a to z:
  - <awk>: a powerful programming language used for pattern scanning and processing.  more commands: alias, apt, at
  - <bg>: puts a job in the background, bc: binary calcy, bash: the bourne again shell, a widely used command interpreter.
  - <chmod>: changes the file mode(permissions), more commands: chown, cp, cal, chgrp
  - <df>: shows disk space usage, more commands: du, diff, date
  - <env>: displays the env variables. more commands: echo, export
  - <find>: searches for files in a directory hierarchy. more commands: file, fg, free
  - <grep>: searches files for a specified pattern. more commands: gzip, groupadd
  - <history>: shows the command history. more commands: head, hostname
  - <ifconfig>: configures or displays network interface parameters. more commands:ip, id, iptables.
  - <jobs>: lists the current jobs.
  - <kill>: Send a signal to a process.
  - <ls>: lists directory contents. more commands: less, ln, locate.
  - <mkdir>: creates directories. more commands: mv, more
  - <netstat>: prints network connections, routing tables, interface statistics, masquerade connections, and multicast memberships. more commands: nano, nice.
  - <ps>: reports a snapshot of the current processes. more commands: printenv, pwd, passwd
  - <quota>: displays the disk usage and limits for a user or group.
  - <rsync>: fast and versatile file copying tool. more commands: rm, rmdir, reboot.
  - <sudo>: allows a permitted user to execute a command as the superuser or another user. more commands: sed, sort, systemctl
  - <top>: displays the linux tasks. more commands: tail, tar, tee, touch, telnet.
  - <vi/vim editor>: text editor.
  - <wget>: non interactive network downloader. more commands: wc, whoami, which
  - <xxd>: creates a hex dump of a given file or standard input.
  - <yum>: interactive, rpm-based package manager(used in Red Hat-based systems). more commands: yes
  - <zip>: package and compress(archive) files.

