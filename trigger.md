# Trigger

*The materials in this repository were not produced by its owner.*
> https://web.archive.org/web/20170102142211/http://yiap.nfshost.com/esoteric/trigger/trigger.html


## Computational model

I don't know much about different computational models, so can't really tell what model this is. But what you probably want to hear is "Is this Turing-complete or not?". This is clearly not a Turing-complete language, as you can most probably notice when you read this document. This was never meant to be Turing-complete, either. But I'm sure you'll still find this quite enjoyable if you're a friend of esolangs.

## Etymology

The name Trigger comes from a word "trigger" that is meant to indicate a toggle. This language is based on toggles/triggers, as you can soon find out.

## History

Trigger was created 26.7.2005. I noticed at #esoteric that some guy called Gs30ng had an interesting language idea called Switch. However, there were some things that I didn't like in that language. As well, there were some things I wanted see differently and what Gs30ng wanted to do differently (for example he wanted to make his language Turing-complete, and so on), so I decided to make my own language. Without Gs30ng's excellent work I would've never made up Trigger. Or at least not for many years. Thanks! :) Gs30ng continued his work on his language Udage (formerly Switch).

## Language description

Trigger is a limited programming language. It's not Turing-complete and doing many things can get easily complicated and tricky. But well, that's part of the fun. This language is pretty much an experiment language; purposefully left limited to see what kind of interesting programs people can code up with this. Remember to send me your programs!

You can have 256 memory places in Trigger. Each of them is a bit that can have either value 0 or 1. Every trigger is initially set to 0.

Initially the instruction pointer is 0. Instruction pointer moves only right. When instruction pointer is larger than the size of the program execution of the program ends. Programs itself can be as large as the interpreter can handle.

The language has no specific instruction characters like esolangs usually. Trigger works by matching patterns (rows of same character in this case) and doing different things depending on the length of the instruction pattern.

Instruction patterns are treated as one command. The interpreter must slice the program code to find the right commands. To see what I mean, read on.

Trigger uses UNIX new-line (dec 10).

Here are the instruction patterns that Trigger searches and executes. There's four commands. One for changing the values in memory, one for doing jumps (and loops), one for output, and one for input. First I thought to leave the input part out, but then I thought that it's good to have input ability to get more interesting programs made and so on. You can use any ASCII values you like. 

### One-character pattern

This pattern consist only of one character. This performs the NOT operation (0 changes to 1, 1 changes to 0) to the trigger in question. For example the program

`B`

NOTs the value of 'B' trigger. Since all the triggers are set to zero initially, 'B' trigger gets value 1.

### Two-character pattern

This pattern consist of two same characters **and** one different character after them. If the two characters are the very last two characters of the source file, then the interpreter should just quit the program (which is logical IMHO).
For example in the program

`AAB`

two same characters "AA" launch a conditional jump command that takes the next character "B" as argument and works the following way:

- If the value of the character that is there twice ('A' in this example) is 1, the interpreter starts to search for the nearest character, that is the same than the argument character ('B' in this example), from left and right. If it finds the trigger/character, the instruction pointer moves there (it's left or right, doesn't matter). However, if there's the character on both sides (left and right) equally far from the instruction sequence, then the interpreter selects randomly whether it jumps to left or to right. In case there's no character on either side, the interpreter does nothing, and just moves to the next instruction sequence.
- If the value of the character that is there twice ('A' in this example) is 0, then the interpreter does nothing and just continues to the next instruction sequence.

To note, whole instruction pattern (like "AAB" in this example) is ignored when searching (otherwise the argument would be always found). Thus, the following program

`b aab b`

causes the interpreter to randomly select whether go to the left 'b' or the right 'b', since both are equally 'far' away from the instruction pattern.
**Note:** notice that the pattern where the jump lands, will be executed! In the following program

`BA AAB B`

the 'B' has value 0 after the program is executed. When the program starts, 'B' gets value 1, and when the program jumps to the last 'B', then the 'B' trigger gets its value flipped to 0.
**Second note:** notice that the value of 'B' isn't changed during the program "AAB".

### Three-character pattern

This pattern consist of three same characters. The character they all are, will be printed out once as an ASCII character. For example the following program
fff
will print out the ASCII character 'f'. Three UNIX new-lines in a row will print out a new-line. The value of 'f' won't change during that program (see below).

### Four-character pattern

This pattern consist of four same characters. The following program

`((((`

will read the next bit from the input channel and set it to '(' trigger's value. 
The input in Trigger is non-interactive. You can point out the file you want the interpreter to read the input from. If you haven't selected any file, the interpreter will return EOF all the time when asked for input. EOF means "no change" in Trigger. That is, the value of the trigger stays as it is. As well, if you have selected a file for input and you get to the end of that file, then the interpreter returns EOF every time when input bit is requested. 
If there's some input file selected, and there some data, Trigger interpreter handles the input the following way. First take the first character of the file, then change it to binary (where 'W' becomes 01010111), then return it bit by bit whenever a input request is made. The first input request would get the value 0 to the trigger, the second 1, the third 0 etc.. After all eight bits are done, then take the new character (if more input requested). Files are read from left to right, from top to bottom.
When interpreter is reading the code, it must slice the code into instruction patterns. As well, only one instruction pattern is executed, not the parts forming it. For example, program

`zzz`

would **not** be executed this way: NOT(z) NOT(z) NOT(z) OUTPUT('z'). It would be executed as this: OUTPUT('z'). Same goes for the following program

`22222`

It **wouldn't** be executed as: NOT(2) NOT(2) NOT(2) OUTPUT('2') NOT(2) INPUT(2) NOT(2). It would be executed just as: INPUT(2) NOT(2). Code is executed from left to right, and instruction patterns formed from left to right.

## Interpreter

Big thanks to int-e for the interpreter! Download it [here](trigger.c).

## Credits

Thanks to the following people:
Gs30ng: Switch/Udage (where this language evolved from), comments
int-e: discussion, ideas, comments, couple of very useful code snippets, the interpreter
Author

Trigger was made by Keymaker (partly (see above)).
