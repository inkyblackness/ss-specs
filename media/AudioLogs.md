## Audio Logs

### Audio

Audio logs are [MOVI Chunks](moviChunks.md) with only the audio component. The audio recordings are stored in the files
```citalog.res``` (English), ```geralog.res``` (German) and ```frnalog.res``` (French).

> The chunk IDs in these files range from 0x0AB5 to 0x0B69, though not all IDs are used.

The ID of the audio is bound to the ID of the text for a log. A text with ID N will have the corresponding audio with
ID+300.
