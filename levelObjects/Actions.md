## Actions

Level trigger and various panels (buttons and the like) make use of actions that perform some state change and/or give some indication to the player. They are essentially what make a level (and the game in total) come to life.

Actions can be triggered by various sources. They may have conditions that first have to be met before the action is actually performed. The type of the action determines how to interpret the details (parameters) of the action.

### Quest Value Key

Some actions make use of quest value keys, which are used to either resolve a raw value, or a value from the game variables ("quest values").

* If bit ```0x1000``` is set in the key, the current integer game variable value of index ```key & 0x0FFF``` is referenced.
* If bit ```0x2000``` is set in the key, the current boolean game variable value of index ```key & 0x0FFF``` is referenced.
* For resolving values, if the key is less than 0x1000, then the resulting value is the value key itself.


### Action Data

**Action Data** (22 bytes)

    0000  byte       Action type
    0001  byte       Use quota
    0002  [4]byte    Condition
    0006  [16]byte   Action details

The ```Use quota``` indicates how often more this action can be executed. ```0``` means endless.
The value is only decremented if it is not zero and the condition was met.
If the value is decremented to zero after the action was executed, the object where the action is in is deleted.


### Action Type 1: Transport hacker

**Transport Hacker Action Details** (16 byte)

    0000  int32      Target X
    0004  int32      Target Y
    0008  int32      Target Z
    000C  int32      Target Level

For all three target axis values, if they are ```0x4000``` or greater, they are ignored and the respective axis is not changed in the transport.
The ```X``` and ```Y``` axes specify tile numbers, the ```Z``` axis is interpreted in level units.

> In the main game, this axis-preservation is used only on level 2 for the Z axis on the experimental transporter,
> which incidentally has both platforms at the same height.

If to be considered, the target values are then used as quest value keys to resolve the actual location for the respective axis.

> The main game does not make use of this quest value lookup feature. All encounterd values are direct values.

The ```target level``` specifies a cross-level transfer to the identified level if the value is smaller than ```0x1000```.
If it is ```0x1000``` or greater, transportation is only done within the same level, regardless of the value.

> Cross-level transport is used only for a same-level transfer on level 4 (restoration chamber), and twice for two groves, to
> transport the hacker to the restoration chamber on level 6. Other instances are set to ```0x2222```, which are ignored.


### Action Type 2: Change Health

This action is also called "damage", probably based on the first parameter.

**Change Health Action Details** (16 byte)

    0000  int32      Damage delta
    0004  int16      Health delta
    0006  byte       Health change flag; 0: remove delta, 1: add delta
    0007  byte       Damage type -- used if removing health
    0008  int16      Power delta
    000A  int16      Power change flag; 0: remove delta, 1: add delta
    000C  int16      Fatigue delta
    000E  int16      Fatigue change flag; 0: add delta, 1: remove delta

All four ```delta``` values are used as quest value keys to resolve the actual delta value.

> The ```damage delta``` is not used in the main game.
> Although intended to cause explosion damage, due to mixed up parameters in the code, a fixed amount of damage is done,
> with the delta value being interpreted as damage type.

> Reducing health to/below 0 causes death. Reducing power below 0 causes an underflow and hacker is fully powered again.


### Action Type 3: Clone/Move Object

> This action is internally called "Create Object". Name not kept to describe actual functionality better.

**Clone/Move Action Details** (16 byte)

    0000  int16      Source object index -- quest value key
    0002  int16      Move flag
    0004  int32      Target X tile
    0008  int32      Target Y tile
    000C  int32      Target Z

The ```move flag``` has a dual purpose. First, it is used to determine whether the object shall be moved or cloned.
If it is zero, then the object is cloned; If it has bit ```0x1000``` set, then the object is cloned as well; All other values cause the source object to be moved.
If the object is cloned, then the ```move flag``` parameter will be used as quest value key to store the object index of the newly created object.

