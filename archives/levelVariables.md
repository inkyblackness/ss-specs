## Level Variables

```L45``` contains level variables, including environmental settings.

**Level Variables** (94 bytes)

    0000  int16       Data Length. = 94
    0002  byte        Mist; Unused (0x00)
    0003  byte        Gravity; Unused (0x00)
    0004  byte        Radiation poisoning level, in 0.5 LBP
    0005  byte        Gravity, in 25% OR bio contamination level, in 0.5 LBP
    0006  byte        Gravity switch. If 0, ```0005``` is bio contamination. Gravity mod otherwise.
    0007  byte        Bio contamination register. Must be >1 to have contamination to register
    0008  byte        Radiation register. Must be >1 to have radiation to register
    0009  uint32      Exit Time; Unused in savegame
    000D  [3*27]byte  3 Automap Variables entries, for first, second, full-screen (see below)


> Hazards are notified from value 0x01, but affect only starting from value 0x02 (1 LBP).


**Automap Variables** (27 bytes)

    0000  byte        Initialized.
    0001  byte        Zoom; 1 (farthest) .. 5 (nearest)
    0002  sint32      X-coordinate; (as fix32 value)
    0006  sint32      Y-coordinate; (as fix32 value)
    000A  uint16      Unknown
    000C  uint16      Unknown
    000E  uint16      Follow object; The object to focus on; Always 0x0001 (hacker)
    0010  uint16      Sensor object
    0012  uint16      Note object
    0014  uint16      Flags
    0016  uint16      Available flags
    0018  byte        Version ID
    0019  uint16      Sensor radius; Scan radius, in tiles and sub-tiles
