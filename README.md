# Core 2000 Kanji Trees

A companion reference tool for the **Japanese Core 2000** Anki deck. Search a kanji, see its
reading tree (on-yomi / kun-yomi / irregular, with the words that use each reading), track your
progress through the deck, and mark kanji as known — synced across phone and computer.

**Live:** https://cheungeric02.github.io/core2000-kanji/

## Features (v1)
- **Search + browse reading trees** — match on kanji, word (kanji or kana), reading, or English meaning.
- **Progress slider** — "I've studied up through position N of 2007"; kanji you haven't reached yet
  are hidden so there are no spoilers. The slider label shows the actual deck word at position N.
- **Known toggle** — mark kanji known; count shown in the stat bar.
- **Cross-device sync** — type the same *sync key* on each device (bon-echo Firebase Realtime DB).
- Light/dark theme, mobile-responsive (built for the Galaxy Z Fold7, folded + unfolded).

## How it's built
Single self-contained `index.html` — all CSS/JS inline, and the deck data embedded as raw CSV in
non-executed `<script type="text/csv">` blocks, parsed in-browser. No build step, no framework.

`index.html` is generated from `template.html` + the CSVs in `data/`:

```bash
awk '/^__RT_CSV__$/{while((getline l<"data/kanji_reading_trees.csv")>0)print l;next}
     /^__WO_CSV__$/{while((getline l<"data/core2000_word_order.csv")>0)print l;next}{print}' \
  template.html > index.html
```

Edit `template.html` (the app) or the CSVs, re-run the awk step, then commit `index.html`.

## Data
- `data/kanji_reading_trees.csv` — primary source: kanji → reading → word, with reading type + deck position.
- `data/core2000_word_order.csv` — the full 2007-word deck order (drives the slider label).
- `data/kanji_component_families.csv`, `data/kanji_meaning_cheatsheet.csv` — bonus datasets (for a later view).
