---
name: fizz
description: Write and edit music in Fizz, the plain-text music language of Fizz FM (fizz.fm). Use when asked to make a beat, compose a song or track, write a groove, loop or soundtrack, or change an existing Fizz track ("make it darker", "add a bass", "change the key", "make the chorus bigger"), or when working with .fizz files. Covers the language, theory, arrangement, groove and mix rules, a cookbook of 21 producer idioms, 8 worked tracks, and the fizz CLI that checks and analyses a track.
---

# Fizz

Fizz is a small text language for music. A track is a few lines: tempo, key, chords, and one line per instrument saying what it plays on a 16-step grid. Pitches are **scale degrees**, so every note is in key by construction. The Fizz player (fizz.fm, the Fizz FM app, the `<fizz-player>` web component) plays it and mixes it.

```fizz
fizz score v1
bpm 84
key A minor
mix lofi-tape

section "loop" bars=4
  chords: i:2 VI VII
  kick kit_bd : x... ..x. x... ....
  snare kit_sd : .... x... .... x...
  hat kit_hh : x.x. x.x. x.x. x.xx
  bass finger_bass :
    1| x . . . . . x . x . . . . . 5, .
    2| x . . . x . . . x . . x . . . .
  lead epiano :
    1| 5 . 3 . 1 _ _ . 3 . 5 . 6 _ _ .
    2| 5 . 3 . 1 _ _ . 1 _ _ . . . . .
    3| 6 . 1' . 3' _ _ . 1' . 6 . 5 _ _ .
    4| 7 . 2' . 4' _ _ . 2' _ _ . . . . .
```

## Workflow

1. **Brief.** Pin down genre, mood, tempo and length. If the request is vague ("something chill"), pick and say what you picked.
2. **Plan** in a few lines before writing: bpm, key and scale, `mix`, the form (`intro verse chorus …`), each section's progression, which layers play where, and the lead's motif. `references/theory.md`, `arrangement.md` and `groove.md` have the defaults per genre.
3. **Write** the `.fizz` file. Start from the closest track in `examples/` or idiom in `cookbook.md` rather than from nothing.
4. **Check:** `fizz check --strict track.fizz`. Every error and warning names the line and says how to fix it. Fix and check again until it says `ok` with no warnings. Musical warnings (`W_OFF_CHORD_STRONG`, `W_REPEATED_RHYTHM`…) say what will sound wrong; leave one only if you meant it (a blue note), and then check without `--strict`. `W_NORMALISED` means the checker repaired something for you.
5. **Analyse:** `fizz analyse track.fizz`. Without ears, this is how you hear it: the progression per section, which layers play where, density per section, the lead's and bass's chord tones on strong beats, and notes (the same warnings as `fizz check`, with their lines) on what usually sounds wrong. Fix what it flags unless you meant it.
6. **Deliver** the file. To play it elsewhere: `fizz export --strudel track.fizz` prints Strudel code for strudel.cc.

Editing a track: change only what was asked and keep everything else exactly as it was. "Darker" is `tone dark`; "faster" is `bpm`; "add a bass" adds one layer; "in E minor" changes the `key` line only (degrees move with it).

## The fizz CLI

| Command | What it does |
|---|---|
| `fizz check <file> [--strict]` | validate; errors and warnings as `file:line: error CODE path: message`; `--strict` fails on any warning |
| `fizz analyse <file> [--json]` | sections, chords, lanes, density and musical notes |
| `fizz expand <file>` | v1 source to the flat JSON a player stores |
| `fizz print <file.json>` | JSON to the text form |
| `fizz export --strudel <file>` | Strudel code |
| `fizz import <file.js>` | Strudel code to a Fizz track, with a report of what it couldn't read |

Use `fizz` if it is on your PATH (`fizz --version`). Otherwise run it as `npx @hypersocial/fizz check track.fizz`, or install it with `npm i -g @hypersocial/fizz`. Inside the Fizz FM repo it is `pnpm fizz check track.fizz`. With no CLI at all, check by hand: count every bar's steps, and keep instrument ids to `references/instruments.md`.

An assistant with connectors (Claude, ChatGPT) can use Fizz FM's MCP server instead, at `https://fizz.fm/api/mcp`: `check_fizz` and `analyse_fizz` are the CLI's check and analyse, `save_track` saves a track and returns a link that plays it, and `compose_track` has Fizz FM's composer write one.

## The rules that matter most

These cause nearly every failed first try.

1. **Count the steps.** Every bar has exactly 16 steps (or the layer's `steps=`). Write drum bars as four groups of four: `x... ..x. x... ....`.
2. **One step, one character.** In a run without spaces each character is a step. Anything longer (`5,` `1'` `#7` `5!` `~5` `[xx]` `<x .>` `$hook`) stands alone with spaces round it: `1 . 3 . 5, . . .`. A drum roll is `r`, not `[xx]` glued into a run.
3. **`_` holds a note**, so it only follows a note or hit, never a `.`.
4. **Header first.** Line 1 is `fizz score v1`; comments (`# …`) come after it.
5. **Instrument ids exactly** as in `references/instruments.md`; drums play their own role (`909_bd` is a kick).
6. **Chords in the key's quality:** in A minor the ii chord is `ii°`, not `ii`; `V`/`V7` are fine in minor. A chord from outside the key is a warning, and fine if you mean it.
7. **A melody that repeats over changing chords** clashes on the beat: write a line per bar on that bar's chord tones, or put it on a `chords` layer, whose `x` plays the current chord. `fizz check` flags it as `W_OFF_CHORD_STRONG`.
8. **Music, not just valid syntax:** a real progression (3+ chords for a song, resolving V → i or IV → I), lead and bass on chord tones on beats 1 and 3, a lead built on a repeated motif, a bass and lead rhythm that changes from bar to bar, sections that differ in density. See `references/rules.md`.

## Files

| File | Read it when |
|---|---|
| `references/language.md` | always, first time: the text form, taught by example |
| `references/rules.md` | always: the musical rules the in-app composer follows |
| `references/instruments.md` | choosing sounds; the full catalogue, mix intent and fx ranges |
| `references/theory.md` | choosing a key, progression or melody |
| `references/arrangement.md` | a track longer than a loop: form, builds, drops, reuse |
| `references/groove.md` | drums and feel: patterns per genre, swing, ghost notes, humanize |
| `references/mix.md` | the mix: intent words instead of raw numbers |
| `references/spec.md` | an exact rule or an error code |
| `cookbook.md` | a specific technique: swing, ghost notes, fills, slides, risers, modulation… |
| `examples/*.fizz` | a starting point in a genre: lo-fi, trap, funk, house, cinematic, soul, ambient, chiptune |
