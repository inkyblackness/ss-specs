## Access Levels

Doors can be restricted to certain access levels. The engine knows about 32 access levels (0..31).

The names of the access levels are stored as [texts](../media/Texts.md) in chunk ```0x0879```. Per access level there
are two text strings, so 64 in total. The first string is the short name, for example ```STD``` and the second the long
name, ```Standard```.

The first level (0) with name ```None``` is not checked. Doors with level 0 immediately open.
