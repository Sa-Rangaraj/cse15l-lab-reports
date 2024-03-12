# Lab Report 3 - Bugs and Commands (Week 5)

## Part 1 - Bugs

**Student Post:**

Hello, I'm havinng some issues getting my code to work. I keep getting the error "java.lang.OutOfMemoryError:" 

Based off my test case results, I think that my second merge method is casing the issues, but I'm having trouble figuring out why. 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/ea15df06-56dc-4c5d-bb6c-94349eb2f89d)


**Tutor Response:**

Hello Student! Unfortunatly, I can't give you much help without being able to look at you code myself, as there are an infinite amount of cases in which may result in this error. 

However, what I can tell you is that this error is thrown when your code is attempting to create a new object, but finds that it doesn't have enough space in memory to store it. For the scale of the programs we write in CSE 15, memmory allocation shouldn't be an issue. Is there any reason your code may be creating more objects / using more memory than you intended?

If your code has a main method, you can try using the Java debugger from the terminal to look at the creation of your local variables. Another way you could debug your code is by putting print statements near the lines where object creation is occuring, to see if those lines of code are running when you don't intend them to. (Hint: take another look at the error message to see when exactly the error was thrown, you may be able to use it to "work backwards" to find the bug) 

Good luck!





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
















