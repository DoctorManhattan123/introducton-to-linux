# Number Systems and Character Encodings

Welcome to the first chapter of our journey into the world of Linux.

In this chapter, we'll dive into the fundamental concepts of number systems and character
encodings — a crucial foundation for everything you will encounter in the world of computers.

We begin by exploring two essential number systems: binary and hexadecimal.
These systems are the backbone of computer operations, with binary (base-2) being the way how
almost every computer represents data nowadays and hexadecimal (base-16)
serving as a more human-friendly way to represent binary data.

Next, you will be introduced to the world of character encodings, starting with Unicode.
We'll also cover ASCII and UTF-8, two widely used character encodings.

This chapter aims to provide you with a clear and concise understanding of these fundamental concepts,
setting the stage for the more advanced topics that follow in the book.

## The Binary and Hexadecimal System

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

- $ V $ represents the value of the number.
- $ n $ is the number of digits in the number (for example the number `1023` has 4 digits).
- $ a_{i} $ is the digit in the i-th position from the right (0-indexed), 
for example given the number `1928`, $a_{1}$ would be `2` and $a_{2}$ would be `9`.
- $ b $ is the base of the number system.
- The summation $ \sum\_{i=0}^{n-1} $ calculates the total value by multiplying each digit by the
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

- $ 10_{(2)} $ is the number `10` inside the `base-2` (binary) system
- $ 10_{(10)} $ is the number `10` inside the `base-10` (decimal) system

So lets take the formula from before and take the number $ 101011\_{(2)} $ and convert it to a decimal number. The only
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

- $ 11111_{2} $
- $ 1000_{2} $
- $ 10101_{2} $

_I wait for you to finish_

You should have come to the following solutions:

- $ 11111_{2} = 31_{10} $
- $ 1000_{2} = 8_{10} $
- $ 10101_{2} = 21_{10} $

And this is how computers, that can only save the state 1 (for signal on) and the state 0 (for signal off) can still represent
every possible number (_well not really every number because the computer has only finite state, but good enough for now_).
In the next subchapter we will also cover how the computer can represent text given these numbers.

> As an exercise, try to figure out properties of a binary number, for example if the number consists of only ones,
> is there an easy way to calculate the number? Or how do I know by looking at a binary number whether it is even or odd?

As you might have already guessed the binary system is not really human readable, especially because it needs much more
space than the corresponding decimal number. Therefore if you want to look at the actual binary data inside a computer
you will get the representation via the hexadecimal `base-16` system. As we do not have 16 digits, mathematics invented
6 new digits with the following symbols and corresponding values:

- `A` = 10
- `B` = 11
- `C` = 12
- `D` = 13
- `E` = 14
- `F` = 15

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

- $ aaa_{16} $
- $ 111_{16} $
- $ f7a_{16} $

_I wait for you to finish_

You should have come to the following solutions:

- $ aaa_{16} = 2730_{10} $
- $ 111_{16} = 273_{10} $
- $ f7a_{16} = 3962_{10} $

We have introduces the most common number systems:

- `base-10` - well you already knew this one
- `base-2` - this is how computers actually represent all of the data
- `base-16` - a smaller and more readable representation of the binary system

The are numerous other number systems like the `base-8` number system, also called `octal` system (which is sometimes used).
And if you think about it, via the method from above you can construct various other systems like `base-6` or `base-11`,
but those do not have real pratical applications.

## Unicode

In the previous chapter, we explored how numbers used by humans are represented inside a computer and how to
programmatically convert from the computer's binary representation to the decimal system that we are all familiar with.
However, you're likely reading this text not as a series of numbers, but as meaningful text. So,
how do we go from numbers to text?

To bridge this gap, we need a system that assigns a unique number to each character. In the early days of computing,
systems like ASCII (American Standard Code for Information Interchange) were developed, but they were limited to
representing a small set of characters, which was primarily sufficient for English text.

### ASCII

ASCII example

![](images/ascii-chars-table-landscape.webp)

You see a 1 to 1 mapping of a number to a specific character. As the file is just a sequence of numbers,
as computers can only represent numbers in its memory, if you actually read a file then the computer converts the number
that it has in its memory to the corresponding character.
You also see the hexadecimal representation of the number and we will use it to have a look at an example.

Lets suppose you have a text file with the sentence: `Hello world!`. Try to find the representation of this sentence
in ASCII using hex numbers.

_I'll give you a second_

If you worked carefully you should have come to the following solution:

Hex:

`48 65 6C 6C 6F 2C 20 77 6F 72 6C 64 21`

Binary:

```
01001000 01100101 01101100 01101100 01101111 00101100 00100000 01110111 01101111 01110010 01101100 01100100 00100001
```

> You might see now, why we usually write things in the hexadecimal representation and not in the binary
> representation.

ASCII was very popular back in the time, where the world of programming was small. You only had to represent english
texts, and no emojis or anything of this kind. But as the need for computing expanded globally,
the limitations of ASCII and similar systems became apparent.
Different languages, with their unique characters and symbols, could not be adequately represented.
This limitation led to the development of Unicode, a universal character encoding system.

### Unicode

So what is Unicode?

A Unicode code point is an abstract number and doesn't directly specify how that number is stored in a
computer's memory. To address this, Unicode defines several encoding forms:

- UTF-8
- UTF-16
- UTF-16

#### UTF-8

UTF-8 (8-bit Unicode Transformation Format) is a variable-length encoding that can use 1 to 4 bytes for each
character. It's designed to be backward compatible with ASCII, which means that the first 128 characters in
Unicode (which correspond to standard ASCII characters) are represented in UTF-8 using exactly the same
single byte, making it efficient for English and other Latin-based languages.

Examples:

- Character `A` (U+0041): In UTF-8, `A` is represented as 41 in hexadecimal, which is the same as its
  ASCII representation.
- Character `€` (Euro Sign, U+20AC): This character is beyond the ASCII range and is encoded in
  UTF-8 as `E2 82 AC` in hexadecimal, using three bytes.

#### UTF-16

UTF-16 (16-bit Unicode Transformation Format) uses 2 bytes for most characters but extends to 4 bytes for characters outside the Basic Multilingual Plane (BMP). It is more efficient than UTF-8 for scripts that require a large number of non-Latin characters, such as many Asian scripts.

- Character `A` (U+0041): In UTF-16, `A` is represented as 00 41 in hexadecimal, using two bytes.
- Character `𐐷` (Deseret Capital Letter Ew, U+10437): This character is in a supplementary plane
  and is encoded in UTF-16 as a surrogate pair D801 DC37 in hexadecimal, using four bytes.

It might be that the second character in this list looks weird. The simple reason for that is that you are probably
reading this on a browser and the default encoding of the browser is `utf-8`. Therefore due to this character being `utf-16`,
`utf-8` does not know how to represent this character and instead shows a placeholder with the unicode
number represented in the decimal system.

Lets have a small look at the intrinsics of utf-16, namely planes and surrogate pairs:

#### Unicode planes

In Unicode, the entire set of code points is divided into 17 planes, each consisting of 65,536 characters
(or $ 2^{16} $ code points). These planes are indexed from 0 to 16.

- Basic Multilingual Plane (BMP): Plane 0, ranging from U+0000 to U+FFFF, contains the most commonly used
  characters, including almost all modern scripts, punctuation, and many symbols. Most of the characters that
  you encounter in everyday text are in the BMP.

- Supplementary Planes: Planes 1 to 16, ranging from U+010000 to U+10FFFF, are known as supplementary planes.
  They include less commonly used characters, historic scripts, various symbols, and emojis. For example, Plane 1
  is the Supplementary Multilingual Plane (SMP), which includes ancient scripts and emoji characters.

#### Surrogate Pairs

In UTF-16 encoding, characters outside the Basic Multilingual Plane (BMP), which are in the range of U+10000
to U+10FFFF, require a special encoding mechanism known as surrogate pairs. A surrogate pair consists of two
16-bit code units: a high surrogate and a low surrogate.

Surrogate Ranges

- High Surrogates: Range from U+D800 to U+DBFF (1,024 possible values).
- Low Surrogates: Range from U+DC00 to U+DFFF (1,024 possible values).

Encoding Process

To encode a character outside the BMP using surrogate pairs, the following steps are taken:

- Subtract 0x10000 from the character's Unicode code point. This aligns the code point to the start of the supplementary planes.
- Represent the result as a 20-bit binary number.
- Split this 20-bit number into two parts: the high 10 bits and the low 10 bits.
- Add the high 10 bits to 0xD800 to get the high surrogate.
- Add the low 10 bits to 0xDC00 to get the low surrogate.

Example: Encoding '𐐷' (U+10437)

Let's encode the character '𐐷' (Deseret Capital Letter Ew, U+10437) using surrogate pairs in UTF-16.

- Subtract 0x10000 from U+10437:
  U+10437 - U+10000 = U+00437

- Represent U+00437 as a 20-bit binary number:
  U+00437 in binary is 0000 0100 0011 0111.

- Split into high and low 10 bits:
  High 10 bits: 0000 0100 00 (binary) = 0x0040 (hexadecimal)
  Low 10 bits: 11 0111 (binary) = 0x0037 (hexadecimal)

- Calculate the high surrogate:
  0xD800 + 0x0040 = 0xD840

- Calculate the low surrogate:
  0xDC00 + 0x0037 = 0xDC37

Therefore, the UTF-16 encoding of '𐐷' (U+10437) is the surrogate pair 0xD840 0xDC37.

Now try it yourself, encode `😊` (U+1F60A) using surrogate pairs.

_You can do it!_

The result should be `0xD83D 0xDE0A`.

#### UTF-32

UTF-32 (32-bit Unicode Transformation Format) is a Unicode encoding that uses a fixed length of four bytes
(32 bits) for every character. Unlike UTF-8 and UTF-16, which are variable-length encodings,
UTF-32 has the characteristic of using a consistent length for all characters, making it simpler in terms of
understanding and handling character encoding.

Let's take the character 'A' (U+0041) as an example:

    In UTF-32, 'A' is represented as 00 00 00 41 in hexadecimal. The Unicode code point U+0041 is directly placed in the 32-bit unit, with leading zeros to fill the four bytes.

Another example is the character '😊' (U+1F60A):

    In UTF-32, '😊' is represented as 00 01 F6 0A in hexadecimal. The Unicode code point U+1F60A is directly placed in the 32-bit unit, again with leading zeros.

## Summary

Following things should stay in your mind: Unicode is a universal character encoding standard that assigns
a unique code point to a specific character from various languages and symbol sets, which leads to consistent
text representation across different platforms and programs. `UTF-8` is a variable-length encoding that uses 1 to 4 bytes per character,
highly efficient for the englisch language and backwards compatible with ASCII, it is widely used on the web
due to its space efficiency and flexibility. UTF-16 is also a variable-length encoding that uses 2 bytes for most
characters and 4 bytes for those outside the Basic Multilingual Plane, through surrogate pairs. UTF-32 is a
fixed-length encoding that uses 4 bytes for every character, providing simplicity and constant-time
access to characters but at the cost of higher memory usage.
