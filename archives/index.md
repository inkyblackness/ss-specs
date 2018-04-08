## Archives
This section covers the archive files. There is one template archive file, called ```archive.dat```, and a number of save-game files, called ```savgamXX.dat```. All of these files are regular resource files.

Any new game is based on the archive.dat file. All these archive files contain the complete game state, including the level layout. In other words: Citadel itself, with all its content, is represented through such a file; Properties and media for the content is in the other resource files.

## Map specific data
Each level has 98 resources reserved, but only 51 in use. The level-specific resources are grouped according to the level identifier. For example, on Citadel level R, the resources start with ID ```4002``` (decimal) up to ```4053```. Level 1 uses ```4102``` to ```4153``` and so forth.

On the following pages, resources identified as ```LXX``` refer to level-specific resource ```XX``` for any level. These numbers will always be given in decimal.

## Extra Resources

### Name of archive, resource 0x0FA0 (4000)
This resource contains the name of the archive (title of the save-game). It is a 0x00 terminated string, with a dynamic length of up to 32 bytes (including the termination character).

> The archive.dat file contains extra (unknown) data after the name in this resource, up to a complete length of 128 bytes. A save-game file has only the name in this resource. The name of the template archive is "Starting Game".

### [Game state resource 0x0FA1 (4001)](gameState.md)
This resource contains information about the game and the hacker.
It also contains the [level variables](levelVariables.md).

### Additional Resources

Save-game files contain 4 further resources that are not present in archive.dat :

* 0x0000 Unknown int32. ```0x00000000``` at first investigation
* 0x024E Unknown.
* 0x024F Unknown.
* 0x0F9F Unknown int32. ```0x00004444``` at first investigation

## Index

### By topic
  * [Map Information](mapInformation.md)
  * [Level Objects](levelObjects.md)
  * [Surveillance Cameras](surveillanceCameras.md)
  * [Loop Configuration](loopConfiguration.md)


### By level-specific resource ID
  * ```L02``` Map version number. int32 with value ```0x0000000B``` for all maps.
  * ```L03``` Object version number. int32 with value ```0x0000001B``` for all maps.
  * ```L04``` [Basic level information](mapInformation.md)
  * ```L05``` [Map Layout](mapInformation.md)
  * ```L06``` [Level timer](levelTimer.md)
  * ```L07``` [Level texture map](mapInformation.md)
  * ```L08``` [Master object table](levelObjects.md#level-object-table)
  * ```L09``` [Level object cross-reference table](levelObjects.md#level-object-cross-reference-table)
  * ```L10``` to ```L24``` 15 object [class specific tables](levelObjects.md#class-tables-and-entries)
  * ```L25``` to ```L39``` 15 object [class default info](levelObjects.md#class-default-information)

  * ```L40``` Miscellaneous savefile version number. int32. For all maps: ```CD-Release```: ```0x0000000D```. All other: ```0x0000000B```
  * ```L41``` Unused entry. byte with value ```0x00``` for all maps.

  * ```L42``` [Texture animation](textureAnimation.md)
  * ```L43``` [Surveillance Sources](surveillanceCameras.md)
  * ```L44``` [Surveillance Surrogates](surveillanceCameras.md)
  * ```L45``` [Level variables](levelVariables.md)
  * ```L46``` [Map notes](mapInformation.md#map-notes)
  * ```L47``` [Map notes pointer](mapInformation.md#map-notes)

  * ```L48``` Unknown. 0x30 byte length.
  * ```L49``` Unknown. 0x1C0 byte length.
  * ```L50``` Unknown int16. ```0x0000``` for all maps.
  * ```L51``` [Loop Configuration](loopConfiguration.md)
  * ```L52``` Unknown int16. (Only present in ```CD-Release```)
  * ```L53``` Unknown. 0x40 byte length, all zeroes. (Only present in ```CD-Release```)


### Levels of Citadel
This is the level list of the base game:

* Level 00: Reactor level
* Level 01-09: Levels 1-9
* Level 10: SHODAN cyberspace
* Level 11: Delta grove
* Level 12: Alpha grove
* Level 13: Beta grove
* Level 14: Cyberspace for levels 1 and 2
* Level 15: Cyberspace
