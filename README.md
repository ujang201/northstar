# northstar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/ujang201/northstar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/ujang201/northstar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

NORTHSTAR — token launch radar: ranks pre-token protocols by what their launch would be worth, from a sealed DefiLlama snapshot, with a sortable dashboard and a farm plan for every protocol.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json|directory>
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=wfjv7kgb&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>

## The model (radar-2)

Implied value follows only from the snapshot's own figures, and no protocol name is ever
special-cased:

```
multiple = median market-cap / TVL of the live-token protocols in the same category
           (at least 3 of them; otherwise the whole market's median)
implied  = round( tvl_usd x multiple
                  x (1 + clamp(change_7d, -60, 150) / 100 x 0.6)
                  x (0.7 + min(age_days, 1095) / 1095 x 0.5)
                  x (1 + min(chains - 1, 9) x 0.01) )
```

The market itself prices what a dollar of TVL is worth per category, so the model learns it from
the snapshot instead of assuming one flat multiple. Because every input is a figure the snapshot
carries, running the program on a later snapshot finds the next early protocols without anyone
keeping a list of names.

The board covers all 40 watchlist protocols and adds 260 pre-token protocols below the
$50,000,000 floor, by TVL — the early finds the radar exists to spot. Live-token protocols are
set aside with their market caps shown, never mixed into the ranking. Every row carries a farm
plan of six steps, each with what it costs and how long it takes, derived from the protocol's own
category, chains, TVL and age.

`main.ts` (Node 22, standard library only) reads the snapshot, writes `board.json` and `data.js`,
copies `dashboard/` and `screenshots/` to the output dir, and writes `dossier.json` with
`buildDossier` from `vendor/makefun-board.ts`. It is deterministic: two runs write byte-identical
output.

## Screenshots

`1-board.png` — the ranked board; `2-detail.png` — a protocol's detail view with its model
breakdown and farm plan; `3-filtered.png` — the board filtered to one category. All are real
renders of this dashboard in headless Chromium, unedited.

Referral noted by the entrant: https://make.fun/r/wfjv7kgb (code `wfjv7kgb`).
