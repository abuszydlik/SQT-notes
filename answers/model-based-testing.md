# Model-based testing answers
## Exercise 1
<img src=images/model-based-1.png>

## Exercise 2
<img src=images/model-based-2.png>

## Exercise 3
|           |   turn off    |   turn on   |   temperature reached   |   too cold    |   too hot   |
|----------:|--------------:|------------:|------------------------:|--------------:|------------:|
|   Idle    |      Off      |             |                         |    Warming    |    Cooling  |
|    Off    |               |    Idle     |                         |               |             |
|  Cooling  |               |             |           Idle          |               |             |
|  Warming  |               |             |           Idle          |               |             |

There is 14 sneaky paths.

## Exercise 4
<img src=images/model-based-3.png>

## Exercise 5
In total there is 6 transitions. The given test includes 4 transitions so its coverage is 4/6 = 66.6%.

## Exercise 6
|                |    received   |  cancelled  |         resumed         |   fulfilled   |   deliverd  |
|---------------:|--------------:|------------:|------------------------:|--------------:|------------:|
|    Submitted   |  Processing   |             |                         |               |             |
|   Processing   |               |  Cancelled  |                         |    Shipped    |             |
|    Cancelled   |               |             |        Processing       |               |             |
|     Shipped    |               |             |                         |               |  Completed  |
|    Completed   |               |             |                         |               |             |

## Exercise 7
There are 20 empty cells in the table hence there are 20 sneaky paths.
