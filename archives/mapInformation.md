## Map Information

### Map layout

#### Level information

```L04```, a compressed chunk, contains the basic information about a level (a map).

**Level Information** (58 bytes)

    0000  int32     Unknown       - always 64 in the archives.
    0004  int32     Unknown       - always 64 in the archives.
    0008  int32     Unknown       - always 6 in the archives.
    000C  int32     Unknown       - always 6 in the archives.
    0010  int32     Height shift
    0014  int32     Placeholder   - ignore
    0018  int32     Cyberspace flag. 0: real life, 1: cyberspace
    001C  byte[9]   Unknown
    0025  int32     Timer value 1 - always 64 in the archives
    0029  int32     Timer count
    002D  int32     Timer value 2 - always 8 in the archives
    0031  byte[9]   Unknown

> The first four fields don't seem to be respected by the engine. A modification of these values does not show any result.
> According to the documentation of TSSHP, these should specify the map width and height. The values add up, but as long as they
> don't show any effect, they are useless.

The ```Height shift``` determines the height of the level as well as the size of one height unit for tiles.
The level is one tile width high (perfect cube) if the shift value is 5.
A shift value of 0 gives 32 tiles of height, a value of 7 one fourth (1/4) of a tile. So it's 32, 16, 8, 4, 2, 1, 1/2, 1/4 for 0..7.
> Most levels have a shift value of 3, giving 4 tile widths of height. Security (Level 8) has a shift value of 1 - 16 tiles.

One height unit is the 32th of the level height. The higher the level is, the larger are the visible jumps of any height value.

Although any level can be marked as cyberspace - even the starting level - only levels 10, 14 and 15 properly work. Apparently these level numbers are hardcoded in the engine. Cyberspace items in other levels don't work as expected, especially the exit port either crashes the game or puts the player at the start of the cyberspace level again.

As a further hardcoded limitation, only cyberspace levels 14 and 15 have a time limit. Level 10 is without limit.

The ```placeholder``` has no use in the archives.
> According to the documentation of TSSHP, this field is used as a placeholder for a pointer within the game logic.

```Timer value 1``` and ```Timer value 2``` require to be set to the values ```0x40``` and ```0x08``` respectively. Otherwise the level timers (stored in ```L06```) may not work properly.


#### Tile map

The tiles are described in the compressed chunk ```L05```, a table of tile map entries. The first entry describes the lower left tile and the next entries go east first, then north (first columns, then rows). All levels have a size of 64x64 tiles as the engine supports (and expects) only this layout.

The outermost tiles of the level (the 'border') should always be solid tiles. If these tiles are non-solid, the graphics engine may have rendering issues (walls not shown, just black) and entering these tiles freezes the game. This gives an effective available space of 62x62 tiles.

**Tile Map Entry** (16 bytes)

    0000  byte     Type
    0001  byte     Floor Info
    0002  byte     Ceiling Info
    0003  byte     Slope height. Valid for any non-flat tile.
    0004  int16    Index to first object in cross-reference table.
    0006  int16    Texture Info
    0008  uint32   Flags
    000C  [3]byte  Unknown -- first byte always 0xFF
    000F  byte     Light delta

**Type** (1 byte)

The type is an enumeration with the following constants:

    0x00  solid
    0x01  open
    0x02  diagonal open s/e (nw edge closed off)
    0x03  diagonal open s/w (ne edge closed off)
    0x04  diagonal open n/w (se edge closed off)
    0x05  diagonal open n/e (sw edge closed off)
    0x06  slope s->n
    0x07  slope w->e
    0x08  slope n->s
    0x09  slope e->w
    0x0A  valley se->nw
    0x0B  valley sw->ne
    0x0C  valley nw->se
    0x0D  valley ne->sw
    0x0E  ridge nw->se
    0x0F  ridge ne->sw
    0x10  ridge se->nw
    0x11  ridge sw->ne

All non-open tiles are given as low -> high. Valleys have 3 vertexes higher and one lower (at floor level). Ridges have one vertex higher and the remaining three at floor level. The given direction is that between the one vertex and its opposite one.


**Floor/Ceiling Info** (1 byte)

    bits  0-4     Height (ceiling is units down from 0x20)
    bits  5-6     Texture rotation. When viewed from above: 1: 90°, 2: 180°, 3: 270° - always clockwise
    bit   7       Hazard flag. Floor has biohazard, ceiling has radiation hazard.

The exact behaviour for the hazard flags is determined by the [level variables](levelVariables.md).

**Texture Info** (2 bytes)

The texture info field contains information for the tile textures of the level.
For real world, it contains index values into the texture map (see next chunk below).
Cyberspace levels specify the palette color index for floor and ceiling of the South-Western corner.

    Real World:
    bits  0-5     Wall texture
    bits  6-10    Ceiling texture
    bits  11-15   Floor texture

    Cyberspace:
    bits  0-7     Floor palette index
    bits  8-15    Ceiling palette index

