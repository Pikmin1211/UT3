# UNIT Blocks

These are unit groups and are the main way to spawn in units on the map.
It should look like this:

```text
SomeUnitGroup: // just one unit here
UNIT charID classID leaderID Level(level,allegiance,autolevel) [x,y] flags 0x0 numberOfREDAs pointerToREDAs [item1, item2, item3, item4] [ai1, ai2, ai3, ai4]
UNIT
```
UNIT must be placed at the end of every unit block to indicate that it has ended. 
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
Vanilla FE8 AI settings are as follows (from the AI documentation thread on FEU):

AI1: Primary AI Byte - The first action a unit will take, if possible.
CHAI changes a unit's AI: In the case of 0xD, this would cause that unit
to charge if their leader is in range of an enemy.

0x00 = Action 100%
0x01 = Action 80%, end turn without moving/acting 20%
0x02 = Action 50%, end turn without moving/acting 50%
0x03 = Action without moving 100%
0x04 = Action without moving 80%, end turn without moving/acting 20%
0x05 = Action without moving 50%, end turn without moving/acting 50%
0x06 = Do Nothing
0x07 = Do not attack character 0xD (Natasha, character at 5A8A00)
0x08 = Do not attack character 0xFC (character at 5A8B3C)
0x09 = Do not attack character 0x0 (character at 5A817C)
0x0A = Only attack character ??? if deployed. (currently 00 01, set it at 5A8BA4)
0x0B = Same as 0x0
0x0C = Attack if within Mov/2+Range(?)
0x0D = CHAI [0x0, 0x0] if the unit's leader has foe in range.(?)
0x0E = Heal allies under 50% HP(?)
0x0F = Alternate between 0xE and 0x3
0x10 = Pick Locks/Steal, then CHAI [0x6, 0xC] (Escape)
0x11 = Pick Locks/Steal
0x12 = Do not attack character ??? (points to 00 01 00 01)
0x13 = Do not attack character ??? (points to 00 01 00 00)
0x14 = Try to use Nightmare (but not on turn one), then try to Summon Units, then act like 0x0 (AttackInRange)

AI2: Secondary AI Byte - The second action
a unit will take, if their first is unavailable.

0x00 = Move towards opponents. If blocked, do nothing
0x01 = Move towards opponents, but not character(s) 0x0 (5A817A)
0x02 = Move towards opponents, but not character(s) 0x0 (5A817C)
0x03 = Do nothing
0x04 = Loot villages/open chests, then change AI2 to 0x0
0x05 = Loot villages/open chests, then change AI2 to 0xC
0x06 = If could reach opponents in two turns, change AI2 to 0x0
0x07 = If could reach opponents in two turns, change AI2 to 0x1
0x08 = Do nothing
0x09 = Random movement
0x0A = Move to character 0x1 Eirika if not in range, or move to opponents if so
0x0B = Move to character 0xF Ephraim if not in range, or move to opponents if so
0x0C = Move to escape point and escape; if cannot escape, do nothing
0x0D = Move on to nearest terrain 0x1B/0x1F (Throne/Fence) (0x5A8182)
0x0E = Attack walls until no more remain(?), then CHAI [0x0,0x0]
0x0F = Move towards opponents. Move as close as possible when blocked.
0x10 = If not in area [13,15]-[18,19], move to [15,17]; if in area, CHAI [0x0,0x0]
0x11 = Wait one turn, then change AI2 to 0x4 (raid then attack)
0x12 = Wait one turn, then change AI2 to 0x0 (Pursue)

AI3: Healing AI Byte - Dictates when a unit will enter recovery mode.
This will cause them to run away from enemies and use any healing
restoration items available (Vulneraries, Elixirs, etc). This also
allows decides whether an allied staff user will heal them or not
(Fortify users need 3 or more injured allies to heal in this case).
Units in recovery mode will also retreat to any Fort tile to heal.


0x00 = Enter recovery mode when HP is less than 50%. Exit when HP is 100%
0x01 = Enter recovery mode when HP is less than 30%. Exit when HP is 80% or greater
0x02 = Enter recovery mode when HP is less than 10%. Exit when HP is 50% or greater
0x03 = Enter recovery mode when HP is less than 80%. Exit when HP is 100%
0x04 = Unable to enter recovery mode

AI4 only has one setting that is nonzero, 0x20. This gives the unit the
"action without moving" AI as seen in the AI1 list. It is commonly used with a
patch to display that an enemy or NPC is stationary. 

 