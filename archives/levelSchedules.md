## Level Schedules

```L06``` is a dynamically sized list of currently active schedules. A schedule performs some delayed action at a specific time in the future.
It contains a number of live entries equal to the ```timer count``` field from ```L04``` [(Level Information)](mapInformation.md#Level-information).
Unless the list is full, this list always has one extra "dummy" entry at the end.

**Level Schedule Entry** (8 bytes)

    0000  int16    Timestamp, with a scale of 0x100 = 10 seconds
    0002  int16    Type
    0004  byte[4]  Data

```Time``` specifies the timestamp accoring to in-game time. Should the level become activated (either through loading of savegame load or level transition) and the current time is beyond the trigger time, the object is immediately triggered.

```Type``` specifies how the schedule should be handled on timeout. ```Data``` is then type-specific.

> The one dummy entry is required to exist, and does not have to contain any useful information at the beginning (can be all zero).
> The engine sorts the schedule entries according to their ```trigger time```.

For the schedule data, any object index refers to the master object table in the level.

### Schedule Type 0: Null Schedule Event

This event does nothing.


### Schedule Type 1: Grenade Schedule Event

**Grenade Schedule Event Data** (4 bytes)

    0000  int16    Grenade object index
    0002  byte     Unique identifier
    0003  byte     Unused

The ```unique identifier``` must match the value from the (live) grenade object in the level to be handled.
This event removes the grenade object on timeout.


### Schedule Type 2: Explosion Schedule Event

This event does nothing.


### Schedule Type 3: Door Schedule Event

**Door Schedule Event Data** (4 bytes)

    0000  int16    Object index
    0002  uint16   Unique identifier

If the ```object index``` references an open door, then this door will be triggered to close.

If the ```object index``` references a "plastiqued antenna relay", then this event causes the explosion and the special handling of these fixtures.


### Schedule Type 4: Trap Schedule Event

**Trap Schedule Event Data** (4 bytes)

    0000  int16    Target object index
    0002  int16    Source object index

Triggers the ```target object``` first (without condition check).
If ```source object``` is not -1, it is attempted next, including condition checks.

