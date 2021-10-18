# Using Event Assembler

Event Assembler is a very powerful tool capable of performing a lot of different operations to your ROM. In this section I'll be going over EA commands you will commonly use while building.

## Writing in Hex - BYTE, SHORT, WORD, POIN

So far, we've used **BYTE** to write a single byte at the current offset. While useful, data is stored as more than just singular bytes, so we can use the other commands for those purposes. A byte has a numeric value from **0x0** to **0xFF** \(or **0 to 255**\). A short \(or _halfword_\) consists of **2 bytes**, and thus has values of **0x0** to **0xFFFF** \(**0 to 65535**\), usually used when we need to store something of **a larger value than a byte** can handle. We can use the code **SHORT** to write shorts just like we use byte. Due note that shorts are written in _little endian_ - more on this later, but for now just know **SHORT 0x1122** is equivalent to **BYTE 0x22 0x11**. Similarly, a word is **4 bytes** long, values **0x0** to **0xFFFFFFFF** \(**0 to 4294967295**\). Obviously that's _incredibly_ large and there is little data that requires a word over a short. The main use of words is to refer to **ROM addresses** using pointers. More on the significance of that later, but importantly we can write a pointer using **POIN** and a word using **WORD**. Again, values are in little endian, so **WORD 0x11223344** is equivalent to **BYTE 0x44 0x33 0x22 0x11**.

Also of note is that we can use decimal values while using these codes as well! If you need the decimal value 18 and don't want to have to convert it to hex, you can safely use **BYTE 18** \(and not **BYTE 0x18**, since that's for 0x18 in hex\).

## Changing the Offset - ORG, PUSH, POP

As we know from before, we can use **ORG** to change our current offset. This is, of course, very useful, but there is a small problem - _what if we want to go back to our original address?_ Well, that's where **PUSH** and **POP** come in. When we use **PUSH**, we save our current address to a list _\(the stack\)_. When we use **POP**, we go back to the last address added, and remove it from the list. You can think of it like a pile of books. When we push, we place our book on the top of the pile. When we pop, we pick up the book on top.

```text
ORG 0x100
PUSH
ORG 0x200
BYTE 0x0
POP
BYTE 0x1
```

Here, we write **0x0** at **0x200**, and then write **0x1** at **0x100**, since the current offset of **0x100** was saved with **PUSH** and recalled with **POP**. We can layer **PUSH**es and **POP**s as much as we like, though remember to always have an equal amount!

```text
ORG 0x100
PUSH
ORG 0x200
PUSH
ORG 0x300
BYTE 0x2
POP
BYTE 0x1
POP
BYTE 0x0
```

## \#define

Define is by far **one of your most powerful assets**. So what does it do? Well, there are many ROM locations of interest to us - the table for class data, for example. There are also values of interest, such as IDs for characters in the character table, and so on. Obviously, it would be tiresome to have to refer to a list for this sort of thing every time we needed it. Instead of memorizing these things, we can instead use definitions to be able to **refer to them as whatever we like!**

A definition allows us to **alias a sequence of characters as something else**. What that means is that I can make a string of text refer to some value. For example, if I wanted to refer to the Knight class, instead of manually finding the ID for the class each time, I can define the word "Knight" as the ID for the class. Then, in any situation I need the ID, **I can just write "Knight" instead!**

For example:

```text
#define Knight 0x9
```

And with this, for example, instead of writing **BYTE 0x9**, we can use **BYTE Knight** instead.

Now, of course, having to manually belt out descriptions for everything on the ROM sounds like quite a chore. Luckily for us, **it's already been done**! In your EA folder, you should see a folder called _"EA Standard Library"_ \(otherwise known as _eastlib_\). In this standard library, many things already exist **predefined**, so we don't have to go through the trouble.

## \#include

Up until now, everything we've written has been within ROM Buildfile.event. For just messing around that's been fine, but once we have more substantial changes, it would become rather cumbersome - this one file would have to write out every single change we want to make! Imagine how much trouble it would be skimming through this massive file to find something specific. Luckily for us, we have better means of organizing ourselves. Introducing - **\#include**!

So, what does include do? Simply put, it allows us to **read another event file**. What this means is that EA will pause on reading the current file, read the entirety of the given file, and then go back. This way, we can **divide up our work into multiple files**! Let's give it a try. Delete the contents of _ROM Buildfile_ and write in this instead -

```text
#include "NewFile.event"
```

Now, create that file _NewFile.event_ and add some code to it, and compile - you'll notice that it still writes the changes like before! We can use as many includes as we like, and we can even **include files from included files** - it's a powerful tool to keep our work organized, so use it liberally!

