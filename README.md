# rk-work

Run data for the rk search, an unattended hunt for Runge-Kutta integrators that hold up in Q15
fixed point on a Cortex-M0+ cycle model. The run container writes this repository at the end of
every cycle. People do not edit it.

- Overview of the project: https://jgoetzmann.github.io/rk-overview/
- Live findings built from this data: https://jgoetzmann.github.io/rk-findings/
- The code that writes it: [rk-harness](https://github.com/jgoetzmann/rk-harness)

## What is in here

| Path | What it holds |
| --- | --- |
| `archive/YYYY-MM-DD.jsonl` | The scored archive for the explicit class. One record per accepted tableau, appended and never rewritten. Each record carries the verifier hash that scored it. |
| `adaptive_archive/`, `implicit_archive/` | The two lane archives. `ledger.jsonl` is one line per candidate the lane search measured; `elites.json` is the capped, ranked list the findings site publishes. Nothing in them is scored against the pinned checker. |
| `sidetrack/` | The side-track measurement ledger (`ledger.jsonl`) and one JSON artifact per measured point, grouped by job. Each artifact states its own arithmetic. |
| `validation/results.json` | The practical validation suite: Q15 and float64 error for each method on each application problem. |
| `benchmark/results.json` | Library comparisons, matched-accuracy tables for the three classes, and measured time per step. |
| `falsification.json` | The early falsification experiment: where roundoff overtakes truncation error. |
| `hypotheses.jsonl` | The hypothesis ledger. Verdicts come from code. |
| `literature/`, `interpretation/` | Model-written literature digests and cycle interpretations. Verify their citations before relying on them. |
| `schedule/` | The lane rotation's own records, one row per cycle. |
| `prototypes/` | Output of the early adaptive and SDIRK prototypes. |
| `LAST_DIRECTIVE.json`, `saturation_state.json` | The most recent search directive and the epoch saturation state. |

Some files exist only on the machine running the container and are gitignored: the event stream
(`events.jsonl`), run state, the heartbeat, the archive checkpoint, and the per-day lane record
files (`*_archive/20*.jsonl`). The per-day lane files run to a few hundred megabytes a day, and
single files already pass GitHub's 100 MB limit. Every record in them can be regenerated from
code and parameters.

## What the public sites read

The findings pages read `archive/`, `validation/results.json`, `benchmark/results.json`,
`falsification.json`, `hypotheses.jsonl`, the side-track ledger and the artifacts it names, and
the two `elites.json` files. The research log also shows the newest entries from `literature/`
and `interpretation/` as model-written text, and the epoch panel on the findings index reads the
run's state files. The overview site reads the validation, benchmark and falsification documents
and its own `key_findings.json`, computed from this archive. Neither site quotes the per-day lane
records, `validation/axes.json` or `schedule/`. The reasoning is in `rk-harness/docs/DECISIONS.md`
(D24, D39, D40).
