## Palettes

The engine uses a graphic mode with 256 colors per pixel. The graphic resources (images and videos) use bitmaps together
with palette information to resolve a pixel value (1 byte) to the corresponding RGB value (3x1 byte).

Depending on the game-state, different palettes are in use. The splash screens at the beginning are created using a
```Private Palette``` (see the [bitmap](Bitmaps.md) format). Cutscenes have their own palette(s) and during the game,
one specific palette is used throughout. This game palette is stored in ```gamepal.res```.

### Format

Palettes always have a size of 768 bytes, for 256 colors with 1 byte for each of the three colour channels.

**Palette** (768 bytes)

    0000  [256*3]byte  RGB values

### Transparency

Palette index 0 is used as a marker for fully transparent pixel. For instance, various decoration or critter sprites need
to have transparent areas in order to lose the rectangular shape.

### Pixel Animation / Palette Looping

During the game various textures can be observed with small animations. Most typically seen with LED-like blinking effects.
Instead of implementing this with multiple frames, the engine simply loops through the colors within a few groups.

The following groups (palette index values) are looped:

    0x03 - 0x07: Changed every ~1200 msec
    0x0B - 0x0F: Changed every ~700 msec
    0x10 - 0x14: Changed every ~360 msec
    0x15 - 0x17: Changed every ~1800 msec
    0x18 - 0x1A: Changed every ~1430 msec
    0x1B - 0x1F: Changed every ~1080 msec

> These groups are looped even while the game is paused.
>
> Above switching times have been determined by counting frames of a video recording from dosbox, running within a VM.
> Exact values may be different.
