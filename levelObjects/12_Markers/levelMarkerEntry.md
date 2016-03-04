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


**Trigger Marker Data** (22 bytes)

    0000  byte       Action
    0001  byte       Unknown
    0002  [4]byte    Condition
    0006  [16]byte   Trigger action details


#### Trigger Action 0: Do nothing or the default


#### Trigger Action 1: Transport player

**Transport Player Trigger Action Details** (16 byte)

    0000  int32      Target X
    0004  int32      Target Y
    0008  int32      Unknown
    000C  int32      Unknown


#### Trigger Action 3: Clone/Move Object

**Clone/Move Trigger Action Details** (16 byte)

    0000  int16      Source object index
    0002  int16      Move flag: 0: clone object, !0: move object
    0004  int32      Target X tile
    0008  int32      Target Y tile
    000C  int32      Height

The orientation and in-tile position of the source object will be kept.


#### Trigger Action 6: Trigger other objects

**Trigger Objects Trigger Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Object 1 delay
    ...
    000C  int16      Object 4 index
    000E  int16      Object 4 delay


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

