# Theory for Fizz

Fizz keeps you in key: pitches are scale degrees, chords are roman numerals. What's left is choosing well. Degrees below are numbered from the tonic: in A minor, 1 = A, 3 = C, 5 = E, 7 = G, and `#7` = G#.

## Key and scale by mood

| Mood | Scale | Keys that sit well | Notes |
|---|---|---|---|
| bright, happy, anthemic | major | C, D, F, G, Bb | pop, house, gospel |
| sad, dark, serious | minor | A, D, E, F, G, C | trap, cinematic, ballads |
| cool, jazzy, funky | dorian | D, E, A | minor with a bright 6th: funk, neo-soul, lo-fi |
| tense, Spanish, menacing | phrygian | E, A | the b2 is the colour: drill, metal, flamenco |
| dreamy, floating | lydian | F, C | the #4 floats: film, ambient |
| bluesy, rocky, laid back | mixolydian | G, D, A | major with a flat 7th: rock, funk, Afrobeat |

Tempo is part of mood: 60–80 slow and heavy, 85–100 lo-fi and hip-hop, 100–118 pop and funk, 120–128 house and disco, 130–150 trap (felt half-time), 160–175 drum and bass.

## Progressions that work

Major keys:

| Feel | Chords |
|---|---|
| pop, anthemic | `I:2 V:2 vi:2 IV:2` |
| uplifting | `vi:2 IV:2 I:2 V:2` |
| doo-wop, ballad | `I:2 vi:2 IV:2 V:2` |
| jazzy, lo-fi | `Imaj7:2 vi7:2 ii7:2 V7:2` |
| gospel lift | `IV:2 V:2 iii:2 vi:2` then `ii7 V7 I:2` |
| borrowed sadness | `I:2 IV:2 iv:2 I:2` (the minor iv is borrowed: fine) |
| rock | `I:2 bVII:2 IV:2 I:2` |

Minor keys:

| Feel | Chords |
|---|---|
| epic, cinematic | `i:2 VI:2 III:2 VII:2` |
| tense, cadence home | `i:2 VI:2 iv:2 V:2` |
| dark trap | `i:2 VI:2 iv:2 V:2` or a two-chord `i:4 VI:4` |
| Andalusian | `i VII VI V` |
| neo-soul (dorian) | `i7:2 IV7:2` |
| house (minor) | `i:4 VI:2 VII:2` |

Rules of thumb:

- **Resolve.** End the loop on the chord that leads home: `V` or `V7` before `i`/`I` (in minor use `V`, not `v`), or `IV → I` for a softer, plagal close.
- **Pace.** Most tracks change chord every 2 bars (`:2`). Faster changes (every bar) suit busy, upbeat songs; slower ones (4 bars) suit ambient and club tracks.
- **A song needs 3+ chords.** A one-chord vamp works for a groove, a drone or techno, not for a song.
- **Sevenths** (`maj7`, `7`, `m7` as `ii7`) soften and jazz up; triads are plainer and stronger.
- **Slash chords** walk the bass: `I V/3 vi IV` (C, G over B, Am, F) gives a falling bass line under static harmony.

## Melody

- **Chord tones on strong beats.** On steps 1 and 9 (beats 1 and 3) put a note of that bar's chord: for `i` in minor that's 1, 3 or 5; for `VI` it's 6, 1 or 3; for `VII`, 7, 2 or 4; for `V`, 5, #7 or 2. Passing notes go between. `fizz analyse` counts this for you.
- **Write a motif**: 3–5 notes with a rhythm you can hum. State it, repeat it with one change, answer it, return. `phrase hook = …` then `$hook`, `$hook+2` (up 2 scale steps), `$hook-1`.
- **Mostly steps, few leaps.** Move by neighbouring degrees; after a leap, step back the other way.
- **Leave space.** Rests are part of the melody. A lead that plays every 16th wears out fast.
- **Leading note.** In minor, `#7` just below the tonic pulls up into `1`, best over `V`.
- **Range.** Keep a lead within about an octave and a half. Use `'` and `,` rather than climbing from octave to octave.

## Registers

Bass low (octave 1–2), chords and pads in the middle (3), lead above (4–5). Never stack everything in one octave. A bass of `x` hits plays each chord's root in the bass's octave. A sub bass (`sub_bass`, `sine`) sits at `octave=1`.

## Voicing

`chords` and `pad` layers play the bar's whole chord on `x`. By default they're voice-led (`voicing=led`): each chord takes the inversion nearest the previous one, so the hands barely move. `voicing=open` or `spread` widens the stack (pads, cinematic); `close` is root position (stabs).
