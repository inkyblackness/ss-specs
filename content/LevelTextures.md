## Level Textures

A level texture is a square bitmap used for level paneling - floors, ceiling and walls. The resources contain 273 textures,
for which there are four different resolutions available.

### Bitmaps

The bitmaps are stored in ```texture.res``` in the following chunks:

    0x004C           16x16 bitmaps, one per block
    0x004D           32x32 bitmaps, one per block
    0x02C3 - 0x03D3  64x64 bitmaps, one per chunk
    0x03E8 - 0x04F8  128x128 bitmaps, one per chunk

### Texts

Each texture comes with a description (name) and a "can't be used" message. [These texts](../media/Texts.md)
are stored in chunks ```0x086A``` and ```0x086B``` respectively.

> Note: The text chunks reserve 512 blocks, but only 273 are used.

### Properties

The [texture properties](../fileFormat/PropertyFiles#texture-Properties-textprop.dat) give further information on textures.

### Animation

Some textures, according to their properties, are in animation groups. These groups are controlled [per level](../archives/textureAnimation.md).

