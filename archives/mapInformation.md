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
    001C  byte[30]  Unknown

> The first four fields don't seem to be respected by the engine. A modification of these values does not show any result.
> According to the documentation of TSSHP, these should specify the map width and height. The values add up, but as long as they
> don't show any effect, they are useless.

The ```Height shift``` determines the height of the level as well as the size of one height unit for tiles.
The level is one tile width high (perfect cube) if the shift value is 5.
A shift value of 0 gives 32 tiles of height, a value of 7 one fourth (1/4) of a tile. So it's 32, 16, 8, 4, 2, 1, 1/2, 1/4 for 0..7.
> Most levels have a shift value of 3, giving 4 tile widths of height. Security (Level 8) has a shift value of 1 - 16 tiles.

One height unit is the 32th of the level height. The higher the level is, the larger are the visible jumps of any height value.

The ```placeholder``` has no use in the archives.
> According to the documentation of TSSHP, this field is used as a placeholder for a pointer within the game logic.

#### Tile map

The tiles are described in the compressed chunk ```L05```, a table of tile map entries. The first entry describes the lower left tile and the next entries go east first, then north (first columns, then rows). All levels have a size of 64x64 tiles as the engine supports (and expects) only this layout.

The outermost tiles of the level (the 'border') should always be solid tiles. If these tiles are non-solid, the graphics engine may have rendering issues (walls not shown, just black) and entering these tiles freezes the game. This gives an effective available space of 62x62 tiles.

**Tile Map Entry** (16 bytes)

    0000  byte    Type
    0001  byte    Floor Info
    0002  byte    Ceiling Info
    0003  byte    Slope height. Valid for any non-flat tile.
    0004  int16   Index to first object in cross-reference table.
    0006  int16   Texture Info
    0008  uint32  Flags
    000C  uint32  State

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

    bits  0-4     Height (ceiling is units down)
    bits  5-6     Texture rotation. When viewed from above: 1: 90°, 2: 180°, 3: 270° - always clockwise
    bit   7       Hazard flag. Floor has biohazard, ceiling has radiation hazard.

The exact behaviour for the hazard flags is determined by the [level variables](levelVariables.md).

**Texture Info** (2 bytes)

The texture info field contains information for the tile textures of the level.
For real world, it contains index values into the texture map (see next chunk below).
Cyberspace levels specify the 'animation' type for floor and ceiling.

    Real World:
    bits  0-5     Wall texture
    bits  6-10    Ceiling texture
    bits  11-15   Floor texture

    Cyberspace:
    bits  0-7     Floor animation
    bits  8-15    Ceiling animation

**Flags** (4 bytes)

    Real World:
    0x80000000    Tile visited (automapper)
    0x0F000000    Ceiling Shadow
    0x000F0000    Floor shadow
    0x0000F000    Music Index
    0x00000C00    Slope control. See below.
    0x00000200    Remodeled flag ("spooky" music)
    0x00000100    Use wall texture from adjacent tile
    0x00000080    Unknown
    0x00000040    Unknown
    0x00000020    Unknown
    0x0000001F    Offset for wall textures

    Cyberspace:
    0x000F0000    Flight pull
    0x0000F000    Music Index
    0x00000060    Game-Of-Life flags (either needs to be set)

```Slope control``` specifies for the floor and ceiling how slopes (and valleys and ridges) should look like.

    0:  Ceiling is inverted to floor; A slope height of 0x1F will have ceiling and floor seal of the tile.
    1:  Ceiling is mirroring the floor; A slope height of 0x10 will have their most extent vertexes touch each other.
    2:  Ceiling is flat.
    3:  Floor is flat.

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

> The engine applies the force to the 'nose' of the hacker, i.e. it will pull the heading until the hacker looks straight
> at the origin of the force. This causes the implicit rotation. Only if the heading is exactly opposite to the origin,
> then no rotation happens (i.e., pulled backwards).
> All but the 'Strong' pulls can be countered.

**State** (4 bytes)

Default value is ```0x000000FF```.


### Texture map

This chunk is a list of int16 texture identifier in chunk ```L07``` with a fixed length of 108 bytes.
The ```Texture Info``` fields in the tile map refer to these identifier.
Although the field for wall textures would allow for 64 textures, the chunk can only hold up to 54 textures.
The first 32 textures can be used for floor/ceiling as well.

> The engine caches these textures when reloading a savegame with the same level.
> The 'texture' index for space is 0x00CD, the observation grating has 0x00B1 (ceiling on groves and bridge)

### Map notes

#### Text Data

Text for the map notes is stored in the level-specific chunk ```L46```. The notes are stored sequentially, with the 0x00 character terminating a note.

The maximum length of one note is 40 bytes (including the termination character). The chunk itself is pre-initialized with 0x00 bytes to a total length of 0x800 bytes.

Data in this chunk is always defragmented. If a note is modified or removed, the notes are rearranged.

#### Next available space
Chunk ```L47``` is a single uint32 offset, pointing to the first available byte in the text data chunk.
