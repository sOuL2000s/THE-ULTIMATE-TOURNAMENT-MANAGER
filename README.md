# Matchday — Football Tournament Manager

A single-file, offline football tournament manager. Create any competition — World Cup, Champions League, a domestic league, a simple cup — add teams, generate fixtures, enter scores and scorers, and watch tables, brackets and next-round fixtures build themselves.

No install, no server, no dependencies. Just open `matchday-tournament-manager.html` in a modern browser (Chrome, Edge, Firefox, Safari).

---

## Contents

- [Quick start](#quick-start)
- [Core concepts](#core-concepts)
- [Presets](#presets)
- [Tabs](#tabs)
- [Entering results](#entering-results)
- [Customising a format](#customising-a-format)
- [Tie-breakers](#tie-breakers)
- [Validation](#validation)
- [Data, backup and export](#data-backup-and-export)
- [Limitations](#limitations)
- [Troubleshooting](#troubleshooting)
- [Project structure and building](#project-structure-and-building)

---

## Quick start

1. Open `matchday-tournament-manager.html`.
2. Click **New tournament**, pick a preset (e.g. *World Cup — 32 teams*), choose a team source (sample teams, placeholders, or empty) and click **Create**.
3. If you started with empty teams, add them in the **Teams** tab (one by one, or paste a list with **Bulk add**).
4. In **Format**, press **Generate fixtures** (this happens automatically if you ticked the option in the wizard and the team count is valid).
5. Go to **Fixtures & Results** and type scores. Tables, brackets and later stages update instantly.

---

## Core concepts

A tournament is an ordered chain of **stages**. Each stage is one of:

| Stage type | What it is |
|---|---|
| **League** | Round robin with any number of legs (1 = everyone plays once, 2 = home & away, …). |
| **Groups** | Several mini-leagues (World Cup / Euros style). |
| **League phase** | One table where each team plays N different opponents, optionally drawn from pots, with balanced home/away (new Champions League style). |
| **Knockout** | Bracket with byes, single or two-legged ties, optional third-place play-off. |

Later stages have **qualification rules** that say who enters them, for example:

- top N of every group
- best N-th placed teams across groups (e.g. best 8 third-placed)
- table positions from–to (e.g. 9–24)
- winners (or losers) of a knockout stage

Rules can be combined, e.g. *league positions 1–8* **plus** *winners of the play-off stage*.

**Auto-progression.** When a stage finishes, the next one generates itself — pairings, byes, legs and dates included. If you correct an earlier score:

- a downstream stage with no results yet is rebuilt automatically;
- a downstream stage that already has results is flagged "out of date" and can be reset by you;
- within a bracket, a later-round fixture whose teams change has its old result cleared, with a notice.

---

## Presets

Presets are just starting points; every setting can be changed afterwards.

**International**
- World Cup — 32 teams (8 groups of 4 → Round of 16 with cross-group pairings → final, third-place play-off)
- World Cup — 48 teams (12 groups, top 2 + 8 best third-placed → Round of 32)
- Euros / AFCON — 24 teams (6 groups, top 2 + 4 best third-placed)
- Copa-style Cup — 16 teams (4 groups → quarter-finals)
- Small Cup — 8 teams (2 groups → semi-finals → final)

**Club Europe**
- Champions League — 36 teams (new format: league phase of 8 matches from 4 pots, top 8 direct, 9–24 play-offs, two-legged knockouts, one-off final)
- Europa League — 36 teams (same structure)
- Conference League — 36 teams (6 matches, 6 pots)
- Champions League — 32 teams (classic groups, home & away)

**Domestic leagues**
- Premier League style (20), La Liga style (20, head-to-head first), Serie A style (20), Bundesliga style (18), Ligue 1 style (18)
- Round robin league — single leg, or home & away (any team count)
- League + Playoffs (top 4)

**Knockout**
- Simple Knockout (single leg, any team count, automatic byes)
- Knockout with two legs and a one-off final
- Random-draw Cup

**Custom**
- Blank — build any chain of stages yourself.

---

## Tabs

| Tab | Purpose |
|---|---|
| **Overview** | Champion banner, progress, stage status, next fixtures, latest results, top scorers, warnings. |
| **Teams** | Add/edit/reorder teams, short code, colour, pot, squad list. Order = seeding. |
| **Format** | Tournament details, stage editor, qualification rules, generation, validation. |
| **Fixtures & Results** | Filterable fixture list with inline score entry. |
| **Tables** | Group or league tables with qualification/relegation zones, best-third ranking, extra columns. |
| **Bracket** | Visual knockout bracket; click a tie to edit it. |
| **Stats** | Top scorers, assists, discipline, team stats, records. |
| **Data** | Export/import, CSV, print, maintenance. |

### Teams tab

- **Seeding** is the list order. It decides byes, bracket positions and default pots.
- **Pots** (optional) control group and league-phase draws: each group gets one team from every pot. **Auto-assign pots** fills them in seed order.
- **Bulk add** accepts one team per line, optionally `Name, CODE, #colour`.
- **Sample teams**: national teams, and English, Spanish, Italian, German, French and European clubs. These are placeholders — edit to taste.
- **Squad**: list players per team; names appear as suggestions when you record scorers. New scorer names are added automatically.

---

## Entering results

**Quick entry.** In *Fixtures & Results*, type the home and away scores and press Enter or Tab. Leave both boxes empty to clear a result.

**Details** opens the full match editor:

- date, time, venue, notes
- goals (normal, penalty, own goal) with scorer, assist and minute
- yellow, second-yellow and red cards
- extra time flag
- penalty shoot-out score
- *Score from goals* button, which sets the score from the goal events listed
- a warning tag appears in the list if listed goals don't add up to the score

**Knockout specifics**

- Two-legged ties are decided on aggregate, then away goals (if enabled), then penalties after the last leg. Penalties can be recorded on the last leg even if that leg alone wasn't level.
- If a tie is level and unresolved, the app prompts you for the shoot-out.
- If shoot-outs are disabled for the stage, you can pick a winner manually (lots / replay).

**Filters**: stage, group, round (current / all / specific), team, status, free-text search. "Current round" jumps to the first round with unplayed matches.

---

## Customising a format

Everything lives in the **Format** tab, per stage.

**All stages**
- Name, type, order, auto-generate toggle
- Schedule: first match day, days between rounds, default kick-off, and an **Apply dates** button
- **Redraw** (new random draw) and **Reset stage**

**League** — legs, randomise round order.

**Groups** — number of groups, legs, draw method (seeded pots / fully random / by team order / manual), **Edit groups** to move teams between groups (before results exist).

**League phase** — matches per team, number of pots. Teams × matches must be even.

**Knockout**
- Pairing method:
  - *Seeded* — best v worst, byes to top seeds, avoids same-group first-round clashes
  - *Random draw*
  - *Cross-group* — A1 v B2, B1 v A2… (needs 2 qualifiers from each of 2, 4, 8… groups)
  - *Seeded v unseeded draw* — top half against bottom half
  - *Paired draw* — seeds in pairs, e.g. 9/10 v 23/24
  - *Manual* — set the first-round pairings yourself (**Edit first-round pairings**)
- Rounds to play (0 = until a winner; e.g. 1 for a play-off round only)
- Legs per round and legs in the final
- Third-place play-off, away goals, extra time, penalties, higher seed hosts the second leg
- Custom round names (comma separated)

**Table stages (league, groups, league phase)**
- Points for win / draw / loss
- Optional "draws go to penalties" with its own points (e.g. 2 / 1)
- Orderable tie-breaker list
- Coloured table zones (e.g. Champions League places, relegation); if none are set, qualification spots are highlighted automatically
- Point adjustments / deductions per team
- "Treat stage as finished using current standings" to skip unplayed matches

**Tournament details** — name, season, organiser, and the *required number of teams* check.

You can also **replace the whole format with another preset** at any time (teams are kept; fixtures and results are deleted).

> Changing structural settings (legs, number of groups, pairing, qualification rules…) on a stage that already has fixtures rebuilds that stage. If results exist you are asked to confirm first.

---

## Tie-breakers

Teams are ranked by points, then by your ordered list of criteria:

- Goal difference
- Goals scored
- Goals conceded (fewer is better)
- Number of wins
- Away goals scored
- Head-to-head points
- Head-to-head goal difference
- Head-to-head goals scored
- Head-to-head away goals
- Fair play (fewer card points: yellow 1, second yellow 3, red 4)
- Drawing of lots (falls back to team list order)

Head-to-head criteria are evaluated only among the teams that are level at that step. Presets ship with sensible defaults (e.g. head-to-head first for La Liga / Serie A).

The "best N-th placed" ranking across groups uses points, goal difference, goals scored, fair play, then team order.

---

## Validation

The app checks your setup continuously and shows problems in *Format* and *Overview*. Errors block fixture generation for the affected stage.

- Required team count not met (e.g. "needs 32 teams, you have 30")
- Fewer than 2 teams, or duplicate team names
- Groups that would have fewer than 2 teams; warnings for uneven groups
- Qualification rules that take more teams than exist (e.g. 3 per group from groups of 2)
- League phase with impossible or odd team × match counts; pot warnings
- Cross-group or draw-style pairings that don't fit the bracket (falls back to seeded, with a warning)
- Qualification sources pointing at missing or later stages

Each stage also shows how many teams it will receive and how byes will be handled.

---

## Data, backup and export

- **Storage**: everything is saved automatically in your browser's local storage on this device and this browser profile.
- **Export JSON** (one tournament or all) — use it as a backup or to move to another device; **Import JSON** restores it.
- **CSV exports**: fixtures & results, standings, scorers.
- **Print**: browser print with controls hidden.
- **Undo**: the Undo button reverts recent changes (this session).
- **Maintenance**: clear all results (keep fixtures), delete all fixtures, duplicate, delete.

Clearing browser data for the file will erase your tournaments — export a JSON backup now and then.

---

## Limitations

- Presets reproduce the **format**, not official draws or schedules. The 48-team World Cup uses a seeded bracket rather than FIFA's fixed table, and the Champions League knockout uses a seeded bracket rather than the official fixed path.
- The league-phase scheduler can use one more matchday than the real competition (e.g. 9 instead of 8 for the Champions League). The draw itself is valid: correct opponents, pot split and home/away balance.
- Double elimination and Swiss-by-round pairing are not included.
- Sample team lists are placeholders, not official entries for any given season.
- Data is per browser; there is no cloud sync.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Generate fixtures" is disabled | Read the red messages on the stage — usually the team count doesn't match the required number, or a group/qualification count doesn't fit. Clear *Required number of teams* to disable that check. |
| Next stage doesn't appear | It generates when the previous stage is complete. Check the stage status; use *Treat this stage as finished* to skip unplayed matches. |
| A bracket tie won't take a winner | The tie is level: enter penalties in **Details** (last leg) or choose a manual winner. |
| "Out of date" warning on a stage | An earlier result or the team list changed after this stage already had results. Use **Reset stage** to rebuild. |
| Data vanished | Browser storage was cleared or you opened the file in a different browser/profile. Import your JSON backup. |
| Can't save | Storage may be blocked (private mode). Use Export to keep a copy. |

---

## Project structure and building

The shipped file is generated from the sources in `src/`:

```
src/
  engine.js       Pure logic: fixtures, tables, tie-breakers, brackets, auto-progression, validation
  presets.js      Preset formats and sample teams
  ui_core.js      State, persistence, events, home screen, new-tournament wizard
  ui_views1.js    Overview, Teams, Format
  ui_views2.js    Fixtures, Tables, Bracket, Stats, Data, modals, match editor
  style.css       Styling (dark and light themes)
  template.html   HTML shell
  main.js         Boot
build.js          Bundles everything into matchday-tournament-manager.html
```

Build (Node.js only, no packages):

```
node build.js
```

The engine has no DOM dependency, so it can be unit-tested in Node by loading `engine.js` and `presets.js` into a script context.

To add a preset, append an entry to `PRESETS` in `presets.js`: give it an id, category, icon, name, default team count, a `strict` flag (whether the team count is enforced) and a `build()` function returning an array of stages built with `newStage(type, options)`. Wire qualification with `sources: [{ stage: previousStage.id, kind: 'groupPos', perGroup: 2 }]`. Supported kinds are `rank`, `groupPos`, `bestOfPos`, `koWinners`, `koLosers` and `all`.
