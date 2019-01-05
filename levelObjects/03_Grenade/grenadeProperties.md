### Grenade Properties

#### Generic Grenade Properties

**Generic Grenade Propertes** (`GrenadeProp` struct) (15 bytes)

    0000  [8]byte  [Generic Weapon Info](../GenericWeaponInfo.md)
    0008  byte     Touchiness
    0009  byte     Blast radius
    000A  byte     Blast core range
    000B  byte     Blast damage
    000C  uint8    Attack mass (impact force - pushback on hit)
    000D  int16    Grenade Flags

`Touchiness` defines how likely a grenade is going to chain-explode if another was exploding nearby.
> This value influences a calculation, based on a random value, as well as damage percentage. If the resulting number is higher than 7, then a chain-reaction is caused.
> In the main game, this value is always `0x00`.

`Blast radius` specifies in tiles how far the explosion reaches, including visual effects (EMP scroll, explosion flash).

The main damage (from the `Generic Weapon Info`) is applied within the `Blast core range` of the explosion. This damage falls off from the center to the edge of the core range.

The `blast damage` is fully applied within the `blast core range`, additionally to the main damage. This damage falls off from the edge of the core range to the edge of the blast radius.


**Grenade Flags** (2 bytes)

    0x01  Explode on contact
    0x02  unused
    0x04  Explode by timer
    0x08  Explode by vicinity (mine)

> The behaviour of grenades is not exclusively governed by the flags field. Some seems to be hardcoded.
> For instance, regular grenades marked with "timer" don't offer a time setting in the HUD and explode right after being thrown.
> Regular timed grenades marked as "contact" ignore their timer setting, explode on object-contact, but not on floor/wall contact.


#### Specific 1 Properties

Properties for timed grenades.

**Specific 1 Properties** (`TimedGrenadeProp` struct) (3 bytes)

    0000  uint8    Minimum time
    0001  uint8    Maximum time
    0002  uint8    Timing deviation -- always 0x00

`Minimum time` and `maximum time` specify the range, in seconds, timed grenades can be set to.

> `Timing deviation` is always set to 0x00. Since it may also let the grenade go off earlier than expected, this may happen while still in the hand - which is presumably why it is unused in the main game.
