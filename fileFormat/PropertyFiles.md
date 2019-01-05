## Property Files
A few files don't have an archive format, and instead are flat files consisting directly serialized tables.

### Texture Properties (textprop.dat)
This file contains a single table of texture property entries, after a header.

> The table appears to be truncated. Since the game only uses 273 textures, this may have never been detected.
> The file has 4004 bytes on disk, meaning 4000 bytes are available for the table.
> In a test, the original engine appeared to be fine to read a smaller file with exactly 273 entries (3007 bytes) -
> which had the unknown fields set to zero.

> The original source explains the difference: The texture property system would support up to 400 textures.
> It also **assumed** a property structure size of 10 bytes (hardcoded value), while the actual structure is 11 bytes long.
> In other words, one could feed the engine more than 273 textures, yet no more than 363. More than 363 would simply be ignored and the extra textures would just have their default properties.
> This also explains why the Mac port reads exactly 363 textures, as reading more would exhaust the file.


#### Header
The file starts with a 4-byte header.

**Texture Properties File Header** (4 bytes)

    0000  int32    file version number, value 0x09


The engine rejects a properties file with a different version number.


#### Properties Entry

**Texture Property Entry** (11 bytes)

    0000  byte     family (unused)
    0001  byte     target (unused)
    0002  sint16   resilience (unused); Default: 10 
    0004  sint16   distance modifier; Default: 0
    0006  byte     Climbable. 0x00: not climbable; != 0x00: wall can be climbed (original name: friction)
    0007  byte     friction walk (unused); Always 0x00
    0008  byte     Transparency control (original name: force_dir)
    0009  byte     Animation group
    000A  byte     Animation index - if texture is animated

The two bytes at the beginning (`family` and `target`) almost always are each set to the low-byte of the index. A few exceptions have both bytes 0x00. Since they are unused, it is unclear what they mean.

The `resilience` is nearly always `0x0A`; The one existing exception has it 0. Since it is unused, it is unclear what it means.

The `distance modifier` always zero. It is used in the renderer to determine which texture resolution to use, based on distance from the camera. A higher modifier causes the smaller resolutions to be chosen at larger distances; A negative value causes an earlier switch.

See [Texture Animation](../archives/textureAnimation.md) for details on animated textures.

**Transparency Control**

    0x00  Regular texture; transparent pixel (palette index 0x00) are drawn black
    0x01  Space texture (pixel data of image is ignored)
    0x02  Texture with space background for transparent pixels


### Object Properties (objprop.dat)
The object properties file contains several tables about properties of [level objects](../levelObjects/index.md). It has the following format:

    | Header | Class 0 Tables   | Class 1 Tables     | ... | Common Table |

#### Header
The file starts with a 4-byte header containing the version number.

**Object Properties File Header** (4 bytes)

    0000  int32   file version number, value 0x2D


The engine rejects a properties file with a different version number.

#### Class Tables
For each object class, one entry exists with a table of class-generic properties and a further table, subclass-specific:

    | Class N Tables                                           |
    | Generic | Subclass 0   | Subclass 1   | ... | Subclass N |

The sizes of the generic and specific tables are dependent on the object class, subclass and type amount.
The generic table contains one entry per object type of this class. The specific tables contain one entry per object type of the subclass.

See [Level Objects](../levelObjects/index.md) for an overview of the amount of types and their entry sizes.


#### Common Table
At the end of the file is the table with common properties. For each object type one entry.

**Common Object Properties** (27 bytes)

    0000  int32    Mass
    0004  int16    Default hitpoints
    0006  uint8    Armour
    0007  byte     Render type
    0008  byte     Physics type; 0x00: none (insubstantial), 0x01: regular, 0x02: special
    0009  byte     Hardness; 0..253
    000A  byte     Unused; Internally called "pep", this value appears to not be used by the engine.
    000B  byte     Physics XR; 0..253
    000C  byte     Unused; Internally called "physics_y". These values are set (almost) identical to 000B and have no effect.
    000D  byte     Physics Z; Set to 0x30 for all barriers (10/x/x).
    000E  byte     Vulnerabilities
    000F  byte     Special vulnerabilities
    0010  [2]byte  Unused
    0012  byte     Defence value
    0013  byte     Toughness; 0x03: Invincible
    0014  int16    Flags
    0016  int16    MFD/Mesh ID
    0018  [2]byte  Bitmap 3D field model index
    001A  byte     Destruction effect


