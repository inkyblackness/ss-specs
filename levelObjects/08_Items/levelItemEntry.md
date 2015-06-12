### Level Item Table (Class 8)

```L18``` is a table describing general items

**Level Item Entry** (16 bytes)

    0000  [6]byte   Level object prefix
    0008  [10]byte  Unknown

#### Junk

**Papers** ```8/0/2```

    0000  [2]byte   Unknown
    0002  int16     Paper ID
    0004  [6]byte   Unknown

The ```Paper ID``` refers to a paper text entry and is the offset from chunk ID ```0x003C```.
The referred text entries have multiple blocks and the engine simply concatenates all these strings to one text.
