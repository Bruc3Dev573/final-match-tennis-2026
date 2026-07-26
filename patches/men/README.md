# Final Match Tennis - Men (2026 ATP roster)

Patch: [`Final_Match_Tennis_ATP_Tour_2026.ips`](Final_Match_Tennis_ATP_Tour_2026.ips)

Replaces the original 1990-era men's roster with the 2026 ATP top players. Names are
updated on **every** screen - player-select grid, the versus face-off, and the in-match
scoreboard / statistics panel.

## Base ROM (apply the patch to this)

| | |
|---|---|
| File | `Final Match Tennis (Japan).pce` |
| Size | 262,144 bytes (256 KB, **no header**) |
| CRC32 | `560D2305` |
| MD5 | `595364eede949021345f1f7735e64355` |
| SHA1 | `e3a97b468d0a6c94effde70bf331dc4b3d90b166` |

This is the standard No-Intro *Final Match Tennis (Japan)* dump. After patching, the
result is **MD5 `cd57ba556706997df661fd2cb153709e` / CRC32 `A059884F`**.

The patch also stamps a small **`ATP` / `2026`** badge on the tennis ball of the title
screen (between "Final Match" and "TENNIS"), so the modded edition is identifiable at a
glance.

> The original game is already in English (menus, "STATISTICS", etc.); only the Japanese
> player names were ever katakana. This patch romanises them into the modern roster.

## Roster

Slots are assigned by **2024-25 ATP ranking order**, not by playing style. Each modern
player therefore **inherits the exact in-game attributes and CPU behaviour** of the
1990-era pro who held that slot - so how they play (serve-and-volley vs. baseline, lefty,
speed, difficulty) is dictated by the original, not the name.

| Slot | In-game | Modern player | Inherited from | Plays like (inherited style) | Tier* |
|:---:|:---|:---|:---|:---|:---:|
| P0 | `SINNER` | Jannik Sinner | Ivan Lendl | Relentless flat baseline power; the toughest CPU seed | ⭐⭐⭐ |
| P1 | `ALCARZ` | Carlos Alcaraz | Boris Becker | Big serve, aggressive attacker | ⭐⭐⭐ |
| P2 | `ZVEREV` | Alexander Zverev | Michael Chang | Fast defensive counter-puncher / retriever | ⭐⭐ |
| P3 | `MEDVED` | Daniil Medvedev | Mats Wilander | Patient, ultra-consistent all-court | ⭐⭐⭐ |
| P4 | `FRITZ` | Taylor Fritz | Mikael Pernfors | Crafty baseliner | ⭐⭐ |
| P5 | `DJOKO` | Novak Djokovic | Shuzo Matsuoka | The Japanese "home hero" slot - the softest opponent in the JP original (ironic for Djokovic) | ⭐ |
| P6 | `RUBLEV` | Andrey Rublev | Guillermo Vilas | Left-handed heavy-topspin grinder | ⭐⭐ |
| P7 | `RUUD` | Casper Ruud | Ken Rosewall | Precision / touch all-courter | ⭐⭐ |
| P8 | `TSITSI` | Stefanos Tsitsipas | Stefan Edberg | Elegant serve-and-volley, one-handed backhand | ⭐⭐⭐ |
| P9 | `HURKAZ` | Hubert Hurkacz | John McEnroe | Serve-and-volley, soft hands at net | ⭐⭐ |
| P10 | `RUNE` | Holger Rune | Andre Agassi | Aggressive early-ball returner | ⭐⭐ |
| P11 | `TIAFOE` | Frances Tiafoe | Jimmy Connors | Flat, aggressive, crowd-pleasing baseliner | ⭐⭐ |
| P12 | `MECIR` | *(kept)* Miloslav Mečíř | - | Smooth all-court "The Cat" | ⭐⭐ |
| P13 | `LECONTE` | *(kept)* Henri Leconte | - | Flashy shot-maker | ⭐⭐ |
| P14 | `BORG` | *(kept)* Björn Borg | - | Ice-cold topspin baseline legend | ⭐⭐⭐ |
| P15 | `LAVER` | *(kept)* Rod Laver | - | All-time great all-courter | ⭐⭐⭐ |

\* **Tier** is an indicative reading of the *original* slot's in-game strength (higher =
tougher CPU / stronger attributes). It reflects the inherited slot, not the real-world
2026 ranking. The four legend slots (P12-P15) are left with their real names.

## Screenshots

| Title | Player select |
|:---:|:---:|
| ![Title](../../docs/screenshots/men-title.png) | ![Player select](../../docs/screenshots/men-player-select.png) |

| Versus | Scoreboard |
|:---:|:---:|
| ![Versus](../../docs/screenshots/men-versus.png) | ![Scoreboard](../../docs/screenshots/men-scoreboard.png) |

## Install

1. Verify your base ROM matches the hashes above.
2. Apply `Final_Match_Tennis_ATP_Tour_2026.ips` with Lunar IPS / Flips / a browser patcher.
3. Run the patched `.pce` in an emulator, on a flash cart, or on **MiSTer**
   (`games/TGFX16/`, headerless - do not add a header).

RetroArch soft-patch: put the `.ips` next to the ROM with the same base name
(`Final Match Tennis (Japan).ips`) and enable automatic soft-patching.
