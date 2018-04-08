## Level Container Table (Class 13)

```L23``` is a table describing containers.

**Level Container Entry** (21 bytes)

    0000  [6]byte   Level object prefix
    0006  [4]int16  Contained object indices

    for crates (13/0/x):
    000E  byte      Width
    000F  byte      Depth
    0010  byte      Height
    0011  byte      Top/Bottom texture number
    0012  byte      Side texture number

For the crate-specific values (dimensions and textures), if a value is 0, the default value from the 3D model is used.
The textures are taken from the resources in ```citmat.res```, based on ```0x0884```.

> Texture numbers from ```0x80``` have been found to take the textures from the level texture list.
> It is unclear if this is intentional or a side-effect of how data is arranged in the engine.

> Barrels (13/1/x) can not be inspected and may still contain items. Destroying a barrel drops its contents.
> There is one radioactive waste barrel (13/1/0) on level 4 which "contains" a hornet clip, which is lying right next to it.
> This is most likely the result of a mistake.
