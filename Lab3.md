# Lab Report 3 - Bugs and Commands (Week 5)

## Part 1 - Bugs

**Example of a failure-inducing input**

*JUnit test case:* 
```
@Test
  public void testReversedInPlaceWithFailure(){
    int[] originalArr = {1, 2, 3};
    int[] reversedOriginalArr = {3, 2, 1};
    ArrayExamples.reverseInPlace(originalArr);
    assertArrayEquals(reversedOriginalArr, originalArr);
  }
```

**Example of a input resulting in a succesful test**

*JUnit test case:*
```
@Test
  public void testReversedInPlaceWithoutFailure(){
    int[] originalArr = {3, 4, 5, 4, 3};
    int[] reversedOriginalArr = {3, 4, 5, 4, 3};
    ArrayExamples.reverseInPlace(originalArr);
    assertArrayEquals(reversedOriginalArr, originalArr);
  }
```


**Symptom analysis**

*JUnit output to terminal*
```
JUnit version 4.13.2
.E.
Time: 0.004
There was 1 failure:
1) testReversedInPlaceWithFailure(ArrayTests)
arrays first differed at element [2]; expected:<1> but was:<3>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReversedInPlaceWithFailure(ArrayTests.java:27)
        ... 32 trimmed
Caused by: java.lang.AssertionError: expected:<1> but was:<3>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 38 more

FAILURES!!!
Tests run: 2,  Failures: 1
```

As shown by the terminal output, the first test case `testReversedInPlaceWithFailure()` resulted in an error. Acording to JUnit, this is because the second element of `originalArr` after calling the `reverseInPlace()` method was `3`, when it was expected to have been `1`. Thus, reversing our array with the method likely resulted in the origional array becoming `{3, 2, 3}`, when it was supposed to have been `{3, 2, 1}`. Not only does this symptom show that the method was incapable of reordering the elements correctly, but that it actually changed the elements within the array. 


**Fixing the bug** 

*Method when containing bug**
```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

The issue with the code above is that it atempts to reverse the argument array, `arr`, directly without making a copy anywhere of the original order. As a result, the code uses the partially reversed `arr` itself as a reference to reverse the latter half of the array. However, because the first half was already reversed, when attempting to reverse the latter half, the array is just reversing useing values it has already reversed. This double reverse simply ends up "cancelling out", meaning that the latter half of `arr` keeps it's origional values. This is why the input `{3, 4, 5, 4, 3}` passed the test case while `{1, 2, 3}` was a failure-inducing input. The former would look the same reversed, thus it didn't cause any noticable symptoms when the reversal didn't occur for the later half. The former on the other hand is not symmetrical, which is why it produced a symptom which made the bug apparent. 


*Method after fixing bug*
```
static void reverseInPlace(int[] arr) {
    int[] copyArr = new int[arr.length];
    for (int i = 0; i < arr.length; i ++){
      copyArr[i] = arr[i];
    }
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = copyArr[arr.length - i - 1];
    }
  }
