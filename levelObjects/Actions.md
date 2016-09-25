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
    0008  int32      Unknown
    000C  int32      Unknown


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

The delays are all relative to the execution start of the action.


### Action Type 7: Change lightning

**Change Lightning Action Details** (16 byte)

    0000  int16      Unknown
    0002  int16      Reference object index
    0004  int16      Transition type. 0x0000: immediate, 0x0001: fade, 0x0100: flicker
    0006  [2]byte    Unknown
    0008  [2]byte    Unknown
    000A  int16      Light surface. 0x0000: floor, 0x0001: ceiling, 0x0002: floor and ceiling
    000C  [4]byte    Unknown


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

For the height fields the value ```0x0FFF``` indicates "don't move". Otherwise it's the target height in level height units.


### Action Type 11: Random Timer

**Random Timer Action Details** (16 byte)

    0000  int32      Object index
    0004  int32      Time limit
    0008  int32      Activation value
    000C  [4]byte    Unknown

The ```Time limit``` specifies within which time span the given object shall be triggered randomly. The first activation
waits until the complete time limit has elapsed, further activations are random within this limit.

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
    000C  int32      Message index; 0: no message


### Action Type 15: Receive EMail

**Receive EMail Action Details** (16 byte)

    0000  int16      EMail index, based on 0x0989
    0002  [14]byte   Unknown


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
    0006  [6]byte    Unused


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


#### Change State Type 7: Unknown

    0000  int32      Object index 1
    0004  int32      Object index 2
    0008  int32      Unknown

> This entry is only used on level R and Alpha grove to define areas between the two objects. Purpose and effect unknown.


#### Change State Type 8: Unknown

> This entry is only used on level 2 in three arbitrary locations. Purpose and effect unknown.


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


#### Change State Type 16: Unknown

    0000  int32      Object type - 0x00CCSSTT
    0004  int32      Unknown
    0008  int32      Unused

> This entry is only used on level 8, for 4 buttons. Purpose and effect unknown.


### Action Type 21: Unknown


### Action Type 22: Trap Message

**Trap Message Action Details** (16 byte)

    0000  sint32     Background image
    0004  int32      Message index
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


### Action Type 24: Change Object Type

**Change Object Type Action Details** (16 byte)

    0000  int32      Object index
    0004  int16      New type
    0006  int16      Unknown
    0008  int32      Unknown
    000C  [4]byte    Unused

This action changes the type of an object. It is primarily used to extend and retract force bridges.
> For bridges in their retracted state, the game uses "Elephant, Jorp" (```7/7/8```), an invisible dummy type.

> The unknown field at ```0006``` is set to ```0x000F``` or ```0x0001``` for a few bridges on level 4 and 5. Effect unknown.
> The unknown field at ```0008``` is set to ```1``` for only one force bridge in the game (on level 2) and does not seem to have any effect.

