# Making Event Macros

After seeing examples of what raws and event codes look like,
you may be thinking that they seem a little illegible or long
to type out. Probably the most common example is that of a
simple text event, which requires 4 lines at the very least:

TEXTSTART
TEXTSHOW text
TEXTEND
REMA

Thankfully, there is a solution to both of these problems.
Using #define, here's an example of a macro that is both
clearer and shorter that's provided with Event Assembler.

#define Text(text) "TEXTSTART; TEXTSHOW text; TEXTEND; REMA"

The semi-colons indicate a new line, so as you can see,
this is functionally identical to the original text
event example. Now, instead of typing out all of that,
Text(text) will do the exact same thing. This is a very
simple example, but these become very useful the longer
the sequence of event codes is. 