### Weapon Properties

#### Generic Weapon Properties

**Generic Weapon Propertes** (2 bytes)

    0000  byte    Firing rate
    0001  byte    Clip description


**Clip Description** (1 byte)

    0xF0  Ammo subclass identifier
    0x0F  Bitmask, which ammo types within the ammo subclass are compatible (0..3)

