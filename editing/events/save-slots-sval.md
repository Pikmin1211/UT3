# Save Slots \(SVAL\)

Save slots, or memory slots, can be written to and read from by events.
Their values will return to their defaults when the running event ends,
so they do not persist across different events, making them perfect
for short term use. They number from 0-13, and some have specific uses:

s0 is always 0. It is commonly used in events where 0 is a possible
outcome (as an example, checking whether an EID is true or not).

s1 and s2 are used with specific event codes. 

s3 is used when giving a unit items or giving/taking money.

sB holds coordinates, so any event code
that needs coordinates will use sB.

sC is used to return the results of a check. The available
FE8 checks are listed at the bottom of this page.

sD is the current size of the event queue. 

Despite s1 and s2's specific use cases, they can
generally be used freely outside of them. 

SVAL slotID value will set the given slot to the given value.
Changing s0 has no effect, because as mentioned, it is always 0.

You can perform a number of mathematical operations with the values in slots.
destSlotID is the destination slot for the value gained from an operation.
What each of these do is as follows:


SADD destSlotID operand1slot operand2slot - operand1 + operand2 (addition)

SSUB destSlotID operand1slot operand2slot - operand1 - operand2 (subtraction)

SMUL destSlotID operand1slot operand2slot - operand1 * operand2 (multiplication)

SDIV destSlotID operand1slot operand2slot - operand1 / operand2 (division)

SMOD destSlotID operand1slot operand2slot - operand1 % operand2 (modulus)

SAND destSlotID operand1slot operand2slot - operand1 & operand2 (bitwise AND)

SORR destSlotID operand1slot operand2slot - operand1 | operand2 (bitwise OR)

SXOR destSlotID operand1slot operand2slot - operand1 ^ operand2 (bitwise XOR)

SLSL destSlotID operand1slot operand2slot - operand1 << operand2 (bitwise left shift)

SLSR destSlotID operand1slot operand2slot - operand1 >> operand2 (bitwise right shift)

Vanilla uses SADD destSlotID sourceSlotID s0 to move the value of one slot into another.

All numbers used are signed, division rounds down, and right shift is an arithmetic
shift. Most of these aren't used frequently, but a fairly relevant example of this
is using SADD to move one slot's value to another using s0. As an example:

SADD s1 s2 s0

This would take s2's value, add 0 to it (aka do nothing), and then shift it to s1.

# Checks

Checks are what they sound like; using them will
return the value of the check to sC, which you
can then reference in your events. Some things
to note about charID is that there are a couple
of special ones:

0 - the first player unit.
-1 - the active unit.
-2 - the unit at the coordinates in sB.
-3 - the unit that is currently in s2.

This is the full list of vanilla FE8 checks:

- Returns as true(1) or false(0)

CHECK_HARD - Is the game in hard mode?

CHECK_TUTORIAL - Is the game in easy mode?

CHECK_EVENTID flagID - Is the flag set (true) or not (false)?

CHECK_POSTGAME - Is the game in postgame mode?

CHECK_EXISTS charID - Does the unit exist?

CHECK_ALIVE charID - Does the unit exist and are they alive?

CHECK_DEPLOYED charID - Is the unit deployed?

CHECK_ACTIVEID charID - Is the unit the active unit?

CHECK_INAREA charID [XTopLeft, YTopLeft] [XTopRight, YTopRight] - Is the unit within these coordinates?

CHECK_EXISTS will return false for dead enemies or NPCs, as they are
erased on death, but it will return true for dead player units.

- The next checks do not return as true/false.

RANDOMNUMBER max will return a random number from 0 to max, inclusive.
You cannot use a slot in place of max; it has to be a number.

CHECK_CHAPTER_NUMBER will return the current chapter ID.

CHECK_TURNS will return the current turn count.
Turn count increments just before the start of each player phase.

CHECK_ENEMIES will return the number of red units on the map.

CHECK_OTHERS will return the number of green units on the map.

CHECK_SKIRMISH will return 0 for story chapters, 1 for tower/ruins, and 2 for skirmishes.

CHECK_MONEY will return the party’s current gold balance.

The next few checks return 0 in the event that what
you're checking doesn't exist:

CHECK_EVENTID will return the flag associated with the
currently running event.

CHECK_AT [X, Y] will return the ID of the character at the given coordinates.

CHECK_ACTIVE will return the ID of the active unit.

Lastly, the following checks will cause the game to hang if the unit does not exist:

CHECK_STATUS charID will return the status byte for the given unit.
The upper nybble is duration and the lower nybble is status ID.

CHECK_ALLEGIANCE charID returns 0 for a player unit,
2 for an enemy unit, and 1 for any other allegiance.

CHECK_COORDS charID will return the coordinates of the
given unit. 

CHECK_CLASS charID will return the class of the given
unit. 

CHECK_LUCK charID will return the class of the given
unit. 

