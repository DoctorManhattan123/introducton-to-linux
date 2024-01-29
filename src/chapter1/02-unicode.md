# Unicode

In the previous chapter, we explored how numbers used by humans are represented inside a computer and how to 
programmatically convert from the computer's binary representation to the decimal system that we are all familiar with. 
However, you're likely reading this text not as a series of numbers, but as meaningful text. So, 
how do we go from numbers to text?

To bridge this gap, we need a system that assigns a unique number to each character. In the early days of computing, 
systems like ASCII (American Standard Code for Information Interchange) were developed, but they were limited to 
representing a small set of characters, which was primarily sufficient for English text.

## ASCII

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
representation.

ASCII was very popular back in the time, where the world of programming was small. You only had to represent english
texts, and no emojis or anything of this kind. But as the need for computing expanded globally, 
the limitations of ASCII and similar systems became apparent.
Different languages, with their unique characters and symbols, could not be adequately represented. 
This limitation led to the development of Unicode, a universal character encoding system.

## Unicode

So what is Unicode?

A Unicode code point is an abstract number and doesn't directly specify how that number is stored in a 
computer's memory. To address this, Unicode defines several encoding forms:

* UTF-8
* UTF-16
* UTF-16

### UTF-8

UTF-8 (8-bit Unicode Transformation Format) is a variable-length encoding that can use 1 to 4 bytes for each 
character. It's designed to be backward compatible with ASCII, which means that the first 128 characters in 
Unicode (which correspond to standard ASCII characters) are represented in UTF-8 using exactly the same 
single byte, making it efficient for English and other Latin-based languages.

Examples:

* Character `A` (U+0041): In UTF-8, `A` is represented as 41 in hexadecimal, which is the same as its 
ASCII representation.
* Character ``€`` (Euro Sign, U+20AC): This character is beyond the ASCII range and is encoded in 
UTF-8 as `E2 82 AC` in hexadecimal, using three bytes.

### UTF-16

UTF-16 (16-bit Unicode Transformation Format) uses 2 bytes for most characters but extends to 4 bytes for characters outside the Basic Multilingual Plane (BMP). It is more efficient than UTF-8 for scripts that require a large number of non-Latin characters, such as many Asian scripts.

* Character `A` (U+0041): In UTF-16, `A` is represented as 00 41 in hexadecimal, using two bytes.
* Character ``𐐷`` (Deseret Capital Letter Ew, U+10437): This character is in a supplementary plane 
and is encoded in UTF-16 as a surrogate pair D801 DC37 in hexadecimal, using four bytes.

It might be that the second character in this list looks weird. The simple reason for that is that you are probably
reading this on a browser and the default encoding of the browser is `utf-8`. Therefore due to this character being `utf-16`,
`utf-8` does not know how to represent this character and instead shows a placeholder with the unicode
number represented in the decimal system.

Lets have a small look at the intrinsics of utf-16, namely planes and surrogate pairs:

### Unicode planes

In Unicode, the entire set of code points is divided into 17 planes, each consisting of 65,536 characters 
(or $ 2^{16} $ code points). These planes are indexed from 0 to 16.

* Basic Multilingual Plane (BMP): Plane 0, ranging from U+0000 to U+FFFF, contains the most commonly used 
characters, including almost all modern scripts, punctuation, and many symbols. Most of the characters that 
you encounter in everyday text are in the BMP.

* Supplementary Planes: Planes 1 to 16, ranging from U+010000 to U+10FFFF, are known as supplementary planes. 
They include less commonly used characters, historic scripts, various symbols, and emojis. For example, Plane 1 
is the Supplementary Multilingual Plane (SMP), which includes ancient scripts and emoji characters.

### Surrogate Pairs

In UTF-16 encoding, characters outside the Basic Multilingual Plane (BMP), which are in the range of U+10000 
to U+10FFFF, require a special encoding mechanism known as surrogate pairs. A surrogate pair consists of two 
16-bit code units: a high surrogate and a low surrogate.

Surrogate Ranges

* High Surrogates: Range from U+D800 to U+DBFF (1,024 possible values).
* Low Surrogates: Range from U+DC00 to U+DFFF (1,024 possible values).

Encoding Process

To encode a character outside the BMP using surrogate pairs, the following steps are taken:

* Subtract 0x10000 from the character's Unicode code point. This aligns the code point to the start of the supplementary planes.
* Represent the result as a 20-bit binary number.
* Split this 20-bit number into two parts: the high 10 bits and the low 10 bits.
* Add the high 10 bits to 0xD800 to get the high surrogate.
* Add the low 10 bits to 0xDC00 to get the low surrogate.

Example: Encoding '𐐷' (U+10437)

Let's encode the character '𐐷' (Deseret Capital Letter Ew, U+10437) using surrogate pairs in UTF-16.

* Subtract 0x10000 from U+10437:
U+10437 - U+10000 = U+00437

* Represent U+00437 as a 20-bit binary number:
U+00437 in binary is 0000 0100 0011 0111.

* Split into high and low 10 bits:
High 10 bits: 0000 0100 00 (binary) = 0x0040 (hexadecimal)
Low 10 bits: 11 0111 (binary) = 0x0037 (hexadecimal)

* Calculate the high surrogate:
0xD800 + 0x0040 = 0xD840

* Calculate the low surrogate:
0xDC00 + 0x0037 = 0xDC37

Therefore, the UTF-16 encoding of '𐐷' (U+10437) is the surrogate pair 0xD840 0xDC37.

Now try it yourself, encode ``😊`` (U+1F60A) using surrogate pairs.

_You can do it!_

The result should be `0xD83D 0xDE0A`.

### UTF-32

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


