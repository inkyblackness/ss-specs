## Level Item Table (Class 8)

```L18``` is a table describing general items

**Level Item Entry** (16 bytes)

    0000  [6]byte   Level object prefix
    0006  [10]byte  Item data

### Junk 8/0/x

#### Papers 8/0/2

**Paper Item** (10 bytes)

    0000  [2]byte   Unused
    0002  int16     Paper ID
    0004  [6]byte   Unused

The ```Paper ID``` refers to a paper text entry and is the offset from resource ID ```0x003C```.
The referred text entries have multiple blocks and the engine simply concatenates all these strings to one text.


#### Briefcases 8/0/7

**Briefcase Item** (10 bytes)

    0000  [2]byte   Unused
    0002  [4]int16  Object index


### Broken Stuff 8/1/x

Nothing special.


### Dead Bodies 8/2/x

#### Corpses 8/2/0 - 8/2/7

**Corpse Item** (10 bytes)

    0000  [2]byte   Unknown
    0002  [4]int16  Object index

> A few corpses have the first byte set to 0x01. Only found with empty corpses, and effect unknown.

#### Severed Heads 8/2/13, 8/2/14

**Severed Head** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Image index
    0003  [7]byte   Unused

If ```Image index``` is zero, the generic mutilated head is shown. Otherwise, it is the index for a picture of a person, using that of e-mails.


### Consumables 8/3/x

Nothing special.


### Access Cards 8/4/x

**Access Card** (10 bytes)

    0000  int16     Unknown
    0002  sint32    Access mask
    0006  [4]byte   Unused

All access cards work the same way: When consumed, they add the given ```Access mask``` to the current priviledges and give a message whether and which new access was gained.

**Access mask** (sint32)

    0x00000001      "None" - This priviledge is none and will not be added. Should not be used.
    0x000000FE      Various generic priviledges
    0x00FFFF00      Used for "group" priviledges; Typically used with 8/4/10 (Group access card)
    0x7F000000      Personal priviledges; 0x01: D'Arcy, 0x10: Diego

8/4/11 items (Personal access cards) have a special handling if personal priviledges are set; When identifying the object, the name of the person is appended to the title.

> 8/4/0 (Current access levels) works like any other access card, though it doesn't have a proper icon (only an 'X'). In the game it is used only on level 1 for a few cases, purpose unknown.


### Cyberspace Items 8/5/x

#### Integrity Restorative 8/5/1

**Integrity Restorative** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Restoration amount
    0003  [7]byte   Unused

Per default (```Restoration amount``` = 0), the item restores about 3/4 of integrity.

#### Security Defense Mine 8/5/2

**Security Defense Mine** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Damage amount
    0003  [7]byte   Unused

Per default (```Damage amount``` = 0), the item removes little amount (about one chevron).


#### Security ID Module 8/5/3

**Security ID Module** (10 bytes)

    0000  int16     Unknown
    0002  sint32    Access mask
    0006  [4]byte   Unused

Works like access cards - see above.

#### Cyber Info Nodes 8/5/6, 8/5/8

**Cyber Info Node** (10 bytes)

    0000  [2]byte   Unused
    0002  int16     Text index
    0004  [6]byte   Unused

For 8/5/6 (information nodes), the text index is for text resource ```0x0878```.
For 8/5/8 (data fragments), the text index is for text resource ```0x087A```.


#### Cyberspace Barricades 8/5/9

**Cyberspace Barricade** (10 bytes)

    0000  [2]byte   Unused
    0002  byte      Size. Bits 0-3: X, bits 4-7: Y
    0003  byte      Height. 0 is default, level height units otherwise.
    0004  [2]byte   Unused
    0006  byte      Colour
    0007  [3]byte   Unused

Layout and properties are the same as for force bridges (7/7/7).


### Wear and Tear Items 8/6/x

Nothing special.


### Quest Items 8/7/x

Nothing special.
