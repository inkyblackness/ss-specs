## Loop Configuration

```L51``` is a table containing loop configuration. 64 entries are stored in this table, with unused entries set to 0x00.

> This table appears to be automatically filled by displaying objects, such as screens and the like.
> Although ```archive.dat``` contains valid information in this table, it is rewritten by the information from the level object properties.

**LoopConfigEntry** (15 bytes)

    0000  int16     Object Table Index
    0002  byte      Loop type
    0003  [10]byte  Unused
    000D  int16     Frame time -- 0x0100 is 0.9 seconds; Default: 0x0080

**Loop Type**

    0   Don't loop, stick with first frame
    1   Loop forward
    2   Play once (on finish, stick with last frame)
    3   Loop backward
    4   Loop back and forth
    5   Loop back and forth
    6   Loop back and forth
    7   Loop back and forth
