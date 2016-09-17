## Level Software Table (Class 6)

```L16``` is a table describing software - programs, logs, data...

**Level Software Entry** (9 bytes)

    0000  [6]byte   Level object prefix
    0006  [3]byte   Software data


### Cyberspace programs 6/0/x, 6/1/x, 6/2/x

**Cyberspace program data** (3 bytes)

    0000  byte      Version
    0001  [2]byte   Unused


> The three subclasses can be considered "aggressive programs", "defensive programs" and "boosts". Boosts ignore the version.
> Although several programs are defined, only a few are used.
> Different version numbers also change the color of the cube.


### Fun pack modules 6/3/0

**Fun pack module data** (3 bytes)

    0000  byte      Game mask
    0001  [2]byte   Unused

**Game mask** (1 byte)

    0x01  Ping
    0x02  Eel Zapper
    0x04  Road
    0x08  Botbounce
    0x10  15
    0x20  TriopToe
    0x40  Gameb (Not working, gives "not installed" message)
    0x80  Wing 0


### Multimedia data 6/4/0, 6/4/1

**Multimedia software data** (3 bytes)

    0000  byte      Unused
    0001  byte      Index
    0002  byte      Type

**Multimedia type** (1 byte)

    0x00  E-Mail
    0x01  Log
    0x02  Data

> In the game, e-mails are received through triggers, logs are only in real-world levels, and data only in cyberspace levels. The engine supports placing any of them in any level type.


#### Logs

For logs, the ```Log chunk offset``` is based on the chunk ID ```0x09B8```. The high nibble is used as the level identifier - into
which category the log should be put.

See [Electronic Messages](../../content/ElectronicMessages.md) for details on messages.

> 6/4/0 are called "TEXT", behave like 6/4/1 "EMAIL", and are only used as logs.



## Cyberspace "Scenery" Software (Class 7, in cyberspace)

In cyberspace levels, ```L17``` is a table for additional software. It acts as an overflow for software if the first table is full.

**Scenery Software Entry** (16 bytes)

    0000  [6]byte   Level object prefix
    0006  [10]byte  Scenery software data


### Scenery software 7/3/2 ("GLOW BULB"), 7/2/0 ("SIGN")

**Scenery software data** (10 bytes)

    0000  int16     Parameter
    0002  int32     Subclass
    0006  int32     Type


For entries that specify a program, ```Parameter``` specifies the version. For fun packs, it is the game mask.

> This type is only used for software with a single parameter; Found only for programs and fun packs.
