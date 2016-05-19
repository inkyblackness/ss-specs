## Level Objects

Instance data of [Level Objects](../levelObjects/index.md) is stored as part of an archive. Two chunks describe general information, then further chunks are object class specific.

### General Information

#### Level Object Table

```L08```, a compressed chunk, contains a table with general entries about the objects in the current level.
This table contains 872 entries, which houses two lists.

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
    0010  byte     Pitch rotation (around X axis), increasing values turn upwards
    0011  byte     Yaw rotation (around Z axis), increasing values turn rightwards
    0012  byte     Roll rotation (around Y axis), increasing values turn rightwards
    0013  byte     Unknown
    0014  byte     Object type
    0015  int16    Hitpoints
    0017  [4]byte  Unknown

The starting point for both lists is the first entry (index zero). This entry is not in use (```In use flag``` is zero).

The ```Next``` field of the starting point refers to the first object of a list of objects in use. For all object entries in use,
their ```Previous``` and ```Next``` fields form a double-linked list, with the exception of the starting point:
The last used object entry refers to the starting point with its ```Next``` value without being referred-to from the starting point.

The ```Previous``` index of the starting point refers to the first object of a list of objects not in use.
For all object entries not in use, their ```Previous``` field forms a single-linked list.
The ```Next``` field of unused objects is not defined and contains arbitrary data.
The last unused object entry refers to the starting point with its ```Previous``` value.

The ```Cross-Reference table index``` of the starting point does not refer to a cross-reference entry, it is the index within the same table of the last object entry in use.

> While the ```In use flag``` is the primary information whether an entry in this table describes a proper object,
> there are entries in the archives "in use" which have bogus data. In these cases, they refer to the cross-reference
> table entry number zero. This entry belongs to the starting point object entry, which is not in use.
> For a robust implementation, both should be checked to determine whether an object entry is OK.

#### Level Object Cross-Reference Table

```L09```, a compressed chunk, is a table that links level objects and the tiles they are in. The index field in the [Tile Map Entry](mapInformation.md) refers to this table, as well as the table index field in the **Level Object Entry**. This table contains 1600 entries.

**Level Object Cross-Reference Entry** (10 bytes)

    0000  int16    Tile X
    0002  int16    Tile Y
    0004  int16    Index into level object entry table
    0006  int16    Index for next object
    0008  int16    Index for next tile of same object

Objects can extend over several tiles. The last field points to the next reference entry about the same object, with different tile coordinates.
This field creates a single-linked list, with the last entry pointing to the first one.
If the object is contained within one tile only, this field points to its own entry.

The major coordinates of the main object entry match those of the last entry of the cross-reference list. The cross-reference list only contains the X/Y values of the tiles - not sub-tile coordinates.

The ```Index for next object``` field creates a single-linked list, terminating with a zero value.
This list either refers to all objects within the same tile (if referred to from a tile), or creates a pool of unused cross-reference entries. The start of this pool is the first entry (index zero).
Unused cross-reference entries contain arbitrary data and only the next object index field is valid.

### Class Specific Information

In the following chunks, all tables have a static count of entries. Each entry has a common prefix.

**Level Object Prefix** (6 bytes)

    0000  int16    Index into level object entry table
    0002  int16    Previous
    0004  int16    Next

The tables contain two lists each, similar to the main object table:
The starting point is the first entry (index zero). The starting point refers to a double-linked list of used entries (via ```Next```), and a single-linked list of unused entries (via ```Previous```). Both lists are terminated by referring back to index zero.

The ```Index``` field of the starting point does not refer to a level object, it is the index within the same table of the last used entry. (The actual "previous" link to the double-linked list of used objects.)

> The sum of all possible entries is slightly higher than the total count of possible **Level Object Entries**.

#### Class Tables and Entries

* ```L10``` [Weapons](../levelObjects/00_Weapons/levelWeaponEntry.md)
* ```L11``` [Ammo Clips](../levelObjects/01_AmmoClips/levelAmmoClipEntry.md)
* ```L12``` Projectiles
* ```L13``` Explosives
* ```L14``` [Patches](../levelObjects/04_Patches/levelPatchEntry.md)
* ```L15``` [Hardware](../levelObjects/05_Hardware/levelHardwareEntry.md)
* ```L16``` [Software](../levelObjects/06_Software/levelSoftwareEntry.md)
* ```L17``` [Scenery](../levelObjects/07_Scenery/levelSceneryEntry.md)
* ```L18``` [Items](../levelObjects/08_Items/levelItemEntry.md)
* ```L19``` [Panels](../levelObjects/09_Panels/levelPanelEntry.md)
* ```L20``` [Barriers](../levelObjects/10_Barriers/levelBarrierEntry.md)
* ```L21``` Animations
* ```L22``` [Markers](../levelObjects/12_Markers/levelMarkerEntry.md)
* ```L23``` [Containers](../levelObjects/13_Containers/levelContainerEntry.md)
* ```L24``` Critters

### Class Extra Information

Each level has 15 further chunks, ```L25``` to ```L39```, one for each object class. The length of these chunks is the same as for one class entry of the corresponding specific entries. Most of the time, all their bytes are 0x00.