> The game archive has various values for the ```move flag```.

For all three target axis values, if they are ```0x4000``` or greater, they are ignored and the respective axis is not changed.
The ```X``` and ```Y``` axes specify tile numbers, the ```Z``` axis is interpreted in level units.

> In the main game, this axis-preservation is used with various values, including ```0x4400```.

If to be considered, the target values are then used as quest value keys to resolve the actual location for the respective axis.

The orientation and in-tile position of the source object will be kept.


### Action Type 4: Set Game Variable

> This action is internally called "Quest Bit". Name not kept to describe actual functionality better.

**Set Game Variable Action Details** (16 byte)

    0000  int32      Quest value key
    0004  int16      Value
    0006  int16      Operation
    0008  int32      Message 1 (within resource 0x0867)
    000C  int32      Message 2 (within resource 0x0867)

Depending on the ```quest value key```, different sets of variables are modified.
The variables are stored in the [Game State Structure](../../archives/gameState.md).
If no bit in the nibble of ```0xF000``` is set, the quest value key defaults to boolean variables.

> For the most part in the main game, the boolean variable selector ```0x2000``` is not set and thus relies on this default handling.
> There are a few cases where this boolean variable selector is set.

For the int16 variables, ```value``` must be in the range ```0..0x0FFF```. ```Operation``` can be one of the following:

    0:  Set value
    1:  Add value
    2:  Subtract value
    3:  Multiply with value
    4:  Divide by value (Resulting fractions are cut, divide by 0 is ignored)
    5:  Modulo by value

For the boolean variables, ```operation``` is ignored and always ```0```.
The values are either ```0``` or ```1``` to set that value, or ```2``` to toggle the current value.

If the resulting value is not zero, the (optional) ```message 1``` is played/shown. If the resulting value is zero, the (optional) ```message 2``` is played/shown. Both message parameters are quest value keys.

> Operations other than Set or Add are not used in the game for the int16 values.


### Action Type 5: Cutscene

**Cutscene Action Details** (16 byte)

    0000  int32      Cutscene index; 0: Death, 1: Intro, 2: Ending
    0004  int32      End game flag
    0008  [8]byte    Unused

```End game flag``` must be ```1```. Setting it to ```0``` leaves the game in an unplayable state after the cutscene.

What happens after the cutscene is dependent on the cutscene itself: The death and intro cutscenes return to the main menu,
the ending cutscene ends the game (showing score and credits).

> In the regular game, this action is used only once when defeating SHODAN.


### Action Type 6: Trigger other objects

**Trigger Objects Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Object 1 delay
    ...
    000C  int16      Object 4 index
    000E  int16      Object 4 delay

The delays are all relative to the execution start of the action. Their unit is 0.1 of a second.

All four object indices are quest value keys. They are resolved when this action is executed, not after the delay.


### Action Type 7: Change lighting

**Change Lighting Action Details** (16 byte)

    0000  int16      Light type extent - quest value key
    0002  int16      Reference object index - quest value key
    0004  int16      Transition type. 0x0000: immediate, 0x0001: fade, 0x0100: flicker
    0006  int16      Transition state
    0008  byte       Light type
    0009  byte       Unused
    000A  int16      Light surface. 0x0000: floor, 0x0001: ceiling, 0x0002: floor and ceiling
    000C  [4]byte    Light type parameter

These actions modify the light deltas of the affected tiles. Such actions can not change the base darkness
values of a tile. If a tile is fully bright by default, nothing can change that.
The configured light parameters are absolute values.

The ```transition state``` is a bitmask storing the state for a change.

**Transition state** (2 bytes)

    0x0FFF  Step counter
    0x1000  0: light on, 1: light off

**Light type** (1 byte)

    0x00  Rectangular
    0x01  Linear gradient (East-West smooth)
    0x02  Linear gradient (North-South smooth)
    0x03  Radial gradient

