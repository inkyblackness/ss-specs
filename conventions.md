## Conventions

### Content
Information in these documents MUST be verifiable, or have been verified (e.g. through testing or cross-reference to original code) to be included. Assumptions, contradictions, discussions and the like MUST be handled through the [issue](https://github.com/inkyblackness/ss-specs/issues) system.

In general, changes to the documents SHOULD be introduced by linking commits to corresponding issues. This way, issues can be used as reference material for verification.

### Language
The human language of this documentation is English. Names of types/fields SHOULD follow that of the source (which is presumably en-us), while prose text SHOULD follow en-uk .

This is primarily to uphold the legacy of this documentation. The original ss-specs was maintained within the context of TSSHP, which came from Britain.

When wording actions, consider differenciating between the character in the game-world ("the hacker"), and the player. ("The hacker has a rotation angle of xyz, so the player sees...").

Sentences referring to individuals SHALL be phrased with inclusive, gender neutral language. The use of [singular they](https://en.wikipedia.org/wiki/Singular_they) is to be used if no other option is available.

> While the hacker is represented with a male body and voice in the original game, a fan-mod could change this. (see [here](https://www.moddb.com/mods/system-shock-female-hacker))
> This documentation states raw engine data, which doesn't care about humans, even if considered to be insects.

More generally, consider online guides about inclusive language, such as [this one from Google](https://developers.google.com/style/inclusive-documentation).

### Formatting

Vanilla [Markdown](http://daringfireball.net/projects/markdown/syntax) SHOULD be used for all pages.

The biggest header available is the H2 header (""## Title"). H1 header are reserved for the main title. Each page SHOULD start with an H2 header, which is also the link in the [index](index.md).


### Data
In general, this documentation describes (serialized) data structures. Such data structures SHOULD be documented using code blocks and follow the following format:

    offset  type  description


#### Example

    0000  int32     some value
    0004  [14]byte  unknown, not used (0x00)
    0012  int16     other value

Offset values MUST start at 0 and be given in hexadecimal (or placeholder). They SHOULD have a width of 4 (or more) digits.

Types are field specific.

Description is a short human-readable text describing the field.

#### Integers

For integer types, they MUST be given in the following format:

    (s|u)?int(8|16|24|32)

's' and 'u' prefixes specify 'signed' and 'unsigned integer' respectively. If no sign-prefix is given, it has not been verified whether this field allows negative values or allows greater positive values.

Implementors MAY treat fields without known sign to be unsigned - this can be the case for length fields for instance. It is unknown whether the original executable would handle them correctly.

Unless otherwise noted, integer values are stored in [little-endian](http://en.wikipedia.org/wiki/Endianness#Little-endian) format.

#### Fixed point numbers

Due hardware limitations and slow perfomance of float point operations, System Shock uses fixed point number represenation of real data type.
Usually engine uses [Q15.16](https://en.wikipedia.org/wiki/Q_(number_format)) format (1 bit for sign, 15 bits for integer part, 16 bits for fractional):

    0x 0000 0000
       |    |
       |    fractional part
       integer part with sign (first bit, 0 means positive, 1 - negative)

This format is refered as `fix` (int32). For example, (2 * Pi) number is `0x0006487f` (6 + 18559/65536 = 6.28319)

Internally engine supports Q23.8 numbers (`fix24`) or even Q2.28 (for some 2D- and 3D-computations).

