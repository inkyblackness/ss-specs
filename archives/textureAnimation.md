## Texture Animation

```L42``` is a table containing animation information for textures. 4 entries are stored in this table.

The ```Animation Group``` field of the [Texture Properties](../fileFormat/PropertyFiles#properties-entry) specifies
the index into this table. A wall that has such a texture set will use further textures. According to the
```Number of frames``` field of this table entry, more textures are used from the [Texture Map](mapInformation.md#texture-map).

Meaning, if the level uses texture X which has an animation group set, the corresponding animation entry specifies 
how many further textures are used for this animation. These then have all to be next to each other in the texture map.

The properties of the first texture determine how the remaining will behave (climbable, ...)

> Since most textures have their animation group set to 0x00 and have no animation, the first entry in this
> table is unused and the bytes always 0.

**Animation Entry** (7 bytes)

    0000  int16  Frame time (in milliseconds)
    0002  int16  Current frame time
    0004  byte   Current frame index
    0005  byte   Number of frames for loop
    0006  byte   Loop type

**Loop Type** (1 byte)

    0  Loop forward
    1  Loop back and forth
