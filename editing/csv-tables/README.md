# Tables

In the FE8 ROM, a lot of data is stored in tables or lists. If you have ever used _Nightmare_ before, you will be familiar with these. For the uninitiated, **a table is a series of data that is indexed** - that is, there are **rows of IDs and columns of data** for each. Of course, these rows and columns are only theoretical - the data is continuous in the ROM. Editing tables is **one of the most basic edits** you can make and thus we will begin our journey here.

## What Kind of Data is in Tables?

The best examples of tables would be the **Character table, Class table and Item table.** As you may guess from the names, these tables house the data for characters, classes and items in the game. This is where you would go to change things like stat numbers. There are also tables for graphics, like the Portrait and Map sprite tables, as well as the table for text entries. **Any kind of "static" data in the game is likely housed in a table.** It makes it possible for the game to get the values it needs during runtime.  


## Changing Table Values Manually

Let's use a famous example - changing the movement of the Troubadour class - to familiarize ourselves with how to edit table data. Firstly, we need to know where the class table actually is. Luckily for us, we do, since **the class table is at 0x807110.** If you're unsure, my recommendation is to open FEBuilder on a clean ROM and see. You can use the "hex address" block on any editor to see where that table is. Next, we need to know the index of the Troubadour. Both FEBuilder and eastlib can tell us this information - **the Troubadour is class 0x4B.** Now, this is where we need to do a bit of math. The table starts at 0x807110, but we need to get to the 0x4Bth index. In order to get there, we need to do **0x807110 + \(0x4B \* the length of one entry\).** The reasoning for this is quite simple - every + length of one entry, we advance one entry. So to advance 0x4B entries, we need to move forward one entry's length 0x4B times. But how do we know the length of one entry? Once again, FEBuilder can tell us - the "size" display will show us that. Here, it's 84 \(that's in decimal\). So now we have **0x807110 + \(0x4B \* 84\)** as the start of the troubadour's data. Great! But we wanted to edit the movement stat. Now, instead of advancing through different rows in the table, we will advance through the columns of this row. That is, instead of changing entries, we will change the value within this entry instead. We can once again use FEBuilder to see the offset for the move stat within the entry - the "selection" bar will update when we highlight it. In this case, move is **+0x12.** So finally, we have **ORG 0x807110 + \(0x4B \* 84\) + 0x12**. This will bring us to the offset of the movement stat for the troubadour. As you may remember, we can use **BYTE** to write a byte of data, so let's do that!

```text
PUSH
ORG 0x807110 + (0x4B * 84) + 0x12
BYTE 7
POP
```

And there we go! This block of code will change the Troubadour's move to seven. This is handy for making changes on the fly, but a bit less practical if this is a table we will be making a lot of edits to. Luckily, there are tools to help us with those types of tables.



