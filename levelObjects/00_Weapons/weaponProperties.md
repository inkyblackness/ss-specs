### Weapon Properties

#### Generic Weapon Properties

**Generic Weapon Propertes** (2 bytes)

    0000  byte    Reload time
    0001  byte    Clip description


**Clip Description** (1 byte)

    0xF0  Ammo subclass identifier
    0x0F  Bitmask, which ammo types within the ammo subclass are compatible (0..3)


#### Specific 2 Properties

This subclass describes projectile weapons.

**Specific 2 Properties** (16 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  int8     Projectile travel speed
    0009  int32    Projectile type (0x00CCSSTT)
    000D  [3]byte  Unknown


#### Specific 3 Properties

This subclass describes melee weapons.

**Specific 3 Properties** (13 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  int8     Power usage
    0009  uint8    Impact force (pushback on hit)
    000A  int8     Range
    000B  [2]byte  Unknown


#### Specific 4 Properties

This subclass describes energy beam weapons.

**Specific 4 Properties** (13 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  int8     Power usage
    0009  uint8    Impact force (pushback on hit)
    000A  int8     Range
    000B  [2]byte  Unknown

