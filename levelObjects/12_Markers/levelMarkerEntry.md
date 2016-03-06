## Level Marker Table (Class 12)

```L22``` is a table describing marker items.

**Level Marker Entry** (28 bytes)

    0000  [6]byte   Level object prefix
    0006  [22]byte  Marker data

### Triggers 12/0/x

The type of the trigger object determines its cause. The ```Action``` field of the trigger data
determines what happens. The action also specifies how the action details are to be interpreted.

Trigger Types:

    0: Entry Trigger; Hacker enters tile
    1: Null Trigger; Must be set off externally. Also used as data storage.
    2:
    3: Player Death; Used to resurrect Hacker
    8: Level Entry Trigger; Used for instance to initialize starting health


**Trigger Marker Data** (22 bytes)

    0000  byte       Action
    0001  byte       Use quota
    0002  [4]byte    Condition
    0006  [16]byte   Trigger action details

The ```Use quota``` indicates how often more this action can be executed. ```0``` means endless.
The value is only decremented if it is not zero and the condition was met.
If the value is decremented to zero after the action was executed, the object is deleted.


#### Trigger Action 0: Do nothing or the default


#### Trigger Action 1: Transport player

**Transport Player Trigger Action Details** (16 byte)

    0000  int32      Target X
    0004  int32      Target Y
    0008  int32      Unknown
    000C  int32      Unknown


#### Trigger Action 2: Change Health

**Change Health Trigger Action Details** (16 byte)

    0000  int32      Unused
    0004  int16      Health delta
    0006  int16      Health change flag; 0: remove delta, 1: add delta
    0008  int16      Power delta
    000A  int16      Power change flag; 0: remove delta, 1: add delta
    000C  [4]byte    Unknown

> Reducing health to/below 0 causes death. Reducing power below 0 causes an underflow and hacker is fully powered again.


#### Trigger Action 3: Clone/Move Object

**Clone/Move Trigger Action Details** (16 byte)

    0000  int16      Source object index
    0002  int16      Move flag: 0: clone object, !0: move object
    0004  int32      Target X tile
    0008  int32      Target Y tile
    000C  int32      Height

The orientation and in-tile position of the source object will be kept.


#### Trigger Action 4: Set Game Variable

**Set Game Variable Trigger Action Details** (16 byte)

    0000  int32      Variable key
    0004  int16      Value
    0006  int16      Operation
    0008  int32      Message 1
    000C  int32      Message 2

Depending on the ```Variable key```, different sets of variables are modified.
The variables are stored in the [Game State Structure](../../archives/gameState.md).

If ```Variable key``` has bit ```0x1000``` set, then a int16 variable is modified. The variable index is ```key & 0x003F```.
If the bit is not set, then a boolean variable is modified. The variable index is ```key & 0x01FF```.

For the int16 variables, ```Value``` must be in the range ```0..0x0FFF```. Operation can be one of the following:

    0:  Set value
    1:  Add value
    2:  Subtract value
    3:  Multiply with value
    4:  Divide by value (Resulting fractions are cut, divide by 0 is ignored)

For the boolean variables, ```Operation``` is always ```0```. The values are either ```0``` or ```1``` to set that value, or ```2``` to toggle the current value.

If the resulting value is not zero, the (optional) ```Message 1``` is played/shown. If the resulting value is zero, the (optional) ```Message 2``` is played/shown.

> There are rare occurrences of ```Variable key``` having bit ```0x2000``` set. These behave identical as for the boolean variables and can be ignored.

> Operations other than Set or Add are not used in the game for the int16 values.


#### Trigger Action 6: Trigger other objects

**Trigger Objects Trigger Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Object 1 delay
    ...
    000C  int16      Object 4 index
    000E  int16      Object 4 delay

The delays are all relative to the execution start of the action.


#### Trigger Action 7: Change lightning

**Change Lightning Trigger Action Details** (16 byte)

    0000  int16      Unknown
    0002  int16      Reference object index
    0004  int16      Transition type. 0x0000: immediate, 0x0001: fade, 0x0100: flicker
    0006  [2]byte    Unknown
    0008  [2]byte    Unknown
    000A  int16      Light surface. 0x0000: floor, 0x0001: ceiling, 0x0002: floor and ceiling
    000C  [4]byte    Unknown


#### Trigger Action 8: Effect

**Effect Trigger Action Details** (16 byte)

    0000  int16      Sound index, based on 0x00C9; 0: Play nothing
    0002  int16      Sound play count; 0: endless
    0004  int16      Visual effect
    0006  [2]byte    Unknown
    0008  int16      Additional visual effect
    000A  [2]byte    Unknown
    000C  [4]byte    Unknown

Visual Effects:

    0: None
    1: Power on: Dim level lights and fade back to normal; Plays generator sound as well
    2: Quake: Shake cam and play rumble sound
    3: Escape pod: Shows launch sequence, cam shake and abort; Plays Shodan message afterwards
    4: Red static (fullscreen)
    5: Red static (interference)

Additional Visual Effects:

    0: None
    1: White flash
    2: Pink flash
    3: Gray static (fullscreen, endless)
    4: Vertical panning (including HUD, endless)


#### Trigger Action 9: Change tile heights

    0000  int32      Tile X
    0004  int32      Tile Y
    0008  int16      Target floor height
    000A  int16      Target ceiling height
    000C  [4]byte    Unknown

For the height fields the value ```0x0FFF``` indicates "don't move". Otherwise it's the target height in level height units.


#### Trigger Action 12: Cycle Objects

**Cycle Objects Trigger Action Details** (16 byte)

    0000  [3]int32   Object indices
    000C  int32      Next object

This action triggers objects from the list of ```Object indices```. The ```Next object``` field indicates which entry to trigger next.
Afterwards, the ```Next object``` field is incremented and reset to ```0``` if either ```3``` is reached or the next object index is ```0```.


#### Trigger Action 13: Delete Object

**Delete Object Trigger Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Unknown
    0004  int16      Object 2 index
    0006  int16      Unknown
    0008  int16      Object 3 index
    000A  int16      Unknown
    000C  int32      Message index; 0: no message


#### Trigger Action 15: Receive EMail

**Receive EMail Trigger Action Details** (16 byte)

    0000  int16      EMail index, based on 0x0989
    0002  [14]byte   Unknown


#### Trigger Action 16: Change Effect

**Change Effect Trigger Action Details** (16 byte)

    0000  int16      Delta value
    0002  int16      Effect change flag; 0: add delta, 1: remove delta
    0004  int32      Effect type
    0008  [8]byte    Unknown

Effect types are:

    4: Radiation poisoning
    8: Bio contamination

> The game uses this only once to remove the radiation in level R - treatment area.


#### Trigger Action 22: Trap Message

**Trap Message Trigger Action Details** (16 byte)

    0000  sint32     Background image
    0004  int32      Message index
    0008  int32      Text colour
    000C  int32      MFD suppression flag; 0: Show in MFD, 1: Show only in HUD

The ```Background image``` and ```Text colour``` are only considered for display in MFD. The user needs to have messages set to "text" or "both" to see them.

For the ```Background image```, the following values are possible:

    -1  SHODAN (black and white)
    -2  Diego

> The game uses other image references as well, apparently they don't work.
