## Electronic Messages

This chapter documents the electronic messages that the hacker receives either through triggers or collecting them.
This means e-mails (including video mails), logs and data fragments - everything the "Multimedia data reader" can show.

### Text Format

The [text chunks](../media/Texts.md#electronic-messages) for the messages contain both the content, as well as the
meta information, encoded in text form. Any such message chunk has the following string blocks:

    1 line   Meta info
    1 line   Title
    1 line   Sender
    1 line   Subject
    N lines  Verbose text
    1 line   Empty
    M lines  Terse text
    1 line   Empty

The ```Meta info``` line describes what to show additionally. This text has the following format:

    meta-info := [event " "] [color " "] [left-mfd] ["," [" "] right-mfd]
    event     := ("i" hex2) | "t"
    color     := "c" hex2
    left-mfd  := dec
    right-mfd := dec

    Examples:
    "13, 18"
    "i17 8"
    "t cD1 23,23"
    "cD1 23,6"


For most of the messages, at least ```left-mfd``` is specified. ```left-mfd``` and ```right-mfd``` are decimal offset
numbers that refer to the image to show in the respective MFD. The offset is based on chunk ```0x0028```.
If ```left-mfd``` is greater or equal 256, then the message refers to a video mail. Video mails don't have an MFD image.

If specified, the ```color``` field identifies a hexadecimal colour palette index. This colour is used to draw the sender and subject lines.

The ```event``` field chains messages together. "i" events identify the message that must immediately follow the current one;
The hexadecimal number is the offset for the next message (based on chunk ```0x0989```). Such interrupt messages then have "t" as event.

> The vanilla resource files contain a few messages with errors in them:
> There are some "stub email" messages with empty header lines, the text "stub email" twice and only one terminating empty line.
> Furthermore, there is one message in ```gerstrng.res``` which misses the terminating empty line in the terse text.

### Text Content

Within the texts, the substring ```$N``` is the placeholder for the name of the hacker.

### Audio

Corresponding audio chunks are based on ID ```0x0AB5```. If an electronic message with ID X has audio, the audio will have chunk ID ```0x0AB5``` + X. (Also: chunk ID of text chunk + 300)

These audio chunks are [MOVI Chunks](../media/moviChunks.md) with only the audio component. The audio recordings are stored in the files
```citalog.res``` (English), ```geralog.res``` (German) and ```frnalog.res``` (French).
