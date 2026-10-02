# livenerf site 2: an independent replication

This fork runs the livenerf instrument unchanged (same locked 78-question panel, same harness, same
pinned CLI, same decision rule) on a second machine and a second Claude subscription, at a different
time of day. It is **not** part of the original pre-registered series in
[ninjahawk/livenerf](https://github.com/ninjahawk/livenerf) ("site 1"). It is a replication of it.
This file is committed and pushed before any site-2 validation or series data exists. Its git
timestamp is the pre-registration. [PREREGISTRATION.md](PREREGISTRATION.md) applies in full except
where this file says otherwise.

## Questions

1. **Primary:** does Opus 5.5, as served through headless Claude Code on this subscription, change
   between site 2's own 10-day baseline and the two 10-day windows that follow? The test is the
   original decision rule, applied unchanged with site 2's own day 1.
2. **Secondary, descriptive only:** do the two sites agree on the calendar dates they share, and
   does running at a busier hour change anything? No decision is drawn from this comparison.

## What is identical to site 1

- Panel `data/standard_panel.json`, lock `data/panel.lock` (sha256 `487c65e1…` of the CRLF checkout).
- Harness content hash **`461391b6fce64167`**: provider, tasks, graders, prompts, `uv.lock` and the
  CLI pin, verified on this machine before the first run.
- Claude Code CLI **2.1.280**: the official npm package `@anthropic-ai/claude-code@2.1.280`, run from
  a pinned copy (`~/.local/share/livenerf/claude-2.1.280.exe`).
- Benchmark sources: the same sha256-pinned downloads, with the same item counts (GPQA 198, MMLU-Pro
  2,000, competition math 78, AIME 60).
- Arms: the primary arm (78 questions × 1 per day, `claude-opus-5-5`, effort high) and the control
  arm (12 GPQA questions × 1 per day, `claude-opus-5`).
- Primary metric, clustered SEs and decision rule: a 99% interval excluding 0 in **both**
  post-baseline windows, |Δ| ≥ 3 points, and no matching move in the control arm.
  `python -m livenerf.analysis` applies it mechanically.
- Budget guard: skip at weekly ≥ 75% or 5-hour ≥ 60%, then retry hourly.

## Deviations from site 1 (fixed now, before any data)

| | site 1 | site 2 |
|---|---|---|
| Machine and subscription | the author's | a different Windows 11 PC and account |
| Start | day 1 = 2026-09-24 (≈2.5 days after launch) | day 1 = the first `ran` line in `logs/daily.jsonl`, expected 2026-10-02 or 2026-10-03 (≈10 days after launch). **No launch-week baseline.** |
| Daily start time | 05:07 local | **11:07 local, UTC−03:00 (14:07 UTC)**. Hourly retries until 20:07 local, so every attempt falls on the same UTC date (`scripts/windows_task.ps1 install -At 11:07 -Hours 9`). |
| Instrument validation | `docs/VALIDATION.md`, `data/validation.json` | rerun on this machine with the same protocol (4 arms × 4 reps, `--weekly-points 8`), reported in `docs/VALIDATION_site2.md` and `data/validation_site2.json`. Site 1's files are kept unchanged. The series starts only if site 2's validation passes the pre-registered criterion. |
| Repo changes | none | `.gitattributes` (`data/standard_panel.json text eol=crlf`, so the lock matches on any checkout), plus the `-At`/`-Hours` parameters of `scripts/windows_task.ps1`. Neither file is sample-shaping, and the harness hash is unchanged. |

## Analyses

- **Primary:** `python -m livenerf.analysis` on site 2's logs, for windows 1–3 relative to site 2's
  day 1. Reported whatever the result: change, no change detected, or improvement.
- **Secondary (descriptive):** for the calendar days the two sites share, the daily primary score and
  the median output tokens side by side. This only uses site 1's published charts and tables; no site-1
  raw logs are needed. Agreement or disagreement is described, not tested.
- Every secondary analysis in PREREGISTRATION.md (tokens, classifier-event rate, item-audit
  sensitivity, and the rest) is run as specified there.

## Data

Raw `.eval` logs stay local and backed up, for the same reason as site 1: they contain GPQA text.
Summaries, charts and `analysis` output are committed to this fork. Any later change gets a dated
entry in the log below, before the data it concerns exists.

## Deviations log (site 2)

- *(none yet)*
