# Vocab Hub — A1–B2

Gamified vocabulary practice for Phil Young's English School.
Live: https://arschul.github.io/vocab/

## Status
- **A1** — 8 themes × 25 words (200 words) — built
- **A2** — 8 themes × 25 words (200 words) — built, adds a common-mistake tip per word
- **B1** — 8 themes × 25 words (200 words) — built, adds collocations and word family
- **B2** — 8 themes × 25 words (200 words) — built, adds usage notes and tags

800 words total across four levels.

All data is American English (spelling and word choice).

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

Richness scales by level. A2 entries add `m`, a one-line common-mistake tip in English, shown in
Study, Print, and on reveal in Word Climb and Board Mode:

```js
{ w:'money', pos:'noun', d:'coins and notes used to buy things',
  x:'I do not have enough money.', m:'Uncountable: much money, not many moneys.' }
```

B1 entries add `c`, an array of collocations, and `f`, the word family:

```js
{ w:'crime', pos:'noun', d:'an action that breaks the law',
  x:'Reporting a crime takes ten minutes.',
  m:'Commit a crime, not do or make a crime.',
  c:['commit a crime','a serious crime','crime rate'],
  f:'criminal (adj, n)' }
```

Collocations render as chips in Study and Print, and appear on reveal in Word Climb and Board Mode.
Use `'\u2014'` for `f` when there is no useful family; it is skipped at render time.
B2 entries add `u`, a usage/register note, and `t`, an array of tags:

```js
{ w:'revenue', pos:'noun', d:'the total money a business takes in',
  x:'Advertising revenue fell sharply.',
  m:'Usually uncountable. Say REV-en-yoo.',
  c:['annual revenue','generate revenue','revenue stream'], f:'—',
  u:'Standard in reports and business writing rather than conversation.',
  t:['formal','uncountable'] }
```

### Tags and the label review mode
Tags are a controlled set defined by `TAG_INFO` in `index.html`: `academic`, `formal`,
`confusable`, `uncountable`, `irregular`, `linking`. Adding a tag outside that set means adding it
to `TAG_INFO` too, or it will not appear in the index.

Tags drive **Review by label** — a level-scope mode that crosses themes, at `#/<level>/tags`:

- `#/b2/tags` — index of labels with word counts
- `#/b2/tags/<tag>` — every word carrying that label, from all eight themes, with usage notes
- `#/b2/tags/<tag>/drill` — a 15-question drill over that set

The entry point appears on the level's theme grid only when the level has `tags: true` in the
manifest. The mode is review, not progression — it awards no badges, since badges are per theme.

### Adding a level
1. Write `data/<level>-<theme>.js` for each theme, then flip `ready: true` in `data/manifest.js`.
2. Every example must contain its headword verbatim in base form (the gap invariant above).
3. Extend `NOUNY_VERBS` / `VERBY_NOUNS` in `index.html` with any new word that works as both noun
   and verb, or Sentence Gap will offer a wrong answer that actually fits.
4. Re-run the pair audit before shipping.

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
