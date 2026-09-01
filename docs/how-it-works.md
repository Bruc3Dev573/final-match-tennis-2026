# How it works - reverse-engineering the roster

A write-up of the techniques used to modernise the rosters of *Final Match Tennis* and
*Final Match Tennis Ladies*, and how the solution was found. No prior documentation of
these games existed; everything below was recovered by disassembling the HuC6280 code and
rendering the tile data.

## 1. The problem: names are graphics, not text

Neither game stores player names as ASCII strings. Searching the men's ROM for `LENDL` -
a name that really is in its original roster - returns nothing, and the Ladies ROM behaves
the same for its original (Japanese) player names. Each name is built from small **8×8 font
tiles**, and every screen that shows a name does so by:

1. taking the player index (0-15),
2. looking up a **per-player tile table** (4 tile IDs per name),
3. writing those tiles into a display buffer with a fixed palette.

So changing a name means editing **two** things in lockstep: the **font tiles** (the
letter shapes) and the **tile tables** (which tiles spell which name).

## 2. The font: two characters per tile

The original names are Japanese (hiragana/katakana), one character per 8×8 tile. To fit
Latin surnames like `SINNER` or `SABALENKA` we re-draw each tile as a **pair of 4-pixel-wide
letters** - two characters per tile. A 6-letter name becomes 3 tiles, which fits the
4-tile-per-name budget everywhere.

Each tile is a 4bpp 8×8 glyph. A letter is defined as 7 rows of a 4-bit column mask; a pair
tile packs the left letter in the high nibble and the right letter in the low nibble of each
row.

The working base for the men's game was **tAz's English translation** (*Final Match Tennis
(J) TEng v1.2*), not the clean Japanese dump - see
[Credits](../patches/men/README.md#credits---this-patch-builds-on-tazs-english-translation).
The Latin letterforms this technique relies on come from that translation; the font in the
untouched Japanese ROM is substantially different (tAz rewrote roughly 46% of the bytes in
these regions). What was drawn from scratch here is `P`, `W` and `Y`, which tAz's font did
not include, plus later tuning - a 4-pixel `N` and `M`, or `J` and `I`, are easy to confuse
and needed distinct shapes.

## 3. Three screens, three code paths

The hardest part was realising a single name is drawn by **three independent routines**,
each with its own table and, crucially, its own **font copy in a different VRAM region**:

| Screen | Draw routine | Name table | Font (VRAM) |
|---|---|---|---|
| Player-select grid | pre-built BAT (tilemap) | static per-cell tiles | main font |
| Versus / "two contenders" | shared name routine | main table | main font (`$2Cxx`) |
| In-match scoreboard + statistics | scoreboard routine | scoreboard table | **separate low-VRAM font** |

Patching only the main font + main table fixes the versus screen but leaves the
**player-select grid garbled** (its BAT still points at old tile arrangements) and the
**scoreboard still showing the old names** (it reads a *different* table and a *different*
font copy that lives lower in VRAM, loaded from its own ROM bank).

This was found the hard way: after the first build, the versus screen was perfect but the
scoreboard was unchanged. Tracing the scoreboard's draw routine revealed a second table and
a second font source; only after patching **both** did the scoreboard update.

## 4. The scoreboard gotchas

The low-VRAM scoreboard font is not just letters - it also holds **UI decorations** in the
same tile range: the `%` sign, the little box-fill and border tiles of the score columns,
and corner pieces. In an early attempt, over-writing the whole tile range replaced those
decorations with stray letter fragments (`0%` became `0CI`, score boxes filled with `MTBOE`
garbage). The fix: render every tile of the font with its ID label, identify exactly which
tiles are letters vs. decorations, and over-write **only the letter tiles**, relocating any
name glyph that would collide with a decoration into a now-unused "dead" tile slot.

The scoreboard font also uses an **inverted pixel convention** from the main font (the glyph
stroke is the *cleared* bits, not the set bits), so the pair-glyph bitmap must be inverted
before it is written there - otherwise names render as black-on-white boxes instead of
white-on-black.

## 5. The player-select grid mapping

The grid is a pre-built BAT (background tilemap) with 16 cells whose on-screen order is
**not** P0...P15. The cell→player mapping was recovered by noticing that each BAT cell's tile
sequence exactly matches one row of the main name table - so matching cells to table rows
gave the layout, and each cell could be rewritten with the correct modern name.

## 6. Verifying without guessing

Every table write was validated by **simulation**: read the patched table, walk the tile IDs
through the patched font, decode each pair-glyph back to letters, and assert the result spells
the intended name - for all 16 players, on all three screens. Font tiles were also rendered to
PNG sheets and read by eye. Only after both checks passed was a build handed off for on-console
testing.

## 7. Final Match Tennis Ladies - same engine, shifted offsets

*Ladies* is a CD title inside *Human Sports Festival*. It is extracted by taking 256 KB from
Track 02 of the disc and applying a tiny boot-fixup patch. It turned out to run the **same
engine** as the men's game (about two-thirds of the ROM is byte-identical), which meant the
whole technique transferred directly - but every table and routine had **moved to a different
address**.

Rather than assume, each structure was re-located from scratch:

- the shared font is at the **same** ROM offset (and is byte-identical - a strong signal the
  extraction was aligned correctly),
- the main name table, scoreboard tables, buffers and the scoreboard font's ROM bank were all
  found by disassembling the equivalent routines and following the exact same patterns as the
  men's game.

The player-select BAT even uses the **identical** cell→player mapping as the men's game
(confirmed by matching BAT rows to table rows), so that part carried over unchanged.

## 8. The title-screen badge (`ATP`/`WTA` `2026`)

The patches also stamp a small tour badge on the tennis ball of the title screen. Unlike the
in-game text, the title screen is a code-built background, so there is no simple string table
to edit. Instead:

- A **savestate** was used to recover the exact title-screen VRAM (BAT + tiles + palette),
  which reconstructs the screen pixel-for-pixel and reveals every tile's ID and where it maps
  back to in the ROM.
- The ball's interior is a single solid-green fill tile repeated many times, but the specific
  cells between "Final Match" and "TENNIS" reference their **own** tile copies (loaded
  uncompressed from a known ROM region). Painting the digit/letter glyphs into just those tiles
  adds the badge **without any code change** and without disturbing the shared fill.
- The title font already contains a full `0-9` + `A-Z` set (the menu text uses it), so the
  glyphs are copied straight from it. The badge is drawn in black on the green ball, centred.
- Curved / italic lettering was tried but is illegible at the 8×8 tile size, so the badge is
  upright: `ATP` (or `WTA`) stacked over `2026`.

## 9. Why gameplay is unchanged

None of this touches the player attribute/AI data, sprite or portrait artwork, or match logic -
only name font tiles, name tables and, in the Ladies patch, the colour entries of two player
slots (see the roster note in `patches/ladies/README.md`). Each modern player is, mechanically,
the original slot they replaced. That is intentional: it keeps the games' balance and feel
exactly as shipped, and makes the diffs tiny (the IPS patches are only a few kilobytes).
