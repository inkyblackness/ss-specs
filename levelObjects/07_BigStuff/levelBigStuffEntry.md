## Level BigStuff Table (Class 7)

```L17``` is a table describing big items in real-world levels.

Cyberspace levels may re-use "big stuff" entries for further software entries. These entries need to be
interpreted differently. See [Software](../06_Software/levelSoftwareEntry.md).


**Level BigStuff Entry** (16 bytes)

    0000  [6]byte   Level object prefix
    0006  [10]byte  BigStuff Info

### Displays

There are several big items that are capable of displaying (animated) content. They reuse the following common types:

**Display Info** (10 bytes)

    0000  int16     Frame count
    0002  int16     Loop type
    0004  int16     Alternation type
    0006  int16     Picture source
    0008  int16     Alternate picture source

Animated screens are also controlled by the [Loop Configuration](../../archives/loopConfiguration.md). If not, they use their own loop type.

**Loop Type** (1 byte)

    0x00  Forward
    0x01  Forward/Backward
    0x02  Backward
    0x03  Forward/Backward

> Other values have been found as well, without effect; The engine defaults to forward looping.


** Alternation Type** (1 byte)

    0x00  Don't alternate
    0x03  Alternate randomly

> Not all alternate source combinations work properly. Commonly it is used to alternate to a text.


**Picture Source** (2 bytes)

This field specifies what shall be shown. It has the following cases:

    0x0000 .. 0x00F5  Image frames, offset from resource 0x0141
    0x00F6            Static fading into SHODAN's face
    0x00F7            Unknown (only found to display static)
    0x00F8 .. 0x00FF  Surveillance screens 0 .. 7
    0x0100 .. 0x017E  Text message from resource 0x0877
    0x017F            Random number for CPU rooms, level 1-6 before destroying nodes.
    0x0180 .. 0x01FF  Text message, scrolling vertically

For surveillance screens, the sources of screens 0-7 are specified in the [Surveillance Sources Table](../../archives/surveillanceCameras.md).


The following big items support displays:
7/0/6 (TV), 7/0/7 (COMPUTER MONITOR),
7/2/6 (SCREEN, small), 7/2/8 (SCREEN, large), 7/2/9 (SCREEN, medium),
with variation: 7/5/6 (CONTROL PEDESTAL)  (see below)

### Electronics 7/0/x

> Apart from some displays, nothing special


### Furniture 7/1/x

#### Cabinets 7/1/2

**Cabinet Data** (10 bytes)

    0000  [2]byte   Unused
    0002  [2]int16  Object index
    0006  [4]byte   Unused

Cabinets can contain up to two objects.


#### Texturable furniture 7/1/5, 7/1/7, 7/1/8 (chairs and couches)

**Texturable Furniture Data** (10 bytes)

    0000  [6]byte   Unused
    0006  int16     Texture index
    0008  [2]byte   Unused

If ```Texture index``` is not zero, it specifies the alternate texture to use for the "main" faces of the furniture.


### Surfaces 7/2/x

These are big objects that are flat and should be placed at another surface.

#### Words 7/2/3

**Words BigStuff Data** (10 bytes)

    0000  int16     Text index (within resource 0x0868)
    0002  int16     Font and size (bits 0-3 are font, 4-7 are size)
    0004  int16     Colour (palette index, 0 defaults to red)
    0006  [4]byte   Unused


#### Texture Maps 7/2/7

**Texture map big stuff** (10 bytes)

    0000  [6]byte   Unused
    0006  int16     Texture index
    0008  [2]byte   Unused

Texture maps allow to place one tile from the level texture list at any arbitrary position.

### Lighting 7/3/x

### Medical equipment 7/4/x

#### Control Pedestals (buttons only) 7/4/0

These can trigger an object if used.

**Button Control Pedestal Info** (10 bytes)

    0000  [2]byte   Unused
    0002  int16     Trigger object index
    0004  [6]byte   Unused


#### Surgery Machines 7/4/3

**Surgery Machine Info** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Broken state; 0x00 = OK, 0xE7 = broken
    0003  [2]byte   Unused
    0005  byte      Broken message index; Broken default: 0x61
    0006  [4]byte   Unused

With all bytes zero, the surgery machine works normally.

When the ```Broken state``` field is set to a certain value (the game has ```0xE7``` for the one on level 2, though some other work, too), then the machine wouldn't work. When not working, the message identified with ```Broken message index``` is displayed in the HUD. (```0x61``` = "This machine is broken").


#### Textureable Furniture 7/4/5 (chairs)

These have the same layout as the general textureable furniture (see above).


### Science and security equipment 7/5/x

#### Security Cameras 7/5/4

**Security Camera Info** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Panning switch; 0x00: stationary, 0x01: panning
    0003  [7]byte   Unused


#### Control Pedestals (with display) 7/5/6

Control pedestals are similar to displays regarding the source of the display.
Instead of having different loop options, they trigger an object if used.

**Display Control Pedestal Info** (10 bytes)

    0000  int16     Frame count
    0002  int16     Trigger object index
    0004  int16     Alternation type
    0006  int16     Picture source
    0008  int16     Alternate picture source


### Garden big stuff 7/6/x

### Bridges 7/7/x

#### Solid Bridges 7/7/0, 7/7/1

**Solid Bridge Info** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Size. Bits 0-3: X, bits 4-7: Y
    0003  byte      Height. 0 is default, level height units otherwise.
    0004  byte      Top/Bottom texture
    0005  byte      Side texture
    0006  [4]byte   Unused

The size fields are defined as 0: 3D model default, 4: tile width.
The texture fields have bit 7 set to indicate texture from level texture list. If 0, the other 7 bits index
into ```citmat.res```.

> If either textures are to be taken from the level list, the object description will be that of the textures.
> If a texture is to be taken from citmat.res, the texture index must be > 0, otherwise it will still be the first
> level texture, even if bit 7 is 0.


#### Force Bridges 7/7/7, 7/7/8, 7/7/9

**Force Bridge Info** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Size. Bits 0-3: X, bits 4-7: Y -- see above
    0003  byte      Height. 0 is default, level height units otherwise.
    0004  [2]byte   Unused
    0006  byte      Colour
    0007  [3]byte   Unused

Object 7/7/8 is a placeholder for force bridges that have not been activated yet. These objects are invisible and are made to force bridges with the use of trigger action 24.

Force bridges of type 7/7/9 differ from the typical ones (7/7/7) in having a default height of zero and no animation when switched from type 8. They furthermore need a size explicitly set.

The engine supports several different values for ```Colour```. Used values are ```0x00``` for red and ```0xFE``` for green.

> Not all 256 colour possibilities have been tested, many resulted in dark red tones or solid colours. Usable variants were found from ```0xF7``` and above.
