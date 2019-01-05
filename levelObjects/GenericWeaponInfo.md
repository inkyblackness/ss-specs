### Generic Weapon Info

This structure is reused for various offensive items.

**Generic Weapon Info** (`COMBAT_DATA_FIELDS` macro) (8 bytes)

    0000  sint16   Damage
    0002  byte     Offence value
    0003  byte     Damage type
    0004  byte     Special damage type
    0005  [2]byte  Unused
    0007  uint8    Armour penetration

The `offence value` is compared to the defence value of the target object. See [Common Object Properties](../fileFormat/PropertyFiles.md#common-table) for details.

If `Armour penetration` is equal or higher than the `Armour` value of the object being hit, the full `Damage` is applied. Otherwise, the difference is subtracted from `Damage`. If the resulting value reaches 0 or lower, "No Damage" is displayed. 

`Special damage type` matches the `Special vulnerabilities` sub-fields from [Common Object Properties](../fileFormat/PropertyFiles.md#common-table);
It is split up into `0x0F` primary damage (double damage) and `0xF0` super damage (quad damage). 

> Similar to the vulnerabilities in the common object properties, the damage type is a single 32bit value from `0003` in the source.


**Damage Type Bitfield** (1 byte)

    0x01  Explosion (impact)
    0x02  Energy
    0x04  Magnetic (EMP)
    0x08  Radiation
    0x10  Gas
    0x20  Tranquilizer
    0x40  Needle
    0x80  Bio
