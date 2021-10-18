# Technicalities

## Little Endian

When a computer loads or stores data, it does so 1 byte at a time. The location it stores to or loads from is always representative of a specific byte's location, regardless of the length of the data it is storing or loading. Because the computer only has 1 address and reads from it a byte at a time, when reading any value more than 1 byte in length it is equally efficient to read the most significant byte first (the "big end") as it is to read the least significant byte first (the "little end") so long as the bytes remain in the correct order. This means that a hexadecimal value `0x12345678` stored to memory will be written in memory in big endian as `12 34 56 78` but in little endian as `78 56 34 12`.

The Game Boy Advance is a computer that elects to store and load data in little endian. This means that values larger than 1 byte will be stored in memory and in ROM in reverse byte order to what they are read as. Event Assembler codes such as `SHORT`, `WORD`, and `POIN` will account for this for you, so it's mostly only relevant when searching for pointers in ROM as the byte order of a pointerized address will need to be reversed when searching.

## Pointers, pointerization and dereferencing \(depointerization\)

## Image palletes

## Defining vs Labelling

When using Event Assembler, there are two kinds of named values you work with: **Definitions** and **Labels**. Definitions are declared using `#define`, while labels are declared with a colon after their name. Labels represent variable locations in your ROM, which is especially useful when the amount of data before them and thus their actual location in the output ROM isn't necessarily always going to be the same.

Labels and definitions are two distinct things, and as such will not be treated the same in certain situations. Whereas defining the same thing twice will throw a redefinition warning, **making a definition with the same name as a label will supercede the label.** Similarly, anything that checks definitions \(`#ifdef`, `#ifndef`, `IsDefined()`, etc.\) does **not** work with labels.

One workaround for this is to define something as the label and check if that definition exists \(e.g. table repointed to label `NewMyTable` then doing `#define MyTable NewMyTable` and checking if `MyTable` is defined\), but even still it's a good idea to keep this restriction in mind lest it cause problems for you down the line.

