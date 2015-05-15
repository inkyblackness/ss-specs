## Level Scenery Table (Class 7)

```L17``` is a table describing scenery and fixture items

**Level Scenery Entry** (16 bytes)

    0000  [6]byte   Level object prefix
    0008  [10]byte  Scenery Info

### Decal 7/2/1

### Control Pedestal 7/4/0

### Surgery Machines 7/4/3

### Camera 7/5/4

### Bridges 7/7/0

### Screens 7/2/6, 7/2/9

**Screen Info** (10 bytes)

    0000  int16     Frame count
    0002  int16     Unknown
    0006  int16     Picture source

**Picture Source**

The picture source field specifies what shall be shown. It has the following cases:

    0x0000 .. 0x00F5  Image frames, offset from chunk 0x0141
    0x00F6            Static fading into SHODAN's face
    0x00F7            Unknown
    0x00F8 .. 0x00FF  Surveillance screens 0 .. 7
    0x0100 .. 0x017E  Text message from chunk 0x0877
    0x017F            Random number for CPU rooms, level 1-6 before destroying nodes.
    0x0180 .. 0x01FF  Text message, scrolling vertically

For surveillance screens, the sources of screens 0-7 are specified in the [Surveillance Sources Table](../../archives/surveillanceScreens.md).
