## Bitmaps

Bitmaps are images using a palette of colours. System Shock uses bitmaps exclusively for its graphics and uses one byte
as index per pixel - thus 256 colours possible. The resources store such bitmaps with content type ```0x02``` and they are always compound, even if only one bitmap is stored.

Bitmaps are stored with a header, the pixel data (sometimes compressed), and an optional private palette.

### Header

**Bitmap Header** (28 bytes)

    0000  [4]byte   Unused. Always zero.
    0004  int16     Type: 0x02: Uncompressed; 0x04 Compressed
    0006  int16     TransparencyFlag
    0008  int16     Width
    000A  int16     Height
    000C  int16     Stride
    000E  byte      WidthFactor
    000F  byte      HeightFactor
    0010  [4]int16  HotspotBox: Left, Top, Right, Bottom
    0018  int32     Private palette offset

The ```TransparencyFlag``` is set to value ```0x01``` when palette index ```0x00``` should be treated as a transparent pixel instead of painting it black.
> Compressed images appear to default to transparency if the resolution of the game is 320x200. Higher resolutions require the flag set to behave identical.
>
> Other factors may also govern whether the bitmap should be treated with transparency - videos and level textures have their special handling in this regard.

The ```WidthFactor``` and ```HeightFactor``` specify the index of the highest set bit of ```Width``` and ```Height```
respectively. For example, if Width is 320 (0x140), then the WidthFactor is 8 (1<<8 == 0x100).

> For the existing images in the original resource files, all images have a ```Stride``` equal to ```Width```.

### Pixel Data

After the header, the pixel data is serialized. If the image is uncompressed (Type ```0x02```),
```Height``` x ```Stride``` bytes make up the image. Compressed images (Type ```0x04```) use a simple run-length-encoding (RLE).

#### Pixel Compression

The compressed byte-stream contains the following byte sequences:

    00 nn zz     write nn bytes of value zz
    nn ..        0 < nn < 0x80: copy the following nn bytes
    80 lo hi     lo hi are a little-endian uint16 nnnn, to be handled as follows:
    80 00 00     End of compressed data (Skip to end)
    80 lo hi     0 < hi < 0x80: skip (nnnn & 0x7FFF) output pixel
    80 lo hi ..  0x80 <= hi < 0xC0: copy the following (nnnn & 0x3FFF) bytes
    80 lo C0     hi == 0xC0: undefined (not encountered)
    80 lo hi zz  0xC0 < hi: write (nnnn & 0x3FFF) bytes of value zz
    nn           0x80 < nn: skip (nn & 0x7F) output pixel

Three cases exist to compress pixel data: skip a list of pixel, compress a list of a constant
value and copy an arbitrary array of bytes. For each of the three, two encoding variants are used: a short and a long form.
If a count is to be serialized that is higher than the long-form can store, the encoder uses the long-form to reduce this number to finalize with the short-form.

> Skipping output pixel is relevant for compressing video clips. Skipped pixel will retain the colour value from the previous frame.
> For still images, initialize the output buffer with 0x00.
>
> To implement a compressor that creates output closest to the original content, use the following sequence:
> * From an array of bytes with length X, calculate the count Z of bytes with value 0x00 at the end. Compress only N bytes, where N = X - (Z % 0x7FFF).
>   The final sequence ```80 00 00``` implies to skip to the end. The encoder
>   seems to have had a limit of counting only up to 0x7FFF, so there may be further (long) skip entries before.
> * Iterate through the pixel data from start to N:
>    * Count the extent of same value bytes
>    * Determine encoding method
>       * If the pixel value shall be retained from the previous frame, skip the count of bytes
>       * Otherwise, if the count is more than three, encode the count and the constant value
>       * Otherwise, count the extent of an arbitrary array; Abort when a zero value is detected, or more than three same values are found in sequence; Encode this array.
>    * Skip the input bytes that have been serialized and continue with the loop
> * Encode end of compressed data sequence.


### Private Palette

The format also allows for an optional palette, which is specific for the bitmap. If an image has no private palette, then this
entry is not present and the resource data ends directly after the pixel data.
Without a private palette, the colours of the bitmap are context specific. See [Palettes](Palettes.md) for more information.

Private palettes are used for the splash screens at the beginning of the game, where nothing else is shown.
The ```Private palette offset``` in the bitmap header specifies the absolute offset *from the beginning* of the resource to the private palette entry. This means in case of a compound resource, this offset measures across the previous resource blocks as well.

**Private Palette** (772 bytes)

    0000 [4]byte      Unknown. Always 0x00, 0x00, 0x00, 0x01
    0004 [256*3]byte  RGB values

> The only working examples for this can be found in ```splash.res```.
>
> The resource file ```gamescr.res``` also contains three bitmaps with a private palette, but these palettes are not used and are ignored by the engine.
> The three bitmaps in question are the help overlay (```ALT+o```), and are shown with the game world in the background. Since in these
> cases the world can't change its colour, the screen can't use a private palette. By extension, the side-effects of the "Berserk" patch
> also affect the colours of the help overlay.
> Furthermore, for these three bitmaps, the values of offset fields are wrong as well.
