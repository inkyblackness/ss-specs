### Level Software Table (Class 6)

```L16``` is a table describing software - both programs and logs.

**Level Software Entry** (9 bytes)

    0000  [6]byte   Level object prefix
    0006  byte      Program version
    0007  byte      Log chunk offset
    0008  byte      Unknown

#### Logs

For logs, the ```Log chunk offset``` is based on the chunk ID ```0x09B8```. The high nibble is used as the level identifier - into
which category the log should be put. See [Electronic Messages](../../content/ElectronicMessages.md) for details.
