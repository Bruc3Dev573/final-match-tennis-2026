# Final Match Tennis Ladies - Women (2026 WTA roster)

Patch: [`Final_Match_Tennis_WTA_Tour_2026.ips`](Final_Match_Tennis_WTA_Tour_2026.ips)

Replaces the original women's roster with the 2026 WTA top players, on every screen
(player-select grid, versus face-off, in-match scoreboard / statistics).

## Base ROM (apply the patch to this)

*Final Match Tennis Ladies* is **not a stand-alone cartridge** - it lives on the
*Human Sports Festival* (Japan) PC Engine **CD**. You extract it into a 256 KB HuCard
image first, then apply this patch. The extracted base must be:

| | |
|---|---|
| File | `Final Match Tennis Ladies.pce` (extracted) |
| Size | 262,144 bytes (256 KB, no header) |
| CRC32 | `5D3E96F1` |
| MD5 | `cf8f2dfcd7ad6696917709abacd105b7` |

After patching, the result is **MD5 `ee7a2f6b337143fba17cc84064690f7d` / CRC32 `38545D96`**.

The patch also stamps a small **`WTA` / `2026`** badge on the tennis ball of the title
screen (between "Final Match" and "TENNIS").

### How to get the base image

From your own dump of *Human Sports Festival (Japan)*:

1. Convert the disc to `.cue`/`.bin` if needed (e.g. `chdman extractcd` for a `.chd`).
2. From **Track 02** (the MODE1 data track), take the cooked ISO bytes at offset
   **`0x1E1000` → `0x221000`** (exactly `0x40000` = 256 KB).
3. Apply the community boot-fixup patch `Final Match Tennis Ladies.pce.ips` (a tiny 2-record
   IPS that fixes the reset vectors so the extracted block boots as a HuCard).
4. You now have `Final Match Tennis Ladies.pce` with the CRC32 above. Apply
   `Final_Match_Tennis_WTA_Tour_2026.ips` on top of it.

## Roster

Assigned by **2024-25 WTA ranking order**. Each modern player inherits the in-game
attributes and CPU behaviour of the original slot she replaces.

| Slot | In-game | Modern player | Notes |
|:---:|:---|:---|:---|
| P0 | `SABALE` | Aryna Sabalenka | World No. 1, US Open '25 |
| P1 | `SWIATE` | Iga Świątek | Wimbledon '25 |
| P2 | `GAUFF` | Coco Gauff | Roland-Garros '25 |
| P3 | `KEYS` | Madison Keys | Australian Open '25 |
| P4 | `OSAKA` | Naomi Osaka | 🇯🇵 the Japanese "home hero" slot - ex-No. 1, 4 Slams |
| P5 | `ANDREE` | Mirra Andreeva | Breakout star |
| P6 | `RYBAKI` | Elena Rybakina | Ex-Wimbledon champion |
| P7 | `PAOLIN` | Jasmine Paolini | 🇮🇹 |
| P8 | `ZHENG` | Zheng Qinwen | Olympic gold '24 |
| P9 | `PEGULA` | Jessica Pegula | |
| P10 | `ANISIM` | Amanda Anisimova | Wimbledon finalist '25 |
| P11 | `KREJCI` | Barbora Krejčíková | 2× Slam champion |
| P12 | `GRAF` | *(legend)* Steffi Graf | |
| P13 | `NAVRAT` | *(legend)* Martina Navrátilová | |
| P14 | `SELES` | *(legend)* Monica Seles | |
| P15 | `EVERT` | *(legend)* Chris Evert | |

The four legend slots (P12-P15) use tennis' all-time greats in place of the game's
original bottom-four roster.

> **In-game power / quality:** as with the men's patch, gameplay is 100% inherited from
> the original slot. The original Japanese women's roster was left un-identified (the
> source names are katakana that were not decoded), so a precise "who-plays-like-whom"
> table isn't provided here - but the relative slot strength (roughly P0 strongest →
> P11 weakest, plus the legend slots) carries over unchanged.

## Screenshots

| Title | Player select |
|:---:|:---:|
| ![Title](../../docs/screenshots/ladies-title.png) | ![Player select](../../docs/screenshots/ladies-player-select.png) |

| Versus | Scoreboard |
|:---:|:---:|
| ![Versus](../../docs/screenshots/ladies-versus.png) | ![Scoreboard](../../docs/screenshots/ladies-scoreboard.png) |

## Install

Same as the men's patch: verify the base hash, apply the `.ips`, load the resulting
`.pce` in an emulator or on **MiSTer** (`games/TGFX16/`, headerless). It's a standard
256 KB HuCard image.
