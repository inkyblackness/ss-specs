### Projectile Properties

#### Generic Properties

**Projectile Generic Properties** (1 byte)

    0000  byte     Unknown


#### Specific 1 Properties

These properties are set for some ```2/1/x``` objects. More specifically: For cyberspace projectiles. The other projectiles of this subclass have their specific bytes zero.

**Projectile Specific 1 Properties** (6 bytes)

    0000  [6]byte  Colour scheme

```Colour scheme``` contains 6 palette indices from the game palette. The index into this list comes from the geometry properties, specifically from the [Set Colour and Shade](../../media/Geometry#set-colour-and-shade) command.
