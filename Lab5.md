# Lab Report 5 - Putting it All Together (Week 9)

## Part 1 - Debugging Scenario

**Student Post:**

Hello, I'm havinng some issues getting my code to work. I keep getting the error "java.lang.OutOfMemoryError:" 

Based off my test case results, I think that my second merge method is casing the issues, but I'm having trouble figuring out why. 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/ea15df06-56dc-4c5d-bb6c-94349eb2f89d)

**Tutor Response:**

Hello Student! Unfortunatly, I can't give you much help without being able to look at you code myself, as there are an infinite amount of cases in which may result in this error. 



However, what I can tell you is that this error is thrown when your code is attempting to create a new object, but finds that it doesn't have enough space in memory to store it. For the scale of the programs we write in CSE 15, memmory allocation shouldn't be an issue. Is there any reason your code may be creating more objects / using more memory than you intended?

If your code has a main method, you can try using the Java debugger from the terminal to look at the creation of your local variables. Another way you could debug your code is by putting print statements near the lines where object creation is occuring, to see if those lines of code are running when you don't intend them to. (Hint: take another look at the error message to see when exactly the error was thrown, you may be able to use it to "work backwards" to find the bug) 

Good luck!

**Debuging Process* 
*Changes made from tutor's advice*
In `~/StudentCode/ListExamples.Java`


![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/81413d7d-a317-4d2b-bc72-63c7e504d239)


*Terminal Output*
```
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
adding from list 2 (loop3)
org.junit.runners.model.TestTimedOutException: test timed out after 500 milliseconds
```

*the code block above only shows a portion of the output, for ease of reading 


As shown from the terminal output after debugging, the code is -unwantedly- copying elements from the second list, and adding them to our results variable. Because the student specified the loop each print statement was in, the student can focus on debugging their thrid while loop, rather than attempting to look through the entire file from errors. 

By isolating the portion of the code that contains the issue, and comparing it to it's parallel in with first while loop, which serves a similar function yet doesn't result in the infinite object creation, the student may realize that they accidently incremented the variable `index1` instead of `index2`. As a result, `index2` will never be greater than or equal to `list2.size()`, resulting in an infinite loop that eventually throws the afermentioned error. 




**Information Needed about the Setup**

*The File and Directory Structure:*

```
StudentCode/
  |-  lib/
  	|-  hamcrest-core-1.3.jar
  	|-  junit-4.13.2.jar
  |-  ListExamples.class
  |-  ListExamples.java
  |-  ListExamplesTests.class
  |-  ListExamplesTests.java
  |-  StringChecker.class
  |-  test.sh
```

*Contents of file (with bugs)*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/e419d642-c956-4792-8a40-9ba76a4d8cae)


![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/0fe7890a-b2ff-4782-abf0-4a62e655f76e)


![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/ddbfb725-9302-41ea-b8e9-f3abf9b3e2a8)


*Command line to trigger bugs*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/5441c4fe-335f-451a-9bbd-5bdd63ed3f13)

Which is a concise way of running the lines 
```
javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests
```

which are used to compile the neccesary files, and run the Junit tester class. 


*Description of what to edit to fix the bug*

In order to fix this bug, the student would have to change `index1` on line 45 to `inedx2`, so the third while loop's exit criteria can actually be met.



## Part 2 - Reflection

Something new I learned during the later half of the quarter was how to use the Java Debugger from the terminal. While I was already familiar with the debugger system in VS Code, I was unaware that it was based off the Java Debugger. Additionally, it was intresting learning about the differences between the two systems. The java debugger for example seemed more effecient to use when the user was already familiar with their problem - as it could isolate certain variables, run methods on them, and even identify when specifically variables were altered or created. On the other hand, the VS Code debugger, while great for hollistic information, did sacrice may of this features in favor for it's visual approach. Sometimes it felt like I had to dig through several diffferent layers, in order to find what should have been vary basic information. 

The "coolest" thing I think I learned about the debugger was the `list` command. When practicing for the Skill Demo, I was getting frustrated with the debugger, as it felt very unintuitive to use. I was used the afermentioned visual approach of the VS Code debugger, and was used to being able to look at the surrounding context of the code. After asking my father for help, he taught me about the `list` command, which could provide that surounding context I was missing. It helped "bridge the gap" in a sence, and made the JDB much less intimadating to use. 






