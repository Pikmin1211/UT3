# Traps

Traps are a broader umbrella for a changeable part of the map. Walls, snags, and tile changes
all fall under this section. A good number of them are made automatically though, so you'll
only need to keep track of the following:

```text
BLST [XX,YY] type // ballista

FIRE [x,y] startTurn repeatTimer // repeating fire trap

GAST [x,y] direction startTurn repeatTimer // repeating gas trap

ARROW [x,y] startTurn repeatTimer // repeating arrow trap

FIRE2 [x,y] // instant fire trap

MINE [x,y] // instant mine

EGG [x,y] startTurn level // gorgon egg
```
[x,y] 

type indicates whether the ballista is normal (0x34), iron (0x35), or killer (0x36).
repeatTimer is how many turns it takes for a trap to trigger again. level is the
level of a hatched Gorgon from the trap.

The macros used for these are:

```text
NormalBallista(XX,YY)

IronBallista(XX,YY)

KillerBallista(XX,YY)

FireTrap(XX,YY,startTurn,repeatTimer)

GasTrap(XX,YY,direction,startTurn,repeatTimer)

PoisonTrap(XX,YY,direction,startTurn,repeatTimer)

ArrowTrap(XX,YY,startTurn,repeatTimer)

InstantFireTrap(XX,YY)

MineTrap(XX,YY)

GorgonEggTrap(XX,YY,startTurn,level)
```
Something to note is that TrapData ends with ENDTRAP instead of END_MAIN like the other categories.

```text
Traps1:
ENDTRAP
ALIGN 4
```