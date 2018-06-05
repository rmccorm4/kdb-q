# KDB-Q
This is a repository for learning some fundamental concepts of Q for KDB

# Lesson 2: Q Console, Types, and Lists

32bit int = 42i
64bit int = 42 = default

Division in Q = % -> always gives float result

Adding lists
1 2 3 + 10 20 30 = 11 22 33
1 2 3 + 10 = 11 12 13

# Lesson 3: Operators and Operator Precedence

count = len() in python

count 1 2 3 = 3

scalar in Q is called "atom"
vector in Q is a list

*In Q, all operators have the same precedence*
* Everything done from right to left

`2*3+4 = 2 * (3+4) = 2*7 = 14`

til = range()
til 3 = 0 1 2

til 100 = 0 1 2 ... 99
count til 100 = 100

1 + til 20 = 1 2 3 ... 20
2 * til 20 = 0 2 4 ... 38
1 + 2 * til 20 = 1 3 5 ... 39

# Lesson 4: Booleans and Temporal Data Types

False = 0b
True = 1b

List of booleans has no spaces = 101b
* True False True

Compare bools
42 = 6*7
* 1b -> True

1 2 3 = 10 2 30
* 010b -> False True False

Comparison is a vector operator, good for indexing

2018.01.01 = January 1st 2018
* YYYY.MM.DD

Date and Time is equal to days since millenium and time since midnight
* 2000.01.01=0
	* 1b -> True
* 1999.12.31 = -1
	* 1b -> True

2000.02.01 - 2000.01.01
* 31i -> 31 days between january and february, only needs 32bits so it uses 'i'

2000.01.01 + til 31
* First 31 dates starting from January 1st 2000

2000.01m
* Month of January 2000
* == Count of months since millenium

2000.01m = 0
* 1b -> True

2000.02m = 1
* 1b -> True
	* February is one month after milennium

1999.12m = -1
* 1b -> True

2000.01.01 = 2000.01m
* 1b -> True

/ Lesson 5: Casting and Date Operators
#/ Casting
`long$1.0
* 1

`float$1
* 1f

`boolean$1
* 1b

`date$31
* 2000.02.01

2018.01.01 + til 31

2018.01.01 + til 365

2000.01m + til 12

15 + 2000.01m + til 3


# Lesson 6: Operations on Lists

## Summation across list (/ operator) starting at 0 (reduction)
```
0 +/ 10 20 30 40 50
* 150

(+/) 10 20 30 40 50
* 150

(*/) 1 2 3 4 5
* 120

(*/) 1 + til 5
* 120

```

| operator returns max of 2 elements
& operator returns min of 2 elements

```
4|5
* 5

10|2
* 10

(|/) 10 20 30 40 50
* 50 -> Gets max of list

(&/) 10 20 30 40 50
* 10 -> Gets min of list

sum 10 20 30 40 50
min 10 20 30 40 50
max 10 20 30 40 50

0b|1b ends up same as OR
* 1b

0b&1b ends up same as AND
* 0b

(+/) 10 20 30 40 50 = sum
* 150
(+\) 10 20 30 40 50 = Running sum 
* 10 30 60 100 150

sums 10 20 30 40 50 = running sum
* 10 30 60 100 150
```

# Lesson 7: Defining Functions

```
{[x]x*x} -> basically a lambda function
[x] = argument
{} = function
x*x = return value

{[x]x*x}5 = 25
{[x]x*x}[5] = 25
{x*x}5 = 25
```

# Lesson 8: Function Examples

## Newton Raphson method
Computes 0's of a polynomial

**f(x)=2-x^2 -> solution is sqrt(2)**

Project guess onto x-axis

**x_n - f(x_n)/f'(x_n)**

Simplified by calculating derivative of f(x) ourselves

`{[xn] xn + (2 - xn*xn) % (2*xn)} / [1.0]`
* / means iterate function over values starting from 1.0

`{[xn] xn + (2 - xn*xn) % (2*xn)} \[1.0]`
* Show each step (running values)

Q has system command to set precision (\P 16) means set precision
to 16 decimal places

```
\P 16
{[xn] xn + (2 - xn*xn) % (2*xn)} \[1.0]
* 1 1.5 1.41666666667 1.41421568627451 1.41421356.... -> sqrt(2)
```

## \/ Operator
Retrieves certain number of elements from list
* Positive number retrieves from start
* Negative number retrieves from end

```
2 # 10 20 30
* 10 20

-2 # 10 20 30
* 20 30
```

## , operator (concatenation)

```
10 20, 100 200
* 10 20 100 200

{-2#x} 1 1
* 1 1
{sum -2#x} 1 1
* 2

{x, sum -2#x} 1 1
* 1 1 2
```

### Fibonacci

```
{x, sum -2#x} / [10; 1 1]
* Calculate 10 fibonacci numbers starting from 1 1 (actually gives 12 numbers, 10 after first 2)
* 1 1 2 3 5 8 13 21 34 55 89 144
```

/ Lesson 9: Function Example, Variables
deltas -> running difference operator

```
deltas 10 20 30 40 50
* 10 10 10 10 10

deltas 110 120 130 140 150
* 110 10 10 10 10 (Implicit difference with first item of 0)

deltas sums -> returns original list
sums deltas -> returns original list

deltas sums 10 20 30 40 50
* 10 20 30 40 50
```

#/ Variables
assignment operator is :

