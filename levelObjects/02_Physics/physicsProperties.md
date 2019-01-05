### Physics Properties

#### Generic Properties

**Physics Generic Properties** (`PhysicsProp` struct) (1 byte)

    0000  byte     Physics flags


**Physics Flags** (1 byte)

    0x01  Emit light
    0x02  Bounce off walls
    0x04  Bounce off objects (real world), Pass through objects (cyberspace)
    0x08  Bounce off projectiles; set for all cyberspace projectiles


> Cyberspace projectiles can't emit light, though they can bounce off walls.


#### Specific 0 Properties

"Tracer" objects.

> These are not used in the engine.

**Physics Specific 0 Properties** (`TracerPhysicsProp` struct) (20 bytes)

    0000  [4]int16  X coordinates
    0008  [4]int16  Y coordinates
    0010  [4]uint8  Z coordinates


#### Specific 1 Properties

These properties are set for some `2/1/x` objects. More specifically: For cyberspace projectiles, also called "slow physics". The other projectiles of this subclass have their specific bytes zero.

**Physics Specific 1 Properties** (`SlowPhysicsProp` struct) (6 bytes)

    0000  [6]byte  Colour scheme

`Colour scheme` contains 6 palette indices from the game palette. The index into this list comes from the geometry properties, specifically from the [Set Colour and Shade](../../media/Geometry#set-colour-and-shade) command.
