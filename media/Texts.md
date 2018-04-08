## Texts

All texts are stored in the language specific files ```cybstrng.res```, ```frnstrng.res``` and ```gerstrng.res```.
These files contain 188 resources of type ```0x01```. They are compound with several blocks. Each block contains
one string, which is always 0x00 terminated.

While the English strings may be readable according to standard ASCII, with the French and German options, texts
need to be interpreted according to the DOS [code page 850](http://en.wikipedia.org/wiki/Code_page_850).

> Since texts are always presented through the game fonts, any one-byte codepage can be used - As long as the
> corresponding font bitmaps match for the requested characters (bytes).

### Control Characters

For texts with multiple lines, ```0x0A``` acts as a newline marker.

```0x02``` is a soft-hyphen between syllables. Not all texts contain this character.

### Resource Overview

* 0x0867 Trap messages, one per block
* 0x0868 Wall texts
* 0x0869 Switch/Panel descriptions
* 0x086A Texture descriptions (Note: From block 273 onwards of the 512 blocks, the text is always "bug")
* 0x086B "Can't use X" texts for textures (Note: bug texts are simply "can't be used")
* 0x086C HUD texts/messages
* 0x086D Object short names
* 0x086E Messages of the targeting unit
* 0x086F HUD messages (top)
* 0x0870 Log category titles ("Level 1  Logs"...)
* 0x0871 Action messages
* 0x0872 Intro cutscene subtitles
* 0x0873 Death cutscene subtitles
* 0x0874 Win cutscene subtitles
* 0x0875 System scanner label texts
* 0x0876 Sign/Logo names
* 0x0877 Monitor/Screen texts
* 0x0878 Cyberspace toggle texts
* 0x0879 Access card names
* 0x087A Datalet messages (cyberspace nodes)
* 0x087B Menu texts
* 0x087C Credits
* 0x087D Keyboard help
* 0x087E Various HUD messages
* 0x087F Tooltip texts
* 0x0880 Main menu texts
* 0x0881 Endgame statistics texts
* 0x0882 Inventory description/version texts
* 0x0883 Texts for MFD games

* 0x0024 Object names (readable)

* 0x003C - 0x0046  Paper text messages

#### [Electronic Messages](../content/ElectronicMessages.md)

* 0x0989 - 0x09B6  E-Mails
* 0x09B8 - 0x09BD  Level R logs
* 0x09C8 - 0x09D7  Level 1 Logs
* 0x09D8 - 0x09E4  ...
* 0x09E8 - 0x09F0
* 0x09F8 - 0x09FE
* 0x0A08 - 0x0A13
* 0x0A18 - 0x0A21
* 0x0A28 - 0x0A30
* 0x0A38 - 0x0A3D  Level 8 logs
* 0x0A98 - 0x0AA4  Data fragments
