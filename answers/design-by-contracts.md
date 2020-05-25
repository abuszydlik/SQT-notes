# Design by contracts answers
## Exercise 1
Only `board != null` can be turned into a _class_ invariant because it requires a variable with class-level scope. `board` is a class variable.

## Exercise 2
After removing `assert result != null` we can't be sure that the property is maintained based only on pre-conditions. The squares inside the board assigned in `Square result = board[x][y]` can be null. To ensure that `result != null` we could write an explicit null-check or add an assertion to the board constructor checking whether no square is null.

## Exercise 3
The concern is justified. Method `void resetSize(int width, int height)` overridden in class Square has its pre-condition `assert width == height` stronger than the pre-condition of parent class. This breaks the rule _require no more, ensure no less_ and _Liskov Substitution Principle_.

## Exercise 4
If the class is used correctly then the invariant should always stay true. Because this is not the case we should suppose that there exists and underlying problem in external library, repairing this fault is probably impossible.

## Exercise 5
Errors 4xx mean that the client did not adhere to the contract which corresponds to breaking a pre-condition. Errors 5xx mean that the server wasn't able to do its tasks which is similar to breaking a post-condition.

## Exercise 6
Answer __1__ is correct, the rule says _require no more, ensure no less_

## Exercise 7
Answer __4__ is correct, one of the reasons to use assertions is facilitating the debugging process.
