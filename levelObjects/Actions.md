## Actions

Level trigger and various panels (buttons and the like) make use of actions that perform some state change and/or give some indication to the player. They are essentially what make a level (and the game in total) come to life.

Actions can be triggered by various sources. They may have conditions that first have to be met before the action is actually performed. The type of the action determines how to interpret the details (parameters) of the action.

**Action Data** (22 bytes)

    0000  byte       Action type
    0001  byte       Use quota
    0002  [4]byte    Condition
    0006  [16]byte   Action details

The ```Use quota``` indicates how often more this action can be executed. ```0``` means endless.
The value is only decremented if it is not zero and the condition was met.
If the value is decremented to zero after the action was executed, the object where the action is in is deleted.


### Action Type 1: Transport player

**Transport Player Action Details** (16 byte)

    0000  int32      Target X
    0004  int32      Target Y
    0008  byte       Target Height (level units)
    0009  byte       Preserve Height Flag -- yes: 0x40, no: 0x00
    000A  [2]byte    Unused
    000C  byte       Cross-Level Transport Destination
    000D  byte       Cross-Level Transport Flag
    000E  [2]byte    Unused

If the ```Preserve Height Flag``` is set to ```0x40```, then the ```Target Height``` field is ignored and the hacker
is transported to the same height in which the outgoing tile was entered. (e.g.: jumping in)

> This height-preservation is used only on level 2 for the experimental transporter, which incidentally has both
> platforms at the same height.

The ```Cross-Level Transportation Flag``` determines whether ```Cross-Level Transport Destination``` is being considered.
If the flag is ```0x00```, then it is considered to be a cross-level transport. Other values make an in-level transport.

> This flag has been found to be arbitrarily ```0x10```, ```0x20```, and ```0x22``` with no detectable difference.

The ```Cross-Level Transportation Destination``` is furthermore split up. The low nibble determines the actual target
level. This may be the same level where the trigger is in, in which case the level is "reloaded" (loading cursor seen).
If the high nibble is set to another value than zero, the transfer is considered to be a cyberspace connection.

> The destination is used only for an same-level transfer on level 4 (restoration chamber), and twice for two groves, to
> transport the hacker to the restoration chamber on level 6. Other instances are set to ```0x22```, which are ignored
> as the respective flag value is set to ```0x22``` as well.


### Action Type 2: Change Health

**Change Health Action Details** (16 byte)

    0000  int32      Unused
    0004  int16      Health delta
    0006  int16      Health change flag; 0: remove delta, 1: add delta
    0008  int16      Power delta
    000A  int16      Power change flag; 0: remove delta, 1: add delta
    000C  [4]byte    Unknown

> Reducing health to/below 0 causes death. Reducing power below 0 causes an underflow and hacker is fully powered again.


### Action Type 3: Clone/Move Object

**Clone/Move Action Details** (16 byte)

    0000  int16      Source object index
    0002  int16      Move flag: 0: clone object, !0: move object
    0004  int32      Target X tile
    0008  int32      Target Y tile
    000C  int32      Height

The orientation and in-tile position of the source object will be kept.


### Action Type 4: Set Game Variable

**Set Game Variable Action Details** (16 byte)

    0000  int32      Variable key
    0004  int16      Value
    0006  int16      Operation
    0008  int32      Message 1 (within chunk 0x0867)
    000C  int32      Message 2 (within chunk 0x0867)

Depending on the ```Variable key```, different sets of variables are modified.
The variables are stored in the [Game State Structure](../../archives/gameState.md).

If ```Variable key``` has bit ```0x1000``` set, then a int16 variable is modified. The variable index is ```key & 0x003F```.
If the bit is not set, then a boolean variable is modified. The variable index is ```key & 0x01FF```.

For the int16 variables, ```Value``` must be in the range ```0..0x0FFF```. Operation can be one of the following:

    0:  Set value
    1:  Add value
    2:  Subtract value
    3:  Multiply with value
    4:  Divide by value (Resulting fractions are cut, divide by 0 is ignored)

For the boolean variables, ```Operation``` is always ```0```. The values are either ```0``` or ```1``` to set that value, or ```2``` to toggle the current value.

If the resulting value is not zero, the (optional) ```Message 1``` is played/shown. If the resulting value is zero, the (optional) ```Message 2``` is played/shown.

> There are rare occurrences of ```Variable key``` having bit ```0x2000``` set. These behave identical as for the boolean variables and can be ignored.

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


### Action Type 7: Change lighting

**Change Lighting Action Details** (16 byte)

    0000  int16      Light type extent
    0002  int16      Reference object index
    0004  int16      Transition type. 0x0000: immediate, 0x0001: fade, 0x0100: flicker
    0006  byte       Unknown -- Only four instances have this byte set to 0x01. No detectable effect.
    0007  byte       Light modification -- 0x00: add light, 0x10: remove light
    0008  byte       Light type
    0009  byte       Unused
    000A  int16      Light surface. 0x0000: floor, 0x0001: ceiling, 0x0002: floor and ceiling
    000C  [4]byte    Light type parameter

