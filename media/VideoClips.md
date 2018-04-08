## Video Clips

Video clips are made out of a sequence of standard [bitmaps](Bitmaps.md). The meta-information about a video clip
is stored in resources of type ```0x04```. This information block refers to the resource that contains the frames, which
is stored in the same resource file.

Contrary to [MOVI Resources](moviChunks.md), video clips contain only the frame information - nothing else.

### Format

**Video Clip Sequence Description** (16+5*N bytes)

    0000  int16      Width of video.
    0002  int16      Height of video.
    0004  int16      Resource ID of frames, referring to a compound resource
    0006  [6]byte    Unknown. Always 0x00
    000C  int16      Unknown. TriOp intro: 0x0001, 0x0000 otherwise
    000E  N*[5]byte  Sub-table of N sequence entries
    MMMM  int16      Unknown. Always 0x010C

**Video Clip Sequence Entry** (5 bytes)

    0000  byte       Unknown. Always 0x04.
    0001  byte       First frame
    0002  byte       Last frame
    0003  int16      Frame time (in milliseconds)


### Video Mails

Video mails are small animated movies sent as mail during the game, all stored in ```vidmail.res```.

A single video mail is split up in several sequences. Most notably, the TriOp intro is a reused sequence for all mails.
The resource file contains 12 such sequences: Per sequence there is one resource of type ```0x04```
and a corresponding bitmap resource (containing the frames in blocks).

Video mails have a size of 200x100 pixel and are the same for the HD and CD releases. They share the palette of the
game (```gamepal.res```).


### Cutscenes

The files ```start1.res```, ```death.res``` and ```win1.res``` contain low-res variants of the corresponding
cutscenes.

The palettes for these cutscenes are stored in ```cutspal.res```.
