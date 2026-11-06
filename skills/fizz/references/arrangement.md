# Arrangement

A loop becomes a track when sections differ. Fizz gives you `form:` (play order), `from=` (a section built from another), `drop=` (leave layers out), `enter=`/`exit=` (a layer's bars inside a section) and `ramp=` (automation). Copying layers between sections by hand is the slow way; reuse is the fast one.

## Forms by genre

| Genre | Form | Bars |
|---|---|---|
| pop, rock, soul | `intro verse chorus verse chorus bridge chorus` | 4 / 8 / 8 / 8 / 8 / 8 / 8 |
| hip-hop, lo-fi | `intro verse hook verse hook` | 4 / 8 / 8 |
| house, techno | `intro build drop break build drop outro` | 8 / 8 / 16 / 8 / 8 / 16 / 8 |
| trap | `intro verse hook verse hook` (hook = drop) | 4 / 8 / 8 |
| cinematic | `build lift climax resolve` | 8 / 8 / 8 / 4 |
| ambient | `drift bloom drift` | 8 / 8 / 8 |
| a loop | one section, 4 or 8 bars | |

Keep sections to 4, 8 or 16 bars.

## Energy

- **Intros and breakdowns thin out**: drop the kick and bass (`drop=kick,bass`), keep pads and a hint of the hook.
- **Choruses and drops add**: an extra layer (lead, open hat, clap on 2 and 4), more hits, `presence=front` on the hook.
- **Density shows it.** `fizz analyse` prints onsets per bar per section. A chorus should be denser than its verse, an intro sparser. If every section has the same density, the track is a loop wearing labels.
- **Register lifts**: a last chorus can move up a key (`section "last" from=chorus key=B`) or move its lead up an octave.

## Reuse patterns

```fizz
fizz score v1
bpm 92
key A minor
form: intro verse chorus verse chorus outro

phrase hook = 3 . 1 . 3 _ 2 . 1 _ _ . 6, _ _ .

section "verse" bars=8
  chords: i:2 VI:2 III:2 V:2
  kick kit_bd : x... ..x. ..x. ....
  snare kit_sd : .... x... .... x...
  hat kit_hh : x.x. x.x. x.x. x.xx
  bass finger_bass :
    1| x . . . . . x . x . . . . . . .
    2| x . . . x . . . x . . x . . 5, .
  chords epiano : x... .... x... ....

section "chorus" from=verse
  chords: VI:2 VII:2 i:2 V:2
  #kick : x... ..x. x... ..x.
  lead#hook guitar presence=front :
    1| $hook
    2| 3 . . . 1 . . . 3 _ _ _ . . . .
    3| $hook-1
    4| 2 . . . 7, . . . 4, _ _ _ . . . .

section "intro" from=verse bars=4 drop=kick,snare,hat,bass
  chords: i:2 VI:2

section "outro" from=chorus bars=4 drop=hook,kick
  chords: VI VII i:2
```

- `chorus` keeps the verse's layers, swaps the progression, makes the kick busier and adds a lead only it has.
- `intro` and `outro` are written after `chorus` but play where `form:` puts them. Each is 4 bars, so each gets a 4-bar progression.
- `drop=` names layer ids: the lead is `hook` (from `lead#hook`), not `lead`.

A base section can be anywhere in the file; `form:` decides what plays. A section that isn't in `form` and isn't anyone's base is reported as unused.

## Inside a section

- **A layer entering halfway:** `lead … enter=5` in an 8-bar section starts on bar 5. Better than writing four empty bars.
- **A fill every 4 bars:** write the layer's 4 bars as bar lines with the fill on bar 4 (`x.x. x.x. x.x. xrrr`), or a separate `snare#fill … enter=4 exit=4`.
- **Builds:** `ramp=gain,0.2,1` on the snare or hats, `ramp=hpf,200,6000` on the music, over the 4–8 bars before a drop.
- **Risers:** noise on an `fx` layer, `fx#riser pink enter=5 ramp=hpf,300,8000 ramp=gain,0,0.5` held with `x___ ____ ____ ____` then `____ ____ ____ ____`.
- **Filter sweeps:** `ramp=lpf,400,6000` opens a pad or bass over its section; `lfo=lpf,400,2400,8` sweeps it back and forth every 8 bars.

## Edits people ask for

| Request | Change |
|---|---|
| "make the chorus bigger" | add a layer, more hits and `presence=front` in the chorus only |
| "add an intro" | `section "intro" from=verse bars=4 drop=…` and put it first in `form:` |
| "strip it down" | `drop=` the busiest layers, or delete them |
| "longer" | add sections to `form:`, or a bridge with a new progression |
| "a breakdown" | `section "break" from=drop drop=kick,bass,hat` plus a pad |
