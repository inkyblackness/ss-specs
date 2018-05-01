## Level Schedules

```L06``` is a dynamically sized list of currently active schedules. A schedule triggers a level object at a specific time in the future.
It contains a number of live entries equal to the ```Timer count``` field from ```L04``` [(Level Information)](mapInformation.md#Level-information).
Unless the list is full, this list always has one extra "dummy" entry at the end.

**Level Schedule Entry** (8 bytes)

    0000  int16    Timestamp, with a scale of 0x100 = 10 seconds
    0002  int16    Type
    0004  byte[4]  Data

```Time``` specifies the timestamp accoring to in-game time. Should the level become activated (either through loading of savegame load or level transition) and the current time is beyond the trigger time, the object is immediately triggered.

```Type``` specifies how the schedule should be handled on timeout. ```Data``` is then type-specific.

> The one dummy entry is required to exist, and does not have to contain any useful information at the beginning (can be all zero).
> The engine sorts the schedule entries according to their ```trigger time```.
