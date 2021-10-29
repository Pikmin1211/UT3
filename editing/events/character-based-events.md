# Character Based Events

CharacterBasedEvents are Talk conversations formulated like this:

```text
CHAR flagID eventPointer [char1,char2] 0
```

char1 is the unit that starts the talk, and char2 is the one that is talked to.
If you want both units to be able to initiate it, then use the BothWays macro
out of the following two.

```text
CharacterEvent(eventID,pointer,char1,char2)
CharacterEventBothWays(eventID,eventPtr,char1,char2)
```

Here's an example of what this may look like in an actual event:

```text
CharacterBasedEvents:
CharacterEventBothWays(0x7,EirikaEphraimTalk, Eirika, Ephraim
END_MAIN


EirikaEphraimTalk:
Text(EirikaEphraimTalkText) // some conversation
NoFade
ENDA
```