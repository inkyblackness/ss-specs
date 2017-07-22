## Game variables

This page lists all game variables with their index, their initital value, and their usage in the original game.

> At the moment not all variables have been catalogued.

### Boolean variables

| Index | Initial Value | Usage                                                                                        |
|------:|--------------:|----------------------------------------------------------------------------------------------|
|    6  | 0             | Shield status (+ System Analyzer)                                                            |
|    8  | 0             | Laser status (+ System Analyzer); 0: charging, 1: destroyed                                  |
|   10  | 0             | Delta grove launch enable (+ System Analyzer)                                                |
|   11  | 0             | Alpha grove launch enable (+ System Analyzer)                                                |
|   12  | 0             | Beta grove launch enable (+ System Analyzer)                                                 |
|   15  | 0             | Beta grove jettisoned (+ System Analyzer)                                                    |
|   20  | 0             | Reactor on destruct, pods enabled; Quakes (+ System Analyzer)                                |
|  145  | 0             | "Online" help (tooltips); 0: show, 1: off                                                    |


### Integer variables

| Index | Initial Value | Usage                                                   |
|------:|--------------:|---------------------------------------------------------|
|   3   | 2             | Unknown                                                 |
|  12   | 3             | Unknown                                                 |
|  13   | Mission value | Unknown                                                 |
|  14   | Cyber value   | Unknown                                                 |
|  15   | Combat value  | Unknown                                                 |
|  16   | L00 security  | Security value of level                                 |
|  17   | L01 security  | Security value of level                                 |
|  18   | L02 security  | Security value of level                                 |
|  19   | L03 security  | Security value of level                                 |
|  20   | L04 security  | Security value of level                                 |
|  21   | L05 security  | Security value of level                                 |
|  22   | L06 security  | Security value of level                                 |
|  23   | L07 security  | Security value of level                                 |
|  24   | L08 security  | Security value of level                                 |
|  25   | L09 security  | Security value of level                                 |
|  26   | L10 security  | Security value of level                                 |
|  27   | L11 security  | Security value of level                                 |
|  28   | L12 security  | Security value of level                                 |
|  29   | L13 security  | Security value of level                                 |
|  30   | Puzzle value  | Unknown                                                 |
|  31   | random        | Reactor code 1 (3-digit BCD)                            |
|  32   | random        | Reactor code 2 (3-digit BCD)                            |

> The two random codes are stored in [Binary-Coded-Decimal](https://en.wikipedia.org/wiki/Binary-coded_decimal).
