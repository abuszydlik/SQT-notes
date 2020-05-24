# Boundary testing answers
## Exercise 1
For the condition `i < half` we have:
* on-point `i = half`
* off-point `i = half-1`
* in-points `i in [0, half)`
* out-points `i in [half, Integer.MAX_VALUE]`

## Exercise 2
For the condition `x == 10` we have:
* on-point `x = 10`
* off-points `x = 9` and `x = 11`

## Exercise 3
For the condition `numberOfPoints <= 570` we have:
* on-point `x = 570`
* off-point `x = 571`
* example of in-point `x = 420`
* example of out-point `x = 600`

## Exercise 4
Answer __3__ is correct. For an inequality there exists only one off-point, it may or may not be false.

## Exercise 5
For the condition `numberOfPoints` > 1024` we have:
* on-point `x = 1024`
* off-point `x = 1025`
* in-points `x in (1024, Integer.MAX_VALUE]`
* out-points `x in [0, 1024]`
Considering that this is a game we assume that negative points are impossible.

## Exercise 6
For the condition `n % 3 == 0` we have:
* on-point `n = 3k`
* off-points `n = 3k + 1` and `n = 3k - 1`
For the condition `n % 5 == 0` we have:
* on-point `n = 5l`
* off-points `n = 5k + 1` and `n = 5k - 1`

## Exercise 7
Answer __4__ is indeed true. We should check what happens if something that is expected to exist doesn't.

## Exercise 8
|   condition   |   on-point   |   off-point   |    in-points   |   out-points    |
|---------------|:-------------|:--------------|:---------------|:---------------:|
|#legs is even  | 2l for some l|  2l+1 or 2l-1 | 2, 4, 6, 8...  | 1, 3, 5, 7...   | 
|has tail       |     true     |     false     |      true      |      false      |
|#lives in [0,9]| 0, 1, ..., 9 |   -1 or 10    |  0, 1, ..., 9  | l < 0 and l > 9 |
|has sharp nails|     true     |     false     |      true      |      false      |
|"miauws"       |     miauw    |      woof     |      miauw     |   woof, neigh   |
Category/partition method:
1. Parameters include:
* number of legs
* presence of tail
* number of lives left
* presence of sharp nails
* sound produced  
2. Characteristics of each parameter:
* int legs [0, 2, 4, 6, ..., Integer.MAX_VALUE], [1, 3, 5, 7, ..., Integer.MAX_VALUE-1], [negative number]
* boolean tail [`true`], [`false`]
* int lives [0, 1, ..., 9], [10, 11, ..., Integer.MAX_VALUE], [negative number]
* boolean nails[`true`], [`false`]
* string sound [miauw], [woof], [neigh]
3. Constraints:
* numberOfLegs <= 0 is exceptional
* lives <= 0 is exceptional
* lives > 10 can be only one combination
4. Test cases:
* __Cat__:
1. [2, true, 5, true, miauw]
* __Not a cat__:
1. [3, true, 7, true, miauw]
2. [6, false, 6, true, miauw]
3. [4, true, 11, true, miauw]
4. [8, true, 4, false, miauw]
5. [2, true, 3, true, woof]
* __Exceptional__:
1. [-2, true, 5, true, miauw]
2. [2, true, -1, true, miauw]