**Flags** (4 bytes)

    Real World:
    0x80000000    Tile visited (automapper)
    0x10000000    Unknown
    0x0F000000    Ceiling shadow
    0x000F0000    Floor shadow
    0x0000F000    Music index
    0x00000C00    Slope control. See below.
    0x00000200    Remodeled flag ("spooky" music)
    0x00000100    Use wall texture from adjacent tile
    0x00000080    Unknown -- only used for computer room on level 7
    0x00000040    Flip wall texture horizontally alternating
    0x00000020    Flip wall texture horizontally
    0x0000001F    Offset for wall textures

    Cyberspace:
    0x01000000    Flight pull: Strong pull towards floor
    0x000F0000    Flight pull
    0x0000F000    Music Index
    0x00000C00    Slope control. See below.
    0x00000060    Game-Of-Life flags (either needs to be set)


```Ceiling shadow``` and ```Floor shadow``` specify the darkness intensity for the south-western corner of the tile.
The ceiling value darkens all four surrounding ceilings.
The walls made by surrounding top-pillars are darkened starting from the ceiling.
The walls made by surrounding bottom-pillars are darkened starting from their floor.

The floor value darkens all four surrounding floors.
The walls made by surrounding bottom-pillars are darkened starting from the floor.
The walls made by surrounding top-pillars are darkened starting from their ceiling.

> The actions to modify the light values can only give tiles more light, or remove previously added light.
> Thus, with shadow values of 0, these tiles are as bright as possible, without any chance of making them darker.


```Slope control``` specifies for the floor and ceiling how slopes (and valleys and ridges) should look like.

    0:  Ceiling is inverted to floor; A slope height of 0x1F will have ceiling and floor seal of the tile.
    1:  Ceiling is mirroring the floor; A slope height of 0x10 will have their most extent vertexes touch each other.
    2:  Ceiling is flat.
    3:  Floor is flat.

	
```Flip wall texture horizontally```

Wall textures can be flipped horizontally.

In the ```alternating``` case, flipping is based on the parities of the tile's two coordinates (even/odd status of the X and Y values).
Furthermore, North/South wall textures are flipped separately to East/West wall textures; If the North/South walls are flipped for a certain tile, the corresponding East/West walls will not be flipped. If the North/South walls are not flipped, then the East/West walls will be flipped.
> This ensures mirrored patterns are kept across corners without the need to change the flags for every other tile. This is heavily used on level 9 for the infested walls.

The North/South texture is to be flipped if the parities of the tile's two coordinates are different - East/West would be normal in this case.

The North/Shouth texture is normal if the parities of the tile's two coordinates match - East/West would be flipped in this case.

This alternation is inverted if additionally bit ```0x00000020```` is set.


```Flight pull``` specifies what default translation/rotation forces are in play

    0:   None
    1:   Weak pull towards East
    2:   Weak pull towards West
    3:   Weak pull towards North
    4:   Weak pull towards South
    5:   Medium pull towards East
    6:   Medium pull towards West
    7:   Medium pull towards North
    8:   Medium pull towards South
    9:   Strong pull towards East
    10:  Strong pull towards West
    11:  Strong pull towards North
    12:  Strong pull towards South
    13:  Medium pull towards ceiling
    14:  Medium pull towards floor
    15:  Strong pull towards ceiling
    16:  Strong pull towards floor (requires separate extra bit)

> The engine applies the force to the 'nose' of the hacker, i.e. it will pull the heading until the hacker looks straight
> at the origin of the force. This causes the implicit rotation. Only if the heading is exactly opposite to the origin,
> then no rotation happens (i.e., pulled backwards).
> All but the 'Strong' pulls can be countered.

**Light delta** (1 byte)

The high nibble specifies the light delta to the ceiling shadow. The low nibble specifies the light delta to the floor shadow.
The [Change lighting action](../levelObjects/Actions.md#action-type-7-change-lightning) modifies these values.

### Texture map

This chunk is a list of int16 texture identifier in chunk ```L07``` with a fixed length of 108 bytes.
The ```Texture Info``` fields in the tile map refer to these identifier.
Although the field for wall textures would allow for 64 textures, the chunk can only hold up to 54 textures.
The first 32 textures can be used for floor/ceiling as well.

> The engine caches these textures when reloading a savegame with the same level.
> The 'texture' index for space is 0x00CD, the observation grating has 0x00B1 (ceiling on groves and bridge)

See [Level Textures](../content/LevelTextures.md) for further information.


### Map notes

#### Text Data

Text for the map notes is stored in the level-specific chunk ```L46```. The notes are stored sequentially, with the 0x00 character terminating a note.

The maximum length of one note is 40 bytes (including the termination character). The chunk itself is pre-initialized with 0x00 bytes to a total length of 0x800 bytes.

Data in this chunk is always defragmented. If a note is modified or removed, the notes are rearranged.

#### Next available space
Chunk ```L47``` is a single uint32 offset, pointing to the first available byte in the text data chunk.
