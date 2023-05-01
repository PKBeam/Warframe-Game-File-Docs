# Warframe Game Files Documentation

Warframe's game files are located in the directory `...SteamLibrary/steamapps/common/Warframe/Cache.Windows/`

Each archive comes with two files: a `.cache` file containing the data and a `.toc` that provides a table of contents for the former - letting us know where to find all the individual files in the cache.

# .toc files

The `.toc` file begins with the 8-byte magic number `4E C6 67 18 14 00 00 00`.

It is then followed by arbitrarily many blocks (entries) of the following format.

| Size | Description |
|---|---|
| 28 bytes | all set to `0xFF` |
| 4 bytes  | folder depth; `0` for the top level folder, `1` for its containing folders, etc. |
| 64 bytes | folder name (zero-terminated string) |

If the folder has any files directly inside it, they will appear immediately after the folder block in the following format. 

| Size | Description |
|---|---|
| 8 bytes | offset; location of this file in the `.cache` file |
| 4 bytes | ??? |
| 4 bytes | ??? (seems to be a few discrete values ending with `D9 01`)|
| 4 bytes | compressed size in `.cache` |
| 4 bytes | uncompressed size |
| 4 bytes | file depth; i.e. one more than its containing folder |
| 64 bytes | file name (zero-terminated string) |

# .cache files

The `.cache` files contain all the actual game data.

The files are compressed with a mixture of Oodle and custom LZ algorithms.

The Oodle files can be decompressed with the `oo2core_X_win64.dll` found in `...SteamLibrary/steamapps/common/Warframe\Tools\Oodle\x64\final\`.  
See game file extractors (you can find them on GitHub) for the LZ algorithm.
