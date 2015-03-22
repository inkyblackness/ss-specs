## Resource Files
Resource files are structured archives. The contained entries, called "chunks" in this documentation, are identified and each chunk contains one or more data blocks.

### General Format
A resource file consists of a header, chunks and a chunk directory. The header points to the file offset of the directory and the directory points to the file offset of the first chunk.

    |    /--------------------------------------------v            |
    | Header | Chunk0   | Chunk1      | ... | Chunk N | Directory  |
    |        ^----------------------------------------------/      |

All chunks, as well as the directory, start at a 4-byte boundary.

To calculate the file offset of a chunk, the directory has to be read first to determine the offsets (and lengths) of the previous chunks.

> Although the format would allow for a different order, all resource files have the chunk directory at the end of the file.

### File Header
The file header has a length of 128 bytes and has the following format:

**File Header** (128 bytes)

    0000  string  Header text "LG Res File v2\r\n\x1A"
    0011  []byte  not used (0x00)
    007C  int32   file offset to chunk directory

> Although the byte 0x1A is found in all resource files immediately after the header text, it is not part of the string. It serves as a terminator for an optional comment that may follow the string header.

### Directory
The chunk directory consists of a 6 byte header followed directly by its directory entries:

**Chunk directory header** (6 bytes)

    0000  int16  number of chunks in the file
    0002  int32  file offset to beginning of first chunk

**Chunk directory entry** (10 bytes)

    0000  int16  chunk ID
    0002  int24  chunk length (unpacked)
    0005  int8   chunk type (see below)
    0006  int24  chunk length (packed in file)
    0009  int8   content type

### Chunk Types

The following chunk types are known:

    0x00  flat, uncompressed
    0x01  flat, compressed
    0x02  directory, uncompressed
    0x03  directory, compressed

Flat chunks contain only one data block, which is the complete chunk data itself. Chunks with a directory contain 0 or more blocks.

### Chunks with Directories
If a chunk has a type specifying a directory, the chunk data starts with a directory. This directory contains relative offsets to the first bytes of the contained blocks, following the directory.

**Chunk directory** (2 bytes + 4 bytes per block + 4 bytes)

    0000    int16  number of blocks in chunk
    0002    int32  chunk offset to first block
    ....
    nnnn    int32  chunk offset to last block
    nnnn+4  int32  total length of chunk

To calculate the length of a block N, take the start offset of N+1 and subtract the start offset of N. This is why there is one extra offset entry in the directory.

If a chunk has type 0x03 (dir, compressed), the directory itself is not compressed. Chunk offsets always point into the uncompressed data.

> Some chunks with a directory have two zero bytes between the directory and the first block.

### Chunk Compression
Chunk compression is based on the [LZW algorithm](http://en.wikipedia.org/wiki/Lempel%E2%80%93Ziv%E2%80%93Welch). It uses a fixed word-size of 14 bits. Two words are reserved for control.

The words are serialized sequentially big-endian as a continuous stream.
For example, if the first 7 bytes in the chunk are

    aaaaaaaa bbbbbbbb cccccccc dddddddd eeeeeeee ffffffff gggggggg

then the corresponding words are

    aaaaaaaabbbbbb bbccccccccdddd ddddeeeeeeeeff ffffffgggggggg

with the high bit to the left in each case.

The first 256 words of the dictionary are initialized with length 1 for the corresponding byte values. The reserved words are:
* 0x3FFE Reset dictionary to initial state
* 0x3FFF End-Of-Stream

The End-Of-Stream marker is always issued. The serialized byte stream always contains a trailing 0x00 byte after the End-Of-Stream word.

The compressor issues a dictionary reset when it is exhausting the dictionary and can't add a new word to it for the 1001st time.
