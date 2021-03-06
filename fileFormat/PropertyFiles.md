## Property Files
A few files don't have an archive format, and instead are flat files consisting directly serialized tables.

### Texture Properties (textprop.dat)
This file contains a single table of texture property entries, after a header.

> The table appears to be truncated. Since the game only uses 273 textures, this may have never been detected.
> The file has 4004 bytes on disk, meaning 4000 bytes are available for the table.
> In a test, the original engine appeared to be fine to read a smaller file with exactly 273 entries (3007 bytes) -
> which had the unknown fields set to zero.

> The original source explains the difference: The texture property system would support up to 400 textures.
> It also **assumed** a property structure size of 10 bytes (hardcoded value), while the actual structure is 11 bytes long.
> In other words, one could feed the engine more than 273 textures, yet no more than 363. More than 363 would simply be ignored and the extra textures would just have their default properties.
> This also explains why the Mac port reads exactly 363 textures, as reading more would exhaust the file.


#### Header
The file starts with a 4-byte header.

**Texture Properties File Header** (4 bytes)

    0000  int32    file version number, value 0x09


The engine rejects a properties file with a different version number.


#### Properties Entry

**Texture Property Entry** (11 bytes)

    0000  byte     family (unused)
    0001  byte     target (unused)
    0002  sint16   resilience (unused); Default: 10 
    0004  sint16   distance modifier; Default: 0
    0006  byte     Climbable. 0x00: not climbable; != 0x00: wall can be climbed (original name: friction)
    0007  byte     friction walk (unused); Always 0x00
    0008  byte     Transparency control (original name: force_dir)
    0009  byte     Animation group
    000A  byte     Animation index - if texture is animated

The two bytes at the beginning (```family``` and ```target```) almost always are each set to the low-byte of the index. A few exceptions have both bytes 0x00. Since they are unused, it is unclear what they mean.

The ```resilience``` is nearly always ```0x0A```; The one existing exception has it 0. Since it is unused, it is unclear what it means.

The ```distance modifier``` always zero. It is used in the renderer to determine which texture resolution to use, based on distance from the camera. A higher modifier causes the smaller resolutions to be chosen at larger distances; A negative value causes an earlier switch.

See [Texture Animation](../archives/textureAnimation.md) for details on animated textures.

**Transparency Control**

    0x00  Regular texture; transparent pixel (palette index 0x00) are drawn black
    0x01  Space texture (pixel data of image is ignored)
    0x02  Texture with space background for transparent pixels


### Object Properties (objprop.dat)
The object properties file contains several tables about properties of [level objects](../levelObjects/index.md). It has the following format:

    | Header | Class 0 Tables   | Class 1 Tables     | ... | Common Table |

#### Header
The file starts with a 4-byte header containing the version number.

**Object Properties File Header** (4 bytes)

    0000  int32   file version number, value 0x2D


The engine rejects a properties file with a different version number.

#### Class Tables
For each object class, one entry exists with a table of class-generic properties and a further table, subclass-specific:

    | Class N Tables                                           |
    | Generic | Subclass 0   | Subclass 1   | ... | Subclass N |

The sizes of the generic and specific tables are dependent on the object class, subclass and type amount.
The generic table contains one entry per object type of this class. The specific tables contain one entry per object type of the subclass.

See [Level Objects](../levelObjects/index.md) for an overview of the amount of types and their entry sizes.


#### Common Table
At the end of the file is the table with common properties. For each object type one entry.

**Common Object Properties** (27 bytes)

    0000  int16    Mass
    0002  [2]byte  Unused
    0004  int16    Default hitpoints
    0006  uint8    Armour
    0007  byte     Render type
    0008  byte     Physics type; 0x00: none (insubstantial), 0x01: regular, 0x02: special
    0009  sint8    Bounciness (only for Physics type 0x01)
    000A  byte     Unused
    000B  byte     Vertical frame offset
    000C  byte     Unknown. These values are set (almost) identical to 000B and have no effect.
    000D  byte     Unknown. Set to 0x30 for all barriers (10/x/x), without noticable effect.
    000E  byte     Vulnerabilities
    000F  byte     Special vulnerabilities
    0010  [2]byte  Unused
    0012  byte     Defence value
    0013  byte     Receive damage Flag; 0x00: Yes, 0x03: No, 0x04: Unknown 0x04
    0014  int16    Flags
    0016  int16    3D model index, based on resource 0x08FC
    0018  byte     Unknown -- set to 0x80 for many 3D objects
    0019  byte     Extra; High-nibble (bits 4-7): Number of extra bitmaps; bits 0-3: Unknown (see below)
    001A  byte     Destruction effect


