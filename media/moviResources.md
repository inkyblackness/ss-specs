## MOVI Resources

Resources with content type ```0x11``` are a media container themselves, similar to audio/video container formats.
These resources are called "MOVI Resources" since the initial tag is the four letter code ```MOVI```.

### Format

Such a resource contains the following components:
* a header, describing meta information about the media,
* a palette, with initial colours,
* an index, describing the following data entries, and
* the data entries.

**MOVI Header** (`MovieHeader` struct)

    0000  uint32      magicId          Container tag 0x49564F4D ("MOVI")
    0004  int32       numChunks        Number of chunk entries in file
    0008  int32       sizeChunks       Size of index table
    000C  int32       sizeData         Size of content (all data entries)
    0010  fix         totalTime        Length of media (in seconds)
    0014  fix         frameRate        Frame rate (informational only)
    0018  int16       frameWidht       Width of video
    001A  int16       frameHeight      Height of video
    001C  int16       gfxNumBits       For videos may be 0x0008, 0x000F or 0x0018, 0x0000 otherwise
    001E  int16       isPalette        Palette presence in file. For videos 0x0001, 0x0000 otherwise
    0020  int16       audioNumChans    Numbers of audiochannels 0x0000 - no audio, 0x0001 - mono, 0x0002 stereo
    0022  int16       audioSampleSize  Audio sample size, 0x0001 - 8-bit, 0x0002 - 16-bit
    0026  fix         audioSampleRate  Sample rate of audio, in Hz (0x2B770000 - 11127 Hz, 0x56EE0000 - 22254 Hz)
    0030  uint8[216]  reserved         Reserved
    0108  uint8[768]  palette          Palette

If the index table size is greater than the total of index entry number times one entry size, the remaining bytes are zero.

> The index table sizes come in multiples of 0x0400. The rough rule for recreating the existing index sizes is:
> Required size less than 0x0400 -> Use 0x0400. Otherwise: Round up to next 0x0400 border and add another 0x0400.

**Palette** (256 * 3 bytes)

The palette is always present, even if the MOVI contains only audio. In such cases all the colour channels are 0x00.

**Movie chunk Entry** (`MovieChunk` struct) (8 bytes)

    0000  uint32              Chunk info (bitfield)
    0004  uint32   offset     Offset to data (from beginning)

First 4 bytes are uint32 bit field which follow structure:

    bits  00-23  24  time       Fixed point timestamp from movie start (in seconds)
    bits  24-26   3  played     Has this chunk been clocked out?
    bits  27-31   4  flags      Chunk type specific flags
    bits  32-32   1  chunkType  Chunk type (see below)

> In Mac OS port of System Shock these bit fields are reversed due big-endianness - don't be confused!

Chunk specific flags may be:

For Video chunk type (0x01 MOVIE_CHUNK_VIDEO)

    0x0F  MOVIE_FVIDEO_BMTMASK    4 bits of flags is bmtype
    0x0F  MOVIE_FVIDEO_BMF_4X4    4x4 movie format

> Not clear, why those flags uses same code, this may be bug or leftovers from reusing library component

For Palette chunk type (0x04 MOVIE_CHUNK_PALETTE)

    0x00  MOVIE_FPAL_SET          Set palette from data 
    0x01  MOVIE_FPAL_BLACK        Set palette to black
    0x07  MOVIE_FPAL_EFFECTMASK   4 bits of flags is effect
    0x08  MOVIE_FPAL_CLEAR 0x08   If bit set, clear screen

For Table chunk type (0x05 MOVIE_CHUNK_TABLE)

    0x01  MOVIE_FTABLE_COLORSET    Table is color set
    0x02  MOVIE_FTABLE_HUFFTAB     Huffman table (compressed)

### Chunk Types

#### 0 End of chunk (MOVIE_CHUNK_END)

This type is always present as the last entry in the index. It is used to calculate the length of the previous entry as
the table does not contain a length field for entries.

The timestamp of this entry should match the media length from the header.

#### 1 Video (MOVIE_CHUNK_VIDEO)

Video frames are compressed in two variants, for low and high resolution.

Frames are either "full" frames, which set all, or nearly all, pixels in the buffer, typically at a scene change. Other frames are "delta" frames, which re-use (most of) the previous pixel in the buffer. This re-use is done by writing/using the palete index 0x00, which is meant to be transparent.

##### Low-resolution Video

