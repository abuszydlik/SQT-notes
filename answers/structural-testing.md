# Structural testing answers
## Exercise 1
To achueve 100% line coverage we need three tests for example:
```java
class LinkedListTest {

  @Test
  void removeNullWhenPresent() {
    LinkedList list = new LinkedList<>();
    list.add(null);
    assertTrue(list.remove(null));
  }  

  @Test
  void removeObjectWhenPresent() {
    LinkedList list = new LinkedList<>();
    list.add(3);
    assertTrue(list.remove(3));
  }

  @Test
  void removeObjectWhenNotPresent() {
    LinkedList list = new LinkedList<>();
    assertFalse(list.remove(3));
  }
}
```

## Exercise 2
<img src=images/structural-testing-1.png>

## Exercise 3
Option __1__ is false. They require the same number of tests.

## Exercise 4
``` java
class LinkedListTest {

  @Test
  void removeNullWhenPresent() {
    LinkedList list = new LinkedList<>();
    list.add(2);
    list.add(null);
    assertTrue(list.remove(null));
  }  
  
  @Test
  void removeNullWhenNotPresent() {
    LinkedList list = new LinkedList<>();
    list.add(2);
    assertFalse(list.remove(null));
  } 

  @Test
  void removeObjectWhenPresent() {
    LinkedList list = new LinkedList<>();
    list.add(3);
    assertTrue(list.remove(3));
  }

  @Test
  void removeObjectWhenNotPresent() {
    LinkedList list = new LinkedList<>();
    list.add(2);
    assertFalse(list.remove(3));
  }
}
```

## Exercise 5
For A we have only a single pair:
* {2, 6}  
For B we have pairs:
* {1, 3}
* {2, 4}
* {5, 7}
For C we have again a single pair:
* {5, 6}  
To achieve MC/DC coverage we should strive to have 3+1=4 tests. Let's take {2, 5, 6, 7} then A is tested with {2, 6}, B is tested with {5, 7} and C is tested with {5, 6}.

## Exercise 6

## Exercise 7
Example of tests giving 100% line coverage include `abba`

## Exercise 8
Option __4__ is not correct, 100% path coverage cannot be achieved for a code with a loop.

## Exercise 9
After splitting the condition in line 1 into atomic conditions `n % 3 == 0` and `n % 5 == 0` we get 8 edges in the control-flow graph in total. T1 covers only 2 out of 8 edges making the condition from line 1 true. T2 makes all conditions false covering further 3 edges. In total the condition coverage of those two tests is 5/8 = 62.5%.  
For decision coverage we have 6 edges to cover. T1 covers only 1 edge making the method return in method 2. T2 covers further 3 edges. In total we have 4/6 = 66.6% decision coverage.

## Exercise 10
<img src=images/structural-testing-2.png>

## Exercise 11
To achieve 100% line coverage we need __at least 3__ tests. First tests covers lines 1 and 2, second test covers lines 1, 3-10, 14, 15 and the final test covers lines 1, 3-13.

## Exercise 12
To achieve 100% branch coverage we need __at least 4__ tests. First test returns `true` for condition `rf == null`, second test returns `false` for condition `e != null && oldValue != null`, third test returns `false` for condition `v == null` and finally last test returns `true` for the same condition.

## Exercise 13
Basic condition coverage does not subsume branch coverage (__4__).

## Exercise 14
A test suite satisfies the _boundary adequacy criterion_ if for every loop L test cases iterate L zero times, once, and more than once (__1__)

## Exercise 15
((A and B) or C)
  |     A     |     B     |     C     |   result    |
  |----------:|----------:|----------:|------------:|
  |     T     |     T     |     T     |      T      |
  |     T     |     T     |     F     |      T      |
  |     T     |     F     |     T     |      T      |
  |     T     |     F     |     F     |      F      |
  |     F     |     T     |     T     |      T      |
  |     F     |     T     |     F     |      F      |
  |     F     |     F     |     T     |      T      |
  |     F     |     F     |     F     |      F      |

For A we have:
* {2, 6}  
For B we have:
* {2, 4}
For C we have: 
* {3, 4}, {5, 6} {7, 8}  
For MC/DC coverage we should strive for 4 tests so a possible test suite is {2, 3, 4, 6} or {2, 4, 5, 6}.

## Exercise 16
A and (A or B)
  |     A     |     B     |   result    |
  |----------:|----------:|------------:|
  |     T     |     T     |      T      |
  |     T     |     F     |      T      |
  |     F     |     T     |      F      |
  |     F     |     F     |      F      |
For A we have:
* {1, 3}, {2, 4}
For B we don't have any possibilities, the expression is independent of B therefore it is impossible to achieve 100% MC/DC coverage. The expression could (and should) be simplified to `A`.
