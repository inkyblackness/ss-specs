## Level Schedules

```L06``` is a dynamically sized list of currently active schedules. A schedule performs some delayed action at a specific time in the future.
It contains a number of live entries equal to the ```timer count``` field from ```L04``` [(Level Information)](mapInformation.md#Level-information).
Unless the list is full, this list always has one extra "dummy" entry at the end.

**Level Schedule Entry** (`SchedEvent` struct) (8 bytes)

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


### Schedule Type 1: Grenade Schedule Event (`GrenSchedEvent` struct)

**Grenade Schedule Event Data** (4 bytes)

    0000  int16    Grenade object index
    0002  byte     Unique identifier
    0003  byte     Unused

The ```unique identifier``` must match the value from the (live) grenade object in the level to be handled.
This event removes the grenade object on timeout.


### Schedule Type 2: Explosion Schedule Event

This event does nothing.


### Schedule Type 3: Door Schedule Event (`DoorSchedEvent` struct)

**Door Schedule Event Data** (4 bytes)

    0000  int16    Object index
    0002  uint16   Unique identifier

If the ```object index``` references an open door, then this door will be triggered to close.

If the ```object index``` references a "plastiqued antenna relay", then this event causes the explosion and the special handling of these fixtures.


### Schedule Type 4: Trap Schedule Event (`TrapSchedEvent` struct)

**Trap Schedule Event Data** (4 bytes)

    0000  int16    Target object index
    0002  int16    Source object index

Triggers the ```target object``` first (without condition check).
If ```source object``` is not -1, it is attempted next, including condition checks.


### Schedule Type 5: Exposure Schedule Event

**Exposure Schedule Event Data** (`SchedExposeData` struct) (4 bytes)

    0000  sint8    Damage
    0001  uint8    Type
    0002  uint8    TSecs
    0003  uint8    Count -- unused

This event exposes ```type``` to the hacker for half of the amount given in ```damage```. Damage values are rounded towards the greater number.
If the calculated half is not equal to the ```damage``` value, then another event is re-scheduled with an offset of ```tSecs```.

> An earlier version of the engine appeared to use ```count``` as a counter towards zero, though this was replaced with the damage-halving algorithm.


### Schedule Types 6, 7: Floor, Ceiling Schedule Events (`HeightSchedEvent` struct)

These events share the same event data and algorithm.

**Height Schedule Event Data** (4 bytes)

    0000  sint8    Semaphor
    0001  sint8    Key
    0002  sint8    Steps Remaining
    0003  sint8    SFX Code -- unused

This event references the height transition referenced by ```semaphor``` (stored in ```L53```) which has to have given ```key``` to be valid.

```Steps remaining``` indicates both the direction, as well as how many further steps to take. When this counter reaches zero, movement stops.


### Schedule Type 8: Light Schedule Event

This event has no specific data.

It turns off any muzzle light source and restores the state of the lamp hardware.


### Schedule Type 9: Bark Schedule Event (`BarkSchedEvent` struct)

This event has no specific data.

It is scheduled by specific ```trap message``` actions (action type 22) to automatically remove the side-image after a certain time from the side-MFD.


### Schedule Type 10: EMail Schedule Event (`EmailSchedEvent` struct)

**EMail Schedule Event Data** (4 bytes)

    0000  sint16   Data Munge (EMail index)
    0002  byte[2]  Unused

This event is the delayed action of ```receive email``` actions (action type 15). This event is used if the action has a delay higher than zero.
