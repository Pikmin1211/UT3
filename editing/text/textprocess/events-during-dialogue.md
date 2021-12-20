# Events During Dialogue

If you ever want to pause text and play an event mid-text entry (for example, a music change),
then you'll need to use \[LoadOverworldFaces\], often defined in ParseDefinitions as \[Event\].
Example of it in use:

```text
## SomeTextEntry
[OpenMidRight][LoadEirika]
[OpenMidLeft][LoadEphraim]
Time to change the music.[A]
[LoadOverworldFaces]
[OpenMidLeft]
Hey, this song stinks![A][X]
```

However, this requires a special event code to continue the text called TEXTCONT.
Here's an example of what a music changing event would look like:

```text
TEXTSTART
TEXTSHOW SomeTextEntry
TEXTEND // pauses the text where [LoadOverworldFaces] is
MUSC 0x9 // in vanilla, this is distant roads
TEXTCONT
TEXTEND
REMA
```

This is a simple example, but you can have multiple \[Event\] breaks in a text entry as long as
you remember to have TEXTCONT and TEXTEND for each one.