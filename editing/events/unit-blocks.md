# UNIT Blocks

These are unit groups and are the main way to spawn in units on the map.
It should look like this:

```text
SomeUnitGroup:
UNIT charID classID leaderID Level(level,allegiance,autolevel) [x,y] flags 0x0 numberOfREDAs pointerToREDAs [item1, item2, item3, item4] [ai1, ai2, ai3, ai4]
UNIT
```

charID is the loaded unit's character ID, classID is what class they load as, and leaderID
is the charID of the unit's leader. leaderID is only used for AI, so it does nothing for player units.

level is the level that the unit is loaded in as. allegiance is what side the unit loads in as.
Ally is a player unit, Enemy is an enemy unit, and NPC is a green unit. autolevel determines whether
the unit should autolevel (the number of autolevels they would receive is set in chapter data).
This will be True or False. Unit with the boss flag do not autolevel, regardless of what's entered here.

[x,y] are the unit's coordinates on the map. flags are a combination of bitflags, and the only
relevant ones are DropItem (makes the last item in the unit's inventory drop after being defeated)
and MonsterTemplate (loads the unit in as a random monster unit based off their class). 

numberOfREDAs and pointerToREDAs dictates how the unit moves after loading in. By default, setting
these both to 0 will mean the unit stays where they start. A REDA pointer should look like this:

```text
SomeUnitGroup:
UNIT Eirika EirikaLord 0 Level(1,Ally,False) [1,20] 0 0 1 REDA1R21 [Rapier] NoAI
UNIT

REDA1R21:
REDA [1,21] 0x0 0x0 0x0 0x0
```

In this case, Eirika has 1 REDA. After spawning, she'll move one tile to the right. REDAs are often used to make
reinforcements unable to be blocked, since if they attempt to move to a blocked tile or spawn on one with a REDA,
the unit will try to path to the closest available tile instead of not spawning.

item1, item2, item3, and item4 are all the items you can give to a unit on spawn. A unit cannot load in with five
items. As shown in the REDA example, you can choose not to fill out all four items, as long as there is something
within the brackets. If you want a unit to spawn with nothing, then their items should be /[0x00/].

ai1, ai2, ai3, and ai4 are the four different AI settings a unit can have. 