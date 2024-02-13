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


## Part 2 - Researching Commands

**Example of a failure-inducing input**
