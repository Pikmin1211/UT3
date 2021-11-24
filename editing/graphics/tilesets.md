# Tilesets

There are three parts to a tileset that form the maps consisting of 32x32 tiles that we know and love.

-The tile object: This consists of a grid of 8x8 tiles that form together to make the more familiar 32x32 tiles that we commonly see in maps.
-The palette: Tilesets have ten palettes. Commonly, five are used for regular use, while the other five are used for fog/darkness.
-The tile config: This tells the ROM what 8x8 tiles to use to form the bigger tiles, and what palettes they should use. (Fact check that palette statement)

## How the game finds them

You might have heard of the pointer list (or Plist for short). Every entry inside is a pointer to different things ranging from tilesets to map changes. Right now we only care about tilesets, palettes, tile configs, and tile animations.

Whenever a chapter is loaded, it refers to the Plist for the many things it will need using specific IDs (eg. many chapters check ID 0x1 for the fields tile object).

## Put into practice

The tileset objects in FE8 are lz77 compressed, so we'll need to do that. Dumping the palette with png2dmp, and compressing the tile config are also easy enough. Of course they should each be 4 aligned, and have labels.
In comes the tricky part: pointing to them with the Plist.

It's actually a simple org to the table slot, and pointing to your labels.

```
PUSH

ORG Plist+(ID * 4)
POIN label

POP
```
    
Event Assembler already has a macro for that thankfully.

```
#define EventPointerTable(id,offset) "PUSH; ORG EventPointerListOffset+(4*id); POIN offset; POP"
```

## Tile animations

After installing the tilesets, you might want to add some animations as well. This is the *actual* tricky part.
Tile animations work on a frame by frame basis, and are based in a list. That list consists of entries 8 bytes long. A short for how long the animation frame will last, a short for how large the animation frame is, and a word used as a pointer to the actual animation frame.
The list terminator consists of two WORDs worth of 0 (0x0000000000000000).

NOTE: vanilla uses 0x1C as an animation speed, so using more than a byte for the animation speed will make the frame last a very long time

Tile animations are also listed in the Plist. The Plist points to the animation list, and the animation list points to the animation frames along with how they should be displayed.

## Now to insert them

The animation frames are not lz77 compressed, and once you get them inserted, you'll need them to each be labeled. (Don't forget to ALIGN 4)

A new (ALIGN 4-ed) label would be needed to mark your new animation list. After that, you'll need to `SHORT` the speed, `SHORT` the size, and `POIN` to the animation frame. This process is used for each frame of animation. Once the list is over, two words consisting of 0s will terninate the list.

Are you noticing repetitious and hard to parse work? try using a macro to help make more sense of what you're doing!

My glorious one frame animation as an example:
```
ALIGN 4
FieldsAnimation0:
#incbin "Fields Animations/Frame_000.dmp"

ALIGN 4
FieldsAnimationLabel:
tilesetAnimation(0x1C,0x1000,FieldsAnimation0) //Macro used for simplification
WORD 0 0
```

## Now what?

Now you can put these tilesets to use. You can use the IDs that your tilesets are using in your chapter data table, or right on your tmx map file.
