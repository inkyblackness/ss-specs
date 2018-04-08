## Surveillance Sources

The sources of surveillance screens are defined with ```L43```. This resource contains a list of up to 8 object table index values.
These referred objects provide the location and orientation for the video source of a screen. A screen then refers to one of these entries by using one of the corresponding [special values as 'picture source'](../levelObjects/07_BigStuff/levelBigStuffEntry.md).

**Surveillance Sources Table** (16 bytes)

    0000  [8]int16  Source object table index

> For a source that is not an actual camera object, it is typical to find 'null' trigger objects to be the reference.
> Unused surveillance sources refer to the unused object at index 0.


## Surveillance Screen Deathwatch

Screens may be bound to a destructible object. If this object is destroyed, the corresponding screen shows static. These associations are stored in ```L44```, a resource with a list of up to 8 object table index values.

**Surveillance Screen Deathwatch Table** (16 bytes)

    0000  [8]int16  Deathwatch object table index

> For camera sources, it is typical to refer to the actual camera in this table.
