## Loop Configuration

```L51``` is a table containing loop configuration. 64 entries are stored in this table, with unused entries set to 0x00.

**LoopConfigEntry** (15 bytes)

    0000  int16     Unknown
	 0002  int16     Object Table Index
	 0004  byte      Loop type
	 0005  [10]byte  Unknown (only found 0x00)

**Loop Type**

    0   Don't loop, stick with first frame
	 1   Loop forward
	 2   Play once (on finish, stick with last frame)
	 3   Loop backward
	 4   Loop back and forth
    5   Loop back and forth
	 6   Loop back and forth
	 7   Loop back and forth
	 8   Don't loop, stick with first frame
	 9   Loop forward
