# Turn Based Events

TurnBasedEvents are, as you'd guess, events that occur based on specified turns.
They look like this:

```text
TURN flagID pointer [startTurn, endTurn] phase
```

phase indicates what phase the event takes place on (0 for Player Phase, 8 for Enemy Phase,
and 4 for NPC Phase), and if the event only takes place on one turn, endTurn should be set
to 0. Generally, you'll want to use the following macros instead.

```text
TurnEventPlayer(eventID,pointer,turn) 

TurnEventPlayer(eventID,pointer,startTurn,amountOfTurns) 

TurnEventEnemy(eventID,pointer,turn) 

TurnEventEnemy(eventID,pointer,startTurn,amountOfTurns) 

TurnEventNPC(eventID,pointer,turn) 

TurnEventNPC(eventID,pointer,startTurn,amountOfTurns)

Survive(pointer,endturn)

OpeningTurnEvent(pointer)
```

One common use for turn events is spawning reinforcments, so here's an example of that.
Refer to the Unit Blocks section if this looks unfamiliar.

```text
TurnBasedEvents:
TurnEventPlayer(0x7,EnemyReinforcementSpawn,5)
END_MAIN

EnemyReinforcementSpawn:
LOAD1 1 EnemyReinforcementGroup // some unit group
ENUN
NoFade
ENDA
```