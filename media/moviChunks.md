## MOVI Chunks

Chunks with content type ```0x11``` are a media container themselves, similar to audio/video container formats.
These chunks are called "MOVI Chunks" since the initial tag is the four letter code ```MOVI```.

### Format

Such a chunk contains the following components:
* a header, describing meta information about the media,
* a palette, with initial colours,
* an index, describing the following data entries, and
* the data entries.

*** MOVI Header *** (256 bytes)

    0000  [4]byte  Container tag: "MOVI"
    0004  int32    Number of index entries
    0008  int32    Size of index table
    000C  int32    Size of content (all data entries)
    0010  [3]byte  Length of media (in unit of 1/0x10000 seconds)
    0013  byte     Unused (see below). Always 0x00

    0014  int32    Unknown.

    0018  int16    Width of video
    001A  int16    Height of video

    001C  int16    Unknown. For videos 0x0008, 0x0000 otherwise
    001E  int16    Unknown. For videos 0x0001, 0x0000 otherwise
    0020  int16    Unknown. Always 0x0001
    0022  int32    Unknown. Always 0x00000001

    0026  int16    Sample rate of audio

    0028  []byte   Unknown. Always 0x00 until full size of header

> Although the engine might treat the media length as 4 bytes, the index table entries provide only 3 for timestamps.
> The total length of the media can't be longer than the last possible entry, so the header will have only 3 as well.

If the index table size is greater than the total of index entry number times one entry size, the remaining bytes are zero.

> The index table sizes come in multiples of 0x0400. The rough rule for recreating the existing index sizes is:
> Required size less than 0x0400 -> Use 0x0400. Otherwise: Round up to next 0x0400 border and add another 0x0400.

*** Palette *** (3*256 bytes)

The palette is always present, even if the MOVI contains only audio. In such cases all the colour channels are 0x00.

*** Index Table Entry *** (8 bytes)

    0000  [3]byte  Start timestamp (in unit of 1/0x10000 seconds)
    0003  byte     Entry type
    0004  int32    Offset to data (from beginning)

*** Entry Type *** (1 byte)

    bits  0-2      Data type
    bits  3-6      Extension info


### Data Types

#### 0 End of media

This type is always present as the last entry in the index. It is used to calculate the length of the previous entry as
the table does not contain a length field for entries.

The timestamp of this entry should match the media length from the header.

#### 1 Video

tbd

#### 2 Audio

The data entry contains unsigned 8-bit PCM samples.

> Audio is chunked into entries of 8192 bytes, even if the MOVI contains only audio.

#### 3 Subtitle

tbd

#### 4 Palette

tbd

#### 5 Dictionary

tbd
