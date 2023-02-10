# Don't use float to represent currency. Here is why

So you are building an e-commerce system or a personal expense tracker or whatever system that makes calculations using currency values, like calculating the total amount of dollars with taxes included or simply calculating the total price by multiplying. And for that, the most obvious choice for a beginner is to represent money as a float, as integers seem not fit for this task.

## The Problem

I will be using Python in these examples, but most programming languages would yield similar results.

Let's calculate the total price for 3 burgers which costs $1.1 each:

```python
burger_price = 1.1
amount = 3
total = burger_price * amount
# Expected 3.3, but got 3.3000000000000003 :sob:
```

What is happening here? This result would look awful on a shopping cart!

Even a simple addition leads to a strange result:

```python
base_bonus = 0.2
additional_bonus = 0.1
total_bonus = base_bonus + additional_bonus
# Expected 0.3, but got 0.30000000000000004 :(
```

You might be thinking that the solution is really simple, just cut off after two digits!

```python
total_bonus = 0.1 + 0.2
# 0.30000000000000004
corrected_total_bonus = round(total_bonus, 2)
# 0.3
```

That might work for something really simple, but if you keep making these floating point calculations and rounding off repeatedly, small rounding errors will accumulate and lead to a large calculation error. This will not be acceptable for an application that needs precision and also, rounding can lead to errors like this:

```python
party_minimum = 5.0
party_result = 4.97
party_result = round(party_result, 1)
# 5.0
if party_result >= party_minimum:
    print('You can seat in the Parlament')
else:
    print('You cannot seat at Parlament anymore')

# You can seat in the Parlament
```

> In German parliamentary elections, a party with less than 5.0% of the vote cannot be seated. The Greens appeared to have a cliff-hanging 5.0%, until it was discovered (after the results had been announced) that they really had only 4.97%. The printout was to two figures, and the actual percentage was rounded to 5.0%.

Disasters already happened due to float precision:

> On February 25, 1991, during the Gulf War, a Patriot missile defense system let a Scud get through. It hit a barracks, killing 28 people. The problem was in the differencing of floating point numbers obtained by converting and scaling an integer timing register.

You can even fail a simple automated test and spend hours trying to figure out why:

```python
assert 0.1 + 0.2 == 0.3
# Raises AssertionError :fearful:
```

## Why does this happen?

Computers use zeros and ones to represent numbers. A bit can be a zero or a one. Modern computers usually use up to 64 bits to represent information, which means you can have up to 16 digits after the decimal point, which is a lot, but there is still a hard limit on the values that it is possible to represent.

As humans, we can easily understand the concept of `0.1`. It is one tenth of something or `10%` of something. But computers using zeros and ones will represent `0.1` as:

> 0.0001100110011...

In which the 0011 part will be repeating infinitely.

But a 64-bit computer will cut the repeating part, so it will have just 16 digits after the decimal point. In this process of cutting to the allowed precision digits, it loses precision. So `0.1` that was fitted into 64-bit data will be just close enough to `0.1`.

If we reveal the 20 digits that follow the decimal sign in a float:

```python
decimal_one = 0.1
print(f'{decimal_one:.20f}') # 0.10000000000000000555
decimal_two = 0.2
print(f"{decimal_two:.20f}") # 0.20000000000000001110
```

We can see that we are indeed getting only the approximate value of a decimal number. And that is why you can see those unexpected results using floats.

## Solution 1: Use the Decimal module

Python has a module called `Decimal` and it behaves like how a human would do Math:

```python
from decimal import Decimal

burger_price = Decimal('1.1')
amount = 3
total = burger_price * amount # Decimal('3.3')
print(total)
# 3.3
```

Note that it is possible to do Math operations with those Decimal objects:

```python
Decimal('0.1') + Decimal('0.2')
# Decimal('0.3')
```

When creating Decimal objects, the float value should be passed as a string, otherwise, you would suffer again from precision issues that we wanted to avoid:

```python
Decimal(0.1)  # Decimal('0.1000000000000000055511151231257827021181583404541015625')

Decimal('0.1') # Decimal('0.1')
```

When creating Decimal objects, it is possible to customize the level of precision, rounding rules, etc.

## Solution 2: Use integers

It is possible to use integers to store the values and do the calculations. Only when there is a need to show the results for a human it would be divided by 100:

```python
burger_price = 110
amount = 3
total = burger_price * amount # 330
print(total/100)
# 3.3
```

This way will keep us in the safe realm of integers.

The downside is when calculating a number with a large number of digits. Float is represented as scientific notation so it is fast doing calculations, but using integers expect slower speeds.

Hope you find this article useful and happy decimal calculations!