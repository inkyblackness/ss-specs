### Critter Properties

#### Generic Critter Properties

**Generic Critter Propertes** (`CritterProp` struct) (75 bytes)
    0000  byte      Intelligence; unused
    0001  [21]byte  Primary attack info
    0016  [21]byte  Secondary attack info
    002B  uint8     Perception
    002C  uint8     Defence
    002D  sint8     Projectile source height offset
    002E  int32     Flags
    0032  byte      Mirrored views; unused
    0033  [8]uint8  Frame numbers; Amount for each posture
    003B  uint8     Animation speed
    003C  byte      Attack sound index
    003D  byte      Near sound index
    003E  byte      Hurt sound index
    003F  byte      Death sound index
    0040  byte      Notice sound index
    0041  int32     Corpse triple - 0x00CCSSTT; zero for "none"
    0045  byte      View count; for multi-view postures
    0046  byte      Secondary attack probability
    0047  byte      Disrupt probability
    0048  byte      Treasure type
    0049  byte      Hit effect -- 0x00: meat, 0x01: plant, 0x02: robot, 0x03: cyborg, 0x04: generic
    004A  byte      Attack key frame

`Perception` is a probability whether an AI detects hacker when visible during an AI interval.

The `projectile source height offset` is a vertical offset specifying from where projectiles should be launched.
> This is set to some positive values for the Elite Guard and Mutated Cyborg, so that their projectils appear from their heads.

The `animation speed` specifies the time of a single frame of an animation. A value of `0xFF` means a time of approximately 0.9 seconds. 

For all `sound indices`, a value of `0xFF` indicates "no sound".
> No enemy has a sound set for being hurt in the main game.

`View count` determines how many frames are available per viewing direction. Cyberspace entities, as well as unused critters, have this set to `0x01`, all others have `0x08`. Changing these values messes up rendering of the critter, and/or all subsequent critters. The sequence of the corresponding images will have to be adjusted accordingly.

The `secondary attack probability` determines how likely the critter will use its secondary attack when close to the hacker. `0xFF` means always, `0x00` will have the critter use the secondary attack only when farther away.

The `disrupt probability` determines how likely the critter will be interrupted when being hit. `0xFF` means always, `0x00` will have the critter execute its attacks and movement without interruptions.

`Treasure type` is an enumeration value (ranging from `0x00` to `0x0E`), pointing into a hardcoded pool of possible loot. See below for the possible constants. The concrete loot is randmoized accordingly.

The `attack key frame` value is an index into the attack animation, when the corresponding attack should take effect.


**Critter Attack Info** (`CritterAttack` struct) (21 bytes)
    0000  uint8     Damage type
    0001  uint8     Special damage type
    0002  [2]byte   Unused
    0004  int16     Damage modifier
    0006  uint8     Offence value
    0007  uint8     Penetration
    0008  uint8     Attack mass
    0009  int16     Attack velocity
    000B  uint8     Accuracy
    000C  uint8     Attack range
    000D  int32     Reload time, with 0x100 being approximately 0.9 seconds
    0011  int32     Projectile triple -- 0x00CCSSTT; zero for "none"

The `damage type` seems to be compatible with the damage type of [generic weapon info](../GenericWeaponInfo.md). Yet all damage types that affect hacker are handled equally; The two damage types `0x04` and `0x08` have no effect at all.
> The main game only uses damage types `0x01` and `0x02` (and some combinations). `0x04` is also set for Diego as well as SHODAN, though this has no further effect.

The `offence value` is compared to the defence value of the target object. See [common properties](../../fileFormat/PropertyFiles.md#common-table) for details.

An `accuracy` of `0x00` will still have the critter hit, though not that often and with less damage.

The critter will want to get within the `attack range` to perform its attack.


**Critter Properties Flags** (4 bytes)

    0x00000001      Flying

> There are further constants defined, though not in use by the engine.
> The properties file has some critters with flag `0x00000004` set, which indicated "small" critters.


**Treasure Type Enumeration** (1 byte)

    0x00            No treasure
    0x01            Humanoid
    0x02            Drone
    0x03            Assassin
    0x04            Warrior cyborg
    0x05            Flier bot
    0x06            Security 1 bot
    0x07            Exec bot
    0x08            Cyborg enforcer
    0x09            Security 2 bot
    0x0A            Elite cyborg
    0x0B            Standard corpse
    0x0C            Loot-oriented corpse
    0x0D            Electro-stuff treasure
    0x0E            Serv-bot


#### Specific 1 Properties

"Cyborg" critters.

**Specific 1 Properties** (`CyborgCritterProp` struct) (2 bytes)

    0000  int16     Shield engergy

> The `shield energy` value is unused in the engine.

#### Specific 3 Properties

These critters (`14/3/x`) are for cyberspace.

**Cyberspace Critters Specific Properties** (`CyberCritterProp` struct) (6 bytes)

    0000  [3]byte   Colour scheme
    0000  [3]byte   Alternative colour scheme

The `colour scheme`s contain 3 palette indices from the game palette. The 'alternative' colours are used for critters attacking or being hostile.
The index into this list comes from the geometry properties, specifically from the [Set Colour and Shade](../../media/Geometry#set-colour-and-shade) command.