Low-resolution video is using type ```0x21```. Data contains a bounding box and then the compressed frame using the [compression algorithm](./Bitmaps.md#pixel-compression) for standard bitmaps. 

**Low Resolution Video Data** (8+N bytes)

    0000  [4]int16  Bounding Box
    0008  []byte    Compressed pixel data

> In the existing videos, the bounding box is always ```0, 0, 320, 150```, specifying the same size as the video itself.

##### High-resolution Video

High-resolution video is using type ```0x79``` with a more complex compression algorithm. This algorithm works in conjunction of the ```Table``` types. See [below](#high-resolution-video-compression) for details on the algorithm.

**High Resolution Video Data** (2+N bytes)

    0000  int16   Offset to Mask-stream in bytes, from beginning (XXXX)
    0002  []byte  Bitstream
    XXXX  []byte  Mask-stream

The ```Bitstream``` is a stream of serialized big-endian integers. The ```Mask-stream``` is a stream of serialized little-endian integers of different lengths (2, 4, 6, and 8 bytes).

The order and layout of the values in the streams is determined by the algorithm. The decoder will want to read past the end of the streams in a few cases. For these cases the missing bits/bytes are to be set to zero. The encoder removed any trailing 0x00 bytes from these streams in order to save further space.

#### 2 Audio (MOVIE_CHUNK_AUDIO)

The data entry contains unsigned 8-bit PCM samples.

> Audio is chunked into entries of 8192 bytes, even if the MOVI contains only audio.

#### 3 Text (MOVIE_CHUNK_TEXT)

**Subtitle Entry** (16+N bytes)

    0000  [4]byte   Subtitle Control
    0004  byte      Unknown -- always 0x10
    0005  [11]byte  Unknown -- always 0x00
    0010  []byte    Null terminated string

##### Subtitle Controls

```'AREA'``` (uint32 ```0x41455241```) is for setting up the subtitle space. It defines the rectangle, in pixel (left, top, right, bottom), where the text is to be displayed. Example:
```
20 365 620 395 CLR
```

```'STD '``` (uint32 ```0x20445453```), ```'FRN '``` (uint32 ```0x204E5246```) and ```'GER '``` (uint32 ```0x20524547```) specify the actual subtitle text in the respective language, with ```'STD '``` being the default language (English).

#### 4 Palette (MOVIE_CHUNK_PALETTE)

There are two palette entry types: ```0x4C``` and ```0x04```. For any change in the palette, two entries are in the container. Both entries always come in sequence (first ```0x4C```, then ```0x04```) and both have identical timestamp values. 

The entry with type ```0x4C``` will have a length of 0 and the same data offset as the next entry, ```0x04```.
The entry with type ```0x04``` points to data of 0x300 bytes (= 256*3) which describes the new palette for upcoming frames. 

> At least for high-resolution videos, the next frame after a palette change does not always entirely set the complete pixel buffer for the next frame.
> In the intro video for instance, SHODAN's gaining of conscience scene has a few pixel left over from the previous scene.
>
> As a recommendation, a decoder should wipe the current frame buffer with 0x00 before applying a new palette/drawing further frames.
> An alternative could be to remap all pixel values with palette indices as close as possible to the new palette, though this might alter the resulting frame too much.

#### 5 Table (MOVIE_CHUNK_TABLE)

Table entries are used in combination with video type ```0x79``` entries for high-resolution videos. There are two table types:
* ```0x05```: Palette lookup list
* ```0x0D```: Control table

See [below](#high-resolution-video-compression) for details on the algorithm.

Such entries are common to several frames. For any change, two entries are in the container. Both entries always come in sequence (first ```0x05```, then ```0x0D```).

> In the existing files, a change of tables is always followed by a change of palette. The exception is the first scene, which uses the initial palette from the MOVI header.

##### Palette lookup list

This entry, with type ```0x05``` is a simple lookup list. Every entry of type byte is a palette index.
This list is truncated as well. The decoder will want to read past the end of this stream, in which case zero bytes must be returned.

##### Packed Control Words

Table type ```0x0D``` contains a runlength encoded list of control words.

**Packed Control Words** (4+(N*4) bytes)

    0000  int32     Unpacked size
    0004  []Packed  Packed entries

The ```Unpacked size``` field equals the number of control words, times three (specifying the size in bytes).
The remainder of the entry is then the list of packed control words, from which the actual list of control words is unpacked.

**Packed Entry** (4 bytes)

    0000  byte    Word count
    0001  uint24  Control Word

The field ```Word count``` specifies how often the ```Control Word``` shall be extracted. A value of 0 is never encountered.

### High-resolution Video Compression

High-resolution video compression uses scene-specific tables shared amongst several frames and frame-specific bit- and mask-streams. Frames in this compression are made up of tiles with a size of 4x4 pixel each. For the high-resolution videos of 600x300 pixel this means 150x75 tiles. Tiles are coloured according to different types of control words, from left to right, top to bottom.

**Control Word** (3 bytes, masked)

    0xF00000  Count value
    0x0E0000  Type
    0x01FFFF  Parameter


As a rough guideline, the frame-specific bit-stream provides index values into the control word table. According to the type of the control word, the current tile is coloured either directly, or with the additional help of the mask-stream and palette lookup list.

A note on the bit-stream: Reading from this stream does not automatically advance the current position. Advancing the bit-stream is a separate operation, which may advance with a different number of bits than were previously read. This may cause re-use of some bits to increase compression.

As noted above, the bit- and mask-stream, as well as the palette lookup list, have possible trailing zero values removed in the files. When the decoder wants to read beyond their end, they must provide values set to 0.


#### Decoder sequence

A possible decoder is implemented using the following sequence:

* Iterate over the tiles, outer loop is the rows, inner loop the columns. Finish the loops if either all tiles are covered, or the bit-stream has been exhausted. The stream is exhausted if the current position is at the end of or beyond the available range.
* Read a 12-bit value from the bit-stream. Use this value as an index into the control word table.
* If the ```Count value``` of the control word is zero
    * A ```Count value``` of zero indicates a ```Long Offset```. In this case, the ```Type``` and ```Parameter``` fields are taken together to form a 20-bit ```Long Offset``` value.
    * Advance the bit-stream by 8 bits
    * Enter a loop as long as the current control word indicates a ```Long Offset```.
        * Advance the bit-stream by 4 bits
        * Read a 4-bit value from the bit-stream. Add this value to the ```Long Offset``` of the current control word and use the result as another index into the control word table. The retrieved entry becomes the new current control word.
* Advance the bit-stream by the ```Count``` value of the current control word. This may indicate a different amount of bits than previously read, even less.
* If the type of the current control word is ```6```, use the previously processed control word as the current one for further processing.
* If the type of the current control word is ```5```, tiles shall be skipped
    * Read a 5-bit value from the bit-stream and advance it by 5 bits. This is the ```Skip Count``` value.
    * A ```Skip Count``` of 0x1F skips the remainder of the current tile row
    * Any other value skips the given amount of columns, additionally to the current one. A value of 0 skips only the current tile.
* Any type in the range of ```0``` to ```4``` colours the current tile. See below.
* Next loop iteration

> Type ```7``` is never encountered. According to the documentation of TSSHP, this has the same meaning as ```6```: use previous control word.


##### Colouring a tile

To colour a tile, the ```Parameter``` value of the current control word is used, as well as the ```mask-stream``` and the ```palette lookup list```. The 16 pixel of a tile are coloured from left to right, top to bottom.

The palette value for a pixel is determined by an index into a small ```lookup array``` of palette indices. Depending on the type of the control word, this lookup array is either a made up one, or a segment of the ```palette lookup list```. The length of a ```lookup array``` is always a power of 2 and has possible lengths of 2, 4, 8 or 16.

The corresponding index values into such lookup arrays have bit sizes of 1, 2, 3 or 4 bits. The 16 index values for a tile are packed into a ```mask integer``` of 2, 4, 6 or 8 bytes. Depending on the type of the control word, this ```mask integer``` is made up or read from the ```mask-stream```.

> For example, a lookup array of 4 palette values requires an index size of 2 bits, resulting in a mask integer of 4 byte.
>
> For pixel coordinates given in x,y: Mask integer bits 0-1 colour pixel 0,0; mask integer bits 2-3 colour pixel 1,0; ...
> mask integer bits 28-29 colour pixel 2,3 and mask integer bits 30-31 colour pixel 3,3.


| Type | Lookup Array                                           | Mask Integer            |
|:----:|--------------------------------------------------------|-------------------------|
|   0  | ```[ Parameter >> 0 & 0xFF, Parameter >> 8 & 0xFF ]``` | Static ```0xAAAA```     |
|   1  | ```[ Parameter >> 0 & 0xFF, Parameter >> 8 & 0xFF ]``` | 2 byte from mask-stream |
|   2  | Parameter is start index into list, 4 entries          | 4 byte from mask-stream |
|   3  | Parameter is start index into list, 8 entries          | 6 byte from mask-stream |
|   4  | Parameter is start index into list, 16 entries         | 8 byte from mask-stream |

