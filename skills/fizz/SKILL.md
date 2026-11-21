---
name: fizz
description: Write and edit music in Fizz, the plain-text music language of Fizz FM (fizz.fm). Use when asked to make a beat, compose a song or track, write a groove, loop or soundtrack, or change an existing Fizz track ("make it darker", "add a bass", "change the key", "make the chorus bigger"), or when working with .fizz files. Covers the language, theory, arrangement, groove and mix rules, a cookbook of 24 producer idioms, 9 worked tracks, and the fizz CLI that checks and analyses a track.
---

# Fizz

Fizz is a small text language for music. A track is one file: tempo and key once at the top, then its **sections** (its parts: chords, and one line per instrument saying what it plays on a 16-step grid), then the song is the `arrangement:` line, the order the sections play in. Pitches are **scale degrees**, so every note is in key by construction. The Fizz player (fizz.fm, the Fizz FM app, the `<fizz-player>` web component) plays it and mixes it; on Fizz FM each section becomes a pad and the arrangement its song.

```fizz
fizz score v1
bpm 84
key A minor
mix lofi-tape
arrangement: intro verse chorus verse chorus

section "verse" bars=4
  chords: i:2 VI VII
  kick kit_bd : x... ..x. x... ....
  snare kit_sd : .... x... .... x...
  hat kit_hh : x.x. x.x. x.x. x.xx
  bass finger_bass :
    1| x . . . . . x . x . . . . . 5, .
    2| x . . . x . . . x . . x . . . .
  chords epiano : x... .... ..x. ....

section "chorus" from=verse
  lead epiano :
    1| 5 . 3 . 1 _ _ . 3 . 5 . 6 _ _ .
    2| 5 . 3 . 1 _ _ . 1 _ _ . . . . .
    3| 6 . 1' . 3' _ _ . 1' . 6 . 5 _ _ .
    4| 7 . 2' . 4' _ _ . 2' _ _ . . . . .

section "intro" from=verse drop=kick,snare,bass
```

`chorus` is the verse plus a lead (`from=verse`), `intro` is the verse without its drums and bass, and `arrangement:` plays them as a song, repeating the verse and chorus. Each part is written once.

## Defaults

Write only what differs from them.

- **No `arrangement:` line** plays the sections once each, in the order written. Write one only when the order differs or a section repeats.
- **Tempo, key, `swing`, `mix`** are written once, at the top, for the whole track. A section changes key only with `key=` on its own line.
- **`from=`** copies a section's bars, chords and layers; write only what changes (new chords, an added layer, `drop=`, a `#id` override).
- Every layer has a sensible octave, gain and mix for its role; add attributes only to change something.

## Workflow

1. **Brief.** Pin down genre, mood, tempo and length. If the request is vague ("something chill"), pick and say what you picked.
2. **Loop or song?** A loop, a beat or a groove to play over is **one section** (4 or 8 bars). Anything else is a **song**, the default: an intro, two contrasting sections (verse and chorus, verse and hook, build and drop), an ending.
3. **Plan** in a few lines before writing: bpm, key and scale, `mix`, the sections and how each differs (chords, which layers play, density), the arrangement, and the lead's motif. `references/arrangement.md` has the forms per genre; `theory.md` and `groove.md` the defaults.
4. **Write** the `.fizz` file. Start from the closest track in `examples/` or idiom in `cookbook.md` rather than from nothing. Write the main section in full, build the others `from=` it, and write the `arrangement:` line last.
5. **Check:** `fizz check --strict track.fizz`. It says what you wrote and what plays (`ok. 3 sections (intro 4, verse 8, chorus 8), arrangement of 5, 36 bars, 1:34`) and checks every section on its own, including one the arrangement never plays. Every error and warning names the line and says how to fix it. Fix and check again until it says `ok` with no warnings. Musical warnings (`W_OFF_CHORD_STRONG`, `W_REPEATED_RHYTHM`, `W_DRUM_LOOP`…) say what will sound wrong; leave one only if you meant it (a blue note), and then check without `--strict`. `W_NORMALISED` means the checker repaired something for you; a `note:` points at a line that only restates a default.
6. **Analyse:** `fizz analyse track.fizz`. Without ears, this is how you hear it: the progression per section, which layers play where, density per section, the lead's and bass's chord tones on strong beats, and notes (the same warnings as `fizz check`, with their lines) on what usually sounds wrong. Fix what it flags unless you meant it.
7. **Deliver** the file. To play it elsewhere: `fizz export --strudel track.fizz` prints Strudel code for strudel.cc.

Editing a track: change only what was asked and keep everything else exactly as it was. "Darker" is `tone dark`; "faster" is `bpm`; "add a bass" adds one layer; "in E minor" changes the `key` line only (degrees move with it); "make the chorus bigger" changes the chorus section only; "add an intro" adds a section and puts it first in `arrangement:`.

## The fizz CLI

| Command | What it does |
|---|---|
| `fizz check <file> [--strict]` | validate the written sections (each on its own) and the arrangement; errors and warnings as `file:line: error CODE path: message`; `--strict` fails on any warning |
| `fizz sections <file> [--json]` | one line per written section (label, bars, `from`), then the arrangement |
| `fizz analyse <file> [--json]` | each played section's chords, lanes and density, and musical notes |
| `fizz expand <file>` | v1 source to the flat JSON a player stores |
| `fizz print <file> [--flat]` | the track as one file (sections, `from=`, arrangement); `--flat` writes every played section out |
| `fizz export --strudel <file>` | Strudel code |
| `fizz import <file.js>` | Strudel code to a Fizz track, with a report of what it couldn't read |
| `fizz render <file\|slug> [--out dir]` | in the Fizz FM repo only (`pnpm fizz render`): the track offline to 24-bit WAV, never aloud: the mix plus `bass`, `drums` and `other` stems, every file the same length from time zero, so they line up in a DAW; prints bpm, the sections in play order with their bars and start times. A slug renders a saved fizz.fm track; `--no-stems` writes the mix alone. A sound missing from the bank fails it (exit 2, nothing written) unless `--allow-missing`, and a mix far under both level targets fails unless `--allow-quiet` |

Use `fizz` if it is on your PATH (`fizz --version`). Otherwise run it as `npx @hypersocial/fizz check track.fizz`, or install it with `npm i -g @hypersocial/fizz`. Inside the Fizz FM repo it is `pnpm fizz check track.fizz`. With no CLI at all, check by hand: count every bar's steps, and keep instrument ids to `references/instruments.md`.

An assistant with connectors (Claude, ChatGPT) can use Fizz FM's MCP server instead, at `https://fizz.fm/api/mcp`: `check_fizz` and `analyse_fizz` are the CLI's check and analyse, `save_track` saves a track (each section a pad, the arrangement its song) and returns a link that plays it and an edit key, `update_track` saves a change to that track as its next version at the same link (its slug and edit key; save a new track only for a different piece of music), `list_versions` and `get_track` (with a `version`) read its history, `get_track` returns a saved track's file, and `compose_track` has Fizz FM's composer write one.

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
