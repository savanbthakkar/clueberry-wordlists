# Clueberry word lists

The modified dictionaries used by [Clueberry](https://clueberry.app), published
here because their licence asks for it.

Clueberry's Spanish and French word lists are derived from two dictionaries
released under the **Mozilla Public License 2.0**. Section 3.2 of that licence
says that if you distribute software built from MPL-covered files, you have to
let the people who receive it obtain the modified files too. The app is
distributed through Google Play, so this repository is how that is done.

Anyone can read, copy and reuse what is here under the MPL 2.0
(`LICENSE-MPL-2.0.txt`).

## What is in here

| File | What it is | Words |
| --- | --- | --- |
| `es/es-5.txt` | Spanish, five letters, one per line | 9,025 |
| `fr/fr-5.txt` | French, five letters, one per line | 7,044 |
| `NOTICE-es.txt`, `NOTICE-fr.txt` | The attribution each source requires | |
| `LICENSES.md` | Why each source was chosen, and which were rejected | |

These are the *guess* lists — the words the game will accept when a player
types them. They are not the puzzle answers. The answers are chosen from a
separate, much smaller set that is written by hand rather than derived from
these dictionaries, so no part of this licence reaches them, and publishing
them would only spoil the game.

## Where they come from

**Spanish** — [RLA-ES](https://github.com/sbosio/rla-es) v2.9 (`es.oxt`), by
Santiago Bosio and contributors. Offered under a free choice of GPL v3+,
LGPL v3+ or MPL 1.1 *or later*; taken here under MPL 2.0.

**French** — Dictionnaires orthographiques français "Classique" v7.7, by
Olivier R. ([grammalecte.net](https://grammalecte.net)), as redistributed in
[LibreOffice/dictionaries](https://github.com/LibreOffice/dictionaries). MPL
2.0.

Both are the generic, all-regions editions rather than a single country's, so
that a word used in one place is not rejected as a mistake in another.

## How they were modified

The originals are hunspell dictionaries, which are not word lists. They store a
stem plus flags naming the endings it can take — `parler` plus the rule that
makes `parlez` — so a program has to apply the rules to get words out. Clueberry
does that, then narrows the result to what a game board can show:

1. **Expanded** every stem against its affix rules, including prefixes, and a
   second suffix where a rule allows one.
2. **Dropped** anything the game cannot use: entries that are not all lowercase
   (which removes proper nouns), and anything containing an apostrophe or a
   hyphen, because removing the punctuation would invent a word that is not one
   — `d'abord` would become `dabord`.
3. **Folded** each word to plain A–Z: uppercase, accents removed, ligatures
   expanded. `école` becomes `ECOLE` and `cœur` becomes `COEUR`. This is what
   makes a word playable on a keyboard with 26 keys, and it is why `cœur` is
   not in the five-letter list — it is six letters once `œ` is written out.
4. **Kept** only the words that are exactly five letters after all of that.

Step 3 merges words that differ only by accent, which in French is most of the
difference between a verb and its participle: `abîme` and `abîmé` are one entry,
`ABIME`. 8,116 French forms become 7,044 distinct words that way.

Expansion is worth the trouble: the Spanish stems alone would give 4,011
five-letter words, and expanding the rules gives 9,025.

## Rebuilding them

The code that does this is `packages/wordlists/src/hunspell.ts` in the
Clueberry repository. It is not needed to use the lists — these are plain text
files — but the steps above are written out so the result can be checked
against the originals by anyone who wants to.

Generated 26 November 2026.
