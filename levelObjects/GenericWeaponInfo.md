### Generic Weapon Info

This structure is reused for various offensive items.

**Generic Weapon Info** (8 bytes)

    0000  sint16   Damage
    0002  byte     Unknown
    0003  byte     Damage type
    0004  byte     Unknown
    0005  [2]byte  Unused
    0007  byte     Unknown


**Damage Type Bitfield** (1 byte)

    0x01  Impact
    0x02  Energy
    0x04  EMP
    0x08  Ion
    0x10  Gas
    0x20  Tranquilizer
    0x40  Needle
