# Reading the data

For anyone who wants to use the dataset in a program, a spreadsheet or just out
of curiosity. Every file is plain JSON. The meaning of each field is in the
[Schema reference](https://github.com/PaintPlanPlay/dataset-tool/blob/main/docs/schema.md);
this page says where the files are and how they fit together.

## Start from the manifest

Do not read the files of the `main` branch directly: `main` holds whatever was
merged last, released or not. The published data is in **releases** — frozen,
tagged states of this repository — and the **manifest** says which one to read.

```
https://cdn.jsdelivr.net/gh/PaintPlanPlay/dataset@main/manifest.json
```

```json
{
  "schemaVersion": "2.5.0",
  "gameSystem": "wh40k-11e",
  "releaseUrl": "https://cdn.jsdelivr.net/gh/PaintPlanPlay/dataset@{tag}/",
  "current": "mfm-1-5",
  "offered": ["mfm-1-5", "mfm-1-4-r2", "mfm-1-4"],
  "dataslates": [
    {
      "id": "mfm-1-5",
      "name": "MFM 1.5",
      "mfmVersion": "1.5",
      "frozen": false,
      "latest": "wh40k-11e-mfm-1-5-r3",
      "releases": [ … ]
    }
  ]
}
```

1. Take the dataslate you want — `current` for today's rules.
2. Take its `latest` tag.
3. Replace `{tag}` in `releaseUrl`. That is the root of the release.

```
https://cdn.jsdelivr.net/gh/PaintPlanPlay/dataset@wh40k-11e-mfm-1-5-r3/
```

A **dataslate** is a rules period, named after the Munitorum Field Manual
version it follows. `offered` lists the ones worth offering to a player: the
current one and the two before it. An older dataslate is `frozen` — it gets no
new release, and its releases stay readable for ever, so a list written under
old points can still be displayed with them.

`latest` is the release to read. It is the most recent one, unless the
maintainers rolled back to an earlier one — which is why you follow `latest`
rather than picking the highest number yourself.

The CDN keeps its copy of the manifest for up to 12 hours. A release itself
never changes once published, so its files can be cached for as long as you
like.

## The files of a release

From the root of a release:

| File | What it holds |
|---|---|
| `wh40k-11e/index.json` | the list of armies, and the exact versions of the sources this release was built from |
| `wh40k-11e/core.json` | core stratagems, battle sizes, ally rules, the simulator's default targets, a sample list |
| `wh40k-11e/armies/<army>.json` | one army: its units, army rules, detachments and stratagems |

Read `index.json` first; each entry of `armies` gives the `id` that names the
army's file.

```json
{ "id": "orks", "name": "Orks", "faction": "", "units": 95,
  "refs": { "bsdata": "Orks", "mfm": "orks", "kdc": "orks" } }
```

→ `wh40k-11e/armies/orks.json`

## Things worth knowing

**Check `schemaVersion`.** Every file carries the version of the schema it
conforms to. A change of the first number means the shape of the files changed.

**Identifiers are permanent.** A unit keeps the identifier BSData gave its
datasheet; armies, detachments, enhancements, rules and stratagems have
readable ones (`blitz-brigade`). They are never reused or renamed across
releases, so you can store them.

**A unit can appear in several army files.** An army file lists every unit the
army can field, including those from another codex, marked `"ally": true`. The
same datasheet has the same `id` everywhere.

**Costs.** `pricing` is the Munitorum Field Manual grid: one band per range of
copies of the unit in a list (`from`…`to`), and in each band a cost per unit
size. `points` and `costBrackets` are a shortcut computed from the first band.
A unit without `pricing` is not in the Munitorum Field Manual and only has the
shortcut.

**Paid wargear.** A unit's `wargear` list holds the amounts
(`{ "item": "Zzap Gun", "points": 10 }`). A wargear option that is paid points
to a line by name — `"wargearCost": { "item": "Zzap Gun" }`, with a `quantity`
when it is paid more than once per model. The amount is only ever in the
`wargear` list.

**Saves and skills** are whole numbers from 2 to 7, where 7 means "none".

**Rules have no text.** An ability, a detachment rule, an enhancement or a
stratagem may carry `modifiers` (what it does, in a structured form), `options`
(for a rule that offers a choice), a `summary` (one line written by this
project) — or nothing but its `name`. Display what is there, and send the
player to their codex for the rest.

**Stratagem targets.** `target` restricts the units a stratagem can be used on,
by keywords. When it is absent, the dataset does not know how to restrict it —
which is not the same as "any unit".

## Licence and attribution

This project's contribution — the model, the identifiers, the corrections, and
the Modifiers and summaries written here — is published under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (see
[LICENSE](../LICENSE)): credit the Paint Plan Play Dataset when you use it.

It is built from three community projects, which have their own terms:
[BSData/wh40k-11e](https://github.com/BSData/wh40k-11e),
[BSData/wh40k-11e-mfm](https://github.com/BSData/wh40k-11e-mfm) and
[wn-mitch/40kdc-data](https://github.com/wn-mitch/40kdc-data).

Warhammer 40,000 and the names related to it belong to Games Workshop Limited.
This repository is neither affiliated with nor endorsed by Games Workshop, and
the game content is not licensed by us.

## Found a mistake?

See [How to contribute](../README.md#how-to-contribute).
