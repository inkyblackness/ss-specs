## Level Variables

```L45``` contains level variables, including environmental settings.

**Level Variables** (94 bytes)

    0000  int32     Unknown. -- Equals chunk length in archive
    0004  byte      Radiation poisoning level, in 0.5 LBP
    0005  byte      Gravity, in 25% OR bio contamination level, in 0.5 LBP
    0006  byte      Gravity switch. If 0, ```0005``` is bio contamination. Gravity mod otherwise.
    0007  byte      Radiation register? (Must be >1 to have radiation register) -- 0xFF default
    0008  byte      Bio contamination register? (Must be >1 to have contamination register) -- 0x04 default

> Hazards are notified from value 0x01, but affect only starting from value 0x02 (1 LBP).
