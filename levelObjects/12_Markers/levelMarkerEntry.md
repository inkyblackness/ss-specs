## Level Marker Table (Class 12)

```L22``` is a table describing marker items.

**Level Marker Entry** (28 bytes)

    0000  [6]byte   Level object prefix
    0006  [22]byte  Marker data

### Repulsors 12/0/10

**Repulsor Data** (22 byte)

    0000  [10]byte  Unused; A few have the first byte 0x06 without effect
    000A  [4]byte   Unknown
    000E  byte      Unused
    000F  int16     Repulsion height; 0x0000: endless, 0x0100: one tile height
    0011  byte      Unused
    0012  int32     Repulsion flags; 0x00000001: disabled (float down); 0x00000008: strong repulsor


### Triggers 12/0/x

The type of the trigger object determines its cause. The ```Action``` field of the trigger data
determines what happens. The action also specifies how the action details are to be interpreted.

Trigger Types:

    0: Entry Trigger; Hacker enters tile
    1: Null Trigger; Must be set off externally. Also used as data storage.
    2:
    3: Player Death; Used to resurrect Hacker
    8: Level Entry Trigger; Used for instance to initialize starting health
    10: This is not a trigger! See repulsors above.


**Trigger Marker Data** (22 bytes)

    0000  byte       Action
    0001  byte       Use quota
    0002  [4]byte    Condition
    0006  [16]byte   Trigger action details

The ```Use quota``` indicates how often more this action can be executed. ```0``` means endless.
The value is only decremented if it is not zero and the condition was met.
If the value is decremented to zero after the action was executed, the object is deleted.


#### Trigger Action 0: Do nothing or the default


#### Trigger Action 1: Transport player

**Transport Player Trigger Action Details** (16 byte)

    0000  int32      Target X
    0004  int32      Target Y
    0008  int32      Unknown
    000C  int32      Unknown


#### Trigger Action 2: Change Health

**Change Health Trigger Action Details** (16 byte)

    0000  int32      Unused
    0004  int16      Health delta
    0006  int16      Health change flag; 0: remove delta, 1: add delta
    0008  int16      Power delta
    000A  int16      Power change flag; 0: remove delta, 1: add delta
    000C  [4]byte    Unknown

> Reducing health to/below 0 causes death. Reducing power below 0 causes an underflow and hacker is fully powered again.


#### Trigger Action 3: Clone/Move Object

**Clone/Move Trigger Action Details** (16 byte)

    0000  int16      Source object index
    0002  int16      Move flag: 0: clone object, !0: move object
    0004  int32      Target X tile
    0008  int32      Target Y tile
    000C  int32      Height

The orientation and in-tile position of the source object will be kept.


#### Trigger Action 4: Set Game Variable

**Set Game Variable Trigger Action Details** (16 byte)

    0000  int32      Variable key
    0004  int16      Value
    0006  int16      Operation
    0008  int32      Message 1
    000C  int32      Message 2

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


#### Trigger Action 5: Cutscene

**Cutscene Trigger Action Details** (16 byte)

    0000  int32      Cutscene index; 0: Death, 1: Intro, 2: Ending
    0004  int32      End game flag
    0008  [8]byte    Unused

```End game flag``` must be ```1```. Setting it to ```0``` leaves the game in an unplayable state after the cutscene.

What happens after the cutscene is dependent on the cutscene itself: The death and intro cutscenes return to the main menu,
the ending cutscene ends the game (showing score and credits).

> In the regular game, this action is used only once when defeating SHODAN.


#### Trigger Action 6: Trigger other objects

**Trigger Objects Trigger Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Object 1 delay
    ...
    000C  int16      Object 4 index
    000E  int16      Object 4 delay

The delays are all relative to the execution start of the action.


#### Trigger Action 7: Change lightning

**Change Lightning Trigger Action Details** (16 byte)

    0000  int16      Unknown
    0002  int16      Reference object index
    0004  int16      Transition type. 0x0000: immediate, 0x0001: fade, 0x0100: flicker
    0006  [2]byte    Unknown
    0008  [2]byte    Unknown
    000A  int16      Light surface. 0x0000: floor, 0x0001: ceiling, 0x0002: floor and ceiling
    000C  [4]byte    Unknown


#### Trigger Action 8: Effect

**Effect Trigger Action Details** (16 byte)

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
    3: Escape pod: Shows launch sequence, cam shake and abort; Plays Shodan message afterwards
    4: Red static (fullscreen)
    5: Red static (interference)

Additional Visual Effects:

    0: None
    1: White flash
    2: Pink flash
    3: Gray static (fullscreen, endless)
    4: Vertical panning (including HUD, endless)


#### Trigger Action 9: Change tile heights

    0000  int32      Tile X
    0004  int32      Tile Y
    0008  int16      Target floor height
    000A  int16      Target ceiling height
    000C  [4]byte    Unknown

For the height fields the value ```0x0FFF``` indicates "don't move". Otherwise it's the target height in level height units.


#### Trigger Action 11: Random Timer

