## Level Objects

Instance data of [Level Objects](../levelObjects/index.md) is stored as part of an archive. Two chunks describe general information, then further chunks are object class specific.

### General Information

#### Level Object Table

```L08```, a compressed chunk, contains a table with general entries about the objects in the current level. This table contains 872 entries, which form a double-linked list with their ```Previous``` and ```Next``` fields.

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

#### Level Object Cross-Reference Table

```L09```, a compressed chunk, is a table that links level objects and the tiles they are in. The index field in the [Tile Map Entry](mapInformation.md) refers to this table, as well as the table index field in the **Level Object Entry**. This table contains 1600 entries, which form a single-linked list with the ```Index for next object``` field.

**Level Object Cross-Reference Entry** (10 bytes)

    0000  int16    Tile X
    0002  int16    Tile Y
    0004  int16    Index into level object entry table
    0006  int16    Index for next object in same tile
    0008  int16    Index for next tile of same object

Objects can extend over several tiles. In this case, the last field in this entry points to the next reference entry.

> Unused entries appear to simply refer to the next entry and have the remaining fiels 0 otherwise.

### Class Specific Information

In the following chunks, all tables have entries with a common prefix.

**Level Object Prefix** (6 bytes)

    0000  int16    Index into level object entry table
    0002  int16    Previous
    0004  int16    Next

The ```Previous``` and ```Next``` fields form a double-linked list within the same table. The first and last entries are linked together, so an endless loop is created. All the tables have a static entry count and unused entries refer to an **Level Object Entry** that is not in use.

> For the archives, this unused entry is always the first one, index 0.

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
