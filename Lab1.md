# Lab Report 1 - Remote Access and FileSystem 
## The cd command 
*The `cd` command is used to change the user's current working directory*

**When providing the command with no arguments, the user will be taken into their home directory** 

As shown below, the user's working directory was previously the lecture1 folder: `~/lecture1`, however after running the command the terminal prompt only displays the `~`, indicating that they were taken back into the home directory. As shown by the second use of the command - which is in the messages folder within the lecture1 directory: `~/lecture1/messages` - the user will be taken to the home no matter what the current directory is. This is not an error, rather a shortcut allowing one to get to home `~` without having to provide a path to the terminal. 
```
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$

[user@sahara ~/lecture1/messages]$ cd
[user@sahara ~]$
```  

**When providing the command a directory path as an argument, the user will be taken into the directory they provided**

In the example below, the current directory is at first home `~`, but after running a command supplied with `lecture1/` as the argument, the folder's path is added onto the terminal prompt, indicating that it's now our current working directory: `~/lecture1`. The same thing occurs when we provide `messages/` as the argument, after running the command it now becomes our working directory: `~/lecture1/messages`. This is an example of the command running correctly, without error.

```
[user@sahara ~]$ cd lecture1/
[user@sahara ~/lecture1]$ cd messages/
[user@sahara ~/lecture1/messages]$
```

If the path given is not a directory within the current working directory, then the terminal will indicate that this is the case by giving the error `No such file or directory`, and no change will be made to the current working directory. In the second example below, this error is given because the user is trying to enter the lecture one folder: `~/lecture1` from within the messages folder: `~/lecture1/messages`, when the lecture1 folder is actually outside the messages folder, as indicated by the way the paths are stuctured. 
```
[user@sahara ~/lecture1/messages]$ cd lecture1/
bash: cd: lecture1/: No such file or directory
```

However this instance of the error will only occur when using relative paths. If given a absolute path, meaning a parth starting from the root directory, so long as the path actually exist the terminal will switch to it no matter the users current directory. Hence why the argument `lecture1/` in the example above gave an error while `/home/lecture1/` below does not, despite the fact that they both end in `lecture1/`. The former is relative, meaning it must be compatable, or rather consistent with the current working direcotry, while the later being an absolute path exists entierly on it's own, and thus can be accessed no matter the current working directory. However, if there happens to be some issue with the absolute path, such as a typo or syntax mistake, the terminal will once again output the error, as when the provided innacurate instructions, it is not able to find the directory the user seeks.

```
[user@sahara ~/lecture1/messages]$ cd /home/lecture1/
[user@sahara ~/lecture1]$ 
```

**When providing the command a file path as an argument, the terminal outputs an error**

In the example below the user is currently in the messages directory: ~/lecture1/messages, which (shown by using the `ls` command) contains multiple text files. However when providing any of these files as the argument for `cd`, a error message appears displaying: the command used `cd`, the file which was provided as an arguemnt, and the message "Not a directory"

This is because the `cd` command is specifically used for the purpose of changing the user's working directory. Because a file is not a directory, when provided the path to one as an argument, even if the file is within the current directory, the terminal is unable to "switch into" it. This results in an error, to which the appropriate is outputed, while the direcotry remains unchanged. 
```
[user@sahara ~/lecture1/messages]$ ls
el.txt  en-us.txt  es-mx.txt  zh-cn.txt
[user@sahara ~/lecture1/messages]$ cd el.txt
bash: cd: el.txt: Not a directory
[user@sahara ~/lecture1/messages]$ cd en-us.txt 
bash: cd: en-us.txt: Not a directory
[user@sahara ~/lecture1/messages]$
```


## The ls command 
*The `ls` command is used to list all the files and folders contained in the specificed directory*

**When provided with no arguments, the command will deafult to the current working directory**

In the example shown below, the first working directory is the lecture1 folder: `~/lecture1`, thus when using only the `ls` command, we're shown all the contents of said folder, which includes a java file, a class file, a `README` text file, and a folder titles messages. When we switch into the messages folder `~/lecture1/messages` (using the `cd` command), and use `ls` again, we're now instead provided a list of the contents, 3 txt files, of our new current directory. This is an example of the command working correctly, without error. When not supplied a specific argument, the `ls` command will deafualt to listing the conntents of the current working directory.
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
[user@sahara ~/lecture1]$ cd messages/
[user@sahara ~/lecture1/messages]$ ls
el.txt  en-us.txt  es-mx.txt  zh-cn.txt
[user@sahara ~/lecture1/messages]$
```

**When provided with a directory path as the argument, the command will show the contents of said directory**

In the example shown below, the user is currently in the home directory `~`. However, when using the `ls` command and providing the path for the lecture1 folder, the terminal outputs a list of the contents within said directory, not the working directory.This is an example of the command working correctly, without error. When provided a valid argument, it no longer has to deafualt to the current working directory, and thus will show the contents in the provided path. 
```
[user@sahara ~]$ ls lecture1/
Hello.class  Hello.java  messages  README
```

However, when swtiching into the messages directory `~/lecture1/messages` and then attempting to use the same command, we get the error message `No such file or directory`. This is because the command only works if the provided argument is a directory that can be found within the current directory. If the path is not within the current directory an error occurs, as shown below. Because the messages folder is inside the lecture1 folder, we are unable to access the former while inside the later. Similarly to the ls command, if using an absolute path rather than a relative path this becomes a nonissue, any directory can be acessed no matter the current working directory as the argument will no longer need rely on the current directory to find the neccesary folder. 
```
[user@sahara ~]$ cd lecture1/messages/
[user@sahara ~/lecture1/messages]$ cd lecture1/
bash: cd: lecture1/: No such file or directory
[user@sahara ~/lecture1/messages]$
```

**When provided with a file path as the argument, the command will only display said file**

The `ls` command is used to list the contents of a given path. If said path points to a file, the only files/folders contained within the path is the file itself - in contrast to path directories can point to multiple files and other directories. As a result, if the argument given is the path to a file, the command can only list the file that the path points to. Thus, in the examples shown below, both uses of the `ls` command result in the terminal printing the name of the file provided below. 

While this is not technically an error - the command runs without an error message and outputs the approrpaite contents - it is not the proper way to use this command, as the user gains no additional information by running it. 

Note that similarly to when using a directory as an argument, if the argument given isn't contained within the current working directory, the terminal will output the `No such file or directory` error. In the example below, the first two files are contained within the current working directory `~/lecture1/messages`, thus the terminal sucesfully outputs the file name. However, when attempting the same with a file not shown when using `ls`, the afermentioned error is produced as the path provided does not exist.  
```
[user@sahara ~/lecture1/messages]$ ls
el.txt  en-us.txt  es-mx.txt  zh-cn.txt
[user@sahara ~/lecture1/messages]$ ls el.txt 
el.txt
[user@sahara ~/lecture1/messages]$ ls en-us.txt 
en-us.txt

