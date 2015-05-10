## Level Objects

A level object is anything that is not part of the basic level layout (the tile map).

Objects are identified using three values. They are divided by class, subclass and type. In this documentation such an identifier will be written as ```c/s/t```. For example, the cyberjack port is ```9/2/0```.

Information about level objects exists in two forms:
* Static properties, describing an object in general
* Instance data, describing the state of a specific object instance in the game

The static properties are stored in a separate file. See [Property Files](../fileFormat/PropertyFiles.md).
Instance data is stored as part of an archive. Two chunks describe general information, then further chunks are object class specific.

### General Information

#### Level Object Table

```L08``` contains a table with general entries about the objects in the current level.

**Level Object Entry** (27 bytes)

    0000  byte     In use flag. 0: free, 1: in use
    0001  byte     Object class
    0002  byte     Object subclass
    0003  int16    Class table index
    0005  int16    Cross-Reference table index
    0007  int16    Previous
    0009  int16    Next
    000B  int16    X Coordinate
    000D  int16    Y Coordinate
    000F  byte     Z Coordinate (height)
    0010  [3]byte  Rotation Angles
    0013  byte     Unknown
    0014  byte     Object type
    0015  int16    Unknown
    0017  [4]byte  Unknown

#### Level Object Cross-Reference Table

```L09``` is a table that links level objects and the tiles they are in. The index field in the [Tile Map Entry](mapInformation.md) refers to this table, as well as the table index field in the **Level Object Entry**.

**Level Object Cross-Reference Entry** (10 bytes)

    0000  int16    Tile X
    0002  int16    Tile Y
    0004  int16    Index into level object entry table
    0006  int16    Index for next object in same tile
    0008  int16    Index for next tile of same object

Objects can extend over several tiles. In this case, the last field in this entry points to the next reference entry.

> The chunk contains 1600 cross reference entries per level. Unused entries appear to simply refer to the next entry and have the remaining fiels 0 otherwise.

### Class Specific Information

In the following chunks, all tables have entries with a common prefix.

**Level Object Prefix** (6 bytes)

    0000  int16  Index into level object entry table
    0002  int16  Previous
    0004  int16  Next

#### Level Weapon Table (Class 0)

```L10``` is a table describing weapons and entries extend the prefix with their current ammo or charge.

**Level Weapon Entry** (8 bytes)

    0000  [6]byte  Level object prefix
    0006  byte     Ammo type / Charge
    0007  byte     Ammo count / Temperature

#### Level Ammo Clip Table (Class 1)

```L11``` is a table for ammo clips. Since clips are always full, entries have no further fields.

**Level Ammo Clip Entry** (6 bytes)

    0000  [6]byte  Level object prefix

#### Level Item Table (Class 8)

```L18``` describes general items

**Level Item Entry** (16 bytes)

    0000  [6]byte   Level object prefix
    0008  [10]byte  Unknown

