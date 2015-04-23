## Map Information

### Map layout

#### Tile map

The tiles are described in ```L05```, a table of tile map entries. The first entry describes the lower left tile and the next entries go west first, then north (first columns, then rows).

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

### Map notes

#### Text Data

Text for the map notes is stored in the level-specific chunk ```L46```. The notes are stored sequentially, with the 0x00 character terminating a note.

The maximum length of one note is 40 bytes (including the termination character). The chunk itself is pre-initialized with 0x00 bytes to a total length of 0x800 bytes.

Data in this chunk is always defragmented. If a note is modified or removed, the notes are rearranged.

#### Next available space
Chunk ```L47``` is a single uint32 pointing to the first available byte in the text data chunk.
