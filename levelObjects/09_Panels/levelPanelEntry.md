## Level Panels Table (Class 9)

```L19``` is a table describing panels and electrical stuff.

**Level Panel Entry** (30 bytes)

    0000  [6]byte   Level object prefix
    0006  [6]byte   Panel Common Info
    0008  [18]byte  Panel Specific Info

**Panel Common Info** (6 bytes)

    0000  int16     Unknown
    0002  int16     Unknown
    0004  int16     Unknown

### Buttons 9/0/x

### Cyberspace Terminals 9/2/0

**Cyberspace Terminal Panel Specific Info** (18 bytes)

    0000  int32     Target X
    0004  int32     Target Y
    0008  int32     Target Z
    000C  int16     Target Level
    000E  [4]byte   Unknown

The first field of the common info determines the terminal state:

    0: Off
    1: Active (Normal state)
    2: Locked. Trying to activate it gives an electrical shock.

### Puzzles 9/3/0 to 9/3/3

Puzzles are either "wire" or "block" puzzles. The type of the puzzle is determined by a bit within the info structure.

#### Wire Puzzles

#### Block Puzzles

**Block Puzzle Panel Specific Info** (18 bytes)

    0000  int32     Target object index (object to toggle on success)
    0004  int16     State store object index (refers to a null trigger)
    0006  int16     Puzzle type - must be 0x1000 for block puzzle
    0008  int32     Puzzle layout

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


### Elevator Panels 9/3/5

**Elevator Panel Specific Info** (18 bytes)

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

### Number Pads 9/3/7

**Number Pad Panel Specific Info** (18 bytes)

    0000  int16     3-digit combination, stored in [Binary-Coded-Decimal](https://en.wikipedia.org/wiki/Binary-coded_decimal)
    0002  int16     Object index of object to trigger on success
    0004  [14]byte  Unknown

The 3-digit combination is stored right-bound, so the combination ```451``` is stored as ```0x0451```.
