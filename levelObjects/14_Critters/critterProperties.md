### Critter Properties

#### Generic Critter Properties

**Generic Critter Propertes** (75 bytes)

    0000  [21]byte  Primary attack info
    0015  [21]byte  Secondary attack info

    002A  int8      Unknown

    002C  sint8     Projectile source height offset
    002D  int8      Flags -- 0x01: Hover, 0x04: Unknown (few robots have this set)

    0031  byte      Unknown -- always 0x01

    003A  byte      Frame time
    003B  byte      Attack sound index
    003C  byte      Idle sound index
    003D  byte      Pain sound index
    003E  byte      Death sound index
    003F  byte      Hostile sound index
    0040  int32     Corpse type - 0x00CCSSTT; zero for "none"
    0044  byte      Frame count
    0045  byte      Secondary attack probability
    0046  byte      Interrupt probability
    0047  byte      Random loot selection -- 0x00: nothing
    0048  byte      Injury type -- 0x00: meat, 0x01: plant, 0x02: metal, 0x03: blood (cyborgs)
    0049  byte      Primary attack key frame
    004A  byte      Secondary attack key frame

The ```projectile source height offset``` is a vertical offset specifying from where projectiles should be launched.
> This is set to some positive values for the Elite Guard and Mutated Cyborg, so that their projectils appear from their heads.

The ```frame time``` specifies the time of a single frame of an animation. A value of ```0xFF``` means a time of approximately 0.9 seconds. 

For all ```sound indices```, a value of ```0xFF``` indicates "no sound".
> No enemy has a sound set for pain in the main game.

```Frame count``` appears to determine how many frames are available per viewing direction. Cyberspace entities, as well as unused critters, have this set to ```0x01```, all others have ```0x08```. Changing these values messes up rendering of the critter, and/or all subsequent critters. The sequence of the corresponding images will have to be adjusted accordingly.

The ```secondary attack probability``` determines how likely the critter will use its secondary attack when close to the hacker. ```0xFF``` means always, ```0x00``` will have the critter use the secondary attack only when farther away.

The ```interrupt probability``` determines how likely the critter will be interrupted when being hit. ```0xFF``` means always, ```0x00``` will have the critter execute its attacks and movement without interruptions.

> The ```random loot selection``` appears to be an enumeration value (ranging from ```0x00``` to ```0x0E```), pointing into a hardcoded pool of possible loot.
> Which parameters go into the loot selection are not known - meaning whether the engine considers the current level, or the state of the hacker (health, inventory). As a rule of thumb, the quality of the loot somewhat increases with the number, though there are exceptions.
> Refer to the set values of the main game to see how they are used.

> The ```injury type``` ```0x02``` (metal) is also set for all cyberspace critters.

The two ```attack key frame``` values are indices into the attack animation, when the corresponding attack should take effect.


**Critter Attack Info** (21 bytes)
    0000  byte      Damage type
    0004  int16     Damage
    0006  byte      Offence value
    0007  byte      Unknown
    0008  uint8     Impact force
    0009  int16     Unknown
    000B  byte      Hit chance
    000C  byte      Attack range
    000D  int16     Reload time, with 0x100 being approximately 0.9 seconds
    000F  [2]byte   unused
    0011  int32     Projectile type -- 0x00CCSSTT; zero for "none"

The ```damage type``` seems to be compatible with the damage type of [generic weapon info](../GenericWeaponInfo.md). Yet all damage types that affect hacker are handled equally; The two damage types ```0x04``` and ```0x08``` have no effect at all.
> The main game only uses damage types ```0x01``` and ```0x02``` (and some combinations). ```0x04``` is also set for Diego as well as SHODAN, though this has no further effect.

The ```offence value``` is compared to the defence value of the target object. See [common properties](../../fileFormat/PropertyFiles.md#common-table) for details.

The ```impact force``` has hardly an effect on hacker. Even at the highest value ```0xFF```, hacker is pushed only a bit.

> The ```unknown``` field at ```0x0009``` may be a kickback value, though this value does not appear to have any effect on the critters.

A ```hit chance``` of ```0x00``` will still have the critter hit, though not that often and with less damage.

The critter will want to get within the ```attack range``` to perform its attack. 

#### Specific 1 Properties

**Specific 1 Properties** (1 byte)

    0000  [1]byte   Unknown


#### Cyberspace Critters Specific Properties

These critters (```14/3/x```) are for cyberspace.

**Cyberspace Critters Specific Properties** (6 bytes)

    0000  [6]byte   Colour scheme

```Colour scheme``` contains 6 palette indices from the game palette. The index into this list comes from the geometry properties, specifically from the [Set Colour and Shade](../../media/Geometry#set-colour-and-shade) command.
