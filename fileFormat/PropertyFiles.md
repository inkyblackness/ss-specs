## Property Files
A few files don't have an archive format, and instead are flat files consisting directly serialized tables.

### Texture Properties (textprop.dat)
This file contains a single table of texture property entries, after a magic header value.

> The table appears to be truncated. Since the game only uses 273 textures, this may have never been detected.
> The file has 4004 bytes on disk, meaning 4000 bytes are available for the table.
> In a test, the original engine appeared to be fine to read a smaller file with exactly 273 entries (3007 bytes) -
> which had the unknown fields set to zero.

#### Header
The file starts with a 4-byte header, purpose unknown.

**Magic Header** (4 bytes)

    0000  int32    magic header, value 0x09

#### Properties Entry

**Texture Property Entry** (11 bytes)

    0000  [2]byte  Unknown
    0002  int32    Unknown
    0006  byte     Climbable. 0x00: not climbable; != 0x00: wall can be climbed
    0007  byte     Unknown. Always 0x00
    0008  byte     Transparency control
    0009  byte     Animation group
    000A  byte     Animation index - if texture is animated

> The two bytes at the beginning almost always are each set to the low-byte of the index. A few exceptions have both bytes 0x00.
> The second field at offset ```0002``` is nearly always ```0x0A```; The one existing exception has it 0.

See [Texture Animation](../archives/textureAnimation.md) for details on animated textures.

**Transparency Control**

    0x00  Regular texture; transparent pixel (palette index 0x00) are drawn black
    0x01  Space texture (pixel data of image is ignored)
    0x02  Texture with space background for transparent pixels


### Object Properties (objprop.dat)
The object properties file contains several tables about properties of [level objects](../levelObjects/index.md). It has the following format:

    | Magic | Class 0 Tables   | Class 1 Tables     | ... | Common Table |

#### Header
The file starts with a 4-byte header, purpose unknown.

**Magic Header** (4 bytes)

    0000  int32   magic header, value 0x2D

#### Class Tables
For each object class, one entry exists with a table of class-generic properties and a further table, subclass-specific:

    | Class N Tables                                           |
    | Generic | Subclass 0   | Subclass 1   | ... | Subclass N |

The sizes of the generic and specific tables are dependent on the object class, subclass and type amount.
The generic table contains one entry per object type of this class. The specific tables contain one entry per object type of the subclass.

See [Level Objects](../levelObjects/index.md) for an overview of the amount of types and their entry sizes.


#### Common Table
At the end of the file is the table with common properties. For each object type one entry.

**Common Object Properties** (27 bytes)

    0000  int16    Mass
    0002  [2]byte  Unused
    0004  int16    Default hitpoints
    0006  uint8    Armour
    0007  byte     Render type
    0008  byte     Physics type; 0x00: none (insubstantial), 0x01: regular, 0x02: special
    0009  sint8    Bounciness (only for Physics type 0x01)
    000A  byte     Unused
    000B  byte     Vertical frame offset
    000C  byte     Unknown. These values are set (almost) identical to 000B and have no effect.
    000D  byte     Unknown
    000E  byte     Vulnerabilities (matching Damage Type from [Generic Weapon Info](../levelObjects/GenericWeaponInfo.md))
    000F  byte     Special vulnerabilities (matching Special Damage Type from [Generic Weapon Info](../levelObjects/GenericWeaponInfo.md))
    0010  [2]byte  Unused
    0012  byte     Defence value
    0013  byte     Receive damage Flag; 0x00: Yes, 0x03: No, 0x04: Unknown 0x04
    0014  int16    Flags
    0016  int16    3D model index, based on chunk 0x08FC
    0018  byte     Unknown
    0019  byte     Extra; High-nibble (bits 4-7): Number of extra bitmaps; bits 0-3: Unknown
    001A  byte     Unknown


```Physics type``` governs how collisions shall be handled. ```0x00``` makes the object insubstantial, ignoring forces applied to it. For instance, most scenery stays where it was put. ```0x01``` is for regular objects meant to react "normal" in a physical world. ```0x02``` behaves strange is set for exactly one object (```14/0/6```, "SONIC THE HEDGEHOG", an unused critter). Objects with a physics type of ```0x02``` aren't affected by gravity for instance, but are repelled if hacker runs into them.
The ```bounciness``` acts as repulsion force on the object if it collides with the ground or walls - or on the hacker if hacker runs into it. (Negative values make no difference for the object itself, hacker is propelled forward if running into such an object.) The bounciness depends on the ```mass```; Objects with more mass bounce around less.

The ```vertical frame offset``` is applied to vertically center (animated) sprites on their position. If ```0x00```, the sprites are rendered with their bottom anchored at the object's position. Higher values will move the sprites downwards.

The ```defence value``` is compared to the ```offence value``` of a weapon or projectile. Higher defence values will decrease the damage, higher offence values will increase the damage.
> It is not clear how much is added/removed. First tests indicated it's not the difference between the two values.

The ```receive damage flag``` determines whether the object would be destructible - this still requires the object to be solid to register damage.
> This flag is set to ```0x04``` only for the non-used SHODAN critter ```14/3/5``` - effect unknown.

> ```3D model index``` is also set to some values for render type ```0x02```, effect unknown.


**Render Type Enumeration** (1 byte)

    0x01  3D object
    0x02  Sprite
    0x03  Screen
    0x04  Critter
    0x06  Fragments (e.g. the Cyberdog)
    0x07  Not drawn
    0x08  Oriented surface (door, wall decoration)
    0x0B  Special
    0x0C  Force door

**Common Object Flags** (2 byte)

    0x0001  Inventory object
    0x0002  Solid (can't be passed through)
    0x0004  Unknown
    0x0008  Unknown
    0x0010  Consumable inventory object
    0x0020  Blocks 3D rendering if closed (= opaque doors)
    0x0040  Unknown
    0x0080  Unknown
    0x0100  Openable (removable) barrier (doors, gratings)
    0x0200  Flat solid (can be stood upon)
    0x0400  Unknown
    0x0800  Explode on contact
    0x1000  Unknown
    0x2000  Unknown
    0x4000  Unknown
    0x8000  Unknown