**Gradient light type parameter** (4 bytes)

    0000  byte       Off light begin intensity
    0001  byte       Off light end intensity
    0002  byte       On light begin intensity
    0003  byte       On light end intensity

#### Rectangular light (type 0x00)

For this light type, the ```light type extent``` specifies a second object index. All tiles within this rectangle are affected.

**Light type parameter** (4 bytes)

    0000  byte       Off light value 0x00..0x0F
    0001  byte       On light value 0x00..0x0F
    0002  [2]byte    Unused

#### Linear gradient light (types 0x01, 0x02)

For this light type, the ```light type extent``` specifies a second object index to define the affected area.

For these types, the ```gradient light type parameter``` intensity values have a range of ```0x00```..```0x0F```.

> These light types are not used in the regular game.


#### Radial gradient light (type 0x03)

For this light type, the ```light type extent``` specifies a radius, in tiles, of affected tiles. The begin of the gradient is always the
tile of the triggering object (```reference object index``` is ignored).

The intensity values of the ```gradient light type parameter``` have a range of ```0x00```..```0x7F```.
A value of around ```0x14``` lights a tile as if it were fully lit.
Values above will spill over to the surrounding tiles.


### Action Type 8: Effect

**Effect Action Details** (16 byte)

    0000  int16      Sound index, based on 0x00C9; 0: Play nothing
    0002  int16      Sound play count; 0: endless
    0004  int16      Visual effect
    0006  [2]byte    Unused
    0008  int16      Additional visual effect
    000A  [2]byte    Unused
    000C  int16      Additional visual effect time; 0: endless
    000E  [2]byte    Unused

All parameters are used as quest value keys.

Visual Effects:

    0: None
    1: Power on: Dim level lights and fade back to normal; Plays generator sound as well
    2: Quake: Shake cam and play rumble sound
    3: Escape pod: Shows launch sequence, cam shake and abort; Plays SHODAN message afterwards
    4: Red static (fullscreen)
    5: Red static (interference)

Additional Visual Effects:

    0: None
    1: White flash
    2: Pink flash
    3: Gray static (fullscreen)
    4: Vertical panning (including HUD)


### Action Type 9: Change tile heights

    0000  int32      Tile X
    0004  int32      Tile Y
    0008  int16      Target floor height; 0..31, measured from floor absolute zero
    000A  int16      Target ceiling height; 1..31, measured from floor absolute zero
    000C  [2]byte    Unused
    000E  int16      Silent flag; 0: play sound, 1: no sound

Tile coordinates, as well as the heights, are used as quest value keys.

For the height fields a value equal to, or greater than ```0x0100``` indicates "don't move".

> The ceiling height has also been encountered to be ```0x0020```, also not moving.

The ```silent flag``` only works for floor changes. If the ceiling changes height, it makes sound regardless of this parameter.


### Action Type 10: Change Terrain

    0000  int32      Tile X
    0004  int32      Tile Y
    0008  int32      Tile type
    000C  int32      Terrain parameter (slope height)

All four parameters are used as quest value keys.

If ```tile type``` or ```terrain parameter``` has the bit ```0x4000``` set, the respective property will not be changed.

> This action is not used in the main game.


### Action Type 11: Scheduled Trap

**Scheduled Trap Action Details** (16 byte)

    0000  int32      Object index
    0004  int32      Time interval
    0008  int32      Activation value
    000C  int16      Variance
    000E  [2]byte    Unused

Parameters ```object index```, ```time interval```, and ```variance``` (if set != 0) are used as quest value keys.

The ```time interval``` specifies in 0.1 seconds units within which time span the identified object shall be triggered regularly.
The ```variance``` adds some randomness to the interval. Usually low numbers are encountered in the main game.

> The unit of the variance is that of the engine-internal ticks.

