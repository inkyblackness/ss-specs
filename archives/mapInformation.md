## Map Information

### Map layout

#### Level information

```L04```, a compressed chunk, contains the basic information about a level (a map).

**Level Information** (58 bytes)

    0000  int32     Map width (number of tiles)  - always 64 in the archives.
    0004  int32     Map height (number of tiles) - always 64 in the archives.
    0008  int32     Unknown                      - always 6 in the archives.
    000C  int32     Unknown                      - always 6 in the archvies.
    0010  int32     Height factor                - always 3 in the archives.
    0014  int32     Placeholder                  - ignore
    0018  int32     Cyberspace flag. 0: real life, 1: cyberspace
    001C  byte[30]  Unknown

> The first four fields don't seem to be respected by the engine. A modification of these values does not show any result.
> It seems these values are a leftover from the Underworld engine.

The height factor is log2 (number of height units per tile width). For a height factor of X, a regular tile with height 2^X is a perfect cube.
> The height factor should always be 3. Although the engine does respect this value, any setting other than 3 will result in weird display.

The placeholder has no use in the archives.
> According to the documentation of TSSHP, this field is used as a placeholder for a pointer within the game logic.

#### Tile map

The tiles are described in the compressed chunk ```L05```, a table of tile map entries. The first entry describes the lower left tile and the next entries go east first, then north (first columns, then rows).

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

All non-open tiles are given as low -> high. Valleys have 3 vertices higher and one lower (at floor level). Ridges have one vertex higher and the remaining three at floor level. The given direction is that between the one vertex and its opposite one.


**Floor/Ceiling Info** (1 byte)

    bits  0-4     Height (ceiling is units down)
    bits  5-6     Orientation
    bit   7       Hazard flag. Floor has biohazard, ceiling has radiation hazard.

**Texture Info** (2 bytes)

The texture info field contains index values into the texture list for the level.

    bits  0-5     Wall texture
    bits  6-10    Ceiling texture
    bits  11-15   Floor texture

**Flags** (4 bytes)

    0x80000000    Tile visited (automapper)

**State** (4 bytes)

> tbd


### Texture map

This chunk is a list of int16 texture identifier in chunk ```L07```. The Texture Info fields in the tile map refer to these identifier.
As a result, a level can use only up to 64 textures for walls, whereas the first 32 can be used for floor/ceiling as well.

> The engine caches these textures when reloading a savegame with the same level.

### Map notes

#### Text Data

Text for the map notes is stored in the level-specific chunk ```L46```. The notes are stored sequentially, with the 0x00 character terminating a note.

The maximum length of one note is 40 bytes (including the termination character). The chunk itself is pre-initialized with 0x00 bytes to a total length of 0x800 bytes.

Data in this chunk is always defragmented. If a note is modified or removed, the notes are rearranged.

#### Next available space
Chunk ```L47``` is a single uint32 offset, pointing to the first available byte in the text data chunk.
