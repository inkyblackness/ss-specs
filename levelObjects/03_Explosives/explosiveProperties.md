### Explosive Properties

#### Generic Explosive Properties

**Generic Explosive Propertes** (15 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  byte     Unused
    0009  byte     Blast range
    000A  byte     Blast core range
    000B  byte     Blast damage
    000C  uint8    Impact force (pushback on hit)
    000D  byte     Explosive Flags
    000E  byte     Unused

The ```blast range``` specifies a radius, in tiles, how far the explosion reaches, including visual effects (EMP scroll, explosion flash).

The main damage (from the ```Generic Weapon Info```) is applied within the ```Blast core range``` of the explosion. This damage falls off from the center to the edge of the core range.

The ```blast damage``` is fully applied within the ```blast core range```, additionally to the main damage. This damage falls off from the edge of the core range to the edge of the blast range.


**Explosive Flags** (1 byte)

    0x01  Explode on contact
    0x04  Explode by timer
    0x08  Explode by vicinity

> The behaviour of explosives is not exclusively governed by the flags field. Some seems to be hardcoded.
> For instance, regular grenades marked with "timer" don't offer a time setting in the HUD and explode right after being thrown.
> Regular timed explosives marked as "contact" ignore their timer setting, explode on object-contact, but not on floor/wall contact.


#### Specific 1 Properties

**Specific 1 Properties** (3 bytes)

    0000  [3]byte  Unknown