The ```activation value``` has three different uses:
* greater than or equal to ```0xFFFF```: endless scheduling.
* bit ```0x1000``` set AND boolean game variable identified by this value key is true: schedule trigger
* value between ```0x0000``` (exclusive) and ```0x1000``` (exclusive): remaining schedule amount, down to zero

> The game has the ```Activation value``` typically set to ```0xFFFF```, ```0x10000``` or ```0x11111``` in the main game.


### Action Type 12: Cycle Objects

> Internally, this action is called "Alternating splitter"

**Cycle Objects Action Details** (16 byte)

    0000  [3]int32   Object indices
    000C  int32      Next object

All three ```object indices``` are interpreted as quest value keys.

This action triggers objects from the list of ```object indices```. The ```next object``` field indicates which entry to trigger next.
Afterwards, the ```next object``` field is incremented. It is reset to ```0``` if it is either greater than ```2```, or it is ```2``` and the third object index is ```0```.


### Action Type 13: Destroy Object

**Destroy Object Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Unknown
    0004  int16      Object 2 index
    0006  int16      Unknown
    0008  int16      Object 3 index
    000A  int16      Unknown
    000C  int32      Message index (within resource 0x0867); 0: no message 

All three ```object indices``` are interpreted as quest value keys.


### Action Type 14: Unused

> This action is not used and does nothing.


### Action Type 15: Receive EMail

**Receive EMail Action Details** (16 byte)

    0000  int16      EMail index, based on 0x0989
    0002  [2]byte    Unused
    0004  int16      Delay
    0008  [10]byte   Unused

The ```Delay``` is roughly counted in seconds. Tests showed their actual unit seems to be somewhere at 0.9 seconds.


### Action Type 16: Expose

**Change Effect Action Details** (16 byte)

    0000  int16      Delta value
    0002  int16      Effect change flag; 0: add delta, 1: remove delta
    0004  int32      Effect type
    0008  [8]byte    Unused

This action exposes the hacker to some typed hazard or treatment.

Effect types are:

    4: Radiation poisoning
    8: Bio contamination

> The main game uses this only once to remove the radiation in level R - treatment area.


### Action Type 17: Set Object Parameter

**Set Object Parameter Action Details** (16 byte)

    0000  int16      Object index 0
    0002  int16      Object index 2
    0004  int32      Value 1
    0008  int32      Value 2
    000C  int32      Value 3

Both object indices are treated as quest value keys.

This action sets parameters of target objects. Which value is applied to which parameter is dependent on the target.

The following table lists the mappings.

| Target Class  | Value 1         | Value 2                          | Value 3         |
|:-------------:|-----------------|----------------------------------|-----------------|
| Big Stuff     | Cosmetic value  | Data1                            | Data2           |
| Door          | Locked          | ```0xFF00```: Message index, ```0x00FF```: Cosmetic value | ```0xFF00```: Access level, ```0x00FF```: Auto-close time |
| Fixture, Trap | Parameter Number (1..4) | Parameter Value          | Unused          |

For ```big stuff``` and ```door``` classes, if a value is ```-1``` it doesn't modify the respective target parameter.

> Which parameters are specifially affected for fixtures and traps depends on their respective interpretation of the 16 bytes.


### Action Type 18: Set Screen Picture

> Internally, this action is called "animate".

**Set Screen Picture Action Details** (16 byte)

    0000  int16      Screen 1 object index
    0002  int16      Screen 2 object index
    0004  int32      Single sequence source
    0008  int32      Loop sequence source
    000C  int32      Unused

This action sets the picture source for the given screen object(s).

If provided, the ```Single sequence``` source is played first. If provided, the ```Loop sequence``` is played
next and looped. If no loop is provided, the last frame of the single sequence will be kept.


### Action Type 19: Hack

This action is a more generic one with several different interpretations, depending on the first field.

**Hack Action Details** (16 byte)

    0000  int32      Hack type
    0004  [12]byte   Hack parameter


#### Hack Action Type 1: Toggle repulsor

**Toggle Repulsor Details** (12 byte)

    0000  int32      Repulsor object index
    0004  byte       "Off" texture index (into level texture list)
    0005  byte       "On" texture index (into level texture list)
    0006  [2]byte    Unused
    0008  byte       Toggle type
    0009  [3]byte    Unused

**Toggle type** (1 byte)
 
    0x00  Toggle On/Off
    0x01  Toggle On, stay on
    0x02  Toggle Off, stay off


#### Hack Action Type 2: Show Game Code Digit

**Show Game Code Digit Details** (12 byte)

    0000  int32      Screen object index
    0004  int32      Digit number 1..6 (= level number)
    0008  [4]byte    Unused


#### Hack Action Type 3: Set Fixture Parameter from Variable

**Set Fixture Parameter From Variable Details** (12 byte)

    0000  int32      Fixture object index
    0004  int32      Parameter number (1..4)
    0008  int32      Game integer variable index

> This hack is used only twice, exclusively to set the codes for the reactor in the base game.
> Similar to the "Set Object Parameter" action, the interpretation of the modified parameter is depending on the action in the target fixture.


#### Hack Action Type 4: Set Frame State

> Although this hack is called "fixture frame", it doesn't check for the type of the target object and allows to modify any object's frame state.

**Set Frame State Details** (12 byte)

    0000  int32      Button object index
    0004  int32      New frame state; 0: off, 1: on
    0008  int32      MFD update flag; 0: ignored, != 0: update MFD


#### Hack Action Type 5: Door Control

**Door Control State Details** (12 byte)

    0000  int32      Door object index
    0004  int32      Control value
    0008  int32      Unused

Control values:

    1: open door
    2: close door
    3: toggle door
    4: disable auto-close


#### Hack Action Type 6: Return to main menu

This hack has no parameters (all 12 bytes 0x00) and directly returns to the main menu.


#### Hack Action Type 7: Rotate Object

**Rotate Object State Details** (12 byte)

    0000  int32      Object index
    0004  byte       Amount
    0005  byte       Type -- 0x00: endless, != 0x00: back/forth
    0006  byte       Direction -- 0x00: forward, 0x01 when running backwards
    0007  byte       Axis -- 0x00: Z (yaw), 0x01: X (pitch), 0x02: Y (roll)
    0008  byte       Forward limit
    0009  byte       Backward limit
    000A  [2]byte    Unused

This hack rotates an object along one axis. This rotation can be endless in one direction, or a back and forth rotation,
depending on the ```Type``` field.

If the rotation type is back and forth, then the object is rotated between the two limits. Should the object start outside of these
limits, then rotation continues normally until it is within the limits.

If the rotation type is endless, whenever one limit is reached, the object immediately jumps to an orientation as per the other limit.


#### Hack Action Type 8: Remove Critters

> This hack is called "armageddon" internally, most likely in a reference to a similar action in the game Lemmings.

**Remove Critters State Details** (12 byte)

    0000  int32      Object type - 0x00CCSSTT
    0004  int32      Radius
    0008  int32      Unused

This hack removes critters of the specified object type from the level.
The ```radius``` specifies how many shall be removed around the tile of the action's object, in tiles.

> Avoid using high amounts. The engine can remove only a certain amount of objects in one tick (100) and has no guard if more are removed.
> A memory overrun occurs in this case.


#### Hack Action Type 9: SHODAN pixelation

This hack has no parameters (all 12 bytes 0x00) and fills the screen with an image. The used image is hardcoded.
The game returns to the main menu after the screen is filled.
It should be used only in cyberspace; Using it in real world messes up tile textures.


#### Hack Action Type 10: Set Condition

**Set Condition Details** (12 byte)

    0000  int32      Object index
    0004  [4]byte    Condition
    0008  [4]byte    Unused

Sets the condition values of specified object (fixture or trap).


#### Hack Action Type 11: Show System Analyzer

    0000  int32      Page index (0..2)
    0004  [8]byte    Unused


#### Hack Action Type 12: Make Item Radioactive

**Make Item Radioactive Details** (12 byte)

    0000  int32      Radioactive object index
    0004  int16      Watched object index
    0006  int16      Watched object trigger state
    0008  [4]byte    Unused

The ```Radioactive object``` emits radiation with a radius of 2 tiles while ```Watched object``` is in given state (1 = active/open).

> This is used only once, on level 2 for the small room in Beta quadrant. The door is both the emitter as well as the watched object.


#### Hack Action Type 13: Oriented Trigger Object

> This hack is exclusively used on level 8 to trigger three different taunts from Diego.
> Hence it is also called "Diego Taunt" in the source.

**Oriented Trigger Object Details** (12 byte)

    0000  uint16     Horizontal direction
    0002  [2]byte    Unused
    0004  int32      Object index
    0008  [4]byte    Unused

This hack triggers the given object only if the player looks to the given ```Horizontal direction``` (+/- 45°).
Value ```0x0000``` specifies north, going clockwise with increasing numbers, meaning:

    0x0000  North (matches between NW and NE)
    0x4000  East (matches between NE and SE)
    0x8000  South (matches between SE and SW)
    0xC000  West (matches between SW and NW)


#### Hack Action Type 14: Close Data MFD

**Close Data MFD Details** (12 byte)

    0000  int32      Displayed object index
    0004  [8]byte    Unused

If the specified object is currently displayed in the "Data" MFD, close it and return to previous display.

> This hack is used only once to close the keypad panel from the reactor if an invalid combination is entered.


#### Hack Action Type 15: Earth destruction by laser

This hack has no parameters (all 12 bytes 0x00) and lets the player receive the message about the fired laser.
The game continues after the message has played. Images and text are hardcoded.


#### Hack Action Type 16: Change Critter Type Global

**Change Critter Type Global Action Details** (16 byte)

    0000  int32      Critter type - 0x00CCSSTT
    0004  byte       New type
    0005  [7]byte    Unused

This hack morphs all critters of given object type in the level and changes their type to the given one.
Class and subclass stay the same. Stats, such as healthpoints, are maintained as well.

If ```new type``` specifies a value greater than ```0x0F```, the object will be destroyed.


### Action Type 20: Set Tile Textures

> This action is not used in the main game.

**Set Tile Textures Action Details** (16 byte)

    0000   int32     Area
    0004   int32     Floor Operation
    0008   int32     Wall Operation
    000C   int32     Ceiling Operation

The ```area``` defines a rectangular area of affected tiles.
If ```area``` is less than ```0x10000```, then only one tile is affected, identified by ```0xXXYY```.
Otherwise, ```area``` is interpreted as two object indices (interpreted as quest value keys), that define the area by their respective location.

The three ```operation``` fields identify, per texture, what to do.

**Texture Operation** (4 byte)

    0000   int16     Source filter
    0002   int16     Destination texture

If ```source filter``` is either equal to or greater than ```0x1000```, or the tile uses the texture identified by this filter, then set the texture according to ```destination texture``` if this value is less than ```0x1000```.

Said differently, try to set ```destination texture``` only if the value is less than ```0x1000```, and consider only tiles if set as a flood-fill, or if the source matches.


### Action Type 21: Set Critter State

**Set Critter State Action Details** (16 byte)

    0000  int16      Filter parameter
    0002  int16      Filter type
    0004  int32      Area
    0008  int32      New critter mood -- ignored if value >= 0x1000
    000C  int32      New critter orders -- ignored if value >= 0x1000

Sets the state of critters.
See [Critters](14_Critter/levelCritterEntry.md) for the enumeration values of critter states.

The ```filter type``` parameter defines via bits which critters should be considered and how the ```filter parameter``` is interpreted.

