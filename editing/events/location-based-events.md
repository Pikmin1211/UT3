# Location Based Events

Location based events set a map object to the coordinates that are provided.
This results in a command appearing in the unit menu, on these tiles.
Shop, door, and chest events all are done in this way. They look like this:

```text
LOCA flagID offset [xpos,ypos] objectType
```
[xpos,ypos] is just whatever tile you want this event to occur on.
The list of object types you can use are:

0x10 - House
0x11 - Seize
0x12 - Door
0x14 - Chest
0x16 - Armory
0x17 - Vendor
0x18 - Secret Shop
0x20 - Village Center

House and Seize events are straightforward. If you set a House event,
then a unit can Visit on that tile, which leads to whatever offset
you've provided. Village centers indicate where they are in order
to make village destruction AI work. It doesn't take an offset.
The Village macro creates both a House and Village Center event.

Seize tiles allow the corresponding Unit Menu entry
to be used. Macros for these are: 

```text
Seize(seizeX,seizeY)
Seize(eventID,offset,seizeX,seizeY)  
Village(flagID,eventOffset,villX,villY)
House(flagID,eventOffset,villX,villY)
```

Note that the first Seize macro will automatically set the win flag (0x3)
and go to EndingScene. The second will take a different offset, so if
you don't want Seize to clear the map, you should use that one.



Armories, vendors, and secret shops all take shop lists as their offset.
A shop list should look something like this:

```text
SomeArmory:
	SHLI IronSword SteelSword SlimSword LightBrand IronLance 
	BYTE 0x0 0x0
	ALIGN 4
```
You can have more or less items than this, of course, but 21 is the hard limit
(above that, the game will glitch). BYTE 0x0 0x0 is the terminator, and all
shop lists require an ALIGN 4 at the end (otherwise, bad things may happen).

```text
Armory(shopListOffset,shopX,shopY)
Vendor(shopListOffset,shopX,shopY)
SecretShop(shopListOffset,shopX,shopY)
```

Door and chest events also have their own type of events they can use with no offset or flag.
Their macros are as follows:

```text
Chest(item,chestX,chestY)
ChestMoney(amountOfMoney,chestX,villY)  
Door(doorX,doorY)                      
```

One small note is that amountOfMoney must be greater than 255; otherwise, the Chest will give
you an item instead, as it still takes the itemID as a parameter.

These macros are generally fine for many use cases, but if you want the door or chest event
to trigger an event after (say, a unit popping out of a chest), then they won't work.
To illustrate, let's show an event where opening a chest gives a unit instead:

```text
LocationBasedEvents:
LOCA 7 UnitPopsOutOfChest [7,4] 0x14
END_MAIN

UnitPopsOutOfChest:
LOAD1 1 ChestUnitGroup // if this looks unfamiliar, check the unit block section
ENUN
NoFade
ENDA
```





