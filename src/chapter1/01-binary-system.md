# The Binary and Hexadecimal System

If you are asking yourself, why we need this system, the answer is very simple: "We (_humans_) do not need it".
But the way modern computers are built, this system is the way how computers actually represent all of the data internally.

You are already familiar with one kind of a number system, namely the decimal number system. It is the number system,
you have used your entire life. But what makes it the decimal (base-10) system and what is the difference to the
binary system or other number systems like the hexadecimal system (base-16). The only difference between those
systems is the base of the system. The base also defined the number of digits. For example, the decimal system 
has the base 10 and therefore the following 10 digits:

0,1,2,3,4,5,6,7,8,9

With those digits you create any number by doing some combination of those digits and place them
next to each other. 

So how does this system actually work? You calculate the value of a number of any given number system 
via the following formula.

$$ V = \sum_{i=0}^{n-1} a_{i} \cdot b^i $$

In this formula:

* $ V $ represents the value of the number.
* $ n $ is the number of digits in the number (for example the number `1023` has 4 digits).
* $ a_{i} $ is the digit in the i-th position from the right (0-indexed), for example given the number `1928`, $a_{1}$ would be `2` and $a_{2}$ would be `9`.
* $ b $ is the base of the number system.
* The summation $ \sum_{i=0}^{n-1} $ calculates the total value by multiplying each digit by the 
base raised to the power of its position and then summing up these products.

Lets calculate the value for the number `7203`.

Now we just apply the formula to this example number:

$$ \sum_{i=0}^{n-1} a_{i} \cdot b^i $$
$$ b^0 \cdot 3 + b^1 \cdot 0 + b^2 \cdot 2 + b^1 \cdot 7 = $$
And as our base of the system is `10`, we substitute $ b $ with the number `10`:
$$ 10^0 \cdot 3 + 10^1 \cdot 0 + 10^2 \cdot 2 + 10^1 \cdot 7 = $$
$$ 1 \cdot 3 + 10 \cdot 0 + 100 \cdot 2 + 1000 \cdot 7 = 7203 $$


This is a good moment to stop for a second and calculate the value of another number via this general definition.
Take the number `18374` for example and calculate the value via this formula. 

_I give you some time to do this_

_Are you already done?_

If you did this calculation, you will come to the _surprising_ solution, that this number `base-10` 
is the same number in `base-10`.
You might be disappointed right now, but actual usecase of this formula is to convert
number from other number systems into the `base-10` system.

So lets take a number in another system. To not get confused while we work with numbers
from different number systems, we will write the base of the system in which the number 
is represented as the footer of the number.

* $ 10_{(2)} $ is the number `10` inside the `base-2` (binary) system
* $ 10_{(10)} $ is the number `10` inside the `base-10` (decimal) system

So lets take the formula from before and take the number $ 101011_{(2)} $ and convert it to a decimal number. The only
thing we need to do is to apply the formula from above.

$$ \sum_{i=0}^{n-1} a_{i} \cdot b^i $$
$$ = b^0 \cdot 1 + b^1 \cdot 1 + b^2 \cdot 0 + b^3 \cdot 1 + b^5 \cdot 0 + b^5 \cdot 1 $$
And as our base of the system is `2`, we substitute $ b $ with the number `2`:
$$ = 2^0 \cdot 1 + 2^1 \cdot 1 + 2^2 \cdot 0 + 2^3 \cdot 1 + 2^5 \cdot 0 + 2^5 \cdot 1 $$
$$ = 1 \cdot 1 + 2 \cdot 1 + 4 \cdot 0 + 8 \cdot 1 + 16 \cdot 0 + 32 \cdot 1 $$
$$ = 43_{(10)} $$

Now we have the following equation given:

$$ 101011_{2} = 43_{10} $$

Now it is good time to calculate for yourself, take the following three binary number and convert them into a decimal number:

* $ 11111_{2} $
* $ 1000_{2} $
* $ 10101_{2} $

_I wait for you to finish_

You should have come to the following solutions:

* $ 11111_{2} = 31_{10} $
* $ 1000_{2} = 8_{10} $
* $ 10101_{2} = 21_{10} $

And this is how computers, that can only save the state 1 (for signal on) and the state 0 (for signal off) can still represent
every possible number (_well not really every number because the computer has only finite state, but good enough for now_).
In the next subchapter we will also cover how the computer can represent text given these numbers.

> As an exercise, try to figure out properties of a binary number, for example if the number consists of only ones,
is there an easy way to calculate the number? Or how do I know by looking at a binary number whether it is even or odd?

As you might have already guessed the binary system is not really human readable, especially because it needs much more
space than the corresponding decimal number. Therefore if you want to look at the actual binary data inside a computer
you will get the representation via the hexadecimal `base-16` system. As we do not have 16 digits, mathematics invented
6 new digits with the following symbols and corresponding values:

* `A` = 10
* `B` = 11
* `C` = 12
* `D` = 13
* `E` = 14
* `F` = 15

and the rest of the values is the same as the values from `base-10`.

Lets now again calculate the `base-10` representation, but now from a hexadecimal number.

$$ \mathrm{5fa8_{16}} = $$
$$ = b^0 \cdot 8 + b^1 \cdot a + b^2 \cdot f + b^3 \cdot 5 $$
Note that `a` and `f` are not variables but actually represent the number `10` and the number `15` respectively:
$$ = b^0 \cdot 8 + b^1 \cdot 10 + b^2 \cdot 15 + b^3 \cdot 5 $$
And as our base of the system is `16`, we substitute $ b $ with the number `16`:
$$ = 16^0 \cdot 8 + 16^1 \cdot 10 + 16^2 \cdot 15 + 16^3 \cdot 5 $$
$$ = 1 \cdot 8 + 16 \cdot 10 + 256 \cdot 15 + 4096 \cdot 5 $$
$$ = 24488_{10} $$

Now it is good time to calculate for yourself, take the following three binary number and convert them into a decimal number:

* $ aaa_{16} $
* $ 111_{16} $
* $ f7a_{16} $

_I wait for you to finish_

You should have come to the following solutions:

* $ aaa_{16} = 2730_{10} $
* $ 111_{16} = 273_{10} $
* $ f7a_{16} = 3962_{10} $

We have introduces the most common number systems:

* `base-10` - well you already knew this one
* `base-2` - this is how computers actually represent all of the data
* `base-16` - a smaller and more readable representation of the binary system 

The are numerous other number systems like the `base-8` number system, also called `octal` system (which is sometimes used). 
And if you think about it, via the method from above you can construct various other systems like `base-6` or `base-11`, 
but those do not have real pratical applications.
