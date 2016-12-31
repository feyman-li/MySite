#Style

What follows is a set of short essays that collectively encourage a philosophy of clarity in programming rather than giving hard rules.

Under no circumstances should you program the way I say to because I say to; program the way you think expresses best what you're trying to accomplish in the program.

Typographic conventions consistently held are important to clear presentation.

Say what you want to say in the program, neatly and consistently.  Then move on. 

This is largely a matter of taste, but taste is relevant to clarity. 

 Procedure names should reflect what they do; function names should reflect what they return.  Functions are used in expressions, often in things like if's, so they need to read appropriately. 
 
        if(checksize(x))

is unhelpful because we can't deduce whether checksize returns true on error or non­error; instead

        if(validsize(x))

makes the point clear and makes a future mistake in using the routine less likely. 

------
#Programming with data
Rule 5.  Data dominates.  If you've chosen the right data structures and organized things well, the algorithms will almost always be self­evident.  Data structures, not algorithms, are central to programming.  (See Brooks p. 102.) 

------
#Function pointers
Another result of the tyranny of Pascal is that beginners don't use function pointers.  (You can't have function­valued variables in Pascal.) Using function pointers to encode complexity has some interesting properties.

Some of the complexity is passed to the routine pointed to.  The routine must obey some standard protocol - it's one of a set of routines invoked identically - but beyond that, what it does is its business alone.  The complexity is distributed.

There is this idea of a protocol, in that all functions used similarly must behave similarly.  This makes for easy documentation, testing, growth and even making the program run distributed over a network - the protocol can be encoded as remote procedure calls.

I argue that clear use of function pointers is the heart of object­oriented programming.  Given a set of operations you want to perform on data, and a set of data types you want to respond to those operations, the easiest way to put the program together is with a group of function pointers for each type.  This, in a nutshell, defines class and method.  The O­O languages give you more of course - prettier syntax, derived types and so on - but conceptually they provide little extra.

Combining data­driven programs with function pointers leads to an astonishingly expressive way of working, a way that, in my experience, has often led to pleasant surprises. Even without a special O­O language, you can get 90% of the benefit for no extra work and be more in control of the result.  I cannot recommend an implementation style more highly.  All the programs I have organized this way have survived comfortably after much development - far better than with less disciplined approaches.  Maybe that's it: the discipline it forces pays off handsomely in the long run. 

------
#Include files
Simple rule: include files should never include include files. 

 There's a little dance involving #ifdef's that can prevent a file being read twice, but it's usually done wrong in practice - the #ifdef's are in the file itself, not the file that includes it.  The result is often thousands of needless lines of code passing through the lexical analyzer, which is (in good compilers) the most expensive phase. 

------
#Comments
A delicate matter, requiring taste and judgement.  I tend to err on the side of eliminating comments, for several reasons.  First, if the code is clear, and uses good type names and variable names, it should explain itself.  Second, comments aren't checked by the compiler, so there is no guarantee they're right, especially after the code is modified.  A misleading comment can be very confusing.  Third, the issue of typography: comments clutter code. 
