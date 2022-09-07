# Technicalities

## Little Endian

When a computer loads or stores data, it does so 1 byte at a time. The location it stores to or loads from is always representative of a specific byte's location, regardless of the length of the data it is storing or loading. Because the computer only has 1 address and reads from it a byte at a time, when reading any value more than 1 byte in length it is equally efficient to read the most significant byte first (the "big end") as it is to read the least significant byte first (the "little end") so long as the bytes remain in the correct order. This means that a hexadecimal value `0x12345678` stored to memory will be written in memory in big endian as `12 34 56 78` but in little endian as `78 56 34 12`.

The Game Boy Advance is a computer that elects to store and load data in little endian. This means that values larger than 1 byte will be stored in memory and in ROM in reverse byte order to what they are read as. Event Assembler codes such as `SHORT`, `WORD`, and `POIN` will account for this for you, so it's mostly only relevant when searching for pointers in ROM as the byte order of a pointerized address will need to be reversed when searching.

## Pointers, pointerization and dereferencing \(depointerization\)

When running, the GBA needs some way to know where different information is stored in order to reference it when needed. To do this, we give its location as a pointer. A pointer is the memory address at which data is located at runtime. When it runs, the GBA maps the contents of the ROM to a section of memory starting at `0x08000000`. In effect, this makes pointers to data in ROM be the offset within the file + `0x08000000`. When we pointerize a value, this is the operation we are performing to it. To Event Assembler, label offsets are relative to the start of the file. Therefore, to get a pointer we add this value to a label. This is unintuitive and inconvenient to do every time, so the code `POIN` will automatically pointerize the value given to it.

## Image palletes

Normally, we would think of an image as a series of pixels with a specific color assigned to each one. However, Game Boy Advance graphics work a bit differently to this. Rather than a specific color assigned to each pixel, an index value is used instead. The colors that the image uses are all stored in one place somewhere else as a palette, and this value is used to index the palette to determine the color to use for that pixel.

One palette contains 16 colors, but the first color is always treated as transparency. Any 8x8 tile can only use one 16-color palette in total, and there is a fairly strict limit on the number of palettes available for use at any given time for any given graphics, though it varies from case to case. For palette-indexed images, .png files work fine, but we need software that can properly handle palette-indexed images, such as [Usenti](http://www.coranac.com/projects/usenti/). Note also that due to this separation of graphics and palette, no palette is tied to any specific graphics and you can have multiple combinations of the same graphics and different palettes that produce valid images.

## Defining vs Labelling

When using Event Assembler, there are two kinds of named values you work with: **Definitions** and **Labels**. Definitions are declared using `#define`, while labels are declared with a colon after their name. Labels represent variable locations in your ROM, which is especially useful when the amount of data before them and thus their actual location in the output ROM isn't necessarily always going to be the same.

Labels and definitions are two distinct things, and as such will not be treated the same in certain situations. Whereas defining the same thing twice will throw a redefinition warning, **making a definition with the same name as a label will supercede the label.** Similarly, anything that checks definitions \(`#ifdef`, `#ifndef`, `IsDefined()`, etc.\) does **not** work with labels.

One workaround for this is to define something as the label and check if that definition exists \(e.g. table repointed to label `NewMyTable` then doing `#define MyTable NewMyTable` and checking if `MyTable` is defined\), but even still it's a good idea to keep this restriction in mind lest it cause problems for you down the line.

