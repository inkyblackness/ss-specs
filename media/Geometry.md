## 3D Object Geometry

This chapter documents the data format of the three dimensional objects encountered in the game (tables, ICE in cyberspace, crates...).

All the geometry is stored in the file ```obj3d.res``` with content type ```0x0F```. Each chunk in this file has a directory with one data block. There is one object per chunk (and data block).

Each three dimensional ```model``` consists of a list vertex definitions and a structure of ```nodes```, with the model itself being the root node. A node contains a list of ```anchors```, with each one having a normal vector and a reference point.
Up to exactly one anchor per node may branch out to two further nodes (creating a binary tree). Further anchors contain lists of rendered ```faces```. Faces are either texture mapped or filled with a single colour.

### Binary Format

The data starts with 8 bytes of header information, followed by a list of "commands" which describe the structure.

**Geometry Header** (8 bytes)

    0000  int16  Unknown -- always 0x0027
    0002  int16  Unknown -- always 0x0008
    0004  int16  Unknown -- always 0x0002
    0006  int16  Total number of Faces

> The ```Total number of Faces``` value does not seem to be authorative. There are a few entries in the resouce file which state a higher count than actually in the model.

**Model Command** (2+N bytes)

    0000  int16  Model Command Identifier
    0002  N      Command specific content

> There is no framing structure which would allow skipping command entries, so an implementation has to know of all commands beforehand.


**fixed32/16** (4 bytes)

In the following, coordinates (and their offsets) are serialized as fixed floating point values in a 32/16 format.

    0000  sint16  Integer value
    0002  uint16  Fraction (in 1/65536 units)

> For a simple implementation, it is sufficient to read the value as a ```sint32```, convert it to floating point and divide by 65536.0 .


**Vector** (12 bytes)

A vector is serialized with three coordinates:

    0000  fixed32/16  X
    0004  fixed32/16  Y
    0008  fixed32/16  Z


### Geometry Commands

#### Vertex Definitions
Vertices are always (and only) defined at the beginning of the model (the root node). Three ways exist: define a single vertex, multiple vertices or references to previous ones with offsets.

##### Define Single Vertex

    0000  int16   Model Command Identifier -- 0x0015
    0002  int16   Unknown -- always 0x0000
    0004  Vector  Vertex coordinates

##### Define Multiple Vertices

    0000  int16      Model Command Identifier -- 0x0003
    0002  int16      Number of vertices
    0004  int16      Unknown -- always 0x0000
    0006  [N]Vector  Coordinates of vertices

##### Define Vertex (One Offset)

    0000  int16       Model Command Identifier -- X: 0x000A, Y: 0x000B, Z: 0x000C
    0002  int16       New vertex index
    0004  int16       Reference vertex index
    0006  fixed32/16  Offset value to coordinate as per command identifier

> In the existing files, the ```New vertex index``` has always been found to equal the current amount of known vertices, appending at the end of the list.

##### Define Vertex (Two Offsets)

    0000  int16       Model Command Identifier -- XY: 0x000D, XZ: 0x000E, YZ: 0x000F
    0002  int16       New vertex index
    0004  int16       Reference vertex index
    0006  fixed32/16  Offset value to first coordinate as per command identifier
    0010  fixed32/16  Offset value to second coordinate as per command identifier

> In the existing files, the ```New vertex index``` has always been found to equal the current amount of known vertices, appending at the end of the list.

> To generate a list of vertex definitions as close as possible to the original files, follow these steps:
> * Determine count of dissimilar vertices:
>    * From the list of vertices, iterate reverse from the tail.
>    * For each vertex, search the list below the current index if there is another vertex which has at least one identical coordinate.
>    * Abort this loop if the current vertex has no similar other. Vertices from beginning to the current index are all dissimilar.
> * Depending on the count of dissimilar vertices, define them with either the single definition or the multiple definition.
> * Now iterate forward through the list (until end), beginning with the first vertex after the dissimilar ones.
>    * For each vertex, again find another most similar from those coming before it (including those already handled in this loop).
>    * Compare the coordinates again.
>    * If two are identical, write a single offset definition (for the one coordinate not equal).
>    * If one is identical, write a double offset definition (for the two coordinates not equal).
>
> This list assumes the vertices are all unique and they were sorted to have the dissimilar ones come first. This must be done in
> combination with modifying the face definitions as they refer to the vertices via indices.
> Following this sequence will create a vertex definition with equal byte length as in the original. The differences to the original files are in which
> reference vertices were taken.

#### Node Control

##### End Of Node

    0000  int16  Model Command Identifier -- 0x0000

Every node is finished with this command.

##### Define Node Anchor

    0000  int16   Model Command Identifier -- 0x0006
    0002  Vector  Normal vector
    000E  Vector  Reference point
    001A  int16   Byte offset to 'left' branch
    001C  int16   Byte offset to 'right' branch

The offset values are based on the start of the command. Branch data comes only after the current node has ben finished; There is always only one ```End Of Node``` command pending. Branches are serialized depth-first.

> In the existing files always the 'right' branch has been found to be serialized first. Although only a few cases, several layers can be found within one branch. If a branch is not needed, it contains only the ```End Of Node``` command.


#### Face Control

##### Define Face Anchor

    0000  int16   Model Command Identifier -- 0x0001
    0002  int16   Anchor byte length (measured from start of this command)
    0004  Vector  Normal vector
    0010  Vector  Reference point
    001C  N       Face drawing commands

Faces within one ```Define Face Anchor``` command are either painted in a single colour (with an optional shade) or texture mapped.

##### Coloured Face

    0000  int16     Model Command Identifier -- 0x0004
    0002  int16     Vertex count
    0004  [N]int16  Vertex indices

This command always has either a ```Set Colour``` or a ```Set Colour And Shade``` command before. It may be that several ```Coloured Face``` commands refer to the same setting.

##### Set Colour

    0000  int16  Model Command Identifier -- 0x0005
    0002  int16  Colour palette index

##### Set Colour And Shade

    0000  int16  Model Command Identifier -- 0x001C
    0002  int16  Colour palette cross index
    0004  int16  Shade value (0..3)

The ```Colour palette cross index``` refers to a small lookup table from the corresponding static object properties. This lookup table is a list of always 6 palette indices.

> The data files contain cross indices of 0..6 . TSSHP skips those with value 0 and subtracts 1 from the remaining 6 values.

##### Texture Mapping

    0000  int16      Model Command Identifier -- 0x0025
    0002  int16      Number of entries
    0004  [N]Entry   Mapping entries

**Mapping Entry (10 bytes)**

    0000  int16       Vertex index
    0002  fixed32/16  texture u coordinate
    0006  fixed32/16  texture v coordinate

##### Textured Face

    0000  int16     Model Command Identifier -- 0x0026
    0002  int16     Texture identifier
    0004  int16     Vertex count
    0006  [N]int16  Vertex indices

This command always has a ```Texture Mapping``` command before.

The ```Texture identifier``` references a texture with a resource identifier base of ```0x01DB```, found in ```citmat.res```.
The value of 0 specifies special, object specific handling.
