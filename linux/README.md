# 💻 Linux

## <mark style="color:blue;">Find text in Files</mark>

`grep '^.' filename` - Get all lines that start with a dot or period

`grep -rnw '/path/' -e 'pattern'`&#x20;

* `-r` or `-R` is recursive,
* `-n` is line number, and
* `-w` stands for match the whole word.
* `-l` (lower-case L) can be added to just give the file name of matching files.
* `-e` is the pattern used during the search

Along with these, `--exclude`, `--include`, `--exclude-dir` flags could be used for efficient searching:

* This will only search through those files which have .c or .h extensions:

`grep --include=*.{c,h} -rnw '/path/to/somewhere/' -e "pattern"`

* This will exclude searching all the files ending with .o extension:

`grep --exclude=*.o -rnw '/path/to/somewhere/' -e "pattern"`

* For directories it's possible to exclude one or more directories using the `--exclude-dir` parameter. For example, this will exclude the dirs dir1/, dir2/ and all of them matching \*.dst/

`grep --exclude-dir={dir1,dir2,*.dst} -rnw '/path/to/somewhere/' -e "pattern"`

### <mark style="color:purple;">Find based on file content</mark>

`find . -type f -exec grep "forinstance" '{}' ; -print`&#x20;

### <mark style="color:orange;">Find SUID/GUID and Sticky bit Files</mark>

`find . -perm /4000` - SUID

`find . -perm /2000` - GUID

`find . -perm /1000` - Sticky Bit

`find . -perm /6000` - Search for both SUID & GUID

### Find files based on modified time

`find -mmin 5` - Find files modified exactly 5 minute ago (In that minute only)

`find -mmin -5` - List all files modified in the last 5 minutes

`find -mmin +5` - Find all files modified more than 5 minutes ago

{% hint style="info" %}
`mtime` is used to find files modified more than 24 hours ago, `ctime` can be used to search by file change times, `size` can also be used.&#x20;

Operators like -o for or and -not can also be used (and is the default operator)
{% endhint %}

## File & Directory Operations

### <mark style="color:blue;">Creating files</mark>

`> file.txt` - Empty `file.txt` or create a new file

### <mark style="color:purple;">Storage Usage</mark>

`du -h --max-depth=1` - Show the size of all sub folders in the current directory

### <mark style="color:orange;">Permission</mark>

`chmod --reference file1 file2` - Makes the permissions of file2 the same as file1, also works with chgrp & chown

### <mark style="color:yellow;">Easy copy/backup</mark>

`cp filename{,.bak}` - Quickly backup or copy a file, copies filename to a file named filename.bak

`rename 'y/ /_/' *` - Replace spaces in filename with underscores

## Processes & PS

**Syntax: ps -\[options]**

|  -a | All users                                                                                         |
| :-: | ------------------------------------------------------------------------------------------------- |
|  -u | list the user who started the process or when -u user is specified list the process for that user |
|  -x | Processes not executed by current terminal                                                        |
|  -e | List all processes regardless of the terminal; alt: -A                                            |
|  -H | Display parent child relationship                                                                 |
|  -j | Jobs format                                                                                       |
|  -f | Format                                                                                            |

{% hint style="info" %}
See which program this port belongs to: `lsof -i tcp:80`
{% endhint %}

#### Useful ps variations:

* **ps axjf:** Show process relationship
* **ps aux:** Show username, pid, cpu & memory, start date, & also the command that started the process.
* **pgrep \[process]:** Get PID without using ps | grep \[process]

### <mark style="color:purple;">**Foreground process**</mark>

|    ctrl+c    | Kill a foreground process \[SIGINT] stands for sig-interrupt |
| :----------: | ------------------------------------------------------------ |
|    ctrl+z    | Sleep a foreground process \[SIGSTOP]/\[SIGCONT]             |
| fg \[job-id] | Bring a background process to foreground                     |

**Note: To check sleeping process use jobs command.**

### <mark style="color:orange;">**Background process**</mark>

|       &      | Execute a process in background. Appended at the end of command or script |
| :----------: | ------------------------------------------------------------------------- |
| bg \[job-id] | Run a process in background                                               |
| fg \[job-id] | Bring a background process to foreground                                  |

**Note: To check sleeping process use jobs command.**

### <mark style="color:red;">**Killing a process**</mark>

**kill \[kill-signal] \[pid]**

