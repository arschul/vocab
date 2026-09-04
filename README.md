# Vocab Hub — A1–B2

Gamified vocabulary practice for Phil Young's English School.
Live: https://arschul.github.io/vocab/

## Status
- **A1** — 8 themes × 25 words (200 words) — built
- A2 / B1 / B2 — planned

## Structure
- `index.html` — hash-routed shell (`#/level/theme/mode`), all modes and game logic inline
- `data/manifest.js` — level and theme index
- `data/<level>-<theme>.js` — word lists, one file per theme

## Word entry shape (A1)
```js
{ w:'apple', pos:'noun', d:'a round red or green fruit', x:'I eat an apple every morning.' }
```
`d` = simple English definition (written below A1). `x` = example sentence, also used to generate
gap-fill items. No L1 translation anywhere.

Meaning is carried by the definition and the example sentence only. Per-word emoji were removed:
emoji sets have no reliable one-to-one mapping to vocabulary (the closest glyph to *belt* is a
swimsuit), and a wrong picture teaches the wrong word. Theme-level icons remain, since those are
category labels rather than word meanings.

Richness scales by level: A2 adds a common-mistake tip, B1 adds collocations and word family, B2 adds usage notes and tags.

## Modes
Study · Sentence Gap · Memory Pairs · Quick Pick · Word Race · Word Climb (Leitner) · Board Mode (two teams, projector) · Print

- **Sentence Gap** — the example sentence with the target word blanked, four options. Inflected forms
  are matched by `gapSentence()`; all 200 A1 words currently produce a clean gap.
- **Memory Pairs** — word card matched to its meaning card.
- **Word Race** — mixes definition prompts and gap prompts so it doesn't duplicate Quick Pick.

## Progress
Badges only — no points, streaks, or leaderboards. Six badges per theme, stored in `localStorage` under `vocab-badges`. Leitner boxes under `vocab-leitner`.
