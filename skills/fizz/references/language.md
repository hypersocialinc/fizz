# The Fizz language

A Fizz track is a text file, `.fizz` by convention. This page teaches it from the top down. `spec.md` is the formal reference.

## A whole track

```fizz
fizz score v1
bpm 96
key E dorian
swing 0.15
mix clean
form: verse chorus verse chorus

phrase hook = 1 . 3 . 5 _ 4 . 3 _ _ . 1 _ _ .

section "verse" bars=8
  chords: i7:2 IV7:2
  kick kit_bd : x... ..x. ..x. ....
  snare kit_sd : .... x..g .g.. x...
  hat kit_hh humanize=0.3 : x.x. x.x. x.x. x.xx
  bass finger_bass :
    1| x . . . . . x . x . . . . . 5, .
    2| x . . 3 . . 4 . x . . . 4 . 3 .
  chords clav : .x.. .x.x .x.. .x..

section "chorus" from=verse
  chords: IV7:2 i7:2 v7:2 i7:2
  lead#hook square_lead presence=front :
    1| $hook
    2| 5 . 3 . 1 _ _ . . . . . . . . .
```

Read it top to bottom:

- **Header.** Line 1 is always `fizz score v1`. Comments start with `# ` and may go anywhere after it.
- **Track lines:** `bpm` (40–240), `key` (tonic, then scale: major, minor, dorian, phrygian, lydian or mixolydian), and optionally `swing` (0–0.5), `mix` (the master's character), `tone`/`presence` (defaults for every layer), `seed` (a whole number; fixes the randomness of `humanize` and `chance`) and `form:`.
- **`form:`** is the play order, by section label. Labels may repeat. Without it, sections play in the order written.
- **`phrase name = steps`** names a motif to reuse as `$name`.
- **`section "label" bars=N`** starts a section. Its `chords:` line comes first, then one line per layer.

## Chords

`chords: i:2 VI:2 III VII` gives one chord per bar, cycled to fill the section; `:2` holds a chord for two bars. Roman numerals are in the key: uppercase major, lowercase minor, `°` diminished, `+` augmented, with suffix `7`, `maj7`, `sus2` or `sus4`.

- The numeral must have the quality the key gives that degree: in C major `ii` (not `II`), in A minor `ii°` and `III`.
- In minor-family keys (minor, dorian, phrygian), `V`, `V7` and `vii°` use the raised 7th. That's the dominant that pulls home.
- `bVII`, `bVI` and `iv` in a major key are borrowed chords. They are allowed, with a warning.
- A slash puts a chord tone in the bass: `I/3`, `V/5`, `V7/7`.
- Chord names work too: `Am`, `Fmaj7`, `G7`, `Bdim`, `Csus4`.

## Layer lines

```text
role[#id] instrument [attr=value …] : pattern
```

- **role**: `kick snare hat clap rim` (drums), `bass chords lead pad fx` (pitched, or any unpitched sound on `fx`).
- **#id** names a layer when a section has two of a role (`hat#open`), or so a later section can change it. It defaults to the role.
- **instrument**: an id from `instruments.md`, exactly.
- **attributes**, all optional:
  - `steps=8|12|16|24|32` (default 16)
  - `octave=0–7`: the octave of degree 1 (defaults: bass 2, chords 3, pad 3, lead 4)
  - `presence=back|mid|front`, `tone=dark|warm|neutral|bright`, `space=dry|room|hall`: mix intent (see `mix.md`)
  - `enter=N` / `exit=N`: the first and last bar the layer plays in its section
  - `ramp=param,from,to`: one-way automation over the layer's bars, repeatable (`ramp=hpf,200,6000 ramp=gain,0.2,0.9`)
  - `lfo=param,min,max,bars`: a repeating sweep (`lfo=lpf,400,2400,8`)
  - `humanize=0–1`: looser timing and velocity (0.2–0.4 feels played)
  - `chance=0–1`: each hit plays with this probability
  - `voicing=led|close|open|spread`: how `chords`/`pad` stack a chord (default `led`, the nearest inversion)
  - `glide=seconds`: how long a `~` slide takes (default 0.1)
  - raw fx as `name=value`: `gain lpf hpf lpq room size delay pan crush shape attack release`
- **pattern**, after ` : `, on the same line (one bar, repeated) or as bar lines under it:

```text
  bass finger_bass :
    1| 1 . . . 5, . . . 1 . . . 3 . . .
    2| 6, . . . 1 . . . 5, . . . 7, . . .
```

The bar numbers are optional and checked. Bars cycle to fill the section, so two bars in an 8-bar section play 1 2 1 2 1 2 1 2.

## Steps

| Step | Meaning |
|---|---|
| `x` `X` `g` | hit, accented hit, ghost (quiet) hit |
| `r` | a roll: two quick hits in one step |
| `.` | rest |
| `_` | hold the previous note or hit one more step (ties across bar lines too) |
| `1`–`7` | a scale degree (pitched layers) |
| `1'` `5,` | an octave up, an octave down (`1''` two up) |
| `#4` `b7` | a degree raised or lowered a semitone (a warning, except `#7` before the tonic in minor) |
| `5!` `5?` | accented, ghosted note |
| `~5` | glide into this note from the last one |
| `[x .]` `[1 3]` | one step split in two |
| `<x .>` | `x` the first time the bar plays, `.` the second |
| `$hook` `$hook+2` | a phrase, or a phrase moved up 2 scale steps |

On a pitched layer, `x` plays **the bar's chord**: the whole chord on `chords` and `pad`, its root on `bass`, `lead` and `fx`. A bass line of `x` hits follows any progression without one pitch written.

**Counting.** In a run with no spaces (`x..x`, `1.3.`) each character is one step. A step written with more than one character stands alone with spaces round it. These are all 16 steps:

```text
x... x... x... x...
x.x. x.r. x.x. xrrr
1 . 3 . 5 _ _ . 1' . 5 . 3 . . .
1.3. 5__. 1 . 3 . 5, . . .
```

A bar that miscounts gets an error naming each group and its count. A bar one or two steps short that ends in a rest or hold is padded for you, with a warning.

## Reuse

**`from=`** starts a section from another: its bars, chords and layers. Then:

- a `chords:` line replaces the progression (leave it out to keep it);
- a layer line whose id matches a base layer changes only what it writes: `#kick : x... ..x. x... ..x.` keeps the kick's instrument and attributes and swaps the pattern; `#pad tone=bright` keeps the pattern;
- any other layer line adds a layer;
- `drop=kick,hat` leaves base layers out;
- `bars=`, `key=`, `scale=`, `tone=` and `presence=` on the section line replace the base's.

```text
section "drop" bars=8
  chords: i:2 VI:2 III:2 VII:2
  kick 909_bd : x... x... x... x...
  hat#open 909_oh : ..x. ..x. ..x. ..x.
  bass analog_bass : x.x. ..x. x.x. ..x.

section "break" from=drop drop=kick,bass
  pad warm_pad space=hall : x___ ____ ____ ____

section "lift" from=drop key=B
```

**Phrases** name a motif once (`phrase hook = 1 . 3 5 1' _ 5 _`, steps only) and use it as `$hook` in any pitched bar, or `$hook+3` / `$hook-1` to move it by scale steps. A phrase's steps count toward the bar: `$hook` above is 8 steps, so `$hook $hook+4` is a 16-step bar.

## JSON

The same track as JSON is what players store: `fizz expand track.fizz` prints it, `fizz print track.json` turns JSON back into text. Field names match the text form's words (`bpm`, `sections[].layers[].pattern`, `enter`, `ramp: [{ param, from, to }]`, `fx: { gain, lpf, … }`).
