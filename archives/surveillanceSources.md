## Surveillance Sources

The sources of surveillance screens are defined with ```L43```. This chunk contains a list of up to 8 object table index values.
These referred objects provide the location and orientation for the video source of a screen. A screen then refers to one of these entries by using one of the corresponding [special values as 'picture source'](../levelObjects/07_Scenery/levelSceneryEntry.md).

**Surveillance Sources Table** (16 bytes)

    0000  [8]int16  Source object table index

> For a source that is not an actual camera object, it is typical to find 'null' trigger objects to be the reference.
> Unused surveillance sources refer to the unused object at index 0.