These actions modify the light deltas of the affected tiles. Such actions can not change the base darkness
values of a tile. If a tile is fully bright by default, nothing can change that.


**Light type** (1 byte)

    0x00  Rectangular
    0x01  Linear gradient
    0x02  (behaves like rectangular)
    0x03  Circular gradient

**Gradient light type parameter** (4 bytes)

    0000  byte       Remove light begin intensity
    0001  byte       Remove light end intensity
    0002  byte       Add light begin intensity
    0003  byte       Add light end intensity

The intensity values have a range of ```0x00```..```0x7F```. A value of around ```0x14``` lights a tile as if it were fully lit.
Values above will spill over to the surrounding tiles.

#### Rectangular light (type 0x00)

For this light type, the ```Light type extent``` specifies a second object index. All tiles within this rectangle are affected.

**Light type parameter** (4 bytes)

    0000  byte       Remove light value 0x00..0x0F
    0001  byte       Add light value 0x00..0x0F
    0002  [2]byte    Unused

#### Linear gradient light (type 0x01)

For this light type, the ```Light type extent``` specifies a second object index. The begin of the gradient is the tile with
of the primary index, the end is the tile of the second index.

> This light type is not used in the regular game


#### Circular gradient light (type 0x03)

For this light type, the ```Light type extent``` specifies a radius, in tiles, of affected tiles. The begin of the gradient is always the
tile of the triggering object (```Reference object index``` is ignored).


### Action Type 8: Effect

**Effect Action Details** (16 byte)

    0000  int16      Sound index, based on 0x00C9; 0: Play nothing
    0002  int16      Sound play count; 0: endless
    0004  int16      Visual effect
    0006  [2]byte    Unknown
    0008  int16      Additional visual effect
    000A  [2]byte    Unknown
    000C  [4]byte    Unknown

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
    3: Gray static (fullscreen, endless)
    4: Vertical panning (including HUD, endless)


### Action Type 9: Change tile heights

    0000  int32      Tile X
    0004  int32      Tile Y
    0008  int16      Target floor height
    000A  int16      Target ceiling height
    000C  [4]byte    Unknown

For the height fields the value ```0x0FFF``` indicates "don't move". Otherwise it's the target height in level height units (0..31).

> The unknown field at 0x000C often has values set, to no effect.


### Action Type 11: Random Timer

**Random Timer Action Details** (16 byte)

    0000  int32      Object index
    0004  int32      Time interval
    0008  int32      Activation value
    000C  int16      Variance
    000E  [2]byte    Unused

The ```Time interval``` specifies in 0.1 seconds units within which time span the given object shall be triggered regularly.
The ```Variance``` adds some randomness to the interval. Usually low numbers are encountered.

> The unit of the variance is not clear. A value of ```0x0200``` caused random delays of up to 25 seconds.

The ```Activation value``` must be ```0xFFFF``` or higher for this action to work.

> The game has the ```Activation value``` typically set to ```0xFFFF```, ```0x10000``` or ```0x11111```. No difference has been found.


### Action Type 12: Cycle Objects

**Cycle Objects Action Details** (16 byte)

    0000  [3]int32   Object indices
    000C  int32      Next object

This action triggers objects from the list of ```Object indices```. The ```Next object``` field indicates which entry to trigger next.
Afterwards, the ```Next object``` field is incremented. It is reset to ```0``` if either ```3``` is reached or the next object index is ```0```.


### Action Type 13: Delete Object

**Delete Object Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Unknown
    0004  int16      Object 2 index
    0006  int16      Unknown
    0008  int16      Object 3 index
    000A  int16      Unknown
    000C  int32      Message index (within chunk 0x0867); 0: no message 


### Action Type 15: Receive EMail

**Receive EMail Action Details** (16 byte)

    0000  int16      EMail index, based on 0x0989
    0002  [2]byte    Unused
    0004  int16      Delay
    0008  [10]byte   Unused

The ```Delay``` is roughly counted in seconds. Tests showed their actual unit seems to be somewhere at 0.9 seconds.


### Action Type 16: Change Effect

**Change Effect Action Details** (16 byte)

    0000  int16      Delta value
    0002  int16      Effect change flag; 0: add delta, 1: remove delta
    0004  int32      Effect type
    0008  [8]byte    Unknown

Effect types are:

    4: Radiation poisoning
    8: Bio contamination

> The game uses this only once to remove the radiation in level R - treatment area.


### Action Type 17: Set Object Parameter

