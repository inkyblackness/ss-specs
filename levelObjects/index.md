## Level Objects

A level object is anything that is not part of the basic level layout (the tile map). The game knows about 476 different objects.

Objects are identified using three values. They are divided by class, subclass and type. In this documentation such an identifier will be written as ```c/s/t```. For example, the energy charge station is ```9/2/1```.

> In the original source, such an identifier is called a ```TRIPLE```.

Information about level objects exists in two places:
* Static properties, describing an object in general
* Instance data, describing the state of a specific object instance in the game

The static properties are stored in a separate file. See [Property Files](../fileFormat/PropertyFiles.md) for basic information.
Instance data is stored as part of an archive. See [Level Objects](../archives/levelObjects.md) for basic information.

### Known Classes, Subclasses and Types

The following table lists the object classes together with the available types per subclass:

| Class | Name                               | 0  | 1  | 2  | 3  | 4  | 5  | 6  | 7  |
|:-----:|------------------------------------|----|----|----|----|----|----|----|----|
|   0   | Gun                                | 5  | 2  | 2  | 2  | 3  | 2  | -  | -  |
|   1   | Ammo                               | 2  | 2  | 3  | 2  | 2  | 2  | 2  | -  |
|   2   | Physics                            | 6  | 16 | 2  | -  | -  | -  | -  | -  |
|   3   | Grenade                            | 5  | 3  | -  | -  | -  | -  | -  | -  |
|   4   | Drug                               | 7  | -  | -  | -  | -  | -  | -  | -  |
|   5   | Hardware                           | 5  | 10 | -  | -  | -  | -  | -  | -  |
|   6   | Software                           | 7  | 3  | 4  | 5  | 3  | -  | -  | -  |
|   7   | Big Stuff                          | 9  | 10 | 11 | 4  | 9  | 8  | 16 | 10 |
|   8   | Small Stuff                        | 8  | 10 | 15 | 6  | 12 | 12 | 9  | 8  |
|   9   | Fixtures                           | 9  | 7  | 3  | 11 | 2  | 3  | -  | -  |
|   10  | Door                               | 10 | 9  | 7  | 5  | 10 | -  | -  | -  |
|   11  | Animating                          | 9  | 11 | 14 | -  | -  | -  | -  | -  |
|   12  | Trap                               | 13 | 1  | 5  | -  | -  | -  | -  | -  |
|   13  | Container                          | 3  | 3  | 4  | 8  | 13 | 7  | 8  | -  |
|   14  | Critter                            | 9  | 12 | 7  | 7  | 2  | -  | -  | -  |

> This information is not stored within the resource files, it is hardcoded in the engine.


### Static Properties

The following table lists the byte sizes of the generic and specific property structures per class.
Many of these bytes are all zero in ```objprop.dat```; In these cases no detailed data definition will be linked.

| Class | Generic                                     | 0  | 1         | 2         | 3         | 4         | 5         | 6  | 7  |
|:-----:|---------------------------------------------|----|-----------|-----------|-----------|-----------|-----------|----|----|
|   0   | [2](00_Gun/gunProperties.md)                | 1  | 1         | [16][0/2] | [13][0/3] | [13][0/4] | [18][0/5] | -  | -  |
|   1   | [14](01_Ammo/ammoProperties.md)             | 1  | 1         | 1         | 1         | 1         | 1         | 1  | -  |
|   2   | [1](02_Physics/physicsProperties.md)        | 20 | [6][2/1]  | 1         | -         | -         | -         | -  | -  |
|   3   | [15](03_Grenade/grenadeProperties.md)       | 1  | [3][3/1]  | -         | -         | -         | -         | -  | -  |
|   4   | 22                                          | 1  | -         | -         | -         | -         | -         | -  | -  |
|   5   | 9                                           | 1  | 1         | -         | -         | -         | -         | -  | -  |
|   6   | 5                                           | 1  | 1         | 1         | 1         | 1         | -         | -  | -  |
|   7   | 2                                           | 1  | 1         | 1         | 1         | 1         | 1         | 1  | 1  |
|   8   | 2                                           | 1  | 1         | 1         | 1         | 1         | [6][8/5]  | 1  | 2  |
|   9   | 1                                           | 1  | 1         | 1         | 1         | 0         | 1         | -  | -  |
|   10  | 1                                           | 1  | 1         | 1         | 1         | 1         | -         | -  | -  |
|   11  | [2](11_Animating/animatingProperties.md)    | 1  | 1         | [1][11/2] | -         | -         | -         | -  | -  |
|   12  | 1                                           | 1  | 0         | 1         | -         | -         | -         | -  | -  |
|   13  | 3                                           | 1  | 1         | 1         | 1         | 1         | 1         | 1  | -  |
|   14  | [75](14_Critter/critterProperties.md)       | 3  | [1][14/1] | 1         | [6][14/3] | 1         | -         | -  | -  |


> There is also the case of objects with a specific entry length of 0. This is the case for object types that didn't make it into the game (vending machines).

[0/2]: 00_Gun/gunProperties.md#specific-2-properties
[0/3]: 00_Gun/gunProperties.md#specific-3-properties
[0/4]: 00_Gun/gunProperties.md#specific-4-properties
[0/5]: 00_Gun/gunProperties.md#specific-5-properties
[2/1]: 02_Physics/physicsProperties.md#specific-1-properties
[3/1]: 03_Grenade/grenadeProperties.md#specific-1-properties
[8/5]: 08_SmallStuff/smallStuffProperties.md#cyberspace-items-specific-properties
[11/2]: 11_Animating/animatingProperties.md#specific-2-properties
[14/1]: 14_Critter/critterProperties.md#specific-1-properties
[14/3]: 14_Critter/critterProperties.md#cyberspace-critters-specific-properties


### Object Names

The names are localized and [stored as texts](../media/Texts.md). The short names are stored in resource ```0x086D```, the long
names are in resource ```0x0024```.


### Object Bitmaps

Every object has bitmaps associated with it. The bitmaps are all stored in resource ```0x0546``` in ```objart.res```, starting with block 1 for the first object. Each object has at least 3 bitmaps associated, with additional in-world bitmaps specified as per ```Extra``` field of the [Common Object Properties](../fileFormat/PropertyFiles.md#common-table).

As a result, to determine the proper block number for a given object, the total number of previous (extra) bitmaps needs to be known.

The first image is the standard image, the second (and additional) bitmaps are in-world bitmaps, and the last one is a micro-image.

> The micro-images are sometimes fully transparent. This has been found for marker objects for instance.
> Maybe a (future) mapping unit would have been able to show objects, except those that weren't meant to be seen by the player.
> For such transparent bitmaps, the first bitmap shows some abstract presentation. For example, the "continuous trigger" has the infinity symbol as first image.
> On the contrary, using the first bitmap exclusively for editors might not be usable either as they sometimes have the same image for different types (software instance).

Objects of render type ```Fragment``` have one extra bitmap. The first one contains the colour information and the extra bitmap the depth information; It is a "gray-scale" bitmap with ```0xD0``` as the center.
