# Text

Text editing is another basic edit that can be performed to your FE8 ROM. The majority of our text editing will be done using the handy [TextProcess ](https://feuniverse.us/t/text-processor-for-use-with-ea-v10-1-updated-to-v2-1/1776)tool, which will allow us to compile and insert text using its own **text buildfile**.

## Text Entries

The majority of the in-game text is housed in the **Text Table**. As such, like other tables, each string of text has its own entry in the table and its own corresponding **Text ID**. The contents of each entry is entirely arbitrary so there is no real separation of text based on purpose. The best way to know the original use of each string is through context. Unlike some other tables, TextIDs use a **SHORT** and not a BYTE. As you may remember, this means that the available range of TextIDs is two BYTE digits and as such, very large \(**0 to 65535**\).

Within the text buildfile, there are two ways to determine which TextID will be used for the text being written. The first and most straightforward is a **direct ID**, signified by a **single hashtag**. 

```text
#0x1
This is sample text.[X]
```

As you may have guessed, the number after the hashtag is the TextID to be used. The \[X\] at the end of the entry is a control code - you may want to read the **Control Codes** section, but for now all that you need to know is that \[X\] will end the current text entry. We can also **create a definition** for this TextID by providing a name after it.

```text
#0x1 SampleText
This is sample text.[X]
```

With this, TextProcess will define **SampleText** as **0x1** automatically on compile.

Direct IDs are useful for overwriting TextIDs, such as changing character's names, but it would be cumbersome for large amounts of custom text to have to define many TextIDs manually. This is where the second way to define TextIDs comes into play - the **sequential ID**, signified by a **double hashtag**.

```text
#0x1 SampleText
This is sample text.[X]

## SampleText2
This is more sample text.[X]
```

Unlike a direct ID, which has the TextID provided explicitly, the sequential ID uses an implicit TextID based on **the last used direct ID +1**. As the name suggests, **many sequential IDs in a row will continue to use TextIDs in sequence**. Because of the nature of sequential IDs, they will be useless without a definition provided. It is also because of this nature that it is recommended to **leave all sequential IDs at the end of the text buildfile**, so that they will be uninterrupted by any additional direct IDs. If you are extending the Text Table, it is recommended that your last direct ID be the first entry in the extended area so that all sequential IDs will be in the new free text table space.

## Text Buildfile

Much like the overall ROM buildfile, the text buildfile would be horribly cluttered if every text entry were to be contained within one singular file. Luckily, **the \#include command is fully functional** within the text buildfile, so it is recommended as before to use it liberally to organize your text entries.

Also like your ROM buildfile, **the text buildfile must be recompiled after any changes**. You can compile it by running the TextProcess program, or a batch script which calls it. The entirety of the new text written, as well as all the definitions created from naming text entries, will be included within _"Install Text Data.event",_ so be sure to \#include that file in your buildfile.

