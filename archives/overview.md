## Archives
This section covers the archive files. There is one template archive file, called ```archive.dat```, and a number of save-game files, called ```savgamXX.dat```. All of these files are regular resource files.

Any new game is based on the archive.dat file. All these archive files contain the complete game state, including the level layout. In other words: Citadel itself, with all its content, is represented through such a file; Properties and media for the content is in the other resource files.

## Map specific data
Each level has 98 chunks reserved, but only 51 in use. The level-specific chunks are grouped according to the level identifier. For example, on Citadel level R, the chunks start with ID ```4002``` (decimal) up to ```4053```. Level 1 uses ```4102``` to ```4153``` and so forth.

On the following pages, chunks identified as ```LXX``` refer to level-specific chunk ```XX``` for any level. These numbers will always be given in decimal.

## Extra Chunks

### Name of archive, chunk 0x0FA0 (4000)
This chunk contains the name of the archive (title of the save-game). It is a 0x00 terminated string, with a dynamic length of up to 32 bytes (including the termination character).

> The archive.dat file contains garbage after the name in this chunk, up to a complete length of 128 bytes. A save-game file has only the name in this chunk. The name of the template archive is "Starting Game".

### [Game state chunk 0x0FA1 (4001)](gameState.md)
This chunk contains information about the game and the hacker.

## Index

* By topic
  * [Map Information](mapInformation.md)

* By level-specific chunk ID
  * ```L02``` Unknown int32. ```0x0000000B``` for all maps.
  * ```L03``` Unknown int32. ```0x0000001B``` for all maps.
  * ```L04``` Basic level spec.
  * ```L05``` [Map Layout](mapInformation.md)
  * ```L06``` Unknown. 8 bytes per map, L1 has 16 (with info doubled).
  * ```L07``` [Level texture map](mapInformation.md)
  * ```L08``` Master object table
  * ```L09``` Object cross-reference table
  * ```L10``` to ...
  * ```L23```    ... Object class specific tables

  * ```L24``` Unknown. 64 of 46 byte entries.

  * ```L25``` Unknown. 8 * 0x00
  * ```L26``` Unknown. 6 * 0x00
  * ```L27``` Unknown. 0x28 * 0x00
  * ```L28``` Unknown. 0x0B * 0x00
  * ```L29``` Unknown. 6 * 0x00
  * ```L30``` Unknown. 7 * 0x00
  * ```L31``` Unknown. 8 * 0x00
  * ```L32``` Unknown. 0x10 * 0x00
  * ```L33``` Unknown. 0x10 * 0x00
  * ```L34``` Unknown. 0x1D * 0x00
  * ```L35``` Unknown. 0x0D * 0x00
  * ```L36``` Unknown. 0x0A * 0x00
  * ```L37``` Unknown. 0x0C * 0x00
  * ```L38``` Unknown. 0x14 * 0x00

  * ```L39``` Unknown. 0x2E byte length.
  * ```L40``` Unknown int32. ```0x0000000D``` for all maps.
  * ```L41``` Unknown byte. ```0x00``` for all maps.
  * ```L42``` Unknown. 0x1C * 0x00

  * ```L43``` Surveillance cameras

  * ```L44``` Unknown. 0x10 byte length.
  * ```L45``` Unknown. 0x5E byte length.

  * ```L46``` [Map notes](mapInformation.md)
  * ```L47``` [Map notes pointer](mapInformation.md)

  * ```L48``` Unknown. 0x30 byte length.
  * ```L49``` Unknown. 0x1C0 byte length, all zeroes.
  * ```L50``` Unknown int16. ```0x0000``` for all maps. 
  * ```L51``` Unknown. Up to 64 of 15 byte entries.
  * ```L52``` Unknown int16.
  * ```L53``` Unknown. 0x40 byte length, all zeroes.
