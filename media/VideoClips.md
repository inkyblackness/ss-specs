## Video Mails

Video mails are small animated movies sent as mail during the game. The videos are made as a sequence of standard
[bitmaps](Bitmaps.md). The bitmaps and the meta-information about the videos are stored in ```vidmail.res```.

A single video mail is split up in several sequences. Most notably, the TriOp intro is a reused sequence for all mails.
The resources contain 12 such sequences: Per sequence there is one chunk of type ```0x04``` and a corresponding
bitmap chunk (containing the frames in blocks).

### Format

**Video Mail Sequence Description** (16+5*N bytes)

    0000  int16      Width of video. Always 0x00C8.
    0002  int16      Height of video. Always 0x0064.
    0004  int16      Chunk ID of frames
    0006  [6]byte    Unknown. Always 0x00
    000C  int16      Unknown. TriOp intro: 0x0001, 0x0000 otherwise
    000E  N*[5]byte  Sub-table of N sequence entries
    MMMM  int16      Unknown. Always 0x010C

**Video Mail Sequence Entry** (5 bytes)

    0000  byte       Unknown. Always 0x04.
    0001  byte       First frame
    0002  byte       Last frame
    0003  [2]byte    Unknown
