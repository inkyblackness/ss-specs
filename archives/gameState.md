## Game state and hacker information, resource 0x0FA1 (`Player` struct)

This resource contains everything about the hacker, together with general game-state information.


    0000  20xbyte   Hacker name (incl. 0x00 termination char). Game only allows 12 characters for input at start.
    0014  byte      Realspace Level; The level to return to after exiting cyberspace.

    0015  4xbyte    Difficulties (Game rating): combat, mission, puzzle, cyber. All 0..3
    0019  3xbyte    Unused.

    001C  uint32    Game time
    0020  uint32    Last second update
    0024  uint32    Last drug update
    0028  uint32    Last ware update
    002C  uint32    Last animation check
    0030  int32     Queue Time
    0034  int32     Delta Time
    0038  byte      Detail Level

    0039  byte      Current level identifier
    003A  22xint16  Initial SHODAN levels
    0066  6xbyte    Controls
    006C  int16     Player Object ID
    006E  [8]byte   Realspace location; The point to return to after exiting cyberspace.
    0076  int32     Version number of the data structure; Value: 6
    007A  [14]int16  General inventory references, Object IDs

    0096  byte      Posture
    0097  byte      Foot planted; 1: on the floor
    0098  byte      Lean X
    0099  byte      Lean Y
    009A  int16     Unused

    009C  byte      Health; 0x00..0xFF
    009D  byte      Cyberspace Integrity
    009E  uint16    Hitpoint regeneration per minute
    00A0  8xbyte    Damage rate per minute [explosion, energy, magnetic, radiation, gas, tranq, needle, bio]
    00A8  uint16    Bio post expose
    00AA  uint16    Rad post expose
    00AC  byte      Power 0x00..0xFF
    00AD  byte      Power usage in JPM
    00AE  byte      Power regeneration
    00AF  byte      Energy out; 1: is out of energy
    00B0  int16     Cyberspace trips
    00B2  int32     Cyberspace time base

    00B6  [64]uint8  512 Boolean game variables; Bytes are counted up, bits within byte counted from LSB; Example: bit[10] = 0x00 0x04 0x..
    00F6  [64]int16  64 Integer game variables

    0176  uint32    HUD modes; Bitfield as per hud.h
    017A  byte      Unused
    017B  sint32    Fatigue
    017F  uint16    Fatigue spending; Current rate of fatigue expenditure in pts/sec
    0181  uint16    Fatigue regeneration
    0183  uint16    Fatigue regeneration base
    0185  uint16    Fatigue regeneration maximum
    0187  sint8     Accuracy
    0188  byte      Shield absorb rate, in percent
    0189  byte      Shield threshold
    018A  byte      Light value; setting of lamp

    Multi-Function-Display (MFD) state
    018B  [10]byte  2x5 MFD slot indices
    0195  [7]byte   MFD status
    019C  [7]byte   MFD function IDs
    01A3  [32]byte  MFD function flags
    01C3  [256]byte MFD function data
    02C3  [2]byte   current MFDs
    02C5  [2]byte   MFD empty functions
    02C7  [64]byte  MFD access puzzles
    0307  [2]byte   MFD save slots


    Installed Hardware (0: not installed):
    0309  byte     version for infrared night vision
    030A  byte     version for target info
    030B  byte     version for Sensaround
    030C  byte     version for "Aim Enhancement Hardware"
    030D  byte     version for HUD (unknown function)
    030E  byte     version for bioscan
    030F  byte     version for navigation unit
    0310  byte     version for shield
    0311  byte     version for data reader
    0312  byte     version for lantern
    0313  byte     version for view control
    0314  byte     version for enviro suit
    0315  byte     version for booster
    0316  byte     version for jump jet
    0317  byte     version for system status
	
    Installed Software(0: not installed):
    0318  [7]byte  offensive software
    031F  [3]byte  defensive software
    0322  [9]byte  misc software

    Full clips of ammunition
    032B  byte     ML STANDARD ROUNDS
    032C  byte     ML TEFLON COATED ROUNDS 
    032D  byte     SV NEEDLE DARTS
    032E  byte     SV TRANQ DARTS
    032F  byte     HOLLOW-TIP 2100 CLIP
    0330  byte     HEAVY SLUG 2100 CLIP
    0331  byte     DC RUBBER SLUGS
    0332  byte     MARK3 MAGNESIUM-TIP SHELLS
    0333  byte     MARK3 PENETRATOR SHELLS
    0334  byte     AM HORNET CLIP
    0335  byte     AM SPLINTER CLIP
    0336  byte     RF SLAG CLIP
    0337  byte     RF LARGE SLAG CLIP
    0338  byte     SG MAG PULSE CART
    0339  byte     MM RAIL CLIP
	
	Ammunition not in full clips
    033A  byte     ML STANDARD ROUNDS
    033B  byte     ML TEFLON COATED ROUNDS
    033C  byte     SV NEEDLE DARTS
    033D  byte     SV TRANQ DARTS
    033E  byte     HOLLOW-TIP 2100 CLIP
    033F  byte     HEAVY SLUG 2100 CLIP
    0340  byte     DC RUBBER SLUGS
    0341  byte     MARK3 MAGNESIUM-TIP SHELLS
    0342  byte     MARK3 PENETRATOR SHELLS
    0343  byte     AM HORNET CLIP
    0344  byte     AM SPLINTER CLIP
    0345  byte     RF SLAG CLIP
    0346  byte     RF LARGE SLAG CLIP
    0347  byte     SG MAG PULSE CART
    0348  byte     MM RAIL CLIP
	
	Patches
    0349  byte     STAMINUP STIMULANT
    034A  byte     SIGHT VISION ENCHANCEMENT
    034B  byte     BERSERK COMBAT BOOSTER
    034C  byte     MEDIPATCH HEALING AGENT
    034D  byte     REFLEX REACTION AID
    034E  byte     GENIUS MIND-ENCHANCER
    034F  byte     DETOX UNIVERSAL ANTIDOTE
	
	Explosives
    0350  byte     FRAGMENTATION GRENADE
    0351  byte     EMP GRENADE
    0352  byte     GAS GRENADE
    0353  byte     CONCUSSION BOMB
    0354  byte     LAND MINE
    0355  byte     NITROPACK
    0356  byte     EARTH SHAKER EXPLOSIVE
    
    0357  [294]byte  Message status bitfield; 47 emails + 23 data fragments + (16 logs * 14 levels)
    047D  [14]byte   Level log marker; On which level logs are in inventory

    048B  [5]byte  First weapon
      +0  byte     Subclass (0xFF means no weapon)
      +1  byte     Type
      +2  byte     Rounds / Heat
      +3  byte     Ammunition type / Energy setting (0x80 bit is overload)
      +4  byte     Unused
    
    0490  [5]byte  Second weapon
    0495  [5]byte  Third weapon
    049A  [5]byte  Fourth weapon
    049F  [5]byte  Fifth weapon
    04A4  [5]byte  Sixth weapon
    04A9  [5]byte  Seventh weapon
    
    04AE  [15]byte  Hardware on/off
    04BD  [19]byte  Software status on/off

    04D0  byte     Jumpjet energy fraction
    04D1  [32]byte email sender count
    04F1  [7]byte  Patches time remainder
    04F8  [7]byte  Patches intensity 

    04FF  [7]uint16  Timer settings of explosives, in 0.1 seconds; Default: 0x46 = 7 seconds

    050D  uint16   Unused
    050F  uint16   Time to completion of current program (seconds). Essentially unused, although used to print some strings

    0511  uint16   ObjectID of current target
    0513  uint32   Last weapon fire time
    0517  uint16   Weapon fire rate; Time needed before next fire.

    0519  [10]byte  HUD actives: weapon, grenade, drug, cart, hardware, combat soft, defense soft, misc soft, general, email

    0523  uint16   Object under cursor before cyberspace
    0525  uint16   Last panel reference

    0527  sint32   Kill count
    052B  sint32   Time in cyberspace
    052F  sint32   Number of shots fired
    0533  sint32   Number of hits

    0537  sint32   Death count; How often hacker was resurrected.
    053B  int32    Tilt (eye position)

    053F  [12]fix  Physics state (EDMS) - see below

    056F  byte     Currently active inventory category
    0570  byte     Active bio tracks

    0571  int16    current email

    0573  [6]byte  version
    0579  byte     dead flag
    057A  uint16   lean filter state
    057C  [2]byte  unused
    057E  byte     MFD save field

    057F  uint32   Auto fire click
    0583  uint32   Posture state
    0587  byte     Option: Text Length (0: normal, 1: terse)
    0588  uint32   Last head bob
    058C  [9]byte  Padding


