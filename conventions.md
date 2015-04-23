## Conventions

### 1. Content
Information in these documents MUST be verifiable, or have been verified (e.g. through testing) to be included. Assumptions, contradictions, discussions and the like MUST be handled through the [issue](https://github.com/inkyblackness/ss-specs/issues) system.

In general, changes to the documents SHOULD be introduced by linking commits to corresponding issues. This way, issues can be used as reference material for verification.

### 2. Formatting

Vanilla [Markdown](http://daringfireball.net/projects/markdown/syntax) SHOULD be used for all pages.

The biggest header available is the H2 header (""## Title"). H1 header are reserved for the main title. Each page SHOULD start with an H2 header, which is also the link in the [index](index.md).


### 3. Data
In general, this documentation describes (serialized) data structures. Such data structures SHOULD be documented using code blocks and follow the following format:

    offset  type  description


#### Example

    0000  string  Header text "LG Res File v2\r\n\x1A"
    0011  []byte  not used (0x00)
    007C  int32   file offset to chunk directory

Offset values MUST start at 0 and be given in hexadecimal (or placeholder). They SHOULD have a width of 4 (or more) digits.

Types are field specific.

Description is a short human-readable text describing the field.

#### On Integers

For integer types, they MUST be given in the following format:

    (s|u)?int(8|16|24|32)

's' and 'u' prefixes specify 'signed' and 'unsigned integer' respectively. If no sign-prefix is given, it has not been verified whether this field allows negative values or allows greater positive values.

Implementors MAY treat fields without known sign to be unsigned - this can be the case for length fields for instance. It is unknown whether the original executable would handle them correctly.

Unless otherwise noted, integer values are stored in [little-endian](http://en.wikipedia.org/wiki/Endianness#Little-endian) format.