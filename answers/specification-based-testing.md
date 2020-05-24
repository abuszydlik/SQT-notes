# Specification-based testing answers

## Exercise 1
__Equivalence partition__ is a group of inputs that all make the method behave in the same way (3).

## Exercise 2
Via equivalence partitioning we have:
1. Inputs not divisible by either 3 or 5: T3
2. Inputs divisible by 3 and not divisible by 5: T4
3. Inputs divisible by 5 and not divisible by 3: T5
4. Inputs divisible by both 3 and 5: T1, T2  
We need at least one test in each partition therefore we can only get rid of either T1 or T2.

## Exercise 3
Via category/partition method we have:
1. Two parameters - key K, value V  
2. Value does not matter for the program, there are several partitions for the key:
* key not in the map
* key already in the map
* key is null
3. There are no additional constraints from description
4. We end up with 3 partitions:
* new key with any value
* replaced key with any value
* null key with any value

## Exercise 4
Invalid partitions include:
* [Integer.MIN_VALUE, 1000)
* (4000, Integer.MAX_VALUE]
* [AA, BZ]
* [NA, ZZ]
* [CCA, MMA]
* [C, M]

## Exercise 5
Via category/partition method we have:
1. One parameter - element E
2. There are several partitions for the element:
* element not in the HashSet
* element already in the HashSet
* element is null
3. There are no additional constraints from description
4. We end up with 3 partitions:
* new element
* replaced element
* null element

## Exercise 6
Answer __4__ is false. Category/partition method is a way of _black-box testing_ meaning that the internal implementation isn't required to derive test cases.

## Exercise 7
There are several ways to reduce the number of combinations:
* empty pattern, improperly quoted pattern, no file with the given name and ommited filename are exceptional so they can be tested only once  
* it is impossible for the pattern to occur in a line if there are no occurences in the file so we can put an additional constraint there

## Exercise 8
Test cases derived when taking parameters and internal state into account include:
* T1 - set is full, parameter is not already present
* T2 - set is not full, parameter is not already present
* T3 - set is not full, parameter is already present
* T4 - set is not full, parameter is null
We treat `isFull` returning `true` as exceptional therefore reducing the number of combinations from 6 to 4.
