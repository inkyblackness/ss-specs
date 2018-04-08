### Animating Properties

#### Generic Animating Properties

**Generic Animating Propertes** (2 bytes)

    0000  byte     Frame time
    0001  byte     Emits light

The ```frame time``` specifies the time of a single frame of the animation. A value of ```0xFF``` means a time of approximately 0.9 seconds. All animations have this value set to ```0x1E``` (30).

If ```emits light``` is set to ```0x01```, then the animation lights up the area with one tile radius for its duration.
> Light is only emitted by objects placed by the engine. This flag is only set for such temporary animations, such as explosions (```11/2/x```), but works for impacts as well (```11/1/x```).


#### Specific 2 Properties

**Specific 2 Properties** (1 byte)

    0000  byte     Unknown
