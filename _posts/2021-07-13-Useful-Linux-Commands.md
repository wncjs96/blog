---
layout: post
title: Useful Linux Commands
date: 2021-07-13 18:19:00
---

### Here are the useful linux/unix commands that I use a lot.

--------------------------------

```
- pwd: path to the current working directory

- ls [-l] [-a]: list files -l to view more details such as users/groups/others, date it was created/touched, permission -a to see hidden files that has prefix . for its name.

- cd <directory-name>: change directory

- mv <source> <destination>: move source to dest, can also copy

- cp [-R] <srce> <dest>: copy source to dest, [-r] for directories

- rm [-R] <file>: remove file

- more <file>: navigate through the file's content

- less <file>: navigate through the file's content without loading the whole content. (only the visible part ~1kb is loaded. Good to use for log files)

- head <file>: output the first 10 lines

- tail [-F] <file>: output the last 10 lines. [-F] for realtime. (Good to use for log files)

- cat <file> : file's output to stdout
- cat <file> | more: file's output to more command
- cat <file> > <file2>: file's output to write into file2
- cat <file> >> <file2>: file's output to the end of the file2

- chmod UGO <file>: change the permission of the file, U for user, G for group, O for others. 3 bit representation.
rwxrwxrwx -> 777, rwxr--r-- -> 744, ...

- chown <user>:<group> <flie>: change the ownership of the file

- sudo <command>: check /etc/sudoers, if the current user is listed, grant temporary super user permission to the current user for 1 command

- su <user>: change current user

- adduser <user> : add user

- deluser <user> : delete user

- uname -a: current system detail

- hostname: current host detail

- reboot: reboot the system

- halt -p: halt the system

- ping <domain>: pings domain including IP

- w: current user's info, who

- ifconfig: ip info.

- ps: process info

- which <command>: find the command's directory path

- top [-d sec]: CPU usage, Memory usage detail

- kill -9 <proces-id>: process kill

- man <command, functions>: manual for command, functions

- grep [-H][-w][-n][-i] <string> <path-to-directory>: find string from the files in path-to-directory. -H to output filename, -n to output line number, -i to match case insensitive, -w to match exactly

- ps -ef | grep <string>: find process info with string in it.

- find <path-to-directory> [-name] <file-name>: from path-to-directory, find the file. recursively for all dirs inside.

- <command1> | xargs <command2>: previous command's output to the argument of the current command

- find <path-to-directory> [-name] <file-name> | xargs grep -n <string>: recursively find all files that contain string from the path-to-directory

- tar cvfz [<file>,] : compress using gzip tar.gz 

- tar xvfz [<file>] : extract using gzip tar.gz

- ln -S <file> <link-file> : soft-link (바로가기)

- ln <file> <link-file>: hard-link, reference connected copy of the file

```
