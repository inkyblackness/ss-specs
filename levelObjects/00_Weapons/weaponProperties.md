### Weapon Properties

#### Generic Weapon Properties

**Generic Weapon Propertes** (2 bytes)

    0000  uint8   Trigger time
    0001  byte    Clip description

The ```Trigger time``` is related to the firing rate. For non-automatic weapons, this time specifies when the next bullet can be shot or the next swing can be made with a melee weapon.
The unit for this value is about 36 milliseconds. A value of ```0x00``` means 'as fast as you can press fire'.
Changing this value does not have any detectable effect for automatic weapons (```0/1/x```).


**Clip Description** (1 byte)

    0xF0  Ammo subclass identifier
    0x0F  Bitmask, which ammo types within the ammo subclass are compatible (0..3)


#### Specific 2 Properties

This subclass describes projectile weapons.

**Specific 2 Properties** (16 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  int8     Projectile travel speed
    0009  int32    Projectile type (0x00CCSSTT)
    000D  byte     Unused
    000E  sint16   Kickback


#### Specific 3 Properties

This subclass describes melee weapons.

**Specific 3 Properties** (13 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  int8     Power usage
    0009  uint8    Impact force (pushback on hit)
    000A  int8     Range
    000B  sint16   Kickback (only applied if hitting something)


#### Specific 4 Properties

This subclass describes energy beam weapons.

**Specific 4 Properties** (13 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  int8     Power usage
    0009  uint8    Impact force (pushback on hit)
    000A  int8     Range
    000B  sint16   Kickback


#### Specific 5 Properties

This subclass describes energy projectile weapons.

**Specific 5 Properties** (18 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  int8     Power usage
    0009  byte     Unknown
    000A  sint16   Kickback
    000C  int8     Projectile travel speed
    000D  int32    Projectile type (0x00CCSSTT)
    0011  byte     Unknown