**Set Object Parameter Action Details** (16 byte)

    0000  int32      Object index
    0004  int32      Value 1
    0008  int32      Value 2
    000C  int32      Value 3

This action sets parameters of a target object. Which value is applied to which parameter is dependent on the target.

The following table lists the known mappings. If a value is ```0xFFFFFFFF``` it doesn't modify the target.

| Target  | Value 1      | Value 2      | Value 3         |
|:-------:|--------------|--------------|-----------------|
| Screen  | Frame count  | Unknown      | Picture source  |
| Door    | Unknown      | Unknown      | Unknown         |
| Trigger | Unknown      | Unknown      | Unknown         |


### Action Type 18: Set Screen Picture

**Set Screen Picture Action Details** (16 byte)

    0000  int16      Screen 1 object index
    0002  int16      Screen 2 object index
    0004  int32      Single sequence source
    0008  int32      Loop sequence source
    000C  int32      Unused

This action sets the picture source for the given screen object(s).

If provided, the ```Single sequence``` source is played first. If provided, the ```Loop sequence``` is played
next and looped. If no loop is provided, the last frame of the single sequence will be kept.


### Action Type 19: Change State

This action is a more generic one with several different interpretations, depending on the first field.

**Change State Action Details** (16 byte)

    0000  int32      Change type
    0004  [12]byte   Change parameter


#### Change State Type 1: Toggle repulsor

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


#### Change State Type 2: Show Game Code Digit

**Show Game Code Digit Details** (12 byte)

    0000  int32      Screen object index
    0004  int32      Digit number 1..6 (= level number)
    0008  [4]byte    Unused


#### Change State Type 3: Set Parameter from Variable

**Set Parameter From Variable Details** (12 byte)

    0000  int32      Target object index
    0004  int32      Parameter number (1 based)
    0008  int32      Game integer variable index

> This change is used only twice, exclusively to set the codes for the reactor in the base game.
> The change itself does not check for target object type and allows modification of (seemingly) arbitrary objects.
> Use with caution.


#### Change State Type 4: Set Button State

**Set Button State Details** (12 byte)

    0000  int32      Button object index
    0004  int32      New state; 0: off, 1: on
    0008  int32      Unknown


#### Change State Type 5: Door Control

**Door Control State Details** (12 byte)

    0000  int32      Door object index
    0004  int32      Control value
    0008  int32      Unused

Control values:

    1: open door
    2: close door
    3: toggle door
    4: suppress auto-close


#### Change State Type 6: Return to main menu

This change has no parameters (all 12 bytes 0x00) and directly returns to the main menu.


#### Change State Type 7: Rotate Object

**Rotate Object State Details** (12 byte)

    0000  int32      Object index
    0004  byte       Amount
    0005  byte       Type -- 0x00: endless, != 0x00: back/forth
    0006  byte       Direction -- 0x00: forward, 0x01 when running backwards
    0007  byte       Axis -- 0x00: Z (yaw), 0x01: X (pitch), 0x02: Y (roll)
    0008  byte       Forward limit
    0009  byte       Backward limit
    000A  [2]byte    Unused

This state change rotates an object along one axis. This rotation can be endless in one direction, or a back and forth rotation,
depending on the ```Type``` field.

If the rotation type is back and forth, then the object is rotated between the two limits. Should the object start outside of these
limits, then rotation continues normally until it is within the limits.

If the rotation type is endless, whenever one limit is reached, the object immediately jumps to an orientation as per the other limit.


#### Change State Type 8: Remove Objects

**Remove Objects State Details** (12 byte)

    0000  int32      Object type - 0x00CCSSTT
    0004  int32      Amount
    0008  int32      Unused

This state change removes objects of the specified object type from the level. The ```Amount``` field appears to be a guideline on
how many shall be removed. Tests showed that an even number was always considered as the lower odd number. (Specify 2, only one was removed.)

> This action only works once. Where its state is stored is not determined yet.
> Objects are not removed at random, tests showed the objects with the highest object indices were always removed first.
> Avoid using high amounts. Tests did lock up the game.


#### Change State Type 9: SHODAN pixelation

This change has no parameters (all 12 bytes 0x00) and fills the screen with an image. The used image is hardcoded.
The game returns to the main menu after the screen is filled.
It should be used only in cyberspace; Using it in real world messes up tile textures.


#### Change State Type 10: Set Condition

**Set Condition Details** (12 byte)

    0000  int32      Object index
    0004  [4]byte    Condition
    0008  [4]byte    Unused

Sets the condition values of specified object.


#### Change State Type 11: Show System Analyzer

This change has no parameters (all 12 bytes 0x00) and forces the display of the "General" tab of the system analyzer in the MFD.


#### Change State Type 12: Make Item Radioactive