```Physics type``` governs how collisions shall be handled. ```0x00``` makes the object insubstantial, ignoring forces applied to it. For instance, most scenery stays where it was put. ```0x01``` is for regular objects meant to react "normal" in a physical world. ```0x02``` behaves strange is set for exactly one object (```14/0/6```, "SONIC THE HEDGEHOG", an unused critter). Objects with a physics type of ```0x02``` aren't affected by gravity for instance, but are repelled if hacker runs into them.
The ```bounciness``` acts as repulsion force on the object if it collides with the ground or walls - or on the hacker if hacker runs into it. (Negative values make no difference for the object itself, hacker is propelled forward if running into such an object.) The bounciness depends on the ```mass```; Objects with more mass bounce around less.

The ```vertical frame offset``` is applied to vertically center (animated) sprites on their position. If ```0x00```, the sprites are rendered with their bottom anchored at the object's position. Higher values will move the sprites downwards.

The ```vulnerabilities``` and ```special vulnerabilities``` match the corresponding damage type fields from [Generic Weapon Info](../levelObjects/GenericWeaponInfo.md).

The ```defence value``` is compared to the ```offence value``` of a weapon or projectile. Higher defence values will decrease the damage, higher offence values will increase the damage.
> It is not clear how much is added/removed. First tests indicated it's not the difference between the two values.

The ```receive damage flag``` determines whether the object would be destructible - this still requires the object to be solid to register damage.
> This flag is set to ```0x04``` only for the non-used SHODAN critter ```14/3/5``` - effect unknown.

> ```3D model index``` is also set to some values for render type ```0x02```, effect unknown.

> For the ```extra``` field, the lower nibble (bits 0-3) have unknown properties. Changing it does not have any visible effect.
> It is set to ```0x04``` for all objects capable of displaying something (screens and the like), and ```0x0C``` for projectiles.

**Render Type Enumeration** (1 byte)

    0x01  3D object
    0x02  Sprite
    0x03  Screen
    0x04  Critter
    0x06  Fragments (e.g. the Cyberdog)
    0x07  Not drawn
    0x08  Oriented surface (door, wall decoration)
    0x0B  Special
    0x0C  Force door

**Common Object Flags** (2 byte)

    0x0001  Useful inventory object (green font)
    0x0002  Solid - can't be passed through
    0x0004  Triggerable object - world object is triggered (used) instead of picked up on double-click
    0x0008  Unusable object - triggerable or not, double-click on these objects always gives "Can't use XXX"
    0x0010  Usable inventory object - object is triggered on double-click in inventory list
    0x0020  Blocks 3D rendering if closed (set for opaque doors)
    0x0040  Unknown
    0x0080  Ignore darkness (only if bit at 0x0040 is not set)
    0x0100  Solid if closed (set for barriers and repulsor object)
    0x0200  Flat solid (objects can be placed on it)
    0x0400  Large explosion (only set for various explosion animations)
    0x0800  Destroy on contact
    0x1000  Unknown
    0x2000  Unknown
    0x4000  Unknown
    0x8000  Unknown


**Object Destruction Effect** (1 byte)

This byte is a bitmask field with the following layout:

    0x1F    Animation frame index -- refers to different types of animations
    0x20    Play destruction sound
    0x40    Play destruction sound (not used in main game)
    0x80    Actual explosion (with damage) -- requires Animation frame index to be 0x0F

> Some objects have their "private" destruction sounds and have none of the sound bits set.
> The ```actual explosion```, set for larger electrical appliances, works only if the full value is ```0x8F```.

