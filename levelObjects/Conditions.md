## Conditions

Conditions are filters for [actions](Actions.md) and for some [panels](09_Panels/levelPanelEntry.md) that don't use actions.

Conditions can examine different aspects of the state of the game or (current) level.


### Game Variable Conditions

These conditions check the value of a game variable.

**Game Variable Condition** (4 bytes)

    0000  uint16    Variable key
    0002  byte      Value
    0003  byte      Failure message

The ```Failure message``` is a message (or indication) the player shall receive if the condition was not met.
```0x00``` means no indication, ```0xFF``` is used for security warnings (blocked by SHODAN security);
All other values are interpreted as text/audio message index.


**Variable key** (2 bytes)

    0x03FF          Variable index
    0x0C00          Unused
    0x1000          Variable type; 0: boolean variable, 1: int16 variable
    0xE000          Comparison

**Comparison**

    0: Variable must be equal to value
    1: Variable must be less than value
    2: Variable must be less than, or equal to, value
    3: Variable must be greater than value
    4: Variable must be greater than, or equal to, value
    5: Variable must be not equal to value


