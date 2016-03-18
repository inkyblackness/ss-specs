## Level Objects

A level object is anything that is not part of the basic level layout (the tile map). The game knows about 476 different objects.

Objects are identified using three values. They are divided by class, subclass and type. In this documentation such an identifier will be written as ```c/s/t```. For example, the energy charge station is ```9/2/1```.

Information about level objects exists in two places:
* Static properties, describing an object in general
* Instance data, describing the state of a specific object instance in the game

The static properties are stored in a separate file. See [Property Files](../fileFormat/PropertyFiles.md) for basic information.
Instance data is stored as part of an archive. See [Level Objects](../archives/levelObjects.md) for basic information.

### Known Classes, Subclasses and Types

The following table lists the object classes together with the available types per subclass:

| Class | Name                               | 0  | 1  | 2  | 3  | 4  | 5  | 6  | 7  |
|:-----:|------------------------------------|----|----|----|----|----|----|----|----|
|   0   | Weapons                            | 5  | 2  | 2  | 2  | 3  | 2  | -  | -  |
|   1   | Ammo                               | 2  | 2  | 3  | 2  | 2  | 2  | 2  | -  |
|   2   | Projectiles                        | 6  | 16 | 2  | -  | -  | -  | -  | -  |
|   3   | Explosives                         | 5  | 3  | -  | -  | -  | -  | -  | -  |
|   4   | Patches                            | 7  | -  | -  | -  | -  | -  | -  | -  |
|   5   | Hardware                           | 5  | 10 | -  | -  | -  | -  | -  | -  |
|   6   | Software (including logs)          | 7  | 3  | 4  | 5  | 3  | -  | -  | -  |
|   7   | Scenery / Fixtures                 | 9  | 10 | 11 | 4  | 9  | 8  | 16 | 10 |
|   8   | Items (gettable and other objects) | 8  | 10 | 15 | 6  | 12 | 12 | 9  | 8  |
|   9   | Panels (switches and the like)     | 9  | 7  | 3  | 11 | 2  | 3  | -  | -  |
|   10  | Barriers (doors and gratings)      | 10 | 9  | 7  | 5  | 10 | -  | -  | -  |
|   11  | Animations                         | 9  | 11 | 14 | -  | -  | -  | -  | -  |
|   12  | Markers (including traps)          | 13 | 1  | 5  | -  | -  | -  | -  | -  |
|   13  | Containers                         | 3  | 3  | 4  | 8  | 13 | 7  | 8  | -  |
|   14  | Critters                           | 9  | 12 | 7  | 7  | 2  | -  | -  | -  |

> This information is not stored within the resource files, it is hardcoded in the engine.


### Static Properties

The following table lists the byte sizes of the generic and specific property structures per class.
Many of these bytes are all zero in ```objprop.dat```; In these cases no detailed data definition will be linked.

| Class | Generic                                     | 0  | 1        | 2         | 3         | 4         | 5         | 6  | 7  |
|:-----:|---------------------------------------------|----|----------|-----------|-----------|-----------|-----------|----|----|
|   0   | [2](00_Weapons/weaponProperties.md)         | 1  | 1        | [16][0/2] | [13][0/3] | [13][0/4] | [18][0/5] | -  | -  |
|   1   | [14](01_AmmoClips/ammoClipProperties.md)    | 1  | 1        | 1         | 1         | 1         | 1         | 1  | -  |
|   2   | [1](02_Projectiles/projectileProperties.md) | 20 | [6][2/1] | 1         | -         | -         | -         | -  | -  |
|   3   | [15](03_Explosives/explosiveProperties.md)  | 1  | [3][3/1] | -         | -         | -         | -         | -  | -  |
|   4   | 22                                          | 1  | -        | -         | -         | -         | -         | -  | -  |
|   5   | 9                                           | 1  | 1        | -         | -         | -         | -         | -  | -  |
|   6   | 5                                           | 1  | 1        | 1         | 1         | 1         | -         | -  | -  |
|   7   | 2                                           | 1  | 1        | 1         | 1         | 1         | 1         | 1  | 1  |
|   8   | 2                                           | 1  | 1        | 1         | 1         | 1         | [6][8/5]  | 1  | 2  |
|   9   | 1                                           | 1  | 1        | 1         | 1         | 0         | 1         | -  | -  |
|   10  | 1                                           | 1  | 1        | 1         | 1         | 1         | -         | -  | -  |
|   11  | [2](11_Animations/animationProperties.md)   | 1  | 1        | [1][11/2] | -         | -         | -         | -  | -  |
|   12  | 1                                           | 1  | 1        | 1         | -         | -         | -         | -  | -  |
|   13  | 3                                           | 1  | 1        | 1         | 1         | 1         | 1         | 1  | -  |
|   14  | [75](14_Critters/critterProperties.md)      | 1  | 2        | [2][14/2] | [6][14/3] | 1         | -         | -  | -  |

[0/2]: 00_Weapons/weaponProperties.md#specific-2-properties
[0/3]: 00_Weapons/weaponProperties.md#specific-3-properties
[0/4]: 00_Weapons/weaponProperties.md#specific-4-properties
[0/5]: 00_Weapons/weaponProperties.md#specific-5-properties
[2/1]: 02_Projectiles/projectileProperties.md#specific-1-properties
[3/1]: 03_Explosives/explosiveProperties.md#specific-1-properties
[8/5]: 08_Items/itemProperties.md#cyberspace-items-specific-properties
[11/2]: 11_Animations/animationProperties.md#specific-2-properties
[14/2]: 14_Critters/critterProperties.md#specific-2-properties
[14/3]: 14_Critters/critterProperties.md#cyberspace-critters-specific-properties

### Object Names

The names are localized and [stored as texts](../media/Texts.md). The short names are stored in chunk ```0x086D```, the long
names are in chunk ```0x0024```.

