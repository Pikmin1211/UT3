# Tilesets

There are three parts to a tileset that form the maps consisting of 32x32 tiles that we know and love.


| File          | Description                                                                                                                   |
|---------------|-------------------------------------------------------------------------------------------------------------------------------|
| Tile object   | This consists of a grid of 8x8 tiles that form together to make the more familiar 32x32 tiles that we commonly see in maps.   |
| Palette       | Tilesets have ten palettes. Commonly, five are used for regular use, while the other five are used for fog/darkness.          |
| Tile config   | This tells the ROM what 8x8 tiles to use to form the bigger tiles, and what palettes they should use.                         |

## How the game finds them

You might have heard of the pointer list (or Plist for short). Every entry inside is a pointer to different things ranging from tilesets to map changes. Right now we only care about tilesets, palettes, tile configs, and tile animations.

Whenever a chapter is loaded, it refers to the Plist for the many things it will need using specific IDs (eg. many chapters check ID 0x1 for the fields tile object).

## Put into practice

### NOTE: We're going to be doing quite a bit of stuff so remember to keep organized by using comments and making new event files when needed.

Tileset objects in FE8 are lz77 compressed, so we'll need to do that with png2dmp. (using the `--lz77` argument)  
For the palette, we'll also need to run png2dmp with the `--palette-only` argument on the full tileset image. (The one with the 32x32 tiles)  
The tile config only needs to have compress run on it.  

(Of course they should each be 4 aligned, and have labels.)

Now for pointing to them in the Plist.

It's actually a simple org to the table slot, and point to your label.

```
PUSH

ORG Plist+(ID * 4)
POIN label

POP
```
    
EAstdlib already has a macro for that thankfully.

```
#define EventPointerTable(id,offset) "PUSH; ORG EventPointerListOffset+(4*id); POIN offset; POP"
```

This will replace whatever pointers were originally in those slots with your new pointers. Said original pointers are likely being used by other things, so keep that in mind.

## Tile animations

After installing the tilesets, you might want to add some animations as well. This is the tricky part.  

Tile animations work on a frame by frame basis, and are sorted in a list. That list consists of entries 8 bytes long. A short for how long the animation frame will last, a short for how large the animation frame is, and a word used as a pointer to the actual animation frame.  
The list terminator consists of two words worth of 0 (0x0000000000000000).

NOTE: Vanilla uses 0x1C as an animation speed, so using more than a byte for the animation speed will make the frame last a very long time

Tile animations are also listed in the Plist. The Plist points to the animation list, and the animation list points to the animation frames along with how they should be displayed.

## Now to insert them

Animaton frames are included the same way as the tile object except they aren't lz77 compressed. Of course once you do include the frames, you'll need to label them (Don't forget ALIGN 4)

A new (ALIGN 4-ed) label will be needed to mark your new animation list.  
After that, you'll need to `SHORT` the speed, `SHORT` the size, and `POIN` the animation frame. This process is used for each frame of animation.  
Once the list is over, two words consisting of 0s will terminate the list.

This is pretty boring and repetitious after the first few times, so here's a macro to make things easier:

```
#define TileAnimFrame(Speed, Size, Pointer) "SHORT Speed Size; POIN Pointer"
```

My glorious one frame animation as an example:
```
ALIGN 4
FieldsAnimation0:
#incbin "Fields Animations/Frame_000.dmp"
//Or
//#incext png2dmp "Fields Animations/Frame_000.png"

ALIGN 4
FullFieldsAnimation:
TileAnimFrame(0x1C,0x1000,FieldsAnimation0)
WORD 0 0
```

## Now what?

Now you can put these tilesets to use. You can use the IDs that your tilesets are using in your chapter data table, or right on your tmx map file.  
(Don't forget to include these files in your buildfile!)
