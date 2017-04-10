## Level Critter Table (Class 14)

```L24``` is a table describing critters.

**Level Critter Entry** (46 bytes)

    0000  [6]byte   Level object prefix
    0006  [40]byte  Level critter data


**Level Critter Data** (40 bytes)
   
    000C  int16     state timeout

    0015  byte      Primary critter state
    0016  byte      Secondary critter state

    0020  [2]int16  Loot object indices
    0024  [2]byte   Unknown, Unused
    0026  [2]byte   Unknown


```State timeout``` is an indicator on how long the critter will stay in this state. Observed only in "cautious" state.
This time is reduced as long as the hacker is in the vicinity. When the time reaches zero, the critter becomes hostile.
The time value is typically quite low. A few high values like 0x0062 have been observed.

The ```Primary critter state``` holds the state the critter is currently behaving in. This state is also reported by the targeting tool.

**Critter State** (1 byte)

    0x00  docile
    0x01  cautious
    0x02  hostile
    0x03  cautious (?)
    0x04  attacking
    0x05  sleeping
    0x06  tranquilized
    0x07  confused
 
