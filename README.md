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
{ w:'apple', pos:'noun', e:'🍎', d:'a round red or green fruit', x:'I eat an apple every morning.' }
```
`d` = simple English definition (written below A1). `e` = emoji cue. No L1 translation anywhere.

Richness scales by level: A2 adds a common-mistake tip, B1 adds collocations and word family, B2 adds usage notes and tags.

## Modes
Study · Match Up · Memory Pairs · Quick Pick · Word Race · Word Climb (Leitner) · Board Mode (two teams, projector) · Print

## Progress
Badges only — no points, streaks, or leaderboards. Six badges per theme, stored in `localStorage` under `vocab-badges`. Leitner boxes under `vocab-leitner`.
