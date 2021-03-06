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

## Exercise 8
For condition C1 we have:
 * {TTT, FTT}
 * {FTF, TTF}
 * {FFF, TFF}
 * {FFT, TFT}  
 
 For condition C2 we have:
 * {TTT, TFT}
 * {TFF, TTF}  
 
 For condition C3 we have:
 * {TTT, TTF}
 * {FTF, FTT}
 * {FFF, FFT}
 * {TFF, TFT}
 
In this case we actually need 6 tests because there are 6 possible outcomes of the method.

## Exercise 9
<img src=images/model-based-4.png>

## Exercise 10
<img src=images/model-based-5.png>

## Exercise 11
<img src=images/model-based-6.png>

## Exercise 12
<img src=images/model-based-9.png>

## Exercise 13
<img src=images/model-based-8.png>

## Exercise 14

|              |       |       |       |       |       |       |       |       |          
|-------------:|------:|------:|------:|------:|------:|------:|------:|------:|
|  JPG format  |  T    |  T    |  T    |  T    |  F    |  F    |  F    |  F    |
|less than 20MB|  T    |  T    |  F    |  F    |  T    |  T    |  F    |  F    |
|   high res   |  T    |  F    |  T    |  F    |  T    |  F    |  T    |  F    |
|              |success|success|failure|failure|failure|failure|failure|failure|  

## Exercise 15

|                        |              |              |              |
|-----------------------:|-------------:|-------------:|-------------:|
|active in past two weeks|       T      |       T      |       T      |    
|got add during last hour|       F      |       F      |       F      |
|has over 1000 followers |       T      |       F      |       F      |
|highly-relevant for user|       T      |       T      |       F      |
|serve add?              |       T      |       T      |       T      |  
