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

- **Sentence Gap** — the example sentence with the target word blanked, four options, no meaning
  hint. The three wrong options are drawn level-wide and are grammatically impossible in the slot
  (see below), so the sentence alone identifies the answer.

### How each game stays distinct
A gap like "Please give me some _____." accepts water, juice or cake, and some definitions within a
theme are near-twins (walk/run, grandmother/grandfather). The three guessing games solve that
differently, which is what keeps them from collapsing into each other:

- **Sentence Gap** — no meaning hint. Wrong options are made *impossible* instead: they are picked
  from across the level with a word class that cannot fill the slot ("Please give me some
  water / eat / today / yellow"). Tests reading the sentence.
- **Quick Pick** — definition is the prompt, gapped sentence is a supporting hint, distractors come
  from the same theme. Tests meaning recall.
- **Word Race** — timed, alternates which cue leads, both always shown.

`impossibleFor()` enforces the distractor rule. A plain part-of-speech test is not enough, because
words like *dress*, *watch*, *cook* and *water* work as both noun and verb — "My dress is broken"
would read fine. `NOUNY_VERBS` and `VERBY_NOUNS` exclude those from the opposite slot. All 13,287
candidate target/distractor pairs across A1 were checked: no violations, no same-word collisions.

### Gap invariant
`gapSentence()` matches the **base form only** — no inflection guessing. Every example sentence must
contain its headword verbatim, so the blank always accepts the word exactly as shown on the option
button. Adding a sentence like "He eats two eggs" would break this: the option reads *egg* but the
blank needs *eggs*. All 200 A1 entries satisfy the invariant and round-trip cleanly.
- **Memory Pairs** — word card matched to its meaning card.
- **Word Race** — mixes definition prompts and gap prompts so it doesn't duplicate Quick Pick.

## Progress
Badges only — no points, streaks, or leaderboards. Six badges per theme, stored in `localStorage` under `vocab-badges`. Leitner boxes under `vocab-leitner`.
