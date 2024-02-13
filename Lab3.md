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



**Option: -i**

