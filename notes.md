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

# Lesson 5: Casting and Date Operators
## Casting
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

Lesson #7: Defining Functions

{[x]x*x} -> basically a lambda function
[x] = argument
{} = function
x*x = return value

{[x]x*x}5 = 25
{[x]x*x}[5] = 25
{x*x}5 = 25

# Lesson 8: Function Examples

## Newton Raphson method
Computes 0's of a polynomial

f(x)=2-x^2 -> solution is sqrt(2)

Project guess onto x-axis

x_n - f(x_n)/f'(x_n)

Simplified by calculating derivative of f(x) ourselves
{[xn] xn + (2 - xn*xn) % (2*xn)} / [1.0]
* / means iterate function over values starting from 1.0

{[xn] xn + (2 - xn*xn) % (2*xn)} \[1.0]
* Show each step (running values)

Q has system command to set precision (\P 16) means set precision
to 16 decimal places

\P 16
{[xn] xn + (2 - xn*xn) % (2*xn)} \[1.0]
* 1 1.5 1.41666666667 1.41421568627451 1.41421356.... -> sqrt(2)

## \# Operator
Retrieves certain number of elements from list
* Positive number retrieves from start
* Negative number retrieves from end

2 # 10 20 30
* 10 20

-2 # 10 20 30
* 20 30

## , operator (concatenation)
10 20, 100 200
* 10 20 100 200

{-2#x} 1 1
* 1 1
{sum -2#x} 1 1
* 2

{x, sum -2#x} 1 1
* 1 1 2

### Fibonacci
{x, sum -2#x} / [10; 1 1]
* Calculate 10 fibonacci numbers starting from 1 1 (actually gives 12 numbers, 10 after first 2)
* 1 1 2 3 5 8 13 21 34 55 89 144

# Lesson 9: Function Example, Variables
deltas -> running difference operator

deltas 10 20 30 40 50
* 10 10 10 10 10

deltas 110 120 130 140 150
* 110 10 10 10 10 (Implicit difference with first item of 0)

deltas sums -> returns original list
sums deltas -> returns original list

deltas sums 10 20 30 40 50
* 10 20 30 40 50

## Variables
assignment operator is :

a:42
a
* 42

a:98.6
a
* 98.6

buys:2 1 4 3 5 4
sell:12

Want to get 2 1 4 3 2 0 (Take from buys until out of sell)

sums 2 1 4 3 2 0
2 3 7 10 12 12

sums buys
2 3 7 10 15 19

sell & sums buys
2 3 7 10 12 12

deltas sell & sums buys
2 1 4 3 2 0

# Lessons 10: Tables