```
a:42
a
* 42

a:98.6
a
* 98.6

buys:2 1 4 3 5 4
sell:12
```

Want to get 2 1 4 3 2 0 (Take from buys until out of sell)

```
sums 2 1 4 3 2 0
2 3 7 10 12 12

sums buys
2 3 7 10 15 19

sell & sums buys
2 3 7 10 12 12

deltas sell & sums buys
2 1 4 3 2 0
```

# Lessons 10: Tables

Tables are **collections of columns**, not rows like SQL or other databases.
* All operations are column-wise operations, which are vector/list operations

## Table Syntax
Easy to construct columns then put them into a table

### Trades Table Example

```
/ Add 10mil random numbers(days) between 0 and 31
dates:2018.01.01+10000000?31
count dates
10000000

/ Generate 10mil random times between 00:00:00 and 23:59:59.9999
times:10000000?24:00:00.0000

/ Generate random quantities in batches of 100 (between 100 and 10000), adding 1 to avoid 0's 
qtys:100*(1+(10000000?100))

qtys
4300 4000 5700 9100 8000 2400 500 1900 2500 7300 2500 7200 1200 3700 5800 310..
idxs:10000000?3
                                ^
/ Starting prices for aapl, amzn, and googl in jan 2018
172.0 1189.0 1073.0

/ Apply numbers to associated indexes
172.0 1189.0 1073.0 idxs
1073 172 1189 1073 1073 1189 172 1073 172 1189 1189 172 172 1073 1189 1073 17..

/ Apply symbols to associated indexes (0=aapl, 1=amzn, 2=googl)
symbols:`aapl`amzn`googl idxs

/ Stretch the prices from 0 to 3%
prices:(1+10000000?0.03) * 172.0 1189.0 1073.0 idxs

prices
1080.679 172.5521 1211.08 1104.971 1097.452 1193.656 175.9138 1087.95 172.029..

/ Create table with columns
t:([] date:dates; time:times; symbol:symbols; qty:qtys; price:prices) 
t
date       time         symbol qty  price   
--------------------------------------------
2018.01.13 02:55:49.666 googl  4300 1080.679
2018.01.09 10:26:51.904 aapl   4000 172.5521
2018.01.11 14:48:07.061 amzn   5700 1211.08 
2018.01.02 12:11:27.794 googl  9100 1104.971
2018.01.27 00:23:39.872 googl  8000 1097.452
2018.01.10 02:05:15.502 amzn   2400 1193.656
2018.01.27 19:17:12.619 aapl   500  175.9138
2018.01.12 06:25:05.298 googl  1900 1087.95 
2018.01.27 06:53:42.222 aapl   2500 172.0294
2018.01.06 12:02:02.283 amzn   7300 1212.974
2018.01.21 08:22:01.131 amzn   2500 1189.415
2018.01.30 03:43:37.124 aapl   7200 175.4103
2018.01.23 05:34:36.103 aapl   1200 174.0633
2018.01.07 14:22:25.651 googl  3700 1094.575
2018.01.02 02:29:19.775 amzn   5800 1203.75 
2018.01.25 17:48:05.755 googl  3100 1104.639
2018.01.06 22:55:43.291 aapl   9700 175.852 
2018.01.27 04:36:03.784 googl  8900 1077.025
2018.01.05 15:52:41.303 amzn   9600 1213.053
2018.01.14 19:44:30.940 aapl   3000 176.5086
..

/ xasc operator sorts table
/ Sort table by date and then by time
t:`date`time xasc t  

/ # is like `head` in bash, 5#t gives first 5 lines
5#t
date       time         symbol qty  price   
--------------------------------------------
2018.01.01 00:00:01.129 aapl   8800 174.7627
2018.01.01 00:00:02.099 amzn   8000 1198.197
2018.01.01 00:00:02.670 aapl   1400 175.7161
2018.01.01 00:00:02.885 amzn   9300 1223.15 
2018.01.01 00:00:03.396 aapl   7600 175.518 

/ Sort by symbol just to see how it looks
t2:`symbol`date`time xasc t
t2
date       time         symbol qty  price   
--------------------------------------------
2018.01.01 00:00:01.129 aapl   8800 174.7627
2018.01.01 00:00:02.670 aapl   1400 175.7161
2018.01.01 00:00:03.396 aapl   7600 175.518 
2018.01.01 00:00:04.772 aapl   2300 174.5059
2018.01.01 00:00:05.190 aapl   5400 176.4733
2018.01.01 00:00:05.254 aapl   4000 175.3628
2018.01.01 00:00:05.263 aapl   1200 175.1635
2018.01.01 00:00:08.121 aapl   7100 174.1475
2018.01.01 00:00:10.048 aapl   4800 174.1638
2018.01.01 00:00:10.226 aapl   6000 173.114 
2018.01.01 00:00:10.536 aapl   4300 172.0891
2018.01.01 00:00:10.994 aapl   4400 175.6005
2018.01.01 00:00:11.018 aapl   6700 173.9291
2018.01.01 00:00:12.494 aapl   3500 175.4076
2018.01.01 00:00:13.978 aapl   5400 173.9431
2018.01.01 00:00:14.013 aapl   7700 175.9006
2018.01.01 00:00:14.505 aapl   4600 174.185 
2018.01.01 00:00:14.843 aapl   1400 176.3245
2018.01.01 00:00:14.900 aapl   3300 176.2433
2018.01.01 00:00:15.417 aapl   1700 172.7038
```
