# Control Codes

## Default Control Codes

\[X\] - Ends the text entry.  
\[NL\] - Starts a new line.  
\[A\] - Prompts for an A press before continuing.  
\[OpenPosition\] - Set the current position to the specified one.  
\[LoadFace\] - Load a character portrait at the current position. Follow this command with the ID of the portrait to be loaded and \[0x1\] \(the ID +0x100\). For example, to load portrait ID 0x2, use \[LoadFace\]\[0x2\]\[0x1\].  
\[MovePosition\] - Move the character portrait at the current position to the specified position.  
\[ClearFace\] - Remove the character portrait at the current position.  
\[ToggleSmile\] - Set the character portrait at the current position to smile or to stop smiling.  
\[ToggleMouthMove\] - Set the character portrait at the current position to stop or continue moving their mouth while speaking.  
\[CloseEyes\] - Set the character portrait at the current position to close or to stop closing their eyes.

## Positional Control Codes

The \[OpenPosition\] and \[MovePosition\] codes expect one of 8 positions on the screen for loading character portraits, speech bubbles, etc. The possible positions, in order of their appearance on screen from left to right, are: FarFarLeft, FarLeft, MidLeft, Left, Right, MidRight, FarRight, FarFarRight. For example, to set the current position to the MidLeft of the screen, use \[OpenMidLeft\].   
The "FarFar" positions are offscreen and can be used to have a character portrait exit stage left or right, or to display text coming from outside the visible area.  
Do note that character portraits loaded to the right side of the screen will face left, and vice versa. To display a character portrait on one side of the screen facing the other way around, you will need to load them on the other side and then move them to the desired position afterwards. To have them turn back around, simply clear and reload the portrait.

## Custom Control Codes \(ParseDefinitions.txt\)

Certain commands, such as loading character portraits, can be cumbersome since they use several commands at once or require you to remember things such as portrait IDs. To avoid this, you can define your own control codes using the file "ParseDefinitions.txt". To define a new control code, simply write the desired code followed by an = and the expected output. For example, here is a custom code to load Eirika's portrait at the current position.

```text
[LoadEirika] = [LoadFace][0x02][0x1]
```

Now, instead of needing to type out \[LoadFace\]\[0x02\]\[0x1\] every time, we can simply use \[LoadEirika\].

