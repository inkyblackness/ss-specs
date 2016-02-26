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
