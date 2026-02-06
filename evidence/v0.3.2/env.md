# Evidence Environment — v0.3.2

This file records the repository baseline and execution environment used to capture the evidence pack referenced in `EVIDENCE_INDEX.md`.

---

## 1) Freeze baseline (repository)

- Freeze baseline tag: `v0.3.2`
- Tagged commit:
  - `0f370b7957eaff82ae0fb33c533c80450a13c59d`
- Working tree (`git describe --tags --always`):
  - `v0.3.2-2-g2c61cf7`
- HEAD commit:
  - `2c61cf7923bb38e400625d77b167b38ec920d0db`
- Origin remote:
  - `https://github.com/leiding01/6CCS3PRJ_Lei-Ding_Distributed-Mutex-Explorer.git`

Notes:
- The working tree includes commits after `v0.3.2` that only adjust formatting/comments (no behavioural changes). For strict reproducibility, re-capture evidence on `git checkout v0.3.2`.

---

## 2) Evidence capture window

Derived from the export session IDs embedded in evidence filenames:

- Earliest export session: `2026-02-01T21-59-14-962Z
- Latest export session: `2026-02-01T22-19-23-377Z

---

## 3) Evidence pack layout (within this repository)

Evidence is organised by test case and stored under:

- `evidence/v0.3.2/<CASE>/raw/`

Where `<CASE>` is one of:

- `TR-01_tokenring_mutex`
- `TR-02_token_loss_regen`
- `TR-03_crash_recover`
- `RA-01_basic`
- `RA-02_tiebreak_p2_p10`
- `RA-03_drop_inflight_stall`
- `RA-04_drop_next_send_stall`

Each export snapshot is a triplet with a shared base name:
- `*_state.json` — serialised simulator state
- `*_trace.txt` — human-readable trace (with header)
- `*_preview.png` — canvas snapshot

Filename base-name pattern:
- `mutex_<timestamp>_<Algorithm>_<Mode>_<NN>`

---

## 4) Local run method (UI)

- Command:
  - `python -m http.server 5500`
- URL opened:
  - `http://localhost:5500/`

---

## 5) Automated smoke tests (GitHub Actions)

Local Node/npm is not installed on the evidence-capture machine, so smoke tests were executed and verified via **GitHub Actions CI**.

### CI workflow configuration

- Workflow: `.github/workflows/ci.yml`
- Job: `smoke-tests`
- Runner: `ubuntu-latest`
- Node version: `20` (via `actions/setup-node@v4`)
- Command: `npm test` (runs `node tools/smoke_tests.mjs` from repo root)

### CI log excerpt (PASS)

```text
> test
> node tools/smoke_tests.mjs

✅ TR basic mutual exclusion
✅ TR token loss + recovery
✅ RA basic entry
✅ RA tie-break by PID
✅ RA drop-next-send stalls (expected)
✅ RA drop next in-flight message (smoke)

6/6 tests passed.
```

### CI run metadata (as observed)

- Status: succeeded
- Date: 20 Dec 2025
- Workflow file revision: `c462044` (as shown by GitHub Actions UI)

## 6) Machine + browser (fill in)

These fields should be recorded for dissertation reproducibility.

- OS: `<e.g., Windows 11 23H2 / macOS 14.x / Ubuntu 22.04>`
- Browser: `<e.g., Edge / Chrome / Firefox + version>`
- Python (`python --version`): `Python 3.12`
- Node (`node -v`): `N/A (not installed locally; tested in CI with Node 20)`
- npm (`npm -v`): `N/A (not installed locally; tested in CI)`

---

## 7) Notes (optional)

- Export download location:
  - `<e.g., Downloads/>`
- Evidence curation:
  - `<e.g., moved into evidence/v0.3.2/<CASE>/raw/>`
- Any anomalies / mitigations:
  - `<e.g., evidence re-captured at time ...>`
- Local environment does not include Node/npm:
  - Regression checks are validated via GitHub Actions CI (Node 20).