**Filter Type Bitfield** (2 byte)

    0x0001  Consider loner critters only. If not set, only consider pack critters.
    0x0002  Ignore loner/pack property of critter.
    0x0004  If set, ```filter parameter``` identifies one specific critter to affect (regardless of remaining bits). Use area otherwise.
    0x0008  Affect every critter in the area, regardless of subtype/type. (Note: only works for small areas)

If neither bits ```0x0004```, nor ```0x0008``` are set, then ```filter parameter``` specifies the subclass and type of affected critters (```0xSSTT```).

The ```area``` parameter defines the affected tiles in the following ways:
* All zero: affect entire level
* Lower 16 bits zero: upper 16 bits define half of the side-length of a square, with the tile of the action object at its center.
* Two 16 bit values identifying reference objects that define a rectangular area.

Unless the ```new critter mood``` is ```friendly```, mood is ignored on combat level 0.


### Action Type 22: Trap Message

> Internally, such messages are called "barks".

**Trap Message Action Details** (16 byte)

    0000  sint32     Background image
    0004  int32      Message index (within resource 0x0867)
    0008  int32      Text colour
    000C  int32      MFD suppression flag; 0: Show in MFD, 1: Show only in HUD

The ```Background image``` and ```Text colour``` are only considered for display in MFD. The user needs to have messages set to "text" or "both" to see them.

For the ```Background image```, the following values are possible:

    -1  SHODAN (black and white)
    -2  Diego

> The game uses other image references as well, yet they don't work. This is due to the code being modified to ignore positive background image values.


### Action Type 23: Spawn Objects

**Spawn Objects Action Details** (16 byte)

    0000  int32      Object type; Format 0x00CCSSTT
    0004  int32      Area
    0008  int32      Quantity - quest value key
    000C  int32      Flags

This action spawns a random number of objects, randomly distributed within an area.
The spawned objects are placed at the floor in the center of the tile.

The ```area``` field specifies a rectangular area, either based on two objects, or a square.
If ```area``` is less than ```0x10000```, then it specifies half of the final side-length of the resulting square, and the trap object's location is at the lower-right corner of this square. Otherwise, ```area``` specifies two object indices that define the rectangle with their locations.

Objects are only spawned if the ```Combat``` value is 1 or higher. If it is 3, then one more object than requested is spawned.
Not all classes can be spawned.
> Although the game uses enemies exclusively, ammo, explosives, and patches can be spawned as well.

**Spawn Objects Flags** (4 byte)

    0x00000001       Avoid spawn if tile is within a distance of 7 based on the sum of X and Y deltas.
    0x00000002       Avoid spawn if hacker looks at the tile within a distance of 14 based on the sum of X and Y deltas.
    0x00000004       Allow spawn if tile is an elevator tile (identified by elevator music).
    0x00000008       Force spawn, even if the ceiling is too low, or the tile contains already a critter.


### Action Type 24: Change Object Type

> Internally, this action is called "transmogrify".

**Change Object Type Action Details** (16 byte)

    0000  int32      Object index
    0004  int16      New type
    0006  int16      Reset mask
    0008  [8]byte    Unused

This action sets, or toggles, the type of an object. It is primarily used to extend and retract force bridges.
> For bridges in their retracted state, the game uses "Elephant, Jorp" (```7/7/8```), an invisible dummy type.

The ```Reset mask``` is used when the specified object is already at given type. If the specified object is already at given type,
then the new type is calculated as ```New type``` XOR ```Reset mask```.
> So, to toggle between ```0x08``` and ```0x09```, ```New type``` should be ```0x08``` and ```Reset mask``` must be set to ```0x01```.
> To toggle between ```0x08``` and ```0x07```, ```New type``` should be ```0x07``` and ```Reset mask``` must be set to ```0x0F```.

> The first byte of unused field at ```0008``` is set to ```1``` for only one force bridge in the game (on level 2) and is ignored.
> It appears that an earlier version of the code supported suppressing of animations.

