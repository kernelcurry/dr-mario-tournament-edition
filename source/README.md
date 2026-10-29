# Source — Dr. Mario Tournament Edition

Everything needed to rebuild the shipped `DrMario_Tournament_SALT.ips` from the public
6502 disassembly of Dr. Mario by Brian Huffman, <https://github.com/brianhuffman/drmario>
(commit `7e23e96`). The disassembly itself is **not** included here.

| File | What it is |
|------|------------|
| `drmario.patch` | Unified diff against the upstream disassembly. Adds the SALT and speed counters, the new clipboard layout, the title-screen nametable/palette, and a bypass of the game's built-in ROM checksum (needed because the title-screen data lives inside the region it checks). Also creates the `title_screen_*.inc` data files the code includes. |
| `title_screen_chr.ips` | IPS patch for the game's 32 KB CHR ROM: writes the DrMC title-screen tiles into CHR banks 3 and 4 and blanks the original checker tiles. |
| `README.md` | This file ^_^ |

## Rebuilding the patch

Requirements: [cc65](https://cc65.github.io/) (`ca65`, `ld65`), `git`, `patch`, `dd`, an IPS tool, and your own copy of
**Dr. Mario (Japan, USA) (En) (Rev 1).nes** (MD5 `a4e02baf4284348c045655b91cb84bde`).

```bash
# 1. Base disassembly at the pinned commit
git clone https://github.com/brianhuffman/drmario.git
cd drmario
git checkout 7e23e96

# 2. Apply the code changes (in place; also creates the three title_screen_*.inc files)
patch -p1 < ../source/drmario.patch

# 3. CHR ROM: extract the original game's tile data, then add the title-screen tiles
dd ibs=16 skip=2049 if="../Dr. Mario (Japan, USA) (En) (Rev 1).nes" of=chrrom.bin
#    ...then apply ../source/title_screen_chr.ips to chrrom.bin with your IPS tool (output must replace chrrom.bin)

# 4. Assemble and link the Rev A build (upstream's own wrapper, macros, audio and linker config)
ca65 -t nes -o drmario_a.o drmario_a.a65
ld65 -C drmario.cfg -o DrMario_Tournament_SALT.nes drmario_a.o

# 5. Make the distributable IPS with your IPS tool:
#    original = "Dr. Mario (Japan, USA) (En) (Rev 1).nes", modified = DrMario_Tournament_SALT.nes -> DrMario_Tournament_SALT.ips
```

Expected results:

| File | MD5 |
|------|-----|
| `DrMario_Tournament_SALT.nes` | `33698a4b12ccb5eb6e50f943231f3761` |

(The IPS file itself is not compared by checksum: different IPS tools encode the same changes differently.)

No ROM data is distributed here. Both patches contain only the modified bytes.

## AI Disclosure

Large language models were used during the development of this code.

## License

GNU General Public License v3.0 or later — see `../LICENSE`. The DrMC logo tiles carried by
`title_screen_chr.ips` (and the title-screen layout data in the `.inc` files) belong to the DrMC
community and are used with their permission; they are not covered by the license. If you build on
this work, replace the logo or ask DrMC.
