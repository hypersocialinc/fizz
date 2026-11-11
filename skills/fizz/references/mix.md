# Mixing

The app mixes and masters every track: each role has its own channel strip (high-pass, EQ, compression, level), layers share one reverb and one delay per section, the kick ducks the bass and pads, and every track masters to the same loudness. You describe intent; the engine sets the numbers.

- `mix` (top level) picks the master's character: `clean`, `lofi-tape` (worn tape, hiss, high cut), `club` (tight low end, pumping sidechain, bright), `cinematic` (wide, long reverb), `bedroom-pop` (soft, airy), `trap` (808 forward, crisp hats). Set it to match the brief.
- Per layer, only when it differs from the role's default:
  - `presence`: `back` (further away, quieter), `mid` (default), `front` (forward, the focus).
  - `tone`: `dark`, `warm`, `neutral` (default), `bright`.
  - `space`: `dry`, `room`, `hall` (reverb send). Omit for the role's default; bass and kick stay dry.
- `tone` and `presence` also go at the top of the score as a default for every layer (tone skips drums). "Darker" = top-level `tone: dark`; "brighter" = top-level `tone: bright`; "more space" = `space: hall` on pads and leads; "punchier" = `mix: club` or kick and bass `presence: front`. Neither changes the rhythm.
- Raw `fx` numbers are fine-tuning on top of the mix, for a sound the intent can't say: a filter sweep (`lfo` on lpf), crush, a deliberate pan. Leave gain, hpf and room to the engine unless the brief needs a specific value. Never room, delay, pan or width on bass or kick.

## Movement

What makes keys and bass sound played rather than "MIDI-like". Any instrument takes them; none of them adds level.

| want | write | notes |
|---|---|---|
| a plucked or "wow" bass, a stab that snaps | `lpf=220 lpenv=4 lpdecay=0.18 lpsustain=0.05` | filter envelope: each note opens the low-pass `lpenv` octaves above `lpf` and closes it over `lpdecay`. Needs an `lpf`. `lpsustain` 0 is a pluck, 0.5 stays half open; `lpattack` slows the opening (a swell). Give `lpdecay` whenever you give a stage: without it the filter stays open after the attack, as in Strudel. A patch with its own filter envelope (analog_bass, pluck, clav) takes yours instead. |
| electric piano or pad that moves | `chorus=0.5` | spreads the sound left and right; 0.3–0.6 for keys and pads. Not on bass (it stays mono). |
| a Rhodes or organ wobble | `tremolo=0.3 tremrate=4.5` | level dips by `tremolo` (0–1), `tremrate` times a second (5 when unset). 0.2–0.4 at 3–6 Hz is classic; deep and fast is an effect. |
| a timed echo | `delay=0.3 delaytime=0.1875 delayfeedback=0.5` | `delaytime` is a fraction of the bar: 0.1875 dotted eighth (the default), 0.25 quarter, 0.125 eighth, 0.375 dotted quarter. `delayfeedback` 0–0.9 is how long the repeats last (0.5 unset). Both need a `delay` send. |

- A section shares one delay when mixed: give every layer that sends to it the same `delaytime` and `delayfeedback` (`W_DELAY_SHARED` says when two differ; the first layer's wins).
- `chorus`, `tremolo` and `delayfeedback` take an `lfo` or `ramp` (`ramp=delayfeedback,0.2,0.8` for echoes that build). `lpenv` doesn't, and an lpf with an envelope can't also be swept.
