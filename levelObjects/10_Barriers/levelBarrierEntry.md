## Level Barrier Table (Class 10)

```L20``` is a table describing barrier items.

**Level Barrier Entry** (14 bytes)

    0000  int16   Unknown
    0002  int16   Unknown
    0004  byte    Required access level
    0005  byte    Unknown
    0006  int16   Second door object index

The ```Required access level``` indicates which [access level](../content/AccessLevels.md) the hacker needs to activate
(open) the door. If zero, no special access is required. This values is ```0xFF``` for doors "locked by SHODAN level security".
Such doors have a separate trigger referring these doors.

The ```Second door object index``` refers to a second door to trigger when activating this one.
Bulkhead doors for instance reference each other this way.

