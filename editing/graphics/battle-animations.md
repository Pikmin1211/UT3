# Battle Animations

## What you'll need

First, we'll need [Animation Assembler](https://feuniverse.us/t/fe7-8-animation-assembler-convert-feditor-format-for-insertion-with-ea/1880). You'll also need the animation you want to install. The animation folder should have a .bin file, and some sprite sheets.  

If the animation you want to install doesn't have those, but still has the .txt file for scripting, and the animation frames, you can import the animation to FEBuilderGBA, then export as bin.  

You'll be wanting to include the `Master Animation Installer.event` file somewhere in your events as well.  

## Inserting the animation

Drag and drop the animation's .bin file onto AA.exe (Or run it in the command prompt if you prefer), and open the generated Installer.event file. Near the top you'll find `AnimTableEntry(0x0) //CHANGE THIS TO THE SLOT YOU ARE REPLACING`. I think this speaks for itself. (SLOT = Animation ID)  

After that, include the generated Installer.event in your buildfile.  

It's worth noting that quite often the animation .bin files will be named after the weapon type. This means there will likely be a lot of labels with the same name, and that's a no-no.  

One solution is to rename all the labels so they'll be unique, and another is to include the Installer.event files with curly braces `{}` around each of them to make sure the labels aren't global.  

## Battle Animation pointers

Battle animation pointers tell the game what animations to use for what items/weapons. They follow this format:

```
first: <Weapon Type> second: <Type Designation> third(short) <AnimationID> repeat
```

-The first byte can be used for one of two things. The first is the weapon type being used, and the second is a specific item.  

-The second byte, which I'll call the "Type Designation" is for whether you want all items of the same type to share an animation, or have the animation
play for one item only (Commonly used for throwing axes). The options are:
```
1 = all items of that type.
0 = Specific item.
```

-The last two bytes are used for the animation ID you want.

The terminator to end the animation pointer list is two words worth of 0 (eg. `0x00000000`). Remember to label your animation pointers (and `ALIGN 4` them too)  

Here's an example of Ephraim lord's animation pointer:
```
#define AnimPointer(Type,Type_Designation,AnimID) "BYTE Type Type_Designation; SHORT AnimID"

EphriamLordAnimPointer_ByteVer:
BYTE $01 $01 $01 $00 $09 $01 $02 $00
WORD 0 0

EphriamLordAnimPointer_MacroVer:
AnimPointer(Lance,1,EphraimLord_Lance) //Using 1 for all lances
AnimPointer(Unarmed,1,EphraimLord_Unarmed)
WORD 0 0
```

EAstdlib has a host of animation pointer macroes you can use in `EventAssembler/EAStandardLibrary/AnimationSetters.txt`

To have the game use your new animation pointer, the class' animation pointer must be changed. This is easily done in the ClassEditor CSV table. Just change the `Animation Pointer` cell for the class you want and type the name of your animation pointer label. (Remember to put `|IsPointer` at the end)

You can also org to the class table and point to your animation pointer like so:
```
PUSH

ORG ClassTable + (ID * 84) + 52 //bytes 0x34 through 0x38 is where the animation pointer is in the class struct
POIN AnimPointer

POP
```

There's actually a macro for this in `AnimationSetters.txt`:
```
#define SetClassAnimation(ClassID,Pointer) "PUSH; ORG ClassTable+(ClassID*84)+52; POIN Pointer; POP"
```

This is basically what editing the table does, but more laser focused.  
NOTE: if you're using the ClassEditor CSV, this must be done after the table is inserted, or else it will be overwritten  

## Palettes

Everyone having the same color palette is boring, so let's fix that.

Thanks to compression, every battle animation palette is actually five palettes in one (Though only the first four seem to be used).  
The palettes are pointed in a word from bytes 0xC to 0x10 of each entry (The first 12 bytes are for text so humans can identify the palettes)  

NOTE: The following applies to FE8 onlys:  
There are two other important tables. One for palette IDs to use, and the other for what classes will use those palette IDs.  
It's easier to get an idea of this by looking at the palette association tables.

## Making and inserting palettes

First, you'll need a palette string. It should look something like this:
```
//Colm palette
5553FF7FFF6B7C36E728AD76C9614541567B6F5E88412541C95973364B15A514
```

This is pretty easy to get via FE Recolor, FEBuilder, or...FEditor.

Inserting the animation without any extra tools isn't worth it, so [pal2EA](https://feuniverse.us/t/pal2ea-the-buildfile-palette-inserter/2646) will probably be your best bet.

TODO: A small example of using pal2EA (Most of the documentation was just linked, so there's no need to go into very much detail)

So let's do a quick rundown of how this works. Let's make a new file and name it something like `Palettes.event`  

Here, we can type something similar to the following:
```
#char{0x3D} "Colm_Thief" set{Colm, 0x1, Thief}
	5553FF7FFF6B7C36E728AD76C9614541567B6F5E88412541C95973364B15A514
```

We start off our palette entry with a `#`. Then we use `char` to decide what ID the palette will have. In this case, it's `0x3D`  
In between the quotes we're giving the palette a label name, and then `set` assigns a palette to a unit for a particular class. (In this case, Colm(`0x9`) when he's a thief(`0xD`), which is his base class)  
Finally, we have the palette that I mentioned before. pal2EA can compress and insert up to four palettes. One for the unit as a player, enemy, ally, or other unit.  

pal2EA has an option to just copy-paste one palette, and just use it for the other factions (Which is what's done in this example). If you want to see more details on pal2EA, check out its [README](https://github.com/Teraspark/Pal2EA/blob/master/README.md) <!-- No wonder he's named Teraspark. I plugged him twice! -->

Here's a list of what numbers mean what for setting the units' class palettes (See how that 1 matches Colm's base class 1 option?):
```
0 = Trainee Class
1 = Base Class 1
2 = Base Class 2 (for trainees)
3 = Promoted Class 1
4 = Promoted Class 2
5 = Promoted Class 3
6 = Promoted Class 4 (for Ewan)
```

Now you can run pal2EA on your palette file. It should generate `Palette Installer.event` and `Palette Setup.event`. The installer file is the one that should be included.

<!-- Someone else can explain palettes in more detail. My brain hurts. (That someone else is going to be me later isn't it?) -->
