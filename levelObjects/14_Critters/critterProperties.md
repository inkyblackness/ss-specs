### Critter Properties

#### Generic Critter Properties

**Generic Critter Propertes** (75 bytes)

    0000  [21]byte  Primary attack info
    0015  [21]byte  Secondary attack info

    003A  byte      Frame time
    003B  byte      Attack sound index
    003C  byte      Idle sound index
    003D  byte      Pain sound index
    003E  byte      Death sound index
    003F  byte      Hostile sound index
    0040  int32     Corpse type - 0x00CCSSTT; zero for "none"

    0045  byte      Secondary attack probability

    0047  byte      Random loot selection -- 0x00: nothing
    0048  byte      Injury type -- 0x00: meat, 0x01: plant, 0x02: metal, 0x03: blood (cyborgs)

The ```frame time``` specifies the time of a single frame of an animation. A value of ```0xFF``` means a time of approximately 0.9 seconds. 

For all ```sound indices```, a value of ```0xFF``` indicates "no sound".
> No enemy has a sound set for pain in the main game.

The ```secondary attack probability``` determines how likely the critter will use its secondary attack when close to the hacker. ```0xFF``` means always, ```0x00``` will have the critter use the secondary attack only when farther away.

> The ```random loot selection``` appears to be an enumeration value (ranging from ```0x00``` to ```0x0E```), pointing into a hardcoded pool of possible loot.
> Which parameters go into the loot selection are not known - meaning whether the engine considers the current level, or the state of the hacker (health, inventory). As a rule of thumb, the quality of the loot somewhat increases with the number, though there are exceptions.
> Refer to the set values of the main game to see how they are used.

> The ```injury type``` ```0x02``` (metal) is also set for all cyberspace critters.


**Critter Attack Info** (21 bytes)

    0011  int32     Projectile type -- 0x00CCSSTT; zero for "none"



#### Specific 1 Properties

**Specific 1 Properties** (1 byte)

    0000  [1]byte   Unknown


#### Cyberspace Critters Specific Properties

These critters (```14/3/x```) are for cyberspace.

**Cyberspace Critters Specific Properties** (6 bytes)

    0000  [6]byte   Colour scheme

```Colour scheme``` contains 6 palette indices from the game palette. The index into this list comes from the geometry properties, specifically from the [Set Colour and Shade](../../media/Geometry#set-colour-and-shade) command.
