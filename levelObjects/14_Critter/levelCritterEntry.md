## Level Critter Table (Class 14)

```L24``` is a table describing critters.

**Level Critter Entry** (46 bytes)

    0000  [6]byte   Level object prefix
    0006  [40]byte  Level critter data


**Level Critter Data** (40 bytes)
   
    0000  int32     Pending Z-rotation. Values up to 0x608000 seen.
    0004  int32     Fixed forward velocity. Rare values of 0x0D000 and 0x090000 seen.
    0008  [2]byte   Unknown, Unused
    000A  int16     Hastiness. Values of 0x0050 and 0x0030 seen.
    000C  int16     State timeout
    000E  int16     Unknown - set to 0x0002 for a few critters.
    0010  int16     Unknown - set to arbitrary values.
    0012  [2]byte   Unused
    0014  byte      Roaming critter state
    0015  byte      Primary critter state
    0016  byte      Secondary critter state
    0017  byte      Tertiary critter state
    0018  [6]byte   AI coordinates A
    001E  byte      Always 0xFF
    001F  byte      AI coordinates B
    0020  [2]int16  Loot object indices
    0024  [2]byte   Unknown, Unused
    0026  [2]byte   Unknown, set only for a few roaming critters.


```Pending Z-rotation``` is a rotation value that the critter wants to perform next. Only when this value is zero, it continues with its regular state.

```Fixed forward veloctiy``` is used for a few critters to give them a fixed forward speed. This speed can be different than their normal speed, and even make them slower than regular.

```Hastiness``` makes a few critter appear as if they were under the effect of a speedup-stimulant. Their actions, including their animations, are quicker.

```State timeout``` is an indicator on how long the critter will stay in this state. Observed only in "cautious" state.
This time is reduced as long as the hacker is in the vicinity. When the time reaches zero, the critter becomes hostile.
The time value is typically quite low. A few high values like 0x0062 have been observed.

```AI coordinates A``` and ```B``` appear to be some sort of coordinates. Changing them has no visible effect, so they may be scratch values for the AI, determining where to go next. They are used only for roaming critters.

```Always 0xFF``` must always be set to ```0xFF```, otherwise the critter is immediately hostile, regardless of where it is on the map. All critters in the main game have it set to ```0xFF```.

### Critter states

The state a critter is in is goverend by four bytes, which have meanings on their own, as well as combined effects. For simple cases, only the ```Primary critter state``` is used. This value is also the one modified by the "Set critter state" action.

A roaming critter has the ```Roaming critter state``` value set, and also needs other states to be set. Working combinations found so far are:
* ```03 01 01 00```
* ```02 01 01 00``` (not found in archive.dat)
A critter only roams on its own if the hacker is within a radius of about 6 tiles.

A patrolling critter follows a path laid out by ```Critter AI Hint``` marker (See [traps](../12_Trap/levelTrapEntry.md#critter-ai-hints-1207)). The first marker of such a path must be referenced in the second ```loot object index```.
> The only working example of such a critter is initially docile, and has a state combination of ```00 00 04 00```.

The ```Primary critter state``` holds the state the critter is currently behaving in. This state is also reported by the targeting tool.

**Primary Critter State** (1 byte)

    0x00  docile
    0x01  cautious
    0x02  hostile
    0x03  cautious (?)
    0x04  attacking
    0x05  sleeping
    0x06  tranquilized
    0x07  confused
 
> Various combinations of the state values can be found in the main game. While they have different effects, their pattern is not clear.
