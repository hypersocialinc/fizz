# Fizz

An agent skill for writing music in **Fizz**, the plain-text music language of [Fizz FM](https://fizz.fm).

A Fizz track is a few lines: the tempo, the key, the chords and what each instrument plays on a step grid. Pitches are scale degrees, so every note is in key. The skill teaches an agent the language, the theory, arrangement, groove and mix rules that make a track sound good, a cookbook of 20 producer idioms, and eight worked tracks across genres.

## Install

```sh
npx skills add hypersocialinc/fizz
```

This installs `skills/fizz` for Claude Code, Codex, Cursor and the other agents the [skills CLI](https://github.com/vercel-labs/skills) supports.

Then ask your agent for a track: "make a lo-fi beat in Fizz", "write a trap loop as a .fizz file".

## Check a track

```sh
npx @hypersocial/fizz check track.fizz
npx @hypersocial/fizz analyse track.fizz
```

## Play it

Embed a track on any web page with the [Fizz player](https://fizz.fm/player), or export it to Strudel with `npx @hypersocial/fizz export --strudel track.fizz`.

---

MIT licensed (see `LICENSE`). This repository is generated from the Fizz FM monorepo; changes made here are overwritten.
