## Level Variables

```L45``` contains level variables, including environmental settings.

**Level Variables** (94 bytes)

    0000  int32       Data Length. = 94
    0004  byte        Radiation poisoning level, in 0.5 LBP
    0005  byte        Gravity, in 25% OR bio contamination level, in 0.5 LBP
    0006  byte        Gravity switch. If 0, ```0005``` is bio contamination. Gravity mod otherwise.
    0007  byte        Bio contamination register. Must be >1 to have contamination to register
    0008  byte        Radiation register. Must be >1 to have radiation to register
    0009  [4]byte     Unused
    000D  [3*27]byte  Unknown


> Hazards are notified from value 0x01, but affect only starting from value 0x02 (1 LBP).

> The three 27 byte entries in the ```Unknown``` field are almost identical for all levels.
> Test levels with all bytes set to zero work as fine.
