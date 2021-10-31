# Text

Text editing is another of the most basic operations to make.&#x20;

## How Text is Stored

The majority of text is referred to using the **Text Table**. As such, like other tables, each string of text has its own entry in the table, consisting of a pointer to that text data, and as such each string of text will have its own corresponding **Text ID**. The contents of each entry is entirely arbitrary so there is no real separation of text based on purpose. The best way to know the original use of each string is through context. Unlike some other tables, TextIDs use a **SHORT** and not a BYTE. As you may remember, this means that the available range of TextIDs is two BYTE digits and as such, very large (**0 to 65535**). In the vanilla game, text is compressed using [Huffman Encoding](https://en.wikipedia.org/wiki/Huffman\_coding), a lossless compression method.
