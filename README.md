# Dr. Mario Tournament Edition — Speed + SALT

A VS mode ROM hack for **Dr. Mario (NES)** that adds real-time competitive stats to the clipboard header. Built for tournament play. Works on real hardware.

**See it in action:** [demo video and write-up on kernelcurry.com](https://kernelcurry.com/blog/dr-mario-tournament-edition-salt/)

## What's Changed

The original clipboard header is replaced with a labeled 5-row live stats display:

```
     Original                    Tournament Edition

   ┌──────────────┐           ┌──────────────────────┐
   │ 1P  ══  2P   │           │   05  ═══════  05    │  Level
   │    LEVEL     │           │        SALT          │  Label
   │  00      05  │           │  000           000   │  SALT Round
   │              │           │                      │
   │  LOW     LOW │           │  15    SP      15    │  Speed
   └──────────────┘           └──────────────────────┘
```

## Reading the Stats

```
         P1 side                    P2 side
        ┌──────┐                   ┌──────┐
        │  05  │ ─── Level ──────  │  05  │
        │ 036  │ ─── SALT Round ─  │ 048  │
        │  16  │ ─── Speed ──────  │  19  │
        └──────┘                   └──────┘
```

| Stat | Range | Description |
|------|-------|-------------|
| **Level** | 00–20 | Selected virus level |
| **SALT Round** | 000–G99 | SALT for **this round**. Resets each round. Values above 999 use an alpha hundreds digit (A00=1000, B00=1100, …, G00=1600). |
| **Speed** | 15–80 | Replaces the original LOW/MED/HI label with a number showing exactly how fast your pills drop. Starts at LOW=15, MED=25, HI=31. Increases by 1 every 10 pills. |

## What is SALT?

**SALT** = **S**tun **A**nd **L**ock **T**ime

When your opponent clears a combo, trash drops onto your board from above — slowly, much slower than the pills you place. While it falls, you can't do anything but watch. SALT measures how far that trash drops through your board. The more rows it falls, the higher your SALT.

- **High SALT** = your opponent's combos are punishing you heavily
- **Low SALT** = you're not receiving much trash

## How to Apply the Patch

Apply `DrMario_Tournament_SALT.ips` to your own legally obtained copy of:

> **Dr. Mario (Japan, USA) (En) (Rev 1).nes**

Use any IPS patching tool.

### Recommended Emulators

- [Mesen 2](https://www.mesen.ca/)
- [FCEUX](https://fceux.com/)
- Any emulator that supports Mapper 1 (MMC1)

## Notes

- VS mode only (2-player) — stats display requires 2P mode
- Title screen shows the DrMC logo
- No changes to gameplay mechanics — pill physics, RNG, speed tables are all original
- Compatible with standard NES controllers and real hardware
- You must provide your own ROM — this patch does not include copyrighted data

## Credits

**ROM hack** by [KernelCurry](https://kernelcurry.com)

**SALT concept** from BSG4000 ([DrMC](https://x.com/DrMChampionship))

**Hardware expertise** from [1UpRetro](https://1upretro.com)

### Special Thanks

- [**brianhuffman/drmario**](https://github.com/brianhuffman/drmario) — 6502 disassembly that made this hack possible
- [**DrMC**](https://x.com/DrMChampionship) — the Dr. Mario Championship community

## License

This project is licensed under the [GNU General Public License v3.0 or later](LICENSE). If you distribute a
modified version, you must release your modified source under the same terms.

The DrMC logo on the title screen is the property of the DrMC community and is used with their permission;
it is not covered by this license.

Dr. Mario is a trademark of Nintendo. No ROM data is distributed here — you must supply your own copy of the
game. This project is not affiliated with or endorsed by Nintendo.

Rebuild instructions are in [`source/README.md`](source/README.md).
