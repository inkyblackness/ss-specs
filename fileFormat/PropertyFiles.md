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

    0000  int32  magic header, value 0x09

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

    0000  int32  magic header, value 0x2D

#### Class Tables
For each object class, one entry exists with a table of class-generic properties and a further table, subclass-specific:

    | Class N Tables                                           |
    | Generic | Subclass 0   | Subclass 1   | ... | Subclass N |

The sizes of the generic and specific tables are dependent on the object class, subclass and type amount.
The generic table contains one entry per object type of this class. The specific tables contain one entry per object type of the subclass.

> It is often the case that no specific information is available for object types. In this case, the corresponding entry (either generic or special) has a length of 1 byte, which is always zero.

> There is also the case of objects with a specific entry length of 0. This is the case for object types that didn't make it into the game (vending machines).

#### Common Table
At the end of the file is the table with common properties. For each object type one entry.

**Common Object Properties** (27 bytes)

    tbd
