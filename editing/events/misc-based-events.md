# Misc Based Events

The broadest and most flexible type of events,
MiscBasedEvents come in two categories:

```text

AFEV flagID eventOffset activationFlag

AREA flagID eventOffset [corner1x, corner1y] [corner2x, corner2y]
```

AFEVs will run whenever the activationFlag is set, presuming their flagID isn't already set.
AREA events will run whenever a unit steps in the zone described by its coordinates.
corner1 is the top left corner, and corner2 is the bottom right corner.

The common macros used for these are:

```
CauseGameOverIfLordDies

DefeatBoss(offset)

DefeatAll(offset)
```

DefeatBoss and DefeatAll are victory conditions (DefeatAll being rout). CauseGameOverIfLordDies
is a loss condition; if a unit's death quote sets the flag 0x65 (the game over global flag),
then this event will call a Game Over if that unit dies.
