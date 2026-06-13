# claude-spinner-themes 🦄

Switchable **Scottish** themes for the wee word Claude Code shows while it's
thinking. Instead of "Thinking…" you get **"Swithering…"**, **"Bicycle-kicking…"**,
**"Walkin'-500-Miles…"** or **"Painting-the-Forth-Bridge…"** — and you can flip
between themes with one command.

> Claude Code lets you customise its spinner words via a `spinnerVerbs` setting.
> This repo turns that into six ready-made Scottish themes plus a safe switcher.

```
  ⚽ Bicycle-kicking…        🦄 Jalousin…           🏰 Up-the-Pars…
  🎶 Painting-the-Forth-Bridge…     🦄 Swithering…     ⚽ McTominaying…
```

*(screen recording / GIF goes here)*

## The themes

| Theme | Words | What's in it |
|---|---:|---|
| `scots-words` | 41 | Real dictionary Scots for thinking, working, faffing — `Swithering`, `Jalousin'`, `Dwammin'`, `Howkin'`, `Plowterin'`, `Dreich` |
| `fife` | 22 | Fife & Dunfermline dialect and heritage — `Slaisterin'`, `Bunkerin'`, `Carnegie-ing`, `Up-the-Pars`, `Howkin' coal` |
| `scottish-football` | 33 | Legends + current squad + the patter — `Bicycle-kicking`, `Gemmilling`, `McCoisting`, `Fergie-ing`, `Ya beauty` |
| `scottish-culture` | 44 | Songs, films, places, food, sayings, landmarks — `Walkin'-500-Miles`, `Munro-bagging`, `Deep-frying`, `Painting-the-Forth-Bridge` |
| `famous-scots` | 22 | Comedy, telly, music, screen — `Big-Yin-ing`, `Connery-ing`, `Still-Gaming`, `Rab-C-ing` |
| `the-full-haggis` | 160 | Everything, mixed. Maximum chaos. |

Full word lists with meanings and sources: **[reference.md](reference.md)**.

## Install

Needs [`jq`](https://jqlang.github.io/jq/) (`brew install jq`).

```sh
git clone https://github.com/cla1redonald/claude-spinner-themes.git
cd claude-spinner-themes
# optional: put the switcher on your PATH
ln -s "$PWD/bin/spinner-theme" /usr/local/bin/spinner-theme   # or /opt/homebrew/bin
```

## Use

```sh
spinner-theme list                    # see all themes; the active one is marked *
spinner-theme set fife                # apply a theme (plain words)
spinner-theme set scottish-football --emoji   # add the theme's default emoji (⚽)
spinner-theme set the-full-haggis --emoji 🦄  # choose your own emoji
spinner-theme current                 # show the active theme + its words
spinner-theme mode append             # mix your theme INTO Claude's defaults
spinner-theme off                     # back to Claude's normal words
spinner-theme restore                 # undo the last change
```

**Restart Claude Code** (or start a new session) to see the change.

### Graphics

The words are just strings, so emoji work: 🦄 (Scotland's national animal),
🏰, ⚽, 🥃. Pass `--emoji` to garnish every word. Two honest limits:

- The **Scotland flag emoji** 🏴󠁧󠁢󠁳󠁣󠁴󠁿 is a "subdivision flag" sequence that
  **most terminal fonts don't render** — you'll often get a plain black flag or
  empty boxes. Use it if your terminal handles it; unicorn/castle/football are safe.
- There is **no thistle emoji** in Unicode. Sorry. 🌿 is the nearest dodge.

## Is it safe?

Yes. The switcher only ever touches the `spinnerVerbs` key in your
`~/.claude/settings.json` — every other setting is preserved (it edits via `jq`,
never a hand-rewrite). It **backs up** to `settings.json.bak.spinner` before any
change, **validates** the result is valid JSON, and `restore` rolls back. Point
it at a different file with `CLAUDE_SETTINGS=/path spinner-theme …`.

## A note on accuracy

The `scots-words` and `fife` themes aren't made up. Every mainstream Scots word
was checked against the **[Dictionaries of the Scots Language](https://dsl.ac.uk)**
(the scholarly national dictionary) or **Wiktionary**; Fife terms come from
local dialect glossaries. The football and culture references were fact-checked
too — which is how a certain midfielder got left out of the squad after he
turned out to be injured. Meanings and sources are in
**[reference.md](reference.md)**. Where a word's spelling or sense was uncertain,
it was left out rather than invented.

## Add your own

Themes are just JSON: `{ "mode": "replace", "emoji": "🦄", "verbs": [ ... ] }`.
Drop a new file in `themes/`, and it shows up in `spinner-theme list`. Rebuild
`the-full-haggis` with:

```sh
jq -s '{mode:"replace", emoji:"🦄", verbs:(map(.verbs)|add|unique)}' \
  themes/scots-words.json themes/fife.json themes/scottish-football.json \
  themes/scottish-culture.json themes/famous-scots.json > themes/the-full-haggis.json
```

## Licence

MIT — see [LICENSE](LICENSE). Wha's like us?
