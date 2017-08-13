## Game variables

This page lists all game variables with their index, their initital value, and their usage in the original game.

> At the moment not all variables have been catalogued. Only the initial values have been determined.

### Boolean variables

| Index | Initial Value | Usage                                                                                        |
|------:|--------------:|----------------------------------------------------------------------------------------------|
|    0  | 0             | Must always be 0, otherwise default conditions may break                                     |
|    1  | 1             | Must always be 1, otherwise some locks may fail                                              |
|    2  | 1             | Unknown                                                                                      |
|    3  | 1             | Unknown                                                                                      |
|    6  | 0             | Shield status (+ System Analyzer)                                                            |
|    8  | 0             | Laser status (+ System Analyzer); 0: charging, 1: destroyed                                  |
|   10  | 0             | Delta grove launch enable (+ System Analyzer)                                                |
|   11  | 0             | Alpha grove launch enable (+ System Analyzer)                                                |
|   12  | 0             | Beta grove launch enable (+ System Analyzer)                                                 |
|   13  | 0             | Relay 428 report status; 0: functioning, 1: faulty; Set by master switch, checked by diag.   |
|   14  | 0             | Relay 428 repair status; 0: faulty, 1: fixed; Set by panel, checked by master switch         |
|   15  | 0             | Beta grove jettisoned (+ System Analyzer)                                                    |
|   16  | 1             | Unknown                                                                                      |
|   18  | 1             | Unknown                                                                                      |
|   20  | 0             | Reactor on destruct, pods enabled; Quakes (+ System Analyzer)                                |
|   21  | 1             | Unknown                                                                                      |
|   22  | 1             | Unknown                                                                                      |
|   23  | 1             | Unknown                                                                                      |
|   24  | 1             | Unknown                                                                                      |
|   25  | 1             | Unknown                                                                                      |
|   26  | 1             | Medical armory lock on doors in level 1                                                      |
|   30  | 0             | Switch to open armory doors on level 1 via cyberspace level 14                               |
|   32  | 1             | Unknown                                                                                      |
|   33  | 1             | Unknown                                                                                      |
|   36  | 1             | Unknown                                                                                      |
|   37  | 1             | Unknown                                                                                      |
|   75  | 1             | Unknown                                                                                      |
|   76  | 1             | Unknown                                                                                      |
|   77  | 1             | Unknown                                                                                      |
|   78  | 1             | Unknown                                                                                      |
|   79  | 1             | Unknown                                                                                      |
|   80  | 1             | Unknown                                                                                      |
|   81  | 1             | Unknown                                                                                      |
|   82  | 1             | Unknown                                                                                      |
|   83  | 1             | Unknown                                                                                      |
|   84  | 1             | Unknown                                                                                      |
|   85  | 1             | Unknown                                                                                      |
|   86  | 1             | Unknown                                                                                      |
|   87  | 1             | Unknown                                                                                      |
|   88  | 1             | Unknown                                                                                      |
|   89  | 1             | Unknown                                                                                      |
|   90  | 1             | Unknown                                                                                      |
|   91  | 1             | Unknown                                                                                      |
|   92  | 1             | Unknown                                                                                      |
|   93  | 1             | Unknown                                                                                      |
|   94  | 1             | Unknown                                                                                      |
|   95  | 1             | Unknown                                                                                      |
|  112  | 1             | Unknown                                                                                      |
|  113  | 1             | Unknown                                                                                      |
|  114  | 1             | Unknown                                                                                      |
|  115  | 1             | Unknown                                                                                      |
|  116  | 1             | Unknown                                                                                      |
|  117  | 1             | Unknown                                                                                      |
|  118  | 1             | Unknown                                                                                      |
|  119  | 1             | Unknown                                                                                      |
|  120  | 1             | Unknown                                                                                      |
|  121  | 1             | Unknown                                                                                      |
|  122  | 1             | Unknown                                                                                      |
|  123  | 1             | Unknown                                                                                      |
|  124  | 1             | Unknown                                                                                      |
|  125  | 1             | Unknown                                                                                      |
|  126  | 1             | Unknown                                                                                      |
|  127  | 1             | Unknown                                                                                      |
|  145  | 0             | "Online" help (tooltips); 0: show, 1: off                                                    |
|  160  | 1             | Unknown                                                                                      |
|  161  | 1             | Unknown                                                                                      |
|  162  | 1             | Unknown                                                                                      |
|  163  | 1             | Unknown                                                                                      |
|  164  | 1             | Unknown                                                                                      |
|  165  | 1             | Unknown                                                                                      |
|  166  | 1             | Unknown                                                                                      |
|  167  | 1             | Unknown                                                                                      |
|  168  | 1             | Unknown                                                                                      |
|  169  | 1             | Unknown                                                                                      |
|  192  | 1             | Unknown                                                                                      |
|  193  | 1             | Unknown                                                                                      |
|  194  | 1             | Unknown                                                                                      |
|  195  | 1             | Unknown                                                                                      |
|  196  | 1             | Unknown                                                                                      |
|  197  | 1             | Unknown                                                                                      |
|  198  | 1             | Unknown                                                                                      |
|  199  | 1             | Unknown                                                                                      |
|  200  | 1             | Unknown                                                                                      |
|  201  | 1             | Unknown                                                                                      |
|  202  | 1             | Unknown                                                                                      |
|  203  | 1             | Unknown                                                                                      |
|  204  | 1             | Unknown                                                                                      |
|  205  | 1             | Unknown                                                                                      |
|  206  | 1             | Unknown                                                                                      |
|  207  | 1             | Unknown                                                                                      |
|  225  | 1             | Unknown                                                                                      |
|  227  | 1             | Unknown                                                                                      |
|  229  | 1             | Unknown                                                                                      |
|  231  | 1             | Unknown                                                                                      |
|  233  | 1             | Unknown                                                                                      |
|  235  | 1             | Unknown                                                                                      |
|  237  | 1             | Unknown                                                                                      |
|  239  | 1             | Unknown                                                                                      |
|  241  | 1             | Unknown                                                                                      |
|  243  | 1             | Unknown                                                                                      |
|  245  | 1             | Unknown                                                                                      |
|  247  | 1             | Unknown                                                                                      |
|  249  | 1             | Unknown                                                                                      |
|  251  | 1             | Unknown                                                                                      |
|  253  | 1             | Unknown                                                                                      |
|  255  | 1             | Unknown                                                                                      |
|  257  | 1             | Unknown                                                                                      |
|  259  | 1             | Unknown                                                                                      |
|  261  | 1             | Unknown                                                                                      |
|  263  | 1             | Unknown                                                                                      |
|  265  | 1             | Unknown                                                                                      |
|  267  | 1             | Unknown                                                                                      |
|  269  | 1             | Unknown                                                                                      |
|  271  | 1             | Unknown                                                                                      |
|  273  | 1             | Unknown                                                                                      |
|  275  | 1             | Unknown                                                                                      |
|  277  | 1             | Unknown                                                                                      |
|  279  | 1             | Unknown                                                                                      |
|  281  | 1             | Unknown                                                                                      |
|  283  | 1             | Unknown                                                                                      |
|  285  | 1             | Unknown                                                                                      |
|  287  | 1             | Unknown                                                                                      |
|  289  | 1             | Unknown                                                                                      |
|  291  | 1             | Unknown                                                                                      |
|  293  | 1             | Unknown                                                                                      |
|  295  | 1             | Unknown                                                                                      |
|  297  | 1             | Unknown                                                                                      |
|  299  | 1             | Unknown                                                                                      |

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
