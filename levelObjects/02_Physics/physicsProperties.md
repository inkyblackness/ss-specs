### Physics Properties

#### Generic Properties

**Physics Generic Properties** (1 byte)

    0000  byte     Physics flags


**Physics Flags Enumeration** (1 byte)

    0x01  Emit light
    0x02  Bounce off walls
    0x04  Bounce off objects (real world), Pass through objects (cyberspace)
    0x08  Unknown -- set for all cyberspace projectiles


> Cyberspace projectiles can't emit light, though they can bounce off walls.
> The flag ```0x08``` has no detectable effect, neither for cyberspace, nor real world.


#### Specific 1 Properties

These properties are set for some ```2/1/x``` objects. More specifically: For cyberspace projectiles. The other projectiles of this subclass have their specific bytes zero.

**Physics Specific 1 Properties** (6 bytes)

    0000  [6]byte  Colour scheme

```Colour scheme``` contains 6 palette indices from the game palette. The index into this list comes from the geometry properties, specifically from the [Set Colour and Shade](../../media/Geometry#set-colour-and-shade) command.