> In archive.dat, this table is (nearly) all 0x00, and also slightly smaller than necessary.


### Physics state (EDMS `State` structure, 48 bytes)

    0000  fix      X Coordinate (East -> West)
    0004  fix      Y Coordinate (North -> South)
    0008  fix      Z Coordinate (above ground)
    000C  fix      Yaw; 0 is facing East, going counter-clockwise
    0010  fix      Upper body back force (?)
    0014  fix      Upper body left force (?)
    0018  fix      Forward force (?) -- X dot product
    001C  fix      Left force (?) -- Y dot product
    0020  fix      Unknown -- Z dot product
    0024  fix      Unknown
    0028  fix      Unknown
    002C  fix      Unknown

Rotation values are in range 0..2PI.

### Message status enumeration (1 byte)

    0x80  Message received
    0x40  Message read
    0x3F  Unused


### Game variables

Game variables are described on a [separate page](gameVariables.md)

### Initial game state

The original engine does not allow for having `archive.dat` specify the initial state for both the game and hacker.

The `System Shock Enhanced Edition Source Port` introduced a modified start behaviour,
which allows for (almost) full control on how and where the protagonist starts:
When the first field of the `physics state` (the `X` coordinate of EDMS state) is not zero,
then most default settings are skipped, and data is taken from `archive.dat`.
Still, some minimal setup is performed either way, in order to have a stable game.

