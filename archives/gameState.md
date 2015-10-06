## Game state and hacker information, chunk 0x0FA1

This chunk contains everything about the hacker, together with general game-state information.


    0000  20xbyte  Hacker name (incl. 0x00 termination char). Game only allows 12 characters for input at start.
    0015  4xbyte   Game rating: combat, mission, puzzle, cyber. All 0..3

    0039  byte     Current level identifier

    007A  14xref   General inventory references, 2 bytes each. Access Cards are 0xFE 0x00

    009C  byte     Health 0x00..0xFF

    00A3  byte     Current radiation poisoning in 0.5 LBP units

    00A7  byte     Current bio contamination in 0.5 LBP units

    00AC  byte     Power 0x00..0xFF
    00AD  uint8    Power usage in JPM

    00B6  512xbool Boolean game variables
                   Game variables are set by SetVariable action in trigger objects
                   Variable is set here when 0x1000 bit is false in the key
                   Key is 0x01FF masked to get index
    
                   Known boolean game variable indices:
                   7 Shield on
                   10 Delta	grove enabled
                   11 Alpha grove enabled
                   12 Beta grove enabled
                   15 Beta grove launched
                   21 Life pods enabled, reactor on destruct
                   145 Online help
    
    00F6  40xuint16 Accumulating game variables
                   Game variables are set by SetVariable action in trigger objects
                   Variable is set here when 0x1000 bit is true in the key
                   Key is 0x003F masked to get index
    
    0148  byte     Option: Audio, Music Volume (0x00 .. 0x64)
    014A  uint16   Option: Video, Gamma (Default: 0x4A3D)
    014C  byte     Option: Audio, Digital FX Volume (0x00 .. 0x64)
    014E  byte     Option: Input, Mouse hand (Right: 0, Left: 1)
    0154  uint16   Option: Input, Double Click (Default: 0x5555)
    0156  byte     Option: Language setting (0: ENG, 1: FRA, 2: GER)
    0158  byte     Option: Audio, Message Volume (0x00 .. 0x64)  
    0159  uint16   Option: Video, Resolution (0x0000: default?, 0x0001: 320x200, 0x0101: 320x400, 0x0201: 640x400, 0x0003: 640x480)  

    0160  byte     Option: Audio, Messages (0: Text, 1: Speech, 2: Both)
    016A  byte     Option: Audio, Channels (0: 2ch, 1: 4ch, 2: 8ch)

    0176  uint32   HUD message flags: 0x80: shield absorbs %d%%, 0x40: night sight active; 0x01000000: time remaining

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
    
    046B  5x byte  First weapon
      +0  byte     Subclass
      +1  byte     Type
      +2  byte     Rounds
      +3  byte     Ammunition type / Energy (0x80 bit is overload)
      +4  byte     Unknown
    
    0470  5x byte  Second weapon
    0475  5x byte  Third weapon
    047A  5x byte  Fourth weapon
    047F  5x byte  Fifth weapon
    0484  5x byte  Sixth weapon
    0489  5x byte  Seventh weapon
    
    0490  byte     Sensaround icon active (dependent on MFD)
    0493  byte     Bioware icon active (dependent on MFD)
    0494  byte     Compass icon active (0: off, 1: on)
    048E  byte     infrared active (0: off, 1: on)
    0498  byte     FullScreen control (0: off, 1: on)

    0507  sint32   Kill count

    051b  int32    tilt (plus further bytes...)

    0520  int16    X Coordinate (east/west)

    0524  int16    Y Coordinate (north/south)

    0528  uint16   height above ground. standard head height: 0x00BD
    052C  float32  pitch. 0 is facing East, going clockwise  

    0530  float32  upper body back force (?)
    0534  float32  upper body left force (?)
    0538  float32  forward force (?)
    053C  float32  left force (?)


    0567  byte     Option: Text Length (0: normal, 1: terse)

### Notes

#### New Game
In archive.dat, this table is (nearly) all 0x00.

#### 0x01D3/0x01D4
For the MFD[s] map displays the game stores 0x05 for side view, apparently treating bit0 as a flag.


### Unmapped game options

#### Options not (yet) found in archives
* Option: Input, Popup Cursor (Off, On)
* Option: Video, Detail
* Option: Audio, Stereo (Normal, Reversed)

#### Options not determined:
* Option: Joystick
* Option: Video, Headset