[user@sahara ~/lecture1/messages]$ ls dne.txt
ls: cannot access 'dne.txt': No such file or directory
[user@sahara ~/lecture1/messages]$
```

## The cat command 
*The `cat` command is used to list out and "concatenate" the contents of the given files*

**When provided with no argument, the command outputs a copy of the user input**

In the example below, after the user initially uses the `cat` command, we see that each subsequent line - until control c (displayed as `^C`) is used to "quit" the command - repeats itself. After the intial command is given, the terminal provides a new line, without a terminal prompt, indicating to the user that it's expecting further input for the current command. Once user input is given, the terminal will print out said user input onto the next line, then once again provide a new line to the user without a terminal prompt, creating a loop until the user exits. 

In the example below, the first "cat" under the intial command was provided by the user, while the second underneath was output by the terminal, similaraly the first instance of "example" was entered by the user, while the "example" underneath was a result of the terminal. This occures due to the user's negligance of not providing a file with the intial `cat` command. 

While other commands may be able to interact with a directory, and thus are able to function without aditinal arguments, simply deafulting to the current working directory (which in this case would be ~/lecture1), this command is intended to print the contents of a file, and thus needs to be provided a file name to function properly. As shown below, if not given one, the command will simply print whatever contents the user provides it as it's output. As a result, while an error message may not have been output to terminal, this use of the command should be considered as producing an error, as it does not adhere to the intended funcionality of the command. 
```
[user@sahara ~/lecture1]$ cat
cat
cat
example
example
^C
[user@sahara ~/lecture1]$
```

**When provided with a directory path as an argument, the command will provide an error message**

As explained in the previous example, the `cat` command requires a file to be provided by the user, and results in an error when forced to work with directories alone. Hence, when provided a valid directory path, the error `Is a directory` will be output by the terminal, indicating to the user that the provided path isn't an appropriate argument for the given command. In the example below, when the messages path is provided, the terminal acknowledges that this is a valid path as `~/lecture1/messages` exists within the currect directory of lecture1 `~/lecture1`, but refutes the argument, as it's not a file, by outputing the afermentioned error message. 
```
[user@sahara ~/lecture1]$ cat messages/
cat: messages/: Is a directory
[user@sahara ~/lecture1]$
```

However, if the argument doesn't lead to a valid directory, a different error message will be outputed. In the second example, the user attempts to acess the lecture1 directory `~/lecture1` while currently in the messages directory `~/lecture1/messages`. As the given argument can't (or rather wasn't told to) step back into a previous directory, the terminal doesn't except this argument as a valid path - as inside the current working directory it sees nothing with the name of "lecture1". Thus instead of the afermenioned error message, the terminal instead outputs `No such file or directory`, as with an incorrect path it cannot dicern wether to the user was trying to acess a file or directory, just that the given path was wrong. Hence why it's error message account for both possibilities.

```
[user@sahara ~/lecture1/messages]$ cat lecture1/
cat: lecture1/: No such file or directory
```

In short, providing a directory path as an argument will always create an error, however the type of error depends on wether the provided path was deemed as valid, actually existing within the machine. 


**When provided with a file path(s) as an argument, the command will print the contents**

In the first example shown below, the user correctly provides a file path that exists within the current working directory `~/lecture1/messages`. As a result, the command works as expected, without error. It prints out the contents of the file on the line below, then readies the terminal for it's next command with a new terminal prompt. As shown by the fact that both terminal prompts are identical, no "changes" to the working direcotry are made when correctly executing this command. 
```
[user@sahara ~/lecture1/messages]$ cat el.txt 
Γειά σου Κόσμε!
[user@sahara ~/lecture1/messages]$
```

The second example shows a case of then mutliple file pathways are given as arguments. The user remains in the messages directory `~/lecture1/messages`, but this time supplies and additional file name found in the folder, seperating the two files with a space. When executed, the terminal prints out the contents of the first file, then immediatly concatenates the contents of the second. In other words, it prints the contents of the files in the order given, and doesn't give any indication (such as adding an extra line) as to when a switch has occured, and a new file is being printed from. This is not an error; the command is not intended to give an indication of this switch as it's purpose is to concatenate, or seamlessly link together. 
```
[user@sahara ~/lecture1/messages]$ cat el.txt en-us.txt 
Γειά σου Κόσμε!
Hello World!
[user@sahara ~/lecture1/messages]$
```

While not shown below, it may also be noted that if given an incorrect file path (one that does not exist or is outside the scope of the current working directory), similary to when given an incorrect directory path, the terminal will result in an error, outputting the errormessage `No such file or directory` to the terminal. 