Using a kill command sends a kill signal: List all kill signals: kill -l. SIGTERM is the default kill signal

|        kill       | SIGTERM \[Kill a process nicely, process can refuse this] |
| :---------------: | --------------------------------------------------------- |
|      kill -9      | SIGKILL \[Kill forcefully]                                |
| fuser -k filename | Kill a process that is ocking a file                      |

Killing with process name: **pkill \[kill-signal] \[name]**

{% hint style="info" %}
Kill a process running in port:&#x20;

* `sudo kill -9 $(lsof -t -i:8080)`
* `sudo fuser -k 8770/tcp`&#x20;
{% endhint %}

## Bash Shell

### <mark style="color:green;">Cut & Paste in Bash</mark>

`Ctrl`+`U`- Cut everything to the left &#x20;

`Ctrl`+`W`- Cut one word to the left

`Ctrl`+`k` - Cut everything to the right

`Alt`+`d`- Cut one word to the right

`Ctrl`+`Y`- Paste a line cut with Ctrl+U&#x20;

### Cursor movement in Bash

`Ctrl`+`L`- Clear the screen and redraw the current line&#x20;

`Ctrl`+`R`- Reverse search history

`Alt`+`F`- Go forward by one word

`Alt`+`B`- Go backward by one word

`Ctrl`+`A`- Go to the beginning of a line

`Ctrl`+`E`- Go to the end of a line

### <mark style="color:purple;">Bash Command Execution:</mark>

`#!/usr/bin/sudo /bin/bash` - Execute entire shell script as sudo (This line replaces shabang line in shell script

`sudo !!` - Execute the last command as root

`!whatever:p` - Find the last command that begins with `whatever`, but avoid running it

`&lt;space>command` - Execute a command without saving it in history

### <mark style="color:yellow;">Spelling errors bash</mark>&#x20;

`!!:gs/foo/bar` <mark style="color:yellow;"></mark> - run the previous command, replace every foo with bar

`^ehco^echo` - Run the previous command, replace first occurence of echo with echo

## VI Editor <a href="#vi" id="vi"></a>

`:w !sudo tee %`  -  Save a file that is opened without sudo

### <mark style="color:blue;">VI Navigation</mark>

`gg` Begning of the file

`G` Move to the last line in the file

`5G` Move to line 5 of the file (5 can be any line number)

`H` Upper left corner (home) of the current screen

`M` Middle line

`L` Lower left corner

`^` Beginning of line

`$` End of line

`l` Forward a character

`w` One word forward

`b` Back one word

### <mark style="color:orange;">Input/Append in VI</mark>

`a` - Append after the cursor

`i` - Append before the cursor

`I` - Append at the beginning of a line

`A` - Append at the end of a line

`o` - Create new line under the cursor

`O` - Create new line above the cursor

### <mark style="color:yellow;">VI Commands</mark>

`u` - Undo last change

`U` - Undo all changes on line

`:w !sudo tee %`  -  **Save a file that is opened without sudo**

`dd` - Delete current line

_`n`_`dd` - Delete _n_ lines of buffer

`%d` - Delete all lines in a file

### <mark style="color:purple;">Search & Replace in VI</mark>

Syntax: `:[address]s/old_text/new_text/`

<details>

<summary>Address components</summary>

* `.` Current line&#x20;
* `n` - Line number&#x20;
* `n.+m` - Current line plus m lines&#x20;
* `$` - Last line&#x20;
* `/string/` - A line that contains "string"&#x20;
* `%`- Entire file&#x20;
* `[addr1],[addr2]` - Specifies a range

</details>

|   :s/foo/bar/  | replace the first match of 'foo' with 'bar' on the current line only          |
| :------------: | ----------------------------------------------------------------------------- |
|  :s/foo/bar/g  | replace all matches (\`g\` flag) of 'foo' with 'bar' on the current line only |
|  :%s/foo/bar/g | replace all matches of 'foo' with 'bar' in the entire file (\`:%s\`)          |
| :%s/foo/bar/gc | ask to manually confirm (\`c\` flag) each replacement                         |

### VI Parameters

`:set number` - Show line numbers

`:set list` - Show invisible characters

## WSL2

`wsl.exe -d wsl-vpnkit service wsl-vpnkit start` - WSL2 with Cisco AnyConnect

`wsl -t Ubuntu` -  Shutdown WSL&#x20;

`wsl --shutdown` - Shutdown all WSL distros
