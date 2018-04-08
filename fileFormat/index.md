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

Resource files are the majority. All files with filename suffix `.res` are resource files. Savegame files have the same format, including the "master" savegame file `archive.dat`. These files contain one or more "resource" entries. Resources are identified and contain one or more data blocks. A resource with more than one data block is called a "compound resource".

The property files are "flat" files containing one or more serialized tables.

### List of Files

The following table shows an overview of the known files, across the different releases.

For resource files, resources may share the same identifier. In this case they represent the alternatives available through configuration. The options are:
* Language. Available are: "English", "French" and "German"
* Cutscene resolution (MOVI format). Available are: "low" (320x150) and "svga" (600x300)

> The "bitmap" based cutscenes have a resolution of 320x200 and have their own identifier.

<table style="width:100%;">
  <tr>
    <th rowspan=2>Content</th>
    <th colspan=3>Languages</th><th colspan=2>Location HD Release</th><th colspan=2>Location CD Release</th>
  </tr>
  <tr>
    <th>ENG</th><th>FRN</th><th>GER</th>
    <th>Demo</th><th>Release</th><th>Demo</th><th>Release</th>
  </tr>
  <tr>
    <th colspan="8">Startup and Cutscenes</th>
  </tr>
  <tr>
    <td>Splash screens</td><td colspan=3>```splash.res```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>CD</td>
  </tr>
  <tr>
    <td>Splash screen palette</td><td colspan=3>```splshpal.res```</td>
    <td>--</td><td>HD</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Palette for low-res bitmap cutscenes</td><td colspan=3>```cutspal.res```</td>
    <td>--</td><td>HD</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Low-Res bitmap intro cutscene</td><td colspan=3>```start1.res```</td>
    <td>--</td><td>HD</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Low-Res MOVI intro cutscene</td><td>```lowintr.res```</td><td>```lofrintr.res```</td><td>```logeintr.res```</td>
    <td>--</td><td>--</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>High-Res MOVI intro cutscene</td><td>```svgaintr.res```</td><td>```svfrintr.res```</td><td>```svgeintr.res```</td>
    <td>--</td><td>--</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Low-Res bitmap end cutscene</td><td colspan=3>```win1.res```</td>
    <td>--</td><td>HD</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Low-Res MOVI end cutscene</td><td colspan="3">```lowend.res```</td>
    <td>--</td><td>--</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>High-Res MOVI end cutscene</td><td colspan="3">```svgaend.res```</td>
    <td>--</td><td>--</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Low-Res bitmap death cutscene</td><td colspan=3>```death.res```</td>
    <td>--</td><td>HD</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Low-Res MOVI death cutscene</td><td colspan="3">```lowdeth.res```</td>
    <td>--</td><td>--</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>High-Res MOVI death cutscene</td><td colspan="3">```svgadeth.res```</td>
    <td>--</td><td>--</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>Main menu screens</td><td colspan=3>```intro.res```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>HD & CD</td>
  </tr>
  <tr>
    <th colspan="8">In-game Resources</th>
  </tr>
  <tr>
    <td>New Game Template</td><td colspan=3>```archive.dat```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>CD</td>
  </tr>
  <tr>
    <td>In-game palette</td><td colspan=3>```gamepal.res```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>CD</td>
  </tr>
  <tr>
    <td>Texts</td><td>```cybstrng.res```</td><td>```frnstrng.res```</td><td>```gerstrng.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>HUD resources</td><td>```mfdart.res```</td><td>```mfdfrn.res```</td><td>```mfdger.res```</td>
    <td>HD 1)</td><td>HD</td><td>HD 1)</td><td>HD</td>
  </tr>
  <tr>
    <td>Trigger audio</td><td>```citbark.res```</td><td>```frnbark.res```</td><td>```gerbark.res```</td>
    <td>--</td><td>--</td><td>CD 1)</td><td>CD</td>
  </tr>
  <tr>
    <td>Log Audio</td><td>```citalog.res```</td><td>```frnalog.res```</td><td>```geralog.res```</td>
    <td>--</td><td>--</td><td>CD 1)</td><td>CD</td>
  </tr>
  <tr>
    <td>Video Mails</td><td colspan=3>```vidmail.res```</td>
    <td>--</td><td>HD</td><td>--</td><td>CD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```objart.res```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>CD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```objart2.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```sideart.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>Level textures</td><td colspan=3>```texture.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```citmat.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>Sound Effects</td><td colspan=3>```digifx.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```gamescr.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```handart.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```obj3d.res```</td>
    <td>HD</td><td>HD</td><td>HD</td><td>HD</td>
  </tr>
  <tr>
    <td>???</td><td colspan=3>```objart3.res```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>HD</td>
  </tr>
  <tr>
    <th colspan="8">Property Files</th>
  </tr>
  <tr>
    <td>Object properties</td><td colspan=3>```objprop.dat```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>HD</td>
  </tr>
  <tr>
    <td>Texture properties</td><td colspan=3>```textprop.dat```</td>
    <td>HD</td><td>HD</td><td>CD</td><td>CD</td>
  </tr>
</table>
1) Only the English files available
