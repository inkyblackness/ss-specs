## Level Marker Table (Class 12)

```L22``` is a table describing marker items.

**Level Marker Entry** (28 bytes)

    0000  [6]byte   Level object prefix
    0006  [22]byte  Marker data

### Repulsors 12/0/10

**Repulsor Data** (22 byte)

    0000  [10]byte  Unused; A few have the first byte 0x06 without effect
    000A  [4]byte   Unknown
    000E  byte      Unused
    000F  int16     Repulsion height; 0x0000: endless, 0x0100: one tile height
    0011  byte      Unused
    0012  int32     Repulsion flags; 0x00000001: disabled (float down); 0x00000008: strong repulsor

### Critter AI Hints 12/0/7



### Triggers 12/0/x (with exceptions)

The type of the trigger object determines its cause. The marker data is then that of [actions](../Actions.md).

Trigger Types:

    0: Entry Trigger; Hacker enters tile
    1: Null Trigger; Must be set off externally. Also used as data storage.
    2:
    3: Player Death; Used to resurrect Hacker
    8: Level Entry Trigger; Used for instance to initialize starting health
    10: This is not a trigger! See repulsors above.

#### Conditions

Nearly all triggers have conditions based on [game variables](../Conditions.md#game-variable-conditions).

The following exceptions exist:
* 4 (Death Watch Trigger): A union of [object type][obj-type-cond] and [object index conditions][obj-index-cond]
* 11 (Ecology Trigger): Unknown

[obj-type-cond]: ./Conditions.md#object-type-conditions
[obj-index-cond]: ./Conditions.md#object-index-conditions
