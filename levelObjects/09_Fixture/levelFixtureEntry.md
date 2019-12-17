## Level Fixture Table (Class 9)

```L19``` is a table describing fixtures and electrical stuff.

**Level Fixture Entry** (30 bytes)

    0000  [6]byte   Level object prefix
    0008  [24]byte  Fixture Specific Info


### Buttons 9/0/x

Buttons come in various shapes and forms. All of them are triggers for actions and the entry data that of [actions](../Actions.md).

**Button Fixture Specific Info** (24 bytes)

    0000  [22]byte  Action data
    0016  uint16    Access mask


The conditions for buttons are based on [game variables](../Conditions.md#game-variable-conditions).

```Access mask``` is an additional condition for the button. If not zero, the lower five bits can specify one required access level the hacker must have. The remaining 7 bits don't work properly or resolve to the "None" level.


### Receptacles 9/1/x, 9/4/x

Most receptacles work in conjunction with the quest items (8/7/x). With the exception of antenna relay panels, they contain regular actions as data.
Unless noted otherwise below, the conditions for receptacles are based on object types, with an optional trap message if the wrong item is used.

**Receptacle Fixture Specific Info** (24 bytes)

    0000  [22]byte  Action data
    0016  [2]byte   Unused

**Receptacle Fixture Condition** (4 bytes)

    0000  byte      Object type
    0001  byte      Object subclass
    0002  byte      Object class
    0003  byte      Trap message (optional)


#### Antenna Relay Panel 9/1/3

**Antenna Relay Panel Specific Info** (24 bytes)

    0000  [6]byte   Unused
    0006  int16     Trigger object index 1
    000A  int16     Trigger object index 2
    000E  int16     Destroy object index

The two ```trigger object index``` values work only with trigger objects, they can't trigger arbitrary objects.

> Behaviour of relays is hardcoded: After accepting the plastique, their type changes to 9/1/4 and a level timer is started. When the timer expires, the panel explodes, the type changes again to 9/1/5, the two objects are triggered and the object-to-destroy is removed.


#### Retinal ID Scanner 9/1/6

When using a head item on the retinal scanner, the condition field is compared against the head's ```Current Frame``` value (for object 8/12/13), or ```Current Frame + 11``` (for object 8/12/14). Note that this is different from the ```Image index``` value which determines how the head appears in the MFD.

### Stations 9/2/x

The conditions for stations are based on [game variables](../Conditions.md#game-variable-conditions).

#### Cyberspace Terminals 9/2/0

**Cyberspace Terminal Specific Info** (24 bytes)

    0000  byte      State
    0001  byte      Unused
    0002  [4]byte   Condition
    0006  int32     Target X
    000A  int32     Target Y
    000E  int32     Target Z
    0012  int32     Target Level
    0016  [2]byte   Unused

The orientation of the player in the target cyberspace level can not be specified. The orientation will be taken over from the real world. 

**State** (1 byte enumeration)

    0: Off
    1: Active (Normal state)
    2: Locked. Trying to activate it gives an electrical shock.


#### Energy Charge Stations 9/2/1

**Energy Charge Station Specific Info** (24 bytes)

    0000  [2]byte   Unused
    0002  [4]byte   Condition
    0006  int32     Power amount
    000A  int32     Recharge time in seconds
    000E  int32     Object index
    0012  int32     Recharged timestamp
    0016  [2]byte   Unused

The object identified by ```Object index``` is triggered every time when energy is drawn from the station.

The ```Recharged timestamp``` will be set to the game time when the station is usable again. There is no dedicated timer running.


### Input Panels 9/3/x

The conditions for input panels are based on [game variables](../Conditions.md#game-variable-conditions).

**Input Panel Info** (24 bytes)

    0000  [2]byte   Unknown
    0002  [4]byte   Condition
    0006  [18]byte  Input panel info


#### Access Panels (Puzzles) 9/3/0 to 9/3/3, 9/3/9, 9/3/10

Puzzles are either "wire" or "block" puzzles. The type of the puzzle is determined by a bit within the info structure.

##### Wire Puzzles

**Wire Puzzle Input Panel Info** (18 bytes)

    0000  int32     Target object index (object to toggle on success)
    0004  byte      Puzzle layout
    0005  uint8     Target power level out of 0xFF
    0006  uint8     Wire properties mask
    0007  int8      Puzzle type - must be 0x00 for wire puzzle
    0008  uint32    Target state of wires
    000C  uint32    Current state of wires
    0010  [2]byte   Unused

**Puzzle Layout** (1 byte bitmask)

    0x0F            Wire count; 0 default to 4 wires
    0xF0            Connector count per size; 0 defaults to 6 connectors

**Wire properties mask** (1 byte bitmask)

    0x0F            Color wires; All pink if not set
    0xF0            Solved state

> For the properties mask, any of the respective four bits needs to be set to count for the whole nibble.
> ```Solved state``` is always ```0xF0```, whereas ```Color wires``` is only set as ```0x01```.

Wire states are stored as three-bit pairs, ordered from LSB to MSB (right to left). The first pair describes the first
wire and the lower triple specifies the left connector.

This allows for puzzles up to 5 wires and 8 connectors, although only up to 6 connectors are visible.


##### Block Puzzles

**Block Puzzle Input Panel Info** (18 bytes)

    0000  int32     Target object index (object to toggle on success)
    0004  int16     State store object index (refers to a null trigger)
    0006  byte      Unused - 0x00
    0007  int8      Puzzle type - must be 0x10 for block puzzle
    0008  int32     Puzzle layout
    000C  [6]byte   Unused

**Puzzle Layout** (4 byte bitmask)

    0x00000001      Puzzle solved flag ("Circuit Activated")
    0x0000000E      Unknown
    0x00000070      Coordinate of source connector
    0x00000180      Source location
    0x00000E00      Unknown
    0x00007000      Coordinate of destination connector
    0x00018000      Destination location
    0x00700000      Width
    0x07000000      Height
    0x70000000      Side effect type

The location values are 0: top, 1: bottom, 2: left, 3: right

Side effects are:

    0: Toggle all blocks horizontally/vertically from the changed one (across the board)
    1: Toggle all surrounding 8 blocks 
    2: Toggle all blocks diagonally from the changed one (across the board)
    3: Toggle all blocks horizontally, vertically and diagonally across the board
    4: None

The state of the puzzle is stored in the referred ```State store object```, a trigger with no action.

The trigger info block is repurposed and at its offset ```0006``` there are 4 uint32 fields, concatenated to a 128 bit array.
Starting with the LSB from this bit array, 3 bits are repeatedly taken to fill the blocks from left to right, top to bottom.
This allows a maximum puzzle size of 42 blocks (6x7 or 7x6).

The blocks are:

    0: Nothing (empty)
    1: Inactive (x)
    2: Active (+)
    3: Active (+) - not used
    4: Full node
    5: Full node - not used
    6: Hollow node
    7: Hollow node - not used

```Full nodes``` require only one incoming connection to forward power to the other three neighbours.
```Hollow nodes``` require two incoming connections to forward power to the other two neighbours.

#### Elevator Panels 9/3/4, 9/3/5, 9/3/6

**Elevator Input Panel Info** (18 bytes)

    0000  int16     Destination object index 2
    0002  int16     Destination object index 1
    0004  int16     Destination object index 4
    0006  int16     Destination object index 3
    0008  int16     Destination object index 6
    000A  int16     Destination object index 5
    000C  int16     Accessible bitmask (which floors are accessible)
    000E  int16     Elevator shaft bitmask (which floors does the shaft connect)
    0010  [2]byte   Unused

If a floor bit is set in the ```Elevator shaft bitmask``` field but not in the ```Accessible bitmask``` field, using
that button gives the "Shaft damage" message. The MFD is capable of displaying up to 12 floors.

The ```Destination object index``` values refer to the object index of the elevator panels in the other levels. Note the swapped order of the index values. Presumably the six values are loaded as three int32 values first, and then the index values are extracted. The Nth bit from the accessible bitmask, counted from LSB, references the Nth destination object index.

For an elevator panel to work properly (and not crash the game), the following conditions must be met:
* The panel must be in a tile which has the elevator music set. (Music index ```0xF``` in tile flags.)
* The panels envolved in a level transition must reference each other. In other words, all panels of one connected elevator must have the same configuration.

> Referencing an object from another level requires an editor to keep track of inter-level references. Or burden the user.
> The game uses only up to three active floors.

> The elevator panel on level 1 has the first byte of the input panel structure set to ```0x01```. Purpose and effect unknown.


#### Number Pads 9/3/7, 9/3/8

**Number Pad Input Panel Info** (18 bytes)

    0000  int16     3-digit combination 1
    0002  int16     Trigger object index 1
    0004  int16     3-digit combination 2
    0006  int16     Trigger object index 2
    0008  int16     3-digit combination 3
    000A  int16     Trigger object index 3
    000C  int16     Fail object index
    000E  [4]byte   Unknown

The 3-digit combinations are stored in [Binary-Coded-Decimal](https://en.wikipedia.org/wiki/Binary-coded_decimal) right-bound,
so the combination ```451``` is stored as ```0x0451```.

A combination must be something else than "000" to be active ("000" is always wrong).
Combinations must not be entered in sequence, any of the three combinations is valid at any time.

If the ```Fail object index``` refers to a valid object, this one is triggered when a wrong combination was entered.

> One number pad on level 0 has the first byte of the input panel structure set to ```0x06```. Purpose and effect unknown.


### Cyberspace Switches 9/5/x

Cyberspace switches are triggers for actions and the entry data that of [actions](../Actions.md).

    0000  [22]byte  Action data
    0016  [2]byte   Unused

The conditions for cyberspace switches are based on [game variables](../Conditions.md#game-variable-conditions).