**Random Timer Trigger Action Details** (16 byte)

    0000  int32      Object index
    0004  int32      Time limit
    0008  int32      Activation value
    000C  [4]byte    Unknown

The ```Time limit``` specifies within which time span the given object shall be triggered randomly. The first activation
waits until the complete time limit has elapsed, further activations are random within this limit.

The ```Activation value``` must be ```0xFFFF``` or higher for this action to work.

> The game has the ```Activation value``` typically set to ```0xFFFF```, ```0x10000``` or ```0x11111```. No difference has been found.


#### Trigger Action 12: Cycle Objects

**Cycle Objects Trigger Action Details** (16 byte)

    0000  [3]int32   Object indices
    000C  int32      Next object

This action triggers objects from the list of ```Object indices```. The ```Next object``` field indicates which entry to trigger next.
Afterwards, the ```Next object``` field is incremented and reset to ```0``` if either ```3``` is reached or the next object index is ```0```.


#### Trigger Action 13: Delete Object

**Delete Object Trigger Action Details** (16 byte)

    0000  int16      Object 1 index
    0002  int16      Unknown
    0004  int16      Object 2 index
    0006  int16      Unknown
    0008  int16      Object 3 index
    000A  int16      Unknown
    000C  int32      Message index; 0: no message


#### Trigger Action 15: Receive EMail

**Receive EMail Trigger Action Details** (16 byte)

    0000  int16      EMail index, based on 0x0989
    0002  [14]byte   Unknown


#### Trigger Action 16: Change Effect

**Change Effect Trigger Action Details** (16 byte)

    0000  int16      Delta value
    0002  int16      Effect change flag; 0: add delta, 1: remove delta
    0004  int32      Effect type
    0008  [8]byte    Unknown

Effect types are:

    4: Radiation poisoning
    8: Bio contamination

> The game uses this only once to remove the radiation in level R - treatment area.


#### Trigger Action 19: Change State

This action is a more generic, with several different interpretations, depending on the first field.

**Change State Trigger Action Details** (16 byte)

    0000  int32      Change type
    0004  [12]byte   Change parameter

##### Change State Type 1: Toggle repulsor

**Toggle Repulsor Details** (12 byte)

    0000  int32      Repulsor object index
    0004  byte       "Off" texture index (into level texture list)
    0005  byte       "On" texture index (into level texture list)
    0006  [6]byte    Unused


#### Change State Type 2: Show Game Code Digit

**Show Game Code Digit Details** (12 byte)

    0000  int32      Screen object index
    0004  int32      Digit number 1..6 (= level number)
    0008  [4]byte    Unused


##### Change State Type 6: Return to main menu

This change has no parameters (all 12 bytes 0x00) and directly returns to the main menu.


##### Change State Type 9: SHODAN pixelation

This change has no parameters (all 12 bytes 0x00) and fills the screen with an image. The used image is hardcoded.
The game returns to the main menu after the screen is filled.
It should be used only in cyberspace; Using it in real world messes up tile textures.


##### Change State Type 15: Earth destruction by laser

This change has no parameters (all 12 bytes 0x00) and lets the player receive the message about the fired laser.
The game continues after the message has played. Images and text are hardcoded.


#### Trigger Action 22: Trap Message

**Trap Message Trigger Action Details** (16 byte)

    0000  sint32     Background image
    0004  int32      Message index
    0008  int32      Text colour
    000C  int32      MFD suppression flag; 0: Show in MFD, 1: Show only in HUD

The ```Background image``` and ```Text colour``` are only considered for display in MFD. The user needs to have messages set to "text" or "both" to see them.

For the ```Background image```, the following values are possible:

    -1  SHODAN (black and white)
    -2  Diego

> The game uses other image references as well, apparently they don't work.


#### Trigger Action 23: Spawn Objects

**Spawn Objects Trigger Action Details** (16 byte)

    0000  int32      Object type; Format 0x00CCSSTT
    0004  int16      Reference object index 1
    0006  int16      Reference object index 2
    0008  int32      Number of objects
    000C  [4]byte    Unknown

This action spawns a random number of objects, randomly distributed within the rectangle defined by the two reference objects.
The spawned objects are placed at the floor in the center of the tile.

Objects are only spawned if the ```Combat``` value is 1 or higher. Not all classes can be spawned.
> Although the game uses enemies exclusively, ammo, explosives, and patches can be spawned as well.


#### Trigger Action 24: Change Object Type

**Change Object Type Trigger Action Details** (16 byte)

    0000  int32      Object index
    0004  int16      New type
    0006  int16      Unknown
    0008  int32      Unknown
    000C  [4]byte    Unused

This action changes the type of an object. It is primarily used to extend and retract force bridges.
> For bridges in their retracted state, the game uses "Elephant, Jorp" (```7/7/8```), an invisible dummy type.

> The unknown field at ```0006``` is set to ```0x000F``` or ```0x0001``` for a few bridges on level 4 and 5. Effect unknown.
> The unknown field at ```0008``` is set to ```1``` for only one force bridge in the game (on level 2) and does not seem to have any effect.

