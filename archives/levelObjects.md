## Level Objects

Instance data of [Level Objects](../levelObjects/index.md) is stored as part of an archive. Two resources describe general information, then further resources are object class specific.

### General Information

#### Level Object Table

```L08```, a compressed resource, contains a table with general entries about the objects in the current level.
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
    0017  [4]byte  Extra

The starting point for both lists is the first entry (index zero). This entry is not in use (```In use flag``` is zero).

The ```Next``` field of the starting point refers to the first object of a list of objects in use. For all object entries in use,
their ```Previous``` and ```Next``` fields form a double-linked list, with the exception of the starting point:
The last used object entry refers to the starting point with its ```Next``` value without being referred-to from the starting point.

The ```Previous``` index of the starting point refers to the first object of a list of objects not in use.
For all object entries not in use, their ```Previous``` field forms a single-linked list.
The ```Next``` field of unused objects is not defined and contains arbitrary data.
The last unused object entry refers to the starting point with its ```Previous``` value.

The ```Cross-Reference table index``` of the starting point does not refer to a cross-reference entry, it is the index within the same table of the last object entry in use.

If an object is placed in the world, it has the X, Y and Z coordinates set, as well as the ```Cross-Reference table index``` pointing to a corresponding entry in the cross-reference table.
If an object was spawned by the engine without a location in the world, such as loot from corpses,
then the ```X``` field is ```0xFFFF```, ```Y``` and ```Z``` fields are zero, and the ```Cross-Reference table index``` is zero as well.

The ```extra``` field is a multi-purpose field, which may hold further properties and/or state. The exact features are environment specific and may also be dependent on the classification of the object.

##### Global extra

For some objects, the ```extra``` field is dependent on the object type or class, and the field is applicable to both real world and cyberspace.

**Panel Extra** (4 bytes)

    0000  byte     Panel name index; Index into cybstrng 0x0869
    0001  [3]byte  Other

If the ```Panel name``` resolves to an unknown block or empty string, then the regular object name is displayed.

##### Cyberspace extra

In case the level is cyberspace, the ```extra``` field contains the following properties.

**Cyberspace Level Object Extra** (4 bytes)

    0000  byte     Other
    0001  byte     ICE presence
    0002  byte     Unknown
    0003  byte     ICE level

Level objects in cyberspace can be locked by ICE (Intrusion Countermeasures Electronics). ICE need to be defeated using the drill tool before the locked object can be acquired/used. The ```ICE presence``` field determines whether ICE is present. Any value != ```0x00``` has ICE present.
If present, the health of the ICE is determined by the health of the object.
The ```ICE level``` then determines how aggressive and powerful the ICE is. ```0x00``` is the weakest, ```0xFF``` is the highest.

> Although the vanilla game uses ICE only on software objects, the engine also supports ICE on other objects, such as cyberspace exits.

#### Level Object Cross-Reference Table

```L09```, a compressed resource, is a table that links level objects and the tiles they are in. The index field in the [Tile Map Entry](mapInformation.md) refers to this table, as well as the table index field in the **Level Object Entry**. This table contains 1600 entries.

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

In the following resources, all tables have a static count of entries. Each entry has a common prefix.

**Level Object Prefix** (6 bytes)

    0000  int16    Index into level object entry table
    0002  int16    Previous
    0004  int16    Next

The tables contain two lists each, similar to the main object table:
The starting point is the first entry (index zero). The starting point refers to a double-linked list of used entries (via ```Next```), and a single-linked list of unused entries (via ```Previous```). Both lists are terminated by referring back to index zero.

The ```Index``` field of the starting point does not refer to a level object, it is the index within the same table of the last used entry. (The actual "previous" link to the double-linked list of used objects.)

> The sum of all possible entries is slightly higher than the total count of possible **Level Object Entries**.

#### Class Tables and Entries

The following table lists the the level resources per class type. It also lists how many entries each table contains and how big one entry is.
The name column links to the corresponding details.

| Class | Level Res   | Name                                      | Entry Count | Entry Size (incl. prefix) |
|:-----:|-------------|-------------------------------------------|-------------|---------------------------|
|   0   | ```L10```   | [Weapons][L10]                            | 16          | 8                         |
|   1   | ```L11```   | [Ammo][L11]                               | 32          | 6                         |
|   2   | ```L12```   | Projectiles                               | 32          | 40                        |
|   3   | ```L13```   | [Explosives][L13]                         | 32          | 12                        |
|   4   | ```L14```   | [Patches][L14]                            | 32          | 6                         |
|   5   | ```L15```   | [Hardware][L15]                           | 8           | 7                         |
|   6   | ```L16```   | [Software (including logs)][L16]          | 16          | 9                         |
|   7   | ```L17```   | [Scenery / Fixtures][L17]                 | 176         | 16                        |
|   8   | ```L18```   | [Items (gettable and other objects)][L18] | 128         | 16                        |
|   9   | ```L19```   | [Panels (switches and the like)][L19]     | 64          | 30                        |
|   10  | ```L20```   | [Barriers (doors and gratings)][L20]      | 64          | 14                        |
|   11  | ```L21```   | Animations                                | 32          | 10                        |
|   12  | ```L22```   | [Markers (including traps)][L22]          | 160         | 28                        |
|   13  | ```L23```   | [Containers][L23]                         | 64          | 21                        |
|   14  | ```L24```   | [Critters][L24]                           | 64          | 46                        |


[L10]: ../levelObjects/00_Weapons/levelWeaponEntry.md
[L11]: ../levelObjects/01_AmmoClips/levelAmmoClipEntry.md
[L13]: ../levelObjects/03_Explosives/levelExplosiveEntry.md
[L14]: ../levelObjects/04_Patches/levelPatchEntry.md
[L15]: ../levelObjects/05_Hardware/levelHardwareEntry.md
[L16]: ../levelObjects/06_Software/levelSoftwareEntry.md
[L17]: ../levelObjects/07_Scenery/levelSceneryEntry.md
[L18]: ../levelObjects/08_Items/levelItemEntry.md
[L19]: ../levelObjects/09_Panels/levelPanelEntry.md
[L20]: ../levelObjects/10_Barriers/levelBarrierEntry.md
[L22]: ../levelObjects/12_Markers/levelMarkerEntry.md
[L23]: ../levelObjects/13_Containers/levelContainerEntry.md
[L24]: ../levelObjects/14_Critters/levelCritterEntry.md

### Class Extra Information

Each level has 15 further resources, ```L25``` to ```L39```, one for each object class. The length of these resources is the same as for one class entry of the corresponding specific entries. Most of the time, all their bytes are 0x00.
