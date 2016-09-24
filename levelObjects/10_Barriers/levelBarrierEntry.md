## Level Barrier Table (Class 10)

```L20``` is a table describing barrier items.

**Level Barrier Entry** (14 bytes)

    0000  [6]byte   Level object prefix
    0006  [8]bytes  Barrier Data


**Door Barrier Data** (8 bytes)

    0000  int16    Lock variable index
    0002  byte     Lock message
    0003  byte     Force door colour index
    0004  byte     Required access level
    0005  byte     Auto-close time; in units of approximately 0.5 seconds - common default is 0x0C
    0006  int16    Second door object index


The ```Lock variable index``` determines whether a door is locked. If the referenced boolean variable is 1, then the door won't open when used. While locked, the message according to ```Lock message``` is shown (```0x00``` gives the default "The door is locked").
If the door is triggered by other means, the door is unlocked and the index is reset to zero.

The ```Required access level``` indicates which [access level](../content/AccessLevels.md) the hacker needs to activate
(open) the door. If zero, no special access is required. This values is ```0xFF``` for doors "locked by SHODAN level security".
Such doors have a separate trigger referring these doors.

The ```Second door object index``` refers to a second door to trigger when activating this one.
Bulkhead doors for instance reference each other this way.
