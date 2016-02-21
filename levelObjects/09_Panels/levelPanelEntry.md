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