**Make Item Radioactive Details** (12 byte)

    0000  int32      Radioactive object index
    0004  int16      Watched object index
    0006  int16      Watched object trigger state
    0008  [4]byte    Unused

The ```Radioactive object``` emits radiation with a radius of 2 tiles while ```Watched object``` is in given state (1 = active/open).

> This is used only once, on level 2 for the small room in Beta quadrant. The door is both the emitter as well as the watched object.


#### Change State Type 13: Oriented Trigger Object

**Oriented Trigger Object Details** (12 byte)

    0000  uint16     Horizontal direction
    0002  [2]byte    Unused
    0004  int32      Object index
    0008  [4]byte    Unused

This change triggers the given object only if the player looks to the given ```Horizontal direction``` (+/- 45°).
Value ```0x0000``` specifies north, going clockwise with increasing numbers, meaning:

    0x0000  North (matches between NW and NE)
    0x4000  East (matches between NE and SE)
    0x8000  South (matches between SE and SW)
    0xC000  West (matches between SW and NW)

> This trigger is exclusively used on level 8 to trigger three different taunts from Diego.


#### Change State Type 14: Close Data MFD

**Close Data MFD Details** (12 byte)

    0000  int32      Displayed object index
    0004  [8]byte    Unused

If the specified object is currently displayed in the "Data" MFD, close it and return to previous display.

> This change is used only once to close the keypad panel from the reactor if an invalid combination is entered.


#### Change State Type 15: Earth destruction by laser

This change has no parameters (all 12 bytes 0x00) and lets the player receive the message about the fired laser.
The game continues after the message has played. Images and text are hardcoded.


#### Change State Type 16: Change Object Type Global

**Change Object Type Global Action Details** (16 byte)

    0000  int32      Object type - 0x00CCSSTT
    0004  byte       New type
    0005  [7]byte    Unused

This state change morphs all objects of given object type in the level and changes their type to the given one.
Class and subclass stay the same. Stats, such as healthpoints, are maintained as well.


### Action Type 21: Set Critter State

**Set Critter State Action Details** (16 byte)

    0000  int16      Unknown
    0002  int16      Unknown
    0004  int16      Reference object index 1
    0006  int16      Reference object index 2
    0008  int32      New critter state
    000C  int16      Unknown
    000E  int16      Unused

Sets the state of critters inside the rectangle defined by the two reference objects.
See [Critters](14_Critters/levelCritterEntry.md) for the enumeration values of critter states.

> There are instances in which the first index is zero, while the unknown field at ```0000``` is set to a value.


### Action Type 22: Trap Message

**Trap Message Action Details** (16 byte)

    0000  sint32     Background image
    0004  int32      Message index (within chunk 0x0867)
    0008  int32      Text colour
    000C  int32      MFD suppression flag; 0: Show in MFD, 1: Show only in HUD

The ```Background image``` and ```Text colour``` are only considered for display in MFD. The user needs to have messages set to "text" or "both" to see them.

For the ```Background image```, the following values are possible:

    -1  SHODAN (black and white)
    -2  Diego

> The game uses other image references as well, apparently they don't work.


### Action Type 23: Spawn Objects

**Spawn Objects Action Details** (16 byte)

    0000  int32      Object type; Format 0x00CCSSTT
    0004  int16      Reference object index 1
    0006  int16      Reference object index 2
    0008  int32      Number of objects
    000C  [4]byte    Unknown

This action spawns a random number of objects, randomly distributed within the rectangle defined by the two reference objects.
The spawned objects are placed at the floor in the center of the tile.

Objects are only spawned if the ```Combat``` value is 1 or higher. Not all classes can be spawned.
> Although the game uses enemies exclusively, ammo, explosives, and patches can be spawned as well.
> The unknown field at 000C has been found with values between 0x00..0x04 - no recognizable effect.


### Action Type 24: Change Object Type

**Change Object Type Action Details** (16 byte)

    0000  int32      Object index
    0004  int16      New type
    0006  int16      Reset mask
    0008  int32      Unknown
    000C  [4]byte    Unused

This action sets, or toggles, the type of an object. It is primarily used to extend and retract force bridges.
> For bridges in their retracted state, the game uses "Elephant, Jorp" (```7/7/8```), an invisible dummy type.

The ```Reset mask``` is used when the specified object is already at given type. If the specified object is already at given type,
then the new type is calculated as ```New type``` XOR ```Reset mask```.
> So, to toggle between ```0x08``` and ```0x09```, ```New type``` should be ```0x08``` and ```Reset mask``` must be set to ```0x01```.
> To toggle between ```0x08``` and ```0x07```, ```New type``` should be ```0x07``` and ```Reset mask``` must be set to ```0x0F```.

> The unknown field at ```0008``` is set to ```1``` for only one force bridge in the game (on level 2) and does not seem to have any effect.

