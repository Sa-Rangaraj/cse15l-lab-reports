# Lab Report 4 - Vim (Week 7)

**Step 4: Log into ieng6**

*Keys pressed* 
1. `<up arrow>`
2. `<enter>`

The `<up arrow>` key allows the user to acess their terminal history. The last command I used (excluding the commands used while I was on the server) was `ssh srangaraj@ieng6-202.ucsd.edu`, thus this was the command that executed when I pressed `<enter>`. The ssh command was used to provide a connection with the server - which was given by the following argument - that would allow me log on and acess it from my local machine. The part of the argument before the "@" is my server-specific username, and the string after is the host of the server. 

*Screenshot of Terminal* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/f5e02f6a-2230-41b3-8c9e-ad6b330f2454)



**Step 5: Clone your fork of the repository from your Github account**

*Keys pressed*  
1. `g` `i` `t`
2. `<space>`
3. `c` `l` `o` `n` `e`
4. `<ctrl> <shift> v`
5. `<enter>`

The command I typed: `git clone` is used to copy all the contents (folders/directories, files, etc...) from a github repository onto the users local machine. The argument for this command, `git@github.com:Sa-Rangaraj/lab7.git` tells the command which specific repo I am attempting to copy over. Note that this argument is not a HTTPS link. This is because we're attempting to clone the repo into our ssh server, thus we must also use the ssh formating of the clone URL.

To type in this argument, I made the consecutive keystroke `<ctrl> <shift> v`. This allowed me to "paste" the url (which i had previous copied) from my clipboard onto the terminal. The addition of the `<shift>` key is used becasue `<ctrl> v` had already been mapped to a different shortcut, thus another key was needed to differentiate when the user was intending to "paste" rather than a different action. 

*Screenshot of Terminal* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/98919d94-cb3f-403d-bb73-d19b4996acff)



**Step 6: Run the tests, demonstrating that they fail**

*Keys pressed*
1. `c` `d`
2. `<space>`
3. `l` `<tab>`
4. `<enter>`

This `cd` command was used to enter the `~/lab7` directory, as it contains the file we will be using to run the tests. The reason for typing `<tab>` after `l` instead of typing out the entire directory name was also for convieneice. Because there is only one folder in the home direcotry that starts with an "l", the terminal can "assume" the directory we're attempting to acess, and "fill in" the rest of the name for us. This also has the additional benfit of allowing us to confirm that the path we're trying to acess can be acessed by this working directory, and reduces the chance for humman errors such as typos. 

5. `b` `a` `s` `h` 
6. `t` `<tab>`
8. `<enter>`

The `bash` command is used to run `.sh` files, similarly to how `java` is used to run Java files. Again, because there is only one file that sharts with the character "t" in the current working directory, using the `<tab>` key will "fill in the blanks" for the argument, finishing the name of the `.sh` file for us. This script contains the commands that both compile and run all the java files in the working directory, and the two files in `~/lab7/lib` that JUnit needs to properly run the test cases. Thus, by running the script, we are also compiling and running our test cases, the results of which are printed to the terminal. 

*Screenshot of Terminal* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/3adcc61f-cda6-440b-9512-160edca42a60)




**Step 7: Edit the code file to fix the failing test**

*Keys pressed*
1. `v` `i` `m`
2. `<space>`
3. `L` `<tab>` `.` `j` `<tab>`
4. `<enter>`

The `vim` command followed by the file containg the test cases opens said file in vim, allowing the user to edit it directly from the terminal. When typing the in the argument for the file the `<tab>` was used once again to make the terminal "fill in" the rest of the fils name for us. However, becasue there were multiple files that started with "ListExamples", the terminal couldn't fill in the prompt all the way, as it had no way of knowing which the user wanted to use. This we needed to add aditional characters before using `<tab>` again, so the terminal could determine we wanted to open `ListExamples.java` as opposed to `ListExamples.class`.

5. `<up arrow>` (6 times)
6. `e`
7. `x`
8. `i`
9. `2`
10. `<excape>`
11. `:` `w` `q`
12. `<enter>`

The `<up arrow>` key was used repeatedly to move the cursor from the bottom of the screen to the line that contained the error. The `e` key brought the cursor to the last character of the first word, where the `x` key was used then used to delete that character, as it deletes whatever character the cursor is currently hovering over. The `i` key was then used to switch into insert mode, so that our next key was typed into the file, rather than acting as a shortcut, as all the previous keystroked had. The `<escape>` key was then used to exit insert mode, allowing us to use the command `:wq` which saves all changes made to the file and before quiting the vim application

*Screenshot of Vim* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/9b5866cb-cf84-4fb4-931f-157a7e198d77)

*Screenshot of Terminal after quiting Vim* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/6ac2c217-ea60-4b59-be31-381b9f28793e)



**Step 8: Run the tests, demonstrating that they now succeed**

*Keys pressed*
1. `<up arrow>` (2 times)
2. `<enter>`

To run our test cases again, instead of retyping `bash` and the file name, we can instead use the `<up arrow>` to acess our history, just as we did in Step 4. As we ran another command after executing our script, we now have to use the kew twice, as opposed to one, seeing as it's "further back" in our history. 

*Screenshot of Terminal* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/e477a845-4e3b-42e5-a527-5eea426cce68)



**Step 9: Commit and push the resulting change to your Github account**

*Keys pressed*
1. `g` `i` `t`
2. `<space>`
3. `c` `o` `m` `m` `i` `t`
4. `<space>`
5. `-` -a`
6. `<enter>`


7. `i`
8. `A` `l` `l`
9. `<space>`
10. `t` `e` `s` `t`
11. `<space>`
12. `c` `a` `s` `e` `s`
13. `<space>`
14. `s` `u` `c` `c` `e` `e` `d`
15. `<escape>`
16. `:` `w` `q`
12. `<enter>`



*Screenshot of Commit* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/a56eb32d-d419-4965-b6fa-c9fa19b858b6)

*Screenshot of Push*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/2577f90c-5181-402f-9e95-520b467e4016)


