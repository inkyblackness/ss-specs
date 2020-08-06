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
    006E  8xbyte    Realspace location; The point to return to after exiting cyberspace.
    0076  int32     Version number of the data structure; Value: 6
    007A  [14]int16  General inventory references, Object IDs. Access Cards are 0xFE 0x00

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

    018B  [10]byte  2x5 MFD slot indices
    0195  5xbyte   MFD[s] button controls. 0: enabled, no function; 1: news flash; 2: enabled, selected; 3: disabled

    01D3  byte     MFD[l] map display: 0x01: side, 0x00: zoomed
    01D4  byte     MFD[r] map display: 0x01: side, 0x00: zoomed

    Installed Hardware (0: not installed):
    02E9  byte     version for infrared night vision
    02EA  byte     version for target info
    02EB  byte     version for Sensaround
    02EC  byte     version for "Aim Enhancement Hardware"
    02ED  byte     version for HUD (unknown function)
    02EE  byte     version for bioscan
    02EF  byte     version for navigation unit
    02F0  byte     version for shield
    02F1  byte     version for data reader
    02F2  byte     version for lantern
    02F3  byte     version for view control
    02F4  byte     version for enviro suit
    02F5  byte     version for booster
    02F6  byte     version for jump jet
    02F7  byte     version for system status
	
    Installed Software(0: not installed):
    02F8  byte     version for ice drill defense breaker
    02F9  byte     version for datastorm assault software
    02FA  byte     version for mine explosive module
    02FB  byte     version for disc heavy attack program
    02FC  byte     version for pulser combat software
    02FD  byte     version for scrambler software grenade
    02FE  byte     version for virus some cool thing
    02FF  byte     version for cyber shield
    0300  byte     version for old fake id
    0301  byte     version for ice shield
    0302  byte     version for turbo navigation booster
    0303  byte     count for fake id ice infiltrator
    0304  byte     count for decoy evasion system
    0305  byte     count for recall escape program
    0306  byte     trioptimum fun pack module (bitmask); See level software entries.

    Full clips of ammunition
    030B  byte     ML STANDARD ROUNDS
    030C  byte     ML TEFLON COATED ROUNDS 
    030D  byte     SV NEEDLE DARTS
    030E  byte     SV TRANQ DARTS
    030F  byte     HOLLOW-TIP 2100 CLIP
    0310  byte     HEAVY SLUG 2100 CLIP
    0311  byte     DC RUBBER SLUGS
    0312  byte     MARK3 MAGNESIUM-TIP SHELLS
    0313  byte     MARK3 PENETRATOR SHELLS
    0314  byte     AM HORNET CLIP
    0315  byte     AM SPLINTER CLIP
    0316  byte     RF SLAG CLIP
    0317  byte     RF LARGE SLAG CLIP
    0318  byte     SG MAG PULSE CART
    0319  byte     MM RAIL CLIP
	
	Ammunition not in full clips
    031A  byte     ML STANDARD ROUNDS
    031B  byte     ML TEFLON COATED ROUNDS
    031C  byte     SV NEEDLE DARTS
    031D  byte     SV TRANQ DARTS
    031E  byte     HOLLOW-TIP 2100 CLIP
    031F  byte     HEAVY SLUG 2100 CLIP
    0320  byte     DC RUBBER SLUGS
    0321  byte     MARK3 MAGNESIUM-TIP SHELLS
    0322  byte     MARK3 PENETRATOR SHELLS
    0323  byte     AM HORNET CLIP
    0324  byte     AM SPLINTER CLIP
    0325  byte     RF SLAG CLIP
    0326  byte     RF LARGE SLAG CLIP
    0327  byte     SG MAG PULSE CART
    0328  byte     MM RAIL CLIP
	
	Patches
    0329  byte     STAMINUP STIMULANT
    032A  byte     SIGHT VISION ENCHANCEMENT
    032B  byte     BERSERK COMBAT BOOSTER
    032C  byte     MEDIPATCH HEALING AGENT
    032D  byte     REFLEX REACTION AID
    032E  byte     GENIUS MIND-ENCHANCER
    032F  byte     DETOX UNIVERSAL ANTIDOTE
	
	Explosives
    0330  byte     FRAGMENTATION GRENADE
    0331  byte     EMP GRENADE
    0332  byte     GAS GRENADE
    0333  byte     CONCUSSION BOMB
    0334  byte     LAND MINE
    0335  byte     NITROPACK
    0336  byte     EARTH SHAKER EXPLOSIVE
    
    0337  [294]byte  Message status bitfield; 47 emails + 23 data fragments + (16 logs * 14 levels)
    045D  [14]byte   Level log marker; On which level logs are in inventory

    046B  [5]byte  First weapon
      +0  byte     Subclass (0xFF means no weapon)
      +1  byte     Type
      +2  byte     Rounds
      +3  byte     Ammunition type / Energy (0x80 bit is overload)
      +4  byte     Unknown
    
    0470  [5]byte  Second weapon
    0475  [5]byte  Third weapon
    047A  [5]byte  Fourth weapon
    047F  [5]byte  Fifth weapon
    0484  [5]byte  Sixth weapon
    0489  [5]byte  Seventh weapon
    
    0490  byte     Sensaround icon active (dependent on MFD)
    0493  byte     Bioware icon active (dependent on MFD)
    0494  byte     Compass icon active (0: off, 1: on)
    048E  byte     infrared active (0: off, 1: on)
    0498  byte     FullScreen control (0: off, 1: on)

    04DF  [7]uint16  Timer settings of explosives, in 0.1 seconds; Default: 0x46 = 7 seconds

    04ED  uint16   Unused
    04EF  uint16   Time to completion of current program (seconds). Essentially unused, although used to print some strings

    04F1  uint16   ObjectID of current target
    04F3  uint32   Last weapon fire time
    04F7  uint16   Weapon fire rate; Time needed before next fire.

    04F9  [10]byte  HUD actives: weapon, grenade, drug, cart, hardware, combat soft, defense soft, misc soft, general, email

    0507  sint32   Kill count

    0517  sint32   Death count; How often hacker was resurrected.
    051B  int32    Tilt (eye position)

    051F  [12]fix  Physics state (EDMS) - see below

    054F  byte     Currently active inventory category
    0550  byte     Active bio tracks

    0567  byte     Option: Text Length (0: normal, 1: terse)


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
|  04F1  | ObjectID of current target | `0`                                                 |
|  04F3  | Last weapon fire time      | `0`                                                 |
|   **   | Various MFD fields         | To a consistent (empty) MFD display                 |
|  02E9  | Installed Hardware[10]     | `1` to hardcode "fullscreen hardware"               |
|  0550  | Active bio tracks          | `0xFF`                                              |
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
|  0337  | Message status[26]         | `0x80`, to mark email from Rebecca received         |
|  046B  | Weapons[*].type            | `0xFF`, to mark no weapon in slot                   |
|  04DF  | Explosives timer[*]        | `70` (= 7 seconds)                                  |
|  04F9  | HUD actives[9]             | `0xFF`, to mark email being active                  |


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
