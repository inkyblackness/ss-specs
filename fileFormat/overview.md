## File Overview
### Variants
Not considering any re-releases, System Shock 1 was available in the following variants:
* For DOS
 * Demo: HD and CD
 * Release: HD and CD
* For MAC
 * Release: CD

> Unless otherwise noted, this documentation considers only the DOS CD release.

The main difference between HD and CD releases is the CD releases contain audio files for the voice logs and provide a high resolution option.

### Locations
Data files described in this documentation are available at the following locations:
* CD-ROM (System Shock 1 CD)
 * /cdrom/data
 * /hd/data (will be copied to HD)
* HD (System Shock installation directory)
 * /data

### File Types
Data files come in the following variants:
* [Resource files](ResourceFiles.md), which are structured archives
* [Property files](PropertyFiles.md), containing one or more serialized tables

Resource files are the majority. All files with filename suffix `.res` are resource files. Savegame files have the same format, including the "master" savegame file `archive.dat`. These files contain one or more entries, called "chunks" in this documentation. Chunks are identified and contain one or more data blocks.

The property files are "flat" files containing one or more serialized tables.

### Resource File Alternatives
Chunks may share the same identifier. In this case they represent the alternatives available through configuration. The options are:
* Language. Available are: "English", "French" and "German"
* Resolution. Available are: "low" (320x240) and "svga" (640x480)
