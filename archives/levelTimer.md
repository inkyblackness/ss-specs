## Level Timer

```L06``` is a dynamically sized list of currently active timers. A timer triggers a level object at a specific time in the future.
The list always has one "dummy" entry, plus a number of live entries equal to the ```Timer count``` field in ```L04``` [(Level Information)](mapInformation.md#Level_information).

**Level Timer Entry** (8 bytes)

    0000  int16  Trigger time, with a scale of 0x100 = 10 seconds
    0002  int16  Unknown
    0004  int16  Object index of object to trigger
    0006  int16  Unknown

```Trigger time``` specifies the timestamp accoring to in-game time. Should the level become activated (either through loading of savegame load or level transition) and the current time is beyond the trigger time, the object is immediately triggered.


> The one dummy entry is required to exist, and does not have to contain any useful information at the beginning (can be all zero).
> How the engine sorts these entries and how the dummy entry is identified is not clear; This dummy entry may appear at any index of the list.