`Physics type` governs how collisions shall be handled. `0x00` makes the object insubstantial, ignoring forces applied to it. For instance, most scenery stays where it was put. `0x01` is for regular objects meant to react "normal" in a physical world. `0x02` behaves strange is set for exactly one object (`14/0/6`, "SONIC THE HEDGEHOG", an unused critter). Objects with a physics type of `0x02` aren't affected by gravity for instance, but are repelled if hacker runs into them.
The `hardness` acts as repulsion force on the object if it collides with the ground or walls - or on the hacker if hacker runs into it.
Hardness depends on the `mass`; Objects with more mass bounce around less.

The `physics xr` value is applied to vertically center (animated) sprites on their position. If `0x00`, the sprites are rendered with their bottom anchored at the object's position. Higher values will move the sprites downwards.

Related, the `physics z` value is inconsistently used as a height value. Only if this value is `0x00` (most of the time), then `physics xr` is used for height calculation. Still, the XR value is used far more often than the Z value.

The `vulnerabilities` and `special vulnerabilities` match the corresponding damage type fields from [Generic Weapon Info](../levelObjects/GenericWeaponInfo.md).
> The engine, internally, calls this field `resistances`, yet the code does the opposite. Furthermore, this `resistances` field covers the four bytes from `000E`.

The field of `special vulnerabilities` is split up into `0x0F` primary vulnerabilities (double damage) and `0xF0` super vulnerabilities (quad damage). Damage is multiplied only if the originating special force equals the vulnerabilities in these fields.
> Quad damage is reduced to triple damage on difficulty level 3 (either for real-world, or cyber, wherever it is calculated).

The `defence value` is compared to the `offence value` of a weapon or projectile to determine a "critical hit" (or critical reduction). Higher defence values will decrease the damage, higher offence values will increase the damage. If it is `0xFF`, then the target is invulnerable to critical hits.

The `toughness` determines how much damage, after most of the calculation, goes through - this still requires the object to be solid to register damage. The value of `0x03` reduces the damage to zero. Otherwise, this value is considered to be a shift factor, so a value of `0x01` halves damage, `0x02` quarters it.
> This value is set to `0x04` only for the non-used SHODAN critter `14/3/5`. This one would have been affected by only the strongest weapons.
> Probably this value was intended to be in range of 0..3 only.

`MFD/Mesh ID` identifies the MFD to open for this type of object, if selected from inventory. For 3D objects in the world, this value identifies the 3D mesh to render, based on resource `0x08FC`.


**Render Type Enumeration** (1 byte)

    0x01  3D object
    0x02  Sprite
    0x03  Screen
    0x04  Critter
    0x06  Fragments (e.g. the Cyberdog)
    0x07  Not drawn
    0x08  Oriented surface (door, wall decoration)
    0x0B  Special
    0x0C  Force door


**Common Object Flags** (2 byte)

    0x0001  Useful inventory object (green font)
    0x0002  Solid - can't be passed through
    0x000C  Use mode (see below)
    0x0010  Usable inventory object - object is triggered on double-click in inventory list
    0x0020  Blocks 3D rendering if closed (set for opaque doors)
    0x00C0  Light type (see below)
    0x0100  Solid if closed (set for barriers and repulsor object)
    0x0200  Flat solid (objects can be placed on it)
    0x0400  Doubled bitmap size (set for various explosion animations)
    0x0800  Destroy on contact
    0x7000  Class-specific flags
    0x8000  Useless object. Unless specifically marked useful otherwise, object can be removed to make room

**Use Mode Enumeration** (2 bit)

    0x00  Pickup - items can be taken
    0x01  Use - items are used, not taken
    0x02  undefined
    0x03  "Null" - can't be used

**Light Type Enumeration** (2 bit)

    0x00  Normal, simple lighting
    0x01  Complex lighting
    0x02  Ignore darkness
    0x03  Deferred light type (instance-specific)


**Bitmap 3D Bitfield** (2 byte)

    0x03FF  Bitmap number low mask
    0x8000  Bitmap number high mask
    0x0400  Animation
    0x0800  Repeat
    0x7000  Frame number mask


**Object Destruction Effect** (1 byte)

This byte is a bitmask field with the following layout:

    0x1F    Animation frame index -- refers to different types of animations
    0x20    Play destruction sound
    0x40    Play destruction sound (not used in main game)
    0x80    Actual explosion (with damage) -- requires Animation frame index to be 0x0F

> Some objects have their "private" destruction sounds and have none of the sound bits set.
> The `actual explosion`, set for larger electrical appliances, works only if the full value is `0x8F`.
