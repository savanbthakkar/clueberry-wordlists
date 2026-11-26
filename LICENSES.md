# Word-list licences

The decision record for every list the game ships. Keep this file in whatever
public repository the generated lists are published to, next to the lists.

## English — ESDB (SCOWL): clear

- **What we use:** lists from the English Speller Database, formerly SCOWL,
  made through its official generator at <https://app.aspell.net/create>
  (US spelling, no variants, diacritics stripped). The guess dictionary is the
  size-80 list; answer candidates come from the size 35 and 40 lists.
  Lowercase entries only, so proper nouns, apostrophes and hyphens are out.
- **Licence:** permissive, in the header of every list the generator makes:
  use, copy, modify, distribute and sell are allowed provided the copyright
  notice and permission notice appear in all copies and in supporting
  documentation. Built from mostly public-domain sources (12dicts, ENABLE2K);
  COCA data was used under the author's own terms.
- **What we owe:** show that notice. The build script writes the full text into
  `generated/credit-en.json`, and the app's credits screen displays it. Keep
  the notice in this repository too.
- **Re-check:** the build script's `ESDB_NOTICE` was copied from the header of a
  list generated on 2026-11-03 (ESDB revision 1e5b7d3). If a later header
  changes, update it.

## Rejected: dwyl/english-words

Used by the first version of the build. Its repository carries an Unlicense
file, but its README says the list was extracted from an Infochimps dataset and
that "copyright still belongs to them". The two statements conflict and the
provenance is not clear enough to distribute. Do not reintroduce it.

## Rejected: FrequencyWords (hermitdave)

Its code is MIT but its word lists are CC BY-SA 4.0. Ranks derived from it would
have fed the answer lists, and publishing those lists would have carried the
share-alike condition with them. ESDB's own size levels now supply the
commonness signal instead.

## Spanish — RLA-ES v2.9: clear, taken under MPL 2.0

- **What we use:** `es.aff` and `es.dic` from the generic `es.oxt` of RLA-ES
  (Recursos lingüísticos abiertos del español) v2.9, by Santiago Bosio and
  contributors, at <https://github.com/sbosio/rla-es>. The generic package, not
  a country one, so a regionalism is accepted as a guess rather than refused.
  The affix rules are expanded into words by `src/hunspell.ts`; lowercase
  entries only, apostrophes and hyphens skipped, exactly as English.
- **Licence:** the project's `LICENSE.md` offers a *disjoint* triple scheme —
  GNU GPL v3 or later, GNU LGPL v3 or later, **or MPL version 1.1 or later** —
  and says the recipient may freely select one ("Puede seleccionar
  libremente"). We select the MPL, and because the grant reads "1.1 **o
  posterior**" we take it under **MPL 2.0**, which is what makes one compliance
  story cover both this and French.
- **Provenance:** `README.txt` names the covered files exactly ("integrado por
  el fichero de afijos y la lista de palabras (es[.aff|.dic])") and states that
  the work is original rather than derived from an earlier dictionary ("NO ESTÁ
  BASADO en el trabajo de…"). That is the statement `dwyl/english-words` could
  not make.
- **What we owe:** the MPL 2.0 notice, and — the part that is easy to miss —
  §3.2 source availability. Our generated list is a *modification* of an MPL
  file: expanded, filtered, folded to A–Z and cut to one length. Distributing
  the app in Executable Form therefore obliges us to tell recipients how to
  obtain the Source Code Form of that modification. The notice in
  `generated/credit-es.json` says where, and the build fails the release if
  that address is not readable without signing in — see "The §3.2 check" below.
- **Not taken:** the same `.oxt` carries a thesaurus, `th_es_v2.dat` and
  `th_es_v2.idx`, which is **LGPL v2.1 only** — no MPL option. The build reads
  two members by name and never touches those. Do not add them.

## French — Dictionnaires français v7.7: clear, MPL 2.0

- **What we use:** `fr.aff` and `fr.dic` of the "Classique" dictionary v7.7 by
  Olivier R. (<https://grammalecte.net>), as redistributed in
  `LibreOffice/dictionaries` under `fr_FR/dictionaries/`. Plain files, so no
  unpacking. Same expansion and same source policy as Spanish.
- **Licence:** `README_dict_fr.txt` states a single licence — "Licence: MPL :
  Mozilla Public License version 2.0". Note for anyone re-checking: search
  results claim this dictionary is tri-licensed GPL/LGPL/MPL, and the
  architecture plan assumed MPL 1.1. Both are wrong. The file in the package is
  the authority, and it says MPL 2.0 alone.
- **What we owe:** the same two things as Spanish — the notice, and §3.2 source
  availability for our modified list.

## The §3.2 check, and why it is in the build

MPL 2.0 §3.2 is satisfied by making the Source Code Form available to whoever
receives the Executable Form. `savanbthakkar/clueberry` is **private**, so it
cannot discharge this, and no amount of care in this file substitutes for a
list that is actually reachable.

So `src/build.ts` tests it instead of trusting it: after generating a language
under MPL it fetches `MODIFIED_LISTS_URL` unauthenticated and sets a non-zero
exit code if the answer is not OK. An unauthenticated request succeeding is
precisely the obligation, which is why that is the check rather than a reminder.

**Watch for this when reading a 404:** GitHub answers 404, not 403, for a
private repository. A 404 from that URL means "not reachable by a player", and
never "does not exist" — the two are indistinguishable from outside, and
mistaking one for the other has already wasted time once on this project.

What the published repository needs: the generated `es` and `fr` lists, a copy
of this file, and the MPL 2.0 text. The lists are build output and gitignored
here, so publishing them is a deliberate step, not a side effect of a push.

## Rejected: bayu01/Wordle-ES

A ready-made list of Spanish five-letter words, and the obvious shortcut. Two
independent reasons not to:

1. **No licence.** GitHub reports `license: NONE DECLARED`, which means all
   rights reserved by default. Nothing about a public repository grants
   permission to redistribute it.
2. **Unstated provenance.** Its 7.4 MB `0_palabras_todas.txt` says nowhere
   where the words came from. That is the same failure that got
   `dwyl/english-words` rejected above, and the reason it matters is identical:
   a list whose origin cannot be established cannot be cleared.

The clean path also costs nothing in quality, which is the useful part of the
comparison: expanding RLA-ES gives 9,025 five-letter Spanish words against that
file's 9,528. There was no trade-off to weigh.

## Rejected: Leipzig Corpora Collection

Considered for Spanish and French answer frequency, which is the one thing
hunspell cannot supply. Its own pages carry conflicting statements — CC BY-NC
in one place and CC BY in another — and elsewhere prohibit commercial use
outright. Either reading is disqualifying for an ad-supported app, and the
conflict itself means the terms are not settled enough to rely on.

This is why the Spanish and French answers in `answers/` are written by hand:
with no licence-compatible frequency list, there is no rank to tier on, and a
word chosen by hand carries no obligation at all. See those files' headers.

## German — open

The only licence option found is the GPL (the OASIS alternative covers only
programs saving OpenDocument). Either comply with the GPL by publishing the
derived list, or find a CC-BY source. Do not ship German until this is settled.
