# Mixing

The app mixes and masters every track: each role has its own channel strip (high-pass, EQ, compression, level), layers share one reverb and one delay per section, the kick ducks the bass and pads, and every track masters to the same loudness. You describe intent; the engine sets the numbers.

- `mix` (top level) picks the master's character: `clean`, `lofi-tape` (worn tape, hiss, high cut), `club` (tight low end, pumping sidechain, bright), `cinematic` (wide, long reverb), `bedroom-pop` (soft, airy), `trap` (808 forward, crisp hats). Set it to match the brief.
- Per layer, only when it differs from the role's default:
  - `presence`: `back` (further away, quieter), `mid` (default), `front` (forward, the focus).
  - `tone`: `dark`, `warm`, `neutral` (default), `bright`.
  - `space`: `dry`, `room`, `hall` (reverb send). Omit for the role's default; bass and kick stay dry.
- `tone` and `presence` also go at the top of the score as a default for every layer (tone skips drums). "Darker" = top-level `tone: dark`; "brighter" = top-level `tone: bright`; "more space" = `space: hall` on pads and leads; "punchier" = `mix: club` or kick and bass `presence: front`. Neither changes the rhythm.
- Raw `fx` numbers are fine-tuning on top of the mix, for a sound the intent can't say: a filter sweep (`lfo` on lpf), crush, a deliberate pan. Leave gain, hpf and room to the engine unless the brief needs a specific value. Never room, delay, pan or width on bass or kick.
