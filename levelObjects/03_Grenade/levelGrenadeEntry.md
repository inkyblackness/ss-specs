## Level Explosive Table (Class 3)

```L13``` is a table describing explosives.

**Level Explosive Entry** (12 bytes)

    0000  [6]byte  Level object prefix
    0006  [6]byte  Explosive data

**Explosive Data** (6 bytes)

    0000  int16  Unknown; Only set for live explosives
    0002  int16  State
    0004  int16  Timer time


**State** (2 bytes)

    0x0000  Inert
    0x0001  Thrown live grenade
    0x0005  Landed live grenade (land mines only, can't be picked up)


The timer settings of explosives are stored in the [game state](gameState.md).

The ```Timer time``` field is set to the same value as in the [level timer](levelTimer.md), though modifying this value doesn't have an effect. The level timer is the authorative entry when to trigger the explosive.


### Contact explosives 3/0/x

Contact explosives start a timer, though its expiry does nothing with the grenade.
Land mines can be catched mid-air and re-thrown, others explode. Land mines enter state ```0x0005``` when touching the ground, after which they can't be picked up safely anymore.

> Although the game state also keeps time settings for these explosives, they can't be changed. Their timer don't have an effect anyway.


### Timed explosives 3/1/0, 3/1/1

These explosives keep the state at ```0x0001```, even when landed. Live explosives can be picked up and re-thrown, until their timer sets them off.


### Object explosion 3/1/2

This item has not been encountered.
