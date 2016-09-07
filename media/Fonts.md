## Fonts

This chapter documents the data format of the fonts. The game uses [bitmap fonts](https://en.wikipedia.org/wiki/Computer_font#Bitmap_fonts) which describe the characters.

The fonts are stored in single blocks within chunks of type ```0x03```. They can be found in the file ```gamescr.res```.

All glyphs in the bitmap are lined up in a single line. There are no Y offsets per glyph, and the height is always equal to the height of the bitmap. An array of ```X-Offsets``` points to the start of a glyph and the width is equal the difference to the next offset.

### Binary Format

**Font Header** (84 bytes)

    0000  uint16    Font Type -- 0x0000: Monochrome, 0xCCCC: Color
    0002  [34]byte  Unknown -- always 0x00
    0024  int16     First character index (inclusive)
    0026  int16     Last character index (inclusive)
    0028  [32]byte  Unknown -- always 0x00
    0048  int32     Offset to X-Offsets, from beginning of data
    004C  int32     Offset to bitmap, from beginning of data
    0050  int16     Width of bitmap in bytes
    0052  int16     Height of bitmap in bytes

**X-Offsets** (2*N bytes)

The X-Offsets is an array of ```int16``` values. The length of the array is equal to the number of characters the font covers, plus one. The width of one glyph is always equal the difference to the next offset, even for the last one.

For a monochrome font, the offset is counted in bits, always beginning with the MSB.

**Bitmap** (W*H bytes)

Monochrome fonts have the glyphs packed as bits within the bytes, color fonts use the bytes as palette indices.

> Fonts that have an outline, primarily seen with the HUD fonts, don't store the outline information in the font bitmaps. The outlines are rendered at runtime.
