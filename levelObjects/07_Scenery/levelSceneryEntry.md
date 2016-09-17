## Level Scenery Table (Class 7)

```L17``` is a table describing scenery and fixture items in real-world levels.

Cyberspace levels may re-use "scenery" entries for further software entries. These entries need to be
interpreted differently. See [Software](../06_Software/levelSoftwareEntry.md).


**Level Scenery Entry** (16 bytes)

    0000  [6]byte   Level object prefix
    0006  [10]byte  Scenery Info


### Decal 7/2/1

### Words 7/2/3

    0000  int16  Text index (within chunk 0x0868)
    0002  int16  Font and size (bits 0-3 are font, 4-7 are size)
    0004  int16  Colour (palette index, 0 defaults to red)

### Control Pedestal 7/4/0

### Surgery Machines 7/4/3

### Camera 7/5/4

### Bridges 7/7/0

**Bridge Info** (10 bytes)

    0000  int16     Unknown
    0002  byte      Size. Bits 0-3: X, bits 4-7: Y
    0003  byte      Height. 0 is default, level height units otherwise.
    0004  byte      Top/Bottom texture
    0005  byte      Side texture
    0006  [4]byte   Unknown

The size fields are defined as 0: 3D model default, 4: tile width.
The texture fields have bit 7 set to indicate texture from level texture list. If 0, the other 7 bits index
into ```citmat.res```.

> If either textures are to be taken from the level list, the object description will be that of the textures.
> If a texture is to be taken from citmat.res, the texture index must be > 0, otherwise it will still be the first
> level texture, even if bit 7 is 0.

### Screens 7/2/6, 7/2/9

**Screen Info** (10 bytes)

    0000  int16     Frame count
    0002  [4]byte   Unknown
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

Animated screens are also controlled by the [Loop Configuration](../../archives/loopConfiguration.md).

### Control Pedestal 7/5/6

    0000  [6]byte   Unknown
    0006  int16     Picture source (see above)

