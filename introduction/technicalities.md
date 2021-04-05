# Technicalities

## Little Endian



## Pointers, pointerization and dereferencing \(depointerization\)



## Image palletes



## Defining vs Labelling

When using Event Assembler, there are two kinds of named values you work with: **Definitions** and **Labels**. Definitions are declared using `#define`, while labels are declared with a colon after their name. Labels represent variable locations in your ROM, which is especially useful when the amount of data before them and thus their actual location in the output ROM isn't necessarily always going to be the same. 

Labels and definitions are two distinct things, and as such will not be treated the same in certain situations. Whereas defining the same thing twice will throw a redefinition warning, **making a definition with the same name as a label will supercede the label.** Similarly, anything that checks definitions (`#ifdef`, `#ifndef`, `IsDefined()`, etc.) does **not** work with labels.

One workaround for this is to define something as the label and check if that definition exists (e.g. table repointed to label `NewMyTable` then doing `#define MyTable NewMyTable` and checking if `MyTable` is defined), but even still it's a good idea to keep this restriction in mind lest it cause problems for you down the line.
