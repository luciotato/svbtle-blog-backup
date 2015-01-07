#Adding a new dimension to source code. Literally.

###Background:
*I'm working on [LiteScript](https://github.com/luciotato/LiteScript) (beta), a literate, compile-to-js,* ***and compile-to-c*** *language based in Javascript design (the good parts).*

Note: *I could have titled this "Curly braces considered harmful", but titling like that is now considered harmful* 

###Unidimensional source code

Source code for the most popular programming languages is unidimensional. Just a unidimensional stream of unicode points. This is an artifact if you like of the Turing machine model, or, if you prefer, it is related to the fact that programming languages are analyzed with the same tools of *natural* languages,  and all *natural* languages, being "spoken", are constrained by its transmission medium to be a unidimensional stream of sounds and silences. On the other side, computer languages, are rarely "spoken", so they're not naturally constrained to a unidimensional stream. 

Let's talk about C. For the C compiler, source is a stream of chars. A c compiler can compile a 100kb source written on one line without line breaks. (Javascript engines do this thousands of times per minute, when somebody visits a page with a minified source). But nobody writes C (or javascript) programs in a single line without line breaks. Also we don't just add line breaks, we properly indent after "{" and de-indent before "}".

For the compiler, the source code is a *unidimensional* stream of chars. Whitespace and line-breaks are ignored. On the other hand, the programmer sees the source code as a *bidimensional* construction with lines and columns, and withespace and indent are fundamental for code readability.

Proper indent is so important for the programmer that looking at code without proper indentation is like staring a picture frame hanging crooked, and trying to resist fixing it.

This different "vision" between the compiler and the programmer creates a hidden source of big problems. The [Apple SSL Bug](https://blog.codecentric.de/en/2014/02/curly-braces/) this year is directly related to this difference in "parsing" from the compiler and the programmer mind. This kind of bug can only slip because the unidimensional vs. bidimensional mismatch. You can make the same mistake of "pasting twice" the "goto fail;" statement, but if you do it in Python or LiteScript it will be just a extra ignored statement, instead of a catastrophic bug lurking hidden in the indentation. In Python or LiteScript the blocks you see is also what the compiler sees. 

###A new dimension in the compiler adds benefits

I believe that a choosing to use indentation for blocks, aka the [Off-side rule](http://en.wikipedia.org/wiki/Off-side_rule) is not only a "style" choice for a modern language, it helps to avoid a large source of subtle, potentially highly problematic bugs.
