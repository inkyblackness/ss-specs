## Level Marker Table (Class 12)

```L22``` is a table describing marker items.

**Level Marker Entry** (28 bytes)

    0000  [6]byte   Level object prefix
    0006  [22]byte  Marker data

### Infrastructure 12/0/x

#### Critter AI Hints 12/0/7

**Critter AI Hint Data** (22 byte)

    0000  [6]byte   Unused; A few have the first byte 0x06
    0006  int16     Next object index
    0008  [10]byte  Unused
    0012  int16     Trigger object flag -- 0: off, 1: on
    0014  int16     Trigger object index

AI hint marker are arranged to form a loop with their ```next object index``` fields. They describe a patrolling path for critters.
If the ```trigger object flag``` is set to ```0x01```, the object identified by ```trigger object index``` will be triggered approximately 1.5 seconds after a critter entered the tile this marker is in.

> These patrolling paths appear to work only under special circumstances. They are rarely used in the main game and so far have been found to properly work only on level 9 for a mutated cyborg.
> The main reason for these hints not properly working is that roaming critters only walk when the hacker is within their vicinity - which often already makes them hostile. The cyborg on level 9 initially is docile, thus not concerned about a (passive) hacker. Its 4 state bytes are ```00 00 04 00``` and requires the second loot object to point to the first hint.
> Critters take these hints as such - they first walk into their direction, and continue to walk after passing them. Without a new hint, they will wander off in random directions. The cyborg on level 9 properly walks back and forth because its linear back-and-forth path has a hint for ever tile, and the two edges of the path are in coves, within which the cyborg bumps against the walls until properly turned around.

#### Repulsors 12/0/10

**Repulsor Data** (22 byte)

    0000  [10]byte  Unused; A few have the first byte 0x06 without effect
    000A  int32     Start height
    000E  int32     End height
    0012  int32     Repulsion flags; 0x00000001: disabled (float down); 0x00000008: strong repulsor

The ```start height``` and ```end height``` values have a unit of ```0x00010000``` for one tile height. An ```end height``` of zero means endless.
Both values are measured from the floor. Repulsion only happens if the hacker is within these limits.


#### Triggers 12/0/x (with exceptions)

The type of the trigger object determines its cause. The marker data is then that of [actions](../Actions.md).

Trigger Types:

    0: Entry;           Hacker enters tile (mid-air or per foot)
    1: Null;            Must be set off externally. Also used as data storage.
    2: Floor;           Hacker touches floor of this tile (per foot exclusively)
    3: Player Death;    Used to resurrect Hacker
    4: Deatch Watch;    When certain objects (or types of objects) are destroyed. Common for CPU nodes.
    5: Area Enter;      Unused
    6: Area Continuous; Unused
    7: This is not a trigger! See AI hints above.
    8: Level Entry;     Used for instance to initialize starting health
    9: Continuous;      Unused
    10: This is not a trigger! See repulsors above.
    11: Ecology;        Certain objects go extinct
    12: SHODAN;         Triggered when level security changes

##### Conditions

Nearly all triggers have conditions based on [game variables](../Conditions.md#game-variable-conditions).

The following exceptions exist:
* 4 (Death Watch Trigger): A union of [object type][obj-type-cond] and [object index conditions][obj-index-cond]
* 11 (Ecology Trigger): A combination of [object type][obj-type-cond] and a limit value. See below.

[obj-type-cond]: ./Conditions.md#object-type-conditions
[obj-index-cond]: ./Conditions.md#object-index-conditions


##### Ecology Triggers

These triggers watch a certain type of objects as their condition, together with a limit value. Whenever the current count of these objects is changed to a number below the limit value, the action is executed.

**Ecology Trigger Condition** (4 bytes)

    0000  byte      Type
    0001  byte      Subclass
    0002  byte      Class
    0003  byte      Limit

Note: The action is executed more than once. How often the action is executed is dependent on the delta. If the new object count equals to (limit-1), then the action is executed twice. If the new object count is less than that, the action is executed three times.

> The layout for the C/S/T is the common one in an int32 value, with the high-byte representing the limit value.

> The game watches only critters, typically to spawn new ones. The trigger also works with other classes.


### Trip Beams 12/1/x

These are not used in the game.


### Map Marker 12/2/x

Only two types are used in the game.


#### Map Notes 12/2/3

**Map Note Marker Data** (22 bytes)

    0000  [18]byte  Unused
    0012  int32     Entry offset

The ```Entry offset``` points into ```L46``` ([map notes](mapInformation.md#map-notes)) for the text entry. When a map note is removed and the whole table is defragmented, all the entry offsets are changed accordingly.


#### Music Voodoo Marker 12/2/4

**Music Voodoo Marker Data** (22 byte)

    0000  [6]byte   Unused
    0006  byte      Music flavour
    0007  [15]byte  Unused

The ```Music flavour``` is an index, with a range of ```0x00```..```0x04```, referencing patches of "music" resembling industrial noise.
They have a rectangular area of effect which covers 14x14 tiles, centered on the tile of the marker.
The music is only played if the current tile has a music index of zero.

> This is used extensively on maintenance level, the 4 flight decks, as well as for the maintenance tunnels on the groves.
