## Property Files
A few files don't have an archive format, but are flat files consisting directly serialized tables.

### Texture Properties (textprop.dat)
This file contains a single table of texture property entries.

**Texture Property Entry** (11 bytes)

    0001  byte   Unknown
    0002  byte   Animation group
    0003  byte   Animation index within group
    0004  byte   Unknown. Always low byte of table index.
    0005  byte   Unknown. Always low byte of table index.
    0006  int32  Unknown. Always 0x0A
    000A  byte   Climbable flag: 1 for things like ladders and overgrowth

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
