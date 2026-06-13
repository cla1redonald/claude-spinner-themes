# claude-spinner-themes 🦄🏴󠁧󠁢󠁳󠁣󠁴󠁿

**Teach Claude Code tae think in Scots.**

Awright. You ken that wee word that flickers awa while Claude Code's grafting —
*Thinking…*, *Pondering…*, *Percolating…*? Pure dreich, so it is. This swaps it
for something wi' a bit mair soul:

```
🏴󠁧󠁢󠁳󠁣󠁴󠁿 Swithering…        ⚽ Bicycle-kicking…        🦕 Nessie-hunting…
🦄 Jalousin…           🏰 Up-the-Pars…           🥃 Drammin…
🎶 Painting-the-Forth-Bridge…              🦄 Havering…
```

Six themes, a one-line switch atween them, and a wee unicorn (or a hale rotating
parade o' Saltires, castles an' Nessie) on every word. Aye, it's daft. It's also
weirdly braw watchin' yer editor have a *swither* instead o' a think.

*(stick a screen recording / GIF in here)*

## Whit's the script? (the honest one-liner)

Claude Code lets ye set yer ain spinner words through a setting cawed
`spinnerVerbs`. This repo is jist **a bundle o' ready-made Scottish word lists**
an' **a wee script that swaps them in an' oot, nae bother**. Nae magic, nuthin'
dodgy — it only ever touches that wan setting.

## The themes

| Theme | A wee taste o' it |
|---|---|
| 🦄 **scots-words** | Real dictionary Scots for thinkin' an' faffin' — *Swithering, Jalousin', Dwammin', Dreich, Howkin'* |
| 🏰 **fife** | Fife & Dunfermline patter an' heritage — *Slaisterin', Bunkerin', Carnegie-ing, Up-the-Pars* |
| ⚽ **scottish-football** | The legends an' the modern lot — *Gemmilling, McCoisting, Fergie-ing, McTominaying, Ya beauty* |
| 🎶 **scottish-culture** | Songs, films, scran, sayings, landmarks — *Walkin'-500-Miles, Deep-frying, Painting-the-Forth-Bridge* |
| 🎬 **famous-scots** | Comedy, telly, music — *Big-Yin-ing, Connery-ing, Still-Gaming, Rab-C-ing* |
| 🏴󠁧󠁢󠁳󠁣󠁴󠁿 **the-full-haggis** | The hale jingbang. 160 words o' glorious chaos. |

Every word, wi' its meaning an' where it's verified, lives in
**[reference.md](reference.md)** — dead handy when somebody asks "whit in the name
o' the wee man does *plowterin'* mean?"

## Gettin' it goin' (aboot twa minutes)

Ye'll need [`jq`](https://jqlang.github.io/jq/) — if ye huvnae got it,
`brew install jq` an' yer sorted.

```sh
# 1. grab the repo
git clone https://github.com/cla1redonald/claude-spinner-themes.git
cd claude-spinner-themes

# 2. (optional) make the switcher work frae anywhere
ln -s "$PWD/bin/spinner-theme" /opt/homebrew/bin/spinner-theme   # Apple Silicon
# or /usr/local/bin/spinner-theme on Intel Macs

# 3. pick yer theme
spinner-theme set the-full-haggis --emoji-rotate

# 4. restart Claude Code — an' awa ye go
```

That's the lot. The change lands in yer `~/.claude/settings.json`; a restart (or a
new session) an' it'll show its face.

## Livin' wi' it

```sh
spinner-theme list                  # see every theme; the active wan has a *
spinner-theme set fife              # switch theme (plain words)
spinner-theme set fife --emoji      # …wi' that theme's wee emoji
spinner-theme set fife --emoji-rotate   # …wi' a rotating Scottish set
spinner-theme current               # whit's on the noo?
spinner-theme mode append           # sprinkle yer words INTAE Claude's normal wans
spinner-theme off                   # back tae plain auld "Thinking…"
spinner-theme restore               # undae the last change
```

## A bit o' sparkle ✨

The words are jist text, so ye can dress them up wi' emoji:

- `--emoji` pits wan wee picture on every word (🦄 by default).
- `--emoji-rotate` **cycles a set** so it changes as Claude thinks — the default's
  🏴󠁧󠁢󠁳󠁣󠁴󠁿 🦄 🦕 🏰 ⚽ 🥃 🎶 (flag, unicorn, Nessie, castle, fitba, whisky,
  bagpipes). Want yer ain? `spinner-theme set fife --emoji-rotate 🏴󠁧󠁢󠁳󠁣󠁴󠁿 🦄 🦕`.

Twa wee honesty notes: there's **nae thistle emoji** (Unicode's done Scotland
dirty there), an' the **Scotland flag** 🏴󠁧󠁢󠁳󠁣󠁴󠁿 disnae draw richt in every
terminal — some show a plain black flag or empty boxes. Gie yours a swatch wi'
`printf '%s\n' "🏴󠁧󠁢󠁳󠁣󠁴󠁿"`; if it's a Saltire, yer laughin'.

## Will it caw the legs frae yer settings? (Naw.)

The switcher's canny. It **only ever touches the `spinnerVerbs` key** — everything
else in yer settings is left exactly as it wis (it edits wi' `jq`, nae clumsy
find-an'-replace). Afore any change it **takes a backup**
(`settings.json.bak.spinner`), it **checks the result's still valid** afore it
saves, an' `spinner-theme restore` pits it straight back. Want tae try it on a
throwaway file first? `CLAUDE_SETTINGS=/tmp/test.json spinner-theme set fife`.

## Nae havers — these are the real McCoy

The Scots an' Fife words are the genuine article. Every mainstream Scots word wis
checked against the **[Dictionaries of the Scots Language](https://dsl.ac.uk)**
(the scholarly national dictionary) or Wiktionary, an' the Fife wans come frae
local dialect glossaries. The fitba an' culture references got fact-checked an'
aw — which is how a certain midfielder quietly stayed in as a Scotland regular
efter it turned oot he'd missed the 2026 finals injured. Where a word wis shoogly
on its spellin' or meanin', it wis left oot rather than invented. The receipts are
in **[reference.md](reference.md)**.

## Roll yer ain

A theme's jist a wee JSON file:

```json
{ "mode": "replace", "emoji": "🦄", "verbs": ["Swithering", "Havering", "Dwammin'"] }
```

Drap a new wan in `themes/`, an' it shows up in `spinner-theme list` aw by itsel.
Want it folded intae the everything-theme? Rebuild it:

```sh
jq -s '{mode:"replace", emoji:"🦄", verbs:(map(.verbs)|add|unique)}' \
  themes/*.json > themes/the-full-haggis.json
```

## Bonus: while yer in there onywey

Fancy seein' how much o' yer session ye've burnt through, richt in Claude Code's
status line? It's there for the takin' — `rate_limits.five_hour.used_percentage`
(the rollin' session limit) an' `rate_limits.seven_day.used_percentage`. A status
line like `~/code  Fable  ctx:34%  session:61%  7d:12%` is a couple o' `jq` lines
awa.

## Licence

MIT — see [LICENSE](LICENSE). Use it, fork it, mak a Geordie wan. **Wha's like us?
Damn few, an' they're aw deid.** 🏴󠁧󠁢󠁳󠁣󠁴󠁿
