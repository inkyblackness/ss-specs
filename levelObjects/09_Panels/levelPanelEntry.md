## Level Panels Table (Class 9)

```L19``` is a table describing panels and electrical stuff.

**Level Panel Entry** (30 bytes)

    0000  [6]byte   Level object prefix
    0008  [24]byte  Panel Specific Info


### Buttons 9/0/x

Buttons come in various shapes and forms. All of them are triggers for actions and the entry data that of [actions](../Actions.md).

    0000  [22]byte  Action data
    0016  uint16    Access mask


The conditions for buttons are based on [game variables](../Conditions.md#game-variable-conditions).

```Access mask``` is an additional condition for the button. If not zero, the lower five bits can specify one required access level the hacker must have. The remaining 7 bits don't work properly or resolve to the "None" level.


### Recepticles 9/1/x


### Stations 9/2/x

The conditions for stations are based on [game variables](../Conditions.md#game-variable-conditions).

#### Cyberspace Terminals 9/2/0

**Cyberspace Terminal Panel Specific Info** (24 bytes)

    0000  byte      State
    0001  byte      Unused
    0002  [4]byte   Condition
    0006  int32     Target X
    000A  int32     Target Y
    000E  int32     Target Z
    0012  int16     Target Level
    0016  [2]byte   Unused


**State** (1 byte enumeration)

    0: Off
    1: Active (Normal state)
    2: Locked. Trying to activate it gives an electrical shock.


#### Energy Charge Stations 9/2/1

    0000  [2]byte   Unused
    0002  [4]byte   Condition
    0006  [18]byte  Unknown


### Input Panels 9/3/x

The conditions for input panels are based on [game variables](../Conditions.md#game-variable-conditions).

**Input Panel Info** (24 bytes)

    0000  [2]byte   Unknown
    0002  [4]byte   Condition
    0006  [18]byte  Input panel info


#### Puzzles 9/3/0 to 9/3/3

Puzzles are either "wire" or "block" puzzles. The type of the puzzle is determined by a bit within the info structure.

##### Wire Puzzles

**Wire Puzzle Input Panel Info** (18 bytes)

    0000  int32     Target object index (object to toggle on success)
    0004  byte      Puzzle layout
    0005  uint8     Target power level out of 0xFF
    0006  uint8     Current power level; Same value as target value when solved
    0007  int8      Puzzle type - must be 0x00 for wire puzzle
    0008  uint32    Target state of wires
    000C  uint32    Current state of wires
    0010  [2]byte   Unused

**Puzzle Layout** (1 byte bitmask)

    0x0F            Wire count; 0 default to 4 wires
    0xF0            Connector count per size; 0 defaults to 6 connectors

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
    0x00000070      Y coordinate of source connector
    0x00000180      Source location
    0x00000E00      Unknown
    0x00007000      Y coordinate of destination connector
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
    4: Solid block
    5: Solid block - not used
    6: Switching node
    7: Switching node - not used


#### Elevator Panels 9/3/5

**Elevator Input Panel Info** (18 bytes)

    0000  [N]int16  Target level destination object index
    000C  int16     Accessible bitmask (which floors are accessible)
    000E  int16     Elevator shaft bitmask (which floors does the shaft connect)
    0010  [2]byte   Unknown

If a floor bit is set in the ```Elevator shaft bitmask``` field but not in the ```Accessible bitmask``` field, using
that button gives the "Shaft damage" message.

The ```Target``` field is the object index of the corresponding elevator panel in the target level. The array contains only
entries for accessible floors and is ordered from top to bottom. This means the first entry relates to the highest bit
of the accessible floors.

> Referencing an object from another level requires an editor to keep track of inter-level references. Or burden the user.
> The target array would allow for up to six entries, it is unknown if all six are supported.


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


### Vending Machines 9/4/x

These are not available.


### Cyberspace Switches 9/5/x

Cyberspace switches are triggers for actions and the entry data that of [actions](../Actions.md).

    0000  [22]byte  Action data
    0016  [2]byte   Unused

The conditions for cyberspace switches are based on [game variables](../Conditions.md#game-variable-conditions).