> This behaviour exists already in the original engine, but it only considers the physics structure itself,
> and no further information, such as the current level or any game variable.

#### Mission-independent hardcoded defaults

The following defaults are being applied regardless of existing data in `archive.dat`.

##### Game state fields

| Offset | Name                       | Initial value                                       |
|--------|----------------------------|-----------------------------------------------------|
|  0000  | Hacker name                | as per start menu                                   |
|  0015  | Difficulties               | as per start menu                                   |
|  0038  | Detail Level               | as per configuration                                |
|  003A  | Initial SHODAN levels      | all to `-1`                                         |
|  006C  | Player Object ID           | `0`                                                 |
|  00A0  | Damage rate per minute     | all to `0`                                          |
|  0511  | ObjectID of current target | `0`                                                 |
|  0513  | Last weapon fire time      | `0`                                                 |
|   **   | Various MFD fields         | To a consistent (empty) MFD display                 |
|  0309  | Installed Hardware[10]     | `1` to hardcode "fullscreen hardware"               |
|  0570  | Active bio tracks          | `0xFF`                                              |
|  00F6  | Integer game variables[48] | Language, as per configuration                      |

The two "random" game variables (index 31 and 32) are randomized only if they are equal.


#### Mission-dependent defaults

The following defaults are being applied *only* if `physics state` field `X` is zero.

##### Game state fields

| Offset | Name                       | Initial value                                       |
|--------|----------------------------|-----------------------------------------------------|
|  0039  | Current level identifier   | `1`                                                 |
|  007A  | General Inventory          | all to `0`                                          |
|  009C  | Health                     | `212`                                               |
|  009D  | Cyberspace Integrity       | `255`                                               |
|  009E  | Hitpoint regeneration      | `0`                                                 |
|  00AC  | Power                      | `255`                                               |
|  00B2  | Cyberspace time base       | `504000` (30 minutes in 280 ticks-per-second)       |
|  0181  | Fatigue regeneration       | `0`                                                 |
|  0183  | Fatigue regeneration base  | `100`                                               |
|  0183  | Fatigue regeneration max   | `400`                                               |
|  0187  | Accuracy                   | `100`                                               |
|  0188  | Shield absorb rate         | `0`                                                 |
|  0357  | Message status[26]         | `0x80`, to mark email from Rebecca received         |
|  048B  | Weapons[*].type            | `0xFF`, to mark no weapon in slot                   |
|  04FF  | Explosives timer[*]        | `70` (= 7 seconds)                                  |
|  0519  | HUD actives[9]             | `0xFF`, to mark email being active                  |
|  053F  | Physics state (EDMS)       | X=30.5, Y=22.5, Z=planted at floor, Alpha=West      |


##### Variables

The following integer variables are set:

| Integer | Value  |
|---------|--------|
|    3    | 2      |
|   12    | 3      |
|   51    | 256    |

And the following boolean variables are set to `true`:

```
0x001, 0x002, 0x003, 0x010, 0x012, 0x015, 0x016, 0x017, 0x018, 0x019, 0x01A,
0x020, 0x021, 0x024, 0x025,
0x04B, 0x04C, 0x04D, 0x04E, 0x04F,
0x050, 0x051, 0x052, 0x053, 0x054, 0x055, 0x056, 0x057, 0x058, 0x059, 0x05A, 0x05B, 0x05C, 0x05D, 0x05E, 0x05F,
0x070, 0x071, 0x072, 0x073, 0x074, 0x075, 0x076, 0x077, 0x078, 0x079, 0x07A, 0x07B, 0x07C, 0x07D, 0x07E, 0x07F,
0x0A0, 0x0A1, 0x0A2, 0x0A3, 0x0A4, 0x0A5, 0x0A6, 0x0A7, 0x0A8, 0x0A9,
0x0C0, 0x0C1, 0x0C2, 0x0C3, 0x0C4, 0x0C5, 0x0C6, 0x0C7, 0x0C8, 0x0C9, 0x0CA, 0x0CB, 0x0CC, 0x0CD, 0x0CE, 0x0CF,
0x0E1, 0x0E3, 0x0E5, 0x0E7, 0x0E9, 0x0EB, 0x0ED, 0x0EF,
0x0F1, 0x0F3, 0x0F5, 0x0F7, 0x0F9, 0x0FB, 0x0FD, 0x0FF,
0x101, 0x103, 0x105, 0x107, 0x109, 0x10B, 0x10D, 0x10F,
0x111, 0x113, 0x115, 0x117, 0x119, 0x11B, 0x11D, 0x11F,
0x121, 0x123, 0x125, 0x127, 0x129, 0x12B,
```
