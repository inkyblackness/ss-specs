## Sound Effects

Sound effects are stored in ```digifx.res```, which contains resources of data type ```0x07```.
The resources have IDs in the range from ```0x00C9``` to ```0x013B```, are uncompressed and have one effect per resource.

Each resource contains one sound effect, which is stored in the
[Creative Voice File](http://wiki.multimedia.cx/index.php?title=Creative_Voice) format.

Although the format would allow for different sample formats, only unsigned 8-bit PCM audio (MIME type ```audio/L8```)
is supported and expected. The sound effects don't use any special block types of the file format,
only one ```Sound data``` block is present, followed by a ```Terminator``` block identifier. All effects have one channel (mono).

### Format
The following is an extract of the above linked page, specialized to the use by the engine.

***Sound Effect***

    0000  [19]byte  Identifier text "Creative Voice File"
    0013  byte      Identifier terminator: 0x1A
    0014  int16     Length of header (commonly 0x1A)
    0016  uint16    Version number (0x010A)
    0018  uint16    Version number validation

    001A  byte      Sound data block type: 0x01
    001B  [3]byte   Length of block data (sample count + 2)

    001E  byte      Frequency divisor
    001F  byte      Codec ID (audio/L8: 0x00)
    0020  []byte    Samples

    NNNN  byte      Terminator block type: 0x00

***Version Number***

The version number is encoded as a major and minor number, in the high and low bytes respectively.

The validation value is the result of building the binary complement of the version number and adding the constant ```0x1234```.

***Frequency Divisor***

The frequency divisor is used to calculate the sample rate with the following formula:

```
sampleRate := 1000000 / (256 - frequencyDivisor)
```
