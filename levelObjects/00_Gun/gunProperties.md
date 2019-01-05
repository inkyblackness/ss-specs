### Gun Properties

#### Generic Gun Properties

**Generic Gun Propertes** (`GunProp` struct) (2 bytes)

    0000  uint8   Fire rate
    0001  byte    Ammo type

For non-automatic guns, the `fire rate` specifies when the next bullet can be shot. For melee weapons it is the time until the next swing can be made.
It is measured in game time, with a unit for this value of about 36 milliseconds. A value of ```0x00``` means 'as fast as you can press fire'.
> Changing this value does not have any detectable effect for automatic guns (```0/1/x```).


**Ammo type** (1 byte)

    0xF0  Ammo subclass identifier
    0x0F  Bitmask, which ammo types within the ammo subclass are compatible (0..3)

> Ammo type is ignored for beam and melee weapons.


#### Specific 2 Properties

This subclass describes special projectile weapons.

**Specific 2 Properties** (`SpecialGunProp` struct) (16 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  uint8    Projectile travel speed
    0009  int32    Projectile triple (0x00CCSSTT)
    000D  uint8    Attack mass
    000E  sint16   Attack speed (kickback)


#### Specific 3 Properties

This subclass describes melee weapons.

**Specific 3 Properties** (`HandtohandGunProp` struct) (13 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  uint8    Energy usage
    0009  uint8    Attack mass (impact force - pushback on hit)
    000A  uint8    Range
    000B  sint16   Attack speed (kickback; only applied if hitting something)


#### Specific 4 Properties

This subclass describes energy beam weapons.

**Specific 4 Properties** (`BeamGunProp` struct) (13 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  uint8    Power usage
    0009  uint8    Attack mass (impact force - pushback on hit)
    000A  uint8    Range
    000B  sint16   Attack speed (kickback)


#### Specific 5 Properties

This subclass describes energy projectile weapons.

**Specific 5 Properties** (`BeamprojGunProp` struct) (18 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  uint8    Power usage
    0009  byte     Attack mass
    000A  sint16   Attack speed (kickback)
    000C  uint8    Projectile travel speed
    000D  int32    Projectile triple (0x00CCSSTT)
    0011  byte     Flags


**Energy Projectile Weapon Flags** (1 byte)

    0x02  Stun effect
