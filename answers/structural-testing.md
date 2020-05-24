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
```

## Exercise 2
<img src=images/structural-testing-1.png>