```
To fix this bug, we simply have to ensure a way for the program to "remember" the previous order before it beings to reverse `arr`. One method of doing this (as shown above) is using a temp array such as `copyArr`, which is a deep copy of `arr`. This way, when we go to reverse `arr` we can reference `copyArr` for the order rather than relying on the array that we are activly attempting to change. 


## Part 2 - Researching Commands: grep

**Option: -i**

When the option `-i` is added onto the `grep` command, it allows the user to search for a pattern in their desired file without worrying about letter cases. Typically the `grep` command only returns strings that match the pattern exactly, which includes letter case. For example, when searching for the pattern "hello", strings such as "Hello" or "heLLo" wouldn't be included in the results, as the "H" and the "LL" respectivally don't match the case of the original pattern. However, if doing the search with the `-i` option the previous strings would be returned to the terminal, as now the differing cases are neglagible, and the string is considered to match the pattern. 

*Example 1* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/e961a389-38d3-4618-9bf5-7a610fa5d499)

In this example, we are using the command `grep -i "time" technical/911report/chapter-11.txt` to search for the pattern "time" regardless of case wthin the file `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/911report/chapter-11.txt`. As we can see, the command succesfully output every instance of "time" within the file regardless of case. For example, there are occurances of the pattern in which the first letter is lowercase, such as in `As time passes`, and instances in which it's uppercase, such as in `York Times article`. 

*Example 2*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/4c37c8e0-807a-4ee4-a595-2c21ff53fc50)

In this example, using the command `grep -i "AlCOhOl" technical/government/Alcohol_Problems/Session2-PDF.txt`, we are now searching for the pattern ""AlCOhOl" within the file `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/government/Alcohol_Problems/Session2-PDF.txt`. Note that the screenshot above does not show the entirety of the command's output. Despite the unconventional cases used within the pattern, which are never matched exactly within the file, the terminal still outputs every instance the word alcohol, as the `-i` option ignores case differences. 

This option is incredibly usefull when searching for information in a plain text file such as the examples shown above. While matching cases might make sence when searching through a code file - as differences in case may have specific meaanings, such as differentiating between class names and variables - they don't make as much sence for languages such as English, where cases may differ simply due to their placement within a sentence. When adding `-i` onto the `grep` command, the user is no longer at risk for missing certain information and pattern matches, simply because they happen to occour at the beginning of a sentence. 

*Citation: https://www.geeksforgeeks.org/grep-command-in-unixlinux/*


<br>

**Option: -r**

When the option `-r` is added onto the `grep` command, it allows the user to search recursivly through a given directory for their specified pattern. Typically, the `grep` command takes a file as an argument, searching through that specific file for strings that match the pattern. While the command can take multiple file arguments, this allows a much faster way to search through a large group of files, so long as they're contained within one directory. When adding the `-r` option, the command's output will no longer only list the occurances of the patteren, but also the file that each occurance belongs to. 

*Example 1* 

 ![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/0ea37eb9-9eb5-4624-b408-5283951491f1)

In this example, we first `cd` into the directory `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/911report`, then use the command `grep -r "public"` to recursivly search for all instances of the pattern "public". As a directory argument isn't given, the command deafults to searching within the current working directory. Again please note that the provided screenshot is unable to show the entirety of the output that was given. For each instance of the pattern found, the file is listed to the left, and the context of the matching string is indented towards the middle. Additionally, using the listed file names, we can see that while the command may be recursive, it does not look at each file linearly; while the chapters are ordered in numerical order within the file itself, the command does not display them in that same order. 

*Example 2*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/1d9e3400-ae8b-473d-b688-fd1baf69fec6)

In this example we are within the directory `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch`, as use the command `grep -r "daily" technical/911report/` to search for the pattern "daily" with the directory `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/911report`. Because we included a directory argument to the command after the pattern, we are now searching in that directory rather than deafaulting to our working directory, as we did in the previous example. Once again the file in which the string was found is displayed to the left, and the order that the files are displayed in are not the order that they exist in within the folder.

As mentioned previously, this command is more useful when attempting to look for a patteren occuring within a large number of files. Rather than having to list each file individually, the `-r` option allows the user a method to itterate through a large number of files with minimal effort. This would be especially usefull when looking through large data sets, as at industry scale looking through single files simply wouldn't be feasible. 

*Citation: https://www.baeldung.com/linux/grep-hidden-files-directories#:~:text=The%20grep%20command%20is%20a,files%20matching%20a%20given%20pattern.&text=The%20%E2%80%9C.%E2%80%9D%20matches%20the%20current,files%20and%20non%2Dhidden%20files." 


<br>

**Option: -n**
When the option `-n` is added onto the `grep` command, it provides the user with additional information regarding their search. Simply using `grep` will output each instance in which a string matching the pattern is found. However when adding `-n`, the terminal will also note the specific line of the file in which each line was found. 

*Example 1* 

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/cd0a6a57-a3dd-4970-b0fb-7e99965db876)

In this example, we are using the command `grep -n "time" technical/government/Alcohol_Problems/Session2-PDF.txt` to search for the pattern "time" wthin the file `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/government/Alcohol_Problems/Session2-PDF.txt`. In the screenshot, at the left of each line, we can see a number with a colon before the expected output of the matching pattern. This number refers to the line in the file at which the corresponding string can be found. 

*Example 2*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/0a571a2a-e998-42dd-a1be-41cfc445ba0d)

In this example, we are using the command `grep -n "vertebrates" technical/biomed/1471-213X-1-6.txt` to search for the pattern "vertebrates" wthin the file `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/biomed/1471-213X-1-6.txt`. Similarly to the previous example, before each matching case the terminal displays the line of the file. 

Similarly to the `-i`, the `-n` option additional is extremely useful when attempting to traverse plain text files. For example, the repository we're using as an example has a multitude of medical files. When attempting to search for a certain word or phrase the user is unlikely to be satisfied with the small context the terminal provides; they may want the larger surrounding context of the text, which would be hard to find soley with `grep` commands. After finding the section they want to read, they may then have to go into that file and use ctrl f in attempt to search with the slight additional information the terminal provided. The `-n` command provides a much easier method for those who wish to read the file themselves, as now having the specific line of their string they can simply navigate to that line of the file, rather than having to use other tedious and conveluted methods of searching. 

*Citation: https://www.geeksforgeeks.org/grep-command-in-unixlinux/*



<br>

**Option: -o**
In contrast to `-n`, when the `-o` option is added onto the `grep` command, it provides the user with less information regarding their search. As stated previously, the command typically outputs the matching pattern, along with some of the text surrounding it. Using the `-o` option however removes this surrounding text, and only outputs the exact matching patter itself. 

*Example 1*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/620fe3bd-6834-40aa-baed-956cd0c543e3)

In this example, we're using nearly the exact same command as we did in the `-n`'s first example, only replacing the option to `-o`. To reiterate, we are using the command `grep -o "time" technical/government/Alcohol_Problems/Session2-PDF.txt` to search for the pattern "time" wthin the file `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/government/Alcohol_Problems/Session2-PDF.txt`. However this time, rather than terminal consisting of several lines containing the specified pattern (along with the line number), it only consists of the exact pattern itself. In short, the command simply outputs the pattern to terminal the number of times that it appears in the given file. 

*Example 2*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/f00e4f55-3cc6-45f0-9be0-ff250cf0b715)


This example also builds off of `-n`'s second example: we are using the command `grep -o "vertebrates" technical/biomed/1471-213X-1-6.txt` to search for the pattern "vertebrates" wthin the file `/home/sahana/Projects/School/CSE_15/Lab4_Workspace/Lab4_docsearch/technical/biomed/1471-213X-1-6.txt`. As the previous example found two lines that included the string, "vertebrates", this output simply consists of "vertebrates" being printed to terminal twice. 

Unlike some of the previous options, `-o` is actually more helpful when the user doesn't intend to read the file or the surrounding context for themselves. Perhaps they simply want to know how many times a certain word or phrase appears. Allowing the user the choice to seperate the patter from it's context provided them an easier route to manipulate the data, as now they can directly preform actions such as counting how many the pattern appears, or diserning it's frequency within the greater file, using additional commands without having to worry about handling data that is not relevant for their usage. 

*Citation: https://www.geeksforgeeks.org/grep-command-in-unixlinux/*
