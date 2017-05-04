# Notes on UNIX, GNU/Linux

## Misc UNIX

**Wheel group**

_for CentOS, Fedora_

A user permission group with more permissions than the basic user. Has access to `sudo`. 

**Adding a public key to the authorized keys (passwordless login)**

`cat ~/.ssh/id_rsa.pub | ssh user@hostname 'cat >> .ssh/authorized_keys'`

**change shell**

When you create a new user with `useradd` (instead of `adduser`) the default shell is `sh` but you probably want `bash`. So do this:

`sudo chsh -s /bin/bash <username>`

**caffeinate**

Keep macOS from falling asleep for an hour:

`caffeinate -t 3600 &`

**List users**

`compgen -u`

**List usergroups**

`compgen -g`

**List the group of the user**

`groups $USERNAME`

**Give user write permissions to a directory**

`sudo chown -R testuser:testgroup /var/www/test/public_html`

**10 Largest (disk usage) directories/files**

_this is a mediocre solution honestly_

`du -a $PATH | sort -n -r | head -n 10`

## Bash

Change the terminal prompt:

~~~bash
PS1="$NEW_PROMPT"
~~~

Count the lines of text in a file:

~~~bash
wc -l file.txt
~~~

Execute a second command when the first command executes successfully:

~~~bash
command1 && command2
~~~

Execute a second command after the first command executes, no matter what:

~~~bash
command1; command2
~~~

Script to unhide all hidden files in a directory. 

~~~bash
GLOBIGNORE=".:.."
for file in .*; do
   mv -n "$file" "${file#.}"
done
~~~

## Vim

Split horizontally (opens foo.txt):

~~~
:split foo.txt
~~~

Split vertically (opens foo.txt):

~~~
:vsplit foo.txt
~~~

Resize split windows:

~~~
:resize 60
:resize +10
~~~

Resize vsplit windows:

~~~
:vertical resize 80
:vertical resize -5
~~~

Print the name of the open file:

~~~
:echo @%
~~~		