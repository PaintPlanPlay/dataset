# Paint Plan Play Dataset

Open data about Warhammer 40,000 — units, detachments, enhancements and
stratagems — used by the Paint Plan Play apps.

**This dataset contains no rules text.** What a rule *does* is described either
in a structured form we call an *Effect*, or by a short one-line summary written
by this project, or simply by the rule's name. Keep your codex at hand: this is
a set of numbers and references, not a copy of the rulebook.

Anyone can help keep it accurate. **You do not need to be a developer** — there
is a point-and-click way to do it, described below.

## What's in here

| Folder | What it holds |
|---|---|
| `corrections/` | one small file per fix, saying what was wrong, what it should be, and why |
| `authored/` | things no source publishes: battle sizes, the default targets used by the simulator, the sample list, and effects written by this project |
| `.github/` | the automation: scheduled rebuilds, and the checks that run on every pull request |

Three more appear with the first release, because they are produced by a build
rather than written by hand:

| Folder | What it holds |
|---|---|
| `wh40k-11e/` | the dataset itself: one file per army, plus a shared `core.json` and an `index.json` |
| `registry/` | the permanent identifiers, so a saved army list keeps working across updates |
| `manifest.json` | which published version the apps should read |

## How the data stays up to date

```
BSData ─────────┐
MFM (via BSData)├──▶  Dataset Tool  ──▶  a pull request here  ──▶  a release
40kdc-data ─────┘      (builds it)        (reviewed by a human)     (apps read it)
```

Three community projects feed this dataset, and each one is trusted for what it
does best:

| Source | Trusted for |
|---|---|
| [BSData](https://github.com/BSData/wh40k-11e) | unit profiles, weapons, wargear options, keywords |
| [Munitorum Field Manual, via BSData](https://github.com/BSData/wh40k-11e-mfm) | points costs, requisition brackets, paid wargear, leader/support attachments, detachment points, force dispositions, enhancements |
| [40kdc-data](https://github.com/wn-mitch/40kdc-data) | detachment rules, enhancement restrictions, stratagems and their targets, and the Effect format |

When two sources disagree, the one trusted for that field wins, and the
disagreement is reported so a human can look at it.

Nothing reaches the apps until a **release** is published: a tagged, frozen
snapshot of this repository. Releases are grouped into **dataslates** (roughly,
a rules period). An army list can follow the current dataslate or stay pinned to
an older one — useful when a tournament list was submitted before an update.

## How to contribute

### The easy way: use the Dataset Tool (no coding)

You will need [Node.js](https://nodejs.org) 24 or newer, [git](https://git-scm.com), and a GitHub
account. Install the [GitHub CLI](https://cli.github.com) too and run `gh auth login` once — the
tool borrows that login to open your pull request, since GitHub has not accepted a password for
git since 2021.

```bash
git clone https://github.com/PaintPlanPlay/dataset-tool.git
cd dataset-tool && npm ci
npm run dev
```

Open `http://127.0.0.1:4173` in your browser, then:

1. Click **Update data** — it downloads this repository and a snapshot of the
   three sources, so you never work on something that is already fixed.
2. Search for what you want to fix, for example a unit name, and click it.
3. The sheet shows every value and **where it comes from**: which source, or
   which correction changed it.
4. Fill in the *New correction* form at the bottom: what should change, and why.
5. Click **Write the Correction**. The file is created and the sheet reloads.
6. Click **Save and build**, so the dataset files carry your correction.
7. Click **Propose my changes**, or copy the commands it shows you, to open a
   pull request here.

That's it. The tool checks your change against the schema and the no-rules-text
rule before writing anything.

### The manual way: edit a file on GitHub

If you would rather not install anything, you can create a correction file
straight from the GitHub website: browse to `corrections/wh40k-11e/<army>/`,
click **Add file → Create new file**, paste the JSON below, and GitHub will walk
you through opening a pull request.

```json
{
  "target": "4ea0-6b70-c17c-bc00",
  "source": "mfm",
  "patch": { "points": 300 },
  "upstream": { "points": 285 },
  "reason": "The September dataslate raised this unit to 300 points."
}
```

| Field | What to put in it |
|---|---|
| `target` | what you are fixing — a unit's id, or an address like `<unitId>::weapon:melee\|Power klaw` |
| `source` | which upstream source got it wrong: `bsdata`, `mfm` or `40kdc` |
| `patch` | the corrected values, and only those |
| `upstream` | what that source says today, so we can tell later whether they fixed it themselves |
| `reason` | one sentence, in your own words |
| `upstreamPr` | optional: the link to the issue or pull request you opened with the source, so the fix lands upstream too |

Unit ids are the long dashed strings you see in `wh40k-11e/armies/<army>.json`;
the Dataset Tool fills them in for you if you use the interface.

### The one rule: never paste rules text

Do not copy ability descriptions, stratagem text, or any wording from a codex,
an app, or another website — not even reworded. This is the legal footing of the
whole project, so it is checked automatically and any pull request that breaks it
is rejected.

Instead, describe what the rule *does*:

- a **summary**: one short line in your own words, such as `+1 to wound in melee on the charge`;
- or an **Effect**: the structured form used by 40kdc-data, which the apps can
  actually simulate. The Dataset Tool can suggest one and let you accept it.

If you are unsure, leave the rule with just its name — that is always allowed.

## What happens after you open a pull request

Automated checks run on every pull request: the files must match the schema, and
must contain no rules text. A maintainer then reviews it. Once merged, your fix
is included in the next release, and the apps pick it up from there.

There is also a scheduled job that rebuilds the dataset when the upstream
sources change, and opens a pull request with a drift report: changed numbers,
disagreements between sources, and the status of every correction.

## Attribution

Built from three community projects, with thanks:

- [BSData/wh40k-11e](https://github.com/BSData/wh40k-11e)
- [BSData/wh40k-11e-mfm](https://github.com/BSData/wh40k-11e-mfm)
- [wn-mitch/40kdc-data](https://github.com/wn-mitch/40kdc-data)

## Licence

This project's contribution — the model, the identifiers, the corrections, and
the effects and summaries written here — is published under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (see [LICENSE](LICENSE)).
Contributions are accepted under that same licence.

**Warhammer 40,000** and the names, marks and logos related to it belong to
**Games Workshop Limited**. This repository is neither affiliated with nor
endorsed by Games Workshop, and the game content is not licensed by us. No rules
text is published here.

**Rights holders**: if you represent a rights holder and have a concern, please
open an issue on this repository.

The tool that builds this dataset is the
[Dataset Tool](https://github.com/PaintPlanPlay/dataset-tool), published under MIT.
