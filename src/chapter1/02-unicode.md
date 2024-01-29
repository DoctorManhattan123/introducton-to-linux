# Unicode

In the previous chapter, we talked how the numbers we are represented inside a computer
and how to programmaticaly go from the computer representation to the decimal system, we all know.
But how is text encoded, how do we go from the a number to a written text you see in a document?
We will cover the most used text encoding standard named unicode in this chapter and utf-8 and ascii
in the next chapter.

Every character you see on the screen and the text that you are probably reading on a screen too,
is a number in the memory of the computer. But at some point the number has to be translated to 
the actual letter (and yes, _emojis_ are text too). But there has to be a standard. The reason for that
is pretty simple, when we transfer data or use some other software, you want to have the same text on every
machine and every application. 

Unicode is probably the most widely used text encoding standard and can encode more than 1.1 million
characters.
Unicode stands for three different things:

* universal (addressing the needs of world languages)
* uniform (fixed-width codes for efficient access), and
* unique (bit sequence has only one interpretation into character codes)
