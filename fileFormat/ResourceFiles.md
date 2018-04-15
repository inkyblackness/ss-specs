## Resource Files
Resource files are structured archives. These files contain one or more "resource" entries. Resources are identified and contain one or more data blocks. A resource with more than one data block is called a "compound resource".

### Identifier

A resource identifier is a ```uint16``` value, and a compound resource uses a ```uint16``` value as index.

Resource identifier are "valid" from value ```0x0003``` and above.
```0x0000``` is the NULL identifier (it means that this resource will be erased from file), and 1 and 2 are used engine-internally.

### General Format
A resource file consists of a header, resources and a file directory. The header points to the file offset of the file directory and the file directory points to the file offset of the first resource.

    |    /--------------------------------------------v            |
    | Header | Res0     | Res1        | ... | Res N   | Directory  |
    |        ^----------------------------------------------/      |

All resources, as well as the file directory, start at a 4-byte boundary.

To calculate the file offset of a resource, the file directory has to be read first to determine the offsets (and lengths) of the previous resources.

> Although the format would allow for a different order, all resource files have the directory at the end of the file.

### File Header
The file header has a length of 128 bytes and has the following format:

**File Header** (`ResFileHeader` struct, 128 bytes)

    0000  [16]byte  signature  signature "LG Res File v2\r\n"
    0010  [96]byte  comment    comment, terminated with "\x1A" (remainder 0x00)
    0070  [12]byte  reserved   reserved, set to 0x00
    007C  sint32    dirOffset  file offset to directory

> No resource file has been found containing a comment. It always only contains the terminator byte 0x1A.
> The comment is terminated with 0x1A as this represents the ```Ctrl+Z``` code. This was a little trick to enable people to ```type``` the file on the command line and not be spammed with binary garbage.

### File Directory
The file directory consists of a 6 byte header followed directly by its directory entries:

**Resource file directory header** (`ResDirHeader` struct, 6 bytes)

    0000  uint16  numEntries  number of resources in the file
    0002  sint32  dataOffset  file offset to beginning of first resource

**Resource file directory entry** (`ResDirEntry` struct, 10 bytes)

    0000  uint16  id     resource ID
    0002  sint24  size   resource length (unpacked)
    0005  sint8   flags  resource flags (see below)
    0006  sint24  csize  resource length (packed in file)
    0009  sint8   type   resource type (see below)

### Resource Flags

The resource flag marks extra information about the resource.

**Resource Flag** (1 byte)

    0x01  RDF_LZW         compressed
    0x02  RDF_COMPOUND    compound
    0x04  RDF_RESERVED    reserved (unused)
    0x08  RDF_LOADONOPEN  load on open (unused)

> The original source lists several more resource flags, which are all unused in System Shock.
> Presumably this is the result of re-used code.

"Flat" resources contain only one data block, which is the complete resource data itself. Compound resources contain 0, one, or more blocks.


### Resource Types

The following resource types appears in System Shock:

    0x00  UNKNOWN  Palette (although originally called "unknown", it is used for palettes exclusively)
    0x01  STRING   String
    0x02  IMAGE    Image
    0x03  FONT     Font
    0x04  ANIM     Animation Script (Video clip)
    0x07  VOC      Sound effects and voices (.voc)
    0x0F  OBJ3D    3D model geometry
    0x11  MOVIE    Movie
    0x30  APP1     Application specific resource (used in archive.dat for storing maps)

> The original source lists several more resource types, which are all unused in System Shock.
> Presumably this is the result of re-used code.
> There exists 0x05 (PALL) resource type specially for palettes, but never used in System Shock.

### Compound Resources
If a resource is flagged to be compound, the resource data starts with a directory. This directory contains relative offsets to the first bytes of the contained blocks, following the directory.

**Resource directory** (2 bytes + 4 bytes per block + 4 bytes)

    0000    uint16  number of blocks in resource
    0002    sint32  resource offset to first block
    ....
    nnnn    sint32  resource offset to last block
    nnnn+4  sint32  total length of resource

To calculate the length of a block N, take the start offset of N+1 and subtract the start offset of N. This is why there is one extra offset entry in the directory.

If a compound resource is compressed, the directory itself is not compressed. Resource offsets always point into the uncompressed data.

> Some compound resources have two zero bytes between the directory and the first block.

### Resource Compression
Resource compression is based on the [LZW algorithm](http://en.wikipedia.org/wiki/Lempel%E2%80%93Ziv%E2%80%93Welch). It uses a fixed word-size of 14 bits. Two words are reserved for control.

The words are serialized sequentially big-endian as a continuous stream.
For example, if the first 7 bytes in the resource are

    aaaaaaaa bbbbbbbb cccccccc dddddddd eeeeeeee ffffffff gggggggg

then the corresponding words are

    aaaaaaaabbbbbb bbccccccccdddd ddddeeeeeeeeff ffffffgggggggg

with the high bit to the left in each case.

The first 256 words of the dictionary are initialized with length 1 for the corresponding byte values. The reserved words are:
* 0x3FFE Reset dictionary to initial state
* 0x3FFF End-Of-Stream

The End-Of-Stream marker is always issued. The serialized byte stream always contains a trailing 0x00 byte after the End-Of-Stream word.

The compressor issues a dictionary reset when it is exhausting the dictionary and can't add a new word to it for the 1001st time.
