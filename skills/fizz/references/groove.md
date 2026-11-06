# Groove and feel

Drum bars are four groups of four, one group per beat: `x... x... x... x...` is four on the floor. Steps 1, 5, 9 and 13 are the beats; 3, 7, 11 and 15 the "and"s; the rest are the 16ths between.

## Patterns by genre

Kick, snare (or clap) and hat, one bar each. Vary the kick or add a fill on the second or fourth bar.

| Genre (bpm) | Kick | Snare / clap | Hat |
|---|---|---|---|
| house (122–126) | `x... x... x... x...` | clap `.... x... .... x...` | open hat on the offbeat `..x. ..x. ..x. ..x.` |
| techno (128–134) | `x... x... x... x...` | rim `.... ..x. .... x...` | `xxxx xxxx xxxx xxxx`, `chance=0.8` |
| disco, nu-disco (115–124) | `x... x... x... x...` | `.... x... .... x...` | `.xx. .xx. .xx. .xx.` |
| hip-hop, boom bap (85–95) | `x... ..x. ..x. ....` | `.... x... .... x...` | `x.x. x.x. x.x. x.x.`, swing 0.15–0.25 |
| lo-fi (70–90) | `x... ...x ..x. ....` | `.... x... .... x...` | `x.x. x.xx x.x. x.x.`, swing 0.33, `humanize=0.3` |
| trap (130–150, half-time) | `x... ...x ..x. ....` | `.... .... x... ....` | `x.x. x.x. x.r. xrrr` (rolls with `r`) |
| drill (140–144) | `x... .... ..x. .x..` | `.... .... x... ..x.` | `x..x ..x. x..x .x..` |
| funk (95–110) | `x... ..x. ..x. .x..` | `.... x..g .g.. x...` (ghosts) | `x.x. x.x. x.x. x.x.` |
| reggaeton (90–100) | `x... x... x... x...` | `...x ..x. ...x ..x.` | `x.x. x.x. x.x. x.x.` |
| drum and bass (170–175) | `x... .... ..x. ....` | `.... x... .... x...` | `x.x. x.x. x.x. x.x.` |
| rock, pop (100–130) | `x... ..x. x... ....` | `.... x... .... x...` | `x.x. x.x. x.x. x.x.` |

Trap hats stay at 16 steps and roll with `r`; don't switch the layer to `steps=32` for a roll.

## Feel

- **Swing** (`swing 0.15` lazy, `0.25` hip-hop, `0.33` triplet shuffle) delays every off-beat 16th. It applies to the whole track; leave it out for straight club music.
- **Ghost notes:** `g` is a quiet hit, `X` an accent. A funk snare of `.... X..g .g.. X..g` breathes; a hat of `xXxX` pumps.
- **Humanize:** `humanize=0.2–0.4` on hand-played parts (keys, bass, hats in lo-fi, soul, jazz) loosens timing and velocity. Keep drum machines tight. Add `seed N` so it plays the same every time.
- **Chance:** `chance=0.8` on a busy hat drops one hit in five at random. Easy movement.
- **Polyrhythm:** a layer with `steps=12` against 16 plays triplets across the bar (`x..x..x..x..`); `steps=24` gives 16th triplets for a fill.
- **Rolls and flams:** `r` doubles a hit inside its step; `[x .]` puts a hit on the first half only.
- **Open and closed hats:** two hat layers, `hat` and `hat#open`, the open one on the offbeat. Give the open hat room before the next closed hit.

## Bass and groove

- Lock the bass to the kick: hit where the kick hits, then add one or two notes of your own.
- A bass of `x` follows the chords; add degrees on weak steps for movement (`x . . . . . x . x . . . . . 5, .`).
- 808 slides: `~5` glides into the note; `glide=0.15` lengthens it.
- Vary the bass every other bar, so no two bars running hit in the same places.
