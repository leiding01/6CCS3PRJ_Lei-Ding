# Evidence Index — v0.3.2

This index maps each formal manual test case (TR-01…RA-04) to curated evidence triplets (**state JSON + trace TXT + preview PNG**) captured from the freeze baseline **`v0.3.2`**.

---

## Conventions

- Evidence root: `evidence/v0.3.2/`
- Each test case has a folder with a `raw/` subfolder containing exported triplets.
- Paths in the table are **relative to** `evidence/v0.3.2/`.
- Each snapshot is a triplet sharing the same base name:
  - `_state.json`, `_trace.txt`, `_preview.png`

---

## Index table

| Case ID | Algorithm | Mode | Setup / Scenario | Fault injected | Expected (pass) | Observed (key trace lines) | Evidence paths (state / trace / png) | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RA-01 | RA | interactive | 4 processes; basic REQUEST/REPLY; enter + release | none | REQUEST broadcast; enter CS only after N−1 REPLY received; Safety OK. | **A** `P1 enters the critical section (all REPLY received).`<br>**B** `P1 enters the critical section (all REPLY received).`; `P1 releases the critical section.` | **A** state: `RA-01_basic/raw/mutex_2026-02-01T22-13-19-037Z_RA_interactive_01_state.json`<br>trace: `RA-01_basic/raw/mutex_2026-02-01T22-13-19-037Z_RA_interactive_01_trace.txt`<br>png: `RA-01_basic/raw/mutex_2026-02-01T22-13-19-037Z_RA_interactive_01_preview.png`<br><br>**B** state: `RA-01_basic/raw/mutex_2026-02-01T22-13-19-037Z_RA_interactive_02_state.json`<br>trace: `RA-01_basic/raw/mutex_2026-02-01T22-13-19-037Z_RA_interactive_02_trace.txt`<br>png: `RA-01_basic/raw/mutex_2026-02-01T22-13-19-037Z_RA_interactive_02_preview.png` | RA baseline correctness. |
| RA-02 | RA | script | Tie-break scenario: P2 vs P10 at same timestamp | none | With equal timestamps, numeric PID tie-break; P2 enters before P10; Safety OK. | **A** `P2 enters the critical section (all REPLY received).`; `P2 releases the critical section.`; `P10 enters the critical section (all REPLY received).` | **A** state: `RA-02_tiebreak_p2_p10/raw/mutex_2026-02-01T22-16-22-140Z_RA_script_01_state.json`<br>trace: `RA-02_tiebreak_p2_p10/raw/mutex_2026-02-01T22-16-22-140Z_RA_script_01_trace.txt`<br>png: `RA-02_tiebreak_p2_p10/raw/mutex_2026-02-01T22-16-22-140Z_RA_script_01_preview.png` | Key teaching point: deterministic tie-break. |
| RA-03 | RA | interactive | Drop next in-flight message (drop queue head) to induce stall | drop next in-flight msg | After drop: may stall (liveness violated); queue can become empty; trace explains missing REPLY; Safety OK. | **A** `! Fault injected: dropped REQUEST #3 P1 -> P4.`; `! Stalled: P1 is waiting for REPLY from P4.` | **A** state: `RA-03_drop_inflight_stall/raw/mutex_2026-02-01T22-18-02-161Z_RA_interactive_01_state.json`<br>trace: `RA-03_drop_inflight_stall/raw/mutex_2026-02-01T22-18-02-161Z_RA_interactive_01_trace.txt`<br>png: `RA-03_drop_inflight_stall/raw/mutex_2026-02-01T22-18-02-161Z_RA_interactive_01_preview.png` | Core teaching evidence: fault → missing reply → stall (safety vs liveness). |
| RA-04 | RA | interactive | Arm drop-next-send (send-side fault) to induce stall | drop-next-send | Next outgoing message is dropped; system may stall; trace explains missing REPLY; Safety OK. | **A** `! Fault armed: next outgoing message will be dropped.`; `! Fault injected: dropped outgoing REQUEST #1 P1 -> P2.`; `! Stalled: P1 is waiting for REPLY from P2.` | **A** state: `RA-04_drop_next_send_stall/raw/mutex_2026-02-01T22-19-23-377Z_RA_interactive_01_state.json`<br>trace: `RA-04_drop_next_send_stall/raw/mutex_2026-02-01T22-19-23-377Z_RA_interactive_01_trace.txt`<br>png: `RA-04_drop_next_send_stall/raw/mutex_2026-02-01T22-19-23-377Z_RA_interactive_01_preview.png` | Send-side fault injection complements RA-03. |
| TR-01 | TokenRing | interactive | 4 processes; mutual exclusion (P1 in CS while P2 requests) | none | At most one process in CS at any time; Safety OK. | **C** `P1 releases the critical section.`; `P2 enters the critical section (holds token).` | **A** state: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_01_state.json`<br>trace: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_01_trace.txt`<br>png: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_01_preview.png`<br><br>**B** state: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_02_state.json`<br>trace: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_02_trace.txt`<br>png: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_02_preview.png`<br><br>**C** state: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_03_state.json`<br>trace: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_03_trace.txt`<br>png: `TR-01_tokenring_mutex/raw/mutex_2026-02-01T21-59-14-962Z_TokenRing_interactive_03_preview.png` | Baseline mutual exclusion (safety). |
| TR-02 | TokenRing | interactive | 4 processes; token loss then regeneration | token loss + regenerate | After token loss, Step makes no progress; after regeneration, progress resumes and CS entry is possible; Safety OK. | **A** `! Fault injected: token lost.`; `! Recovery: token regenerated at P1.`; `P1 enters the critical section (holds token).` | **A** state: `TR-02_token_loss_regen/raw/mutex_2026-02-01T22-04-06-966Z_TokenRing_interactive_01_state.json`<br>trace: `TR-02_token_loss_regen/raw/mutex_2026-02-01T22-04-06-966Z_TokenRing_interactive_01_trace.txt`<br>png: `TR-02_token_loss_regen/raw/mutex_2026-02-01T22-04-06-966Z_TokenRing_interactive_01_preview.png` | Fault + recovery are captured in one trace. |
| TR-03 | TokenRing | script + interactive | Crash/Recover demonstration (scripted scenario + interactive snapshots) | crash + recover | Crash is reflected; recover restores participation; normal progress resumes; Safety OK. | **B** `! Fault injected: P2 crashed.`; `P3 enters the critical section (holds token).`<br>**C** `! Fault injected: P2 crashed.`; `! Recovery: P2 recovered.`; `P4 enters the critical section (holds token).` | **A** state: `TR-03_crash_recover/raw/mutex_2026-02-01T22-08-53-680Z_TokenRing_script_01_state.json`<br>trace: `TR-03_crash_recover/raw/mutex_2026-02-01T22-08-53-680Z_TokenRing_script_01_trace.txt`<br>png: `TR-03_crash_recover/raw/mutex_2026-02-01T22-08-53-680Z_TokenRing_script_01_preview.png`<br><br>**B** state: `TR-03_crash_recover/raw/mutex_2026-02-01T22-11-32-069Z_TokenRing_interactive_01_state.json`<br>trace: `TR-03_crash_recover/raw/mutex_2026-02-01T22-11-32-069Z_TokenRing_interactive_01_trace.txt`<br>png: `TR-03_crash_recover/raw/mutex_2026-02-01T22-11-32-069Z_TokenRing_interactive_01_preview.png`<br><br>**C** state: `TR-03_crash_recover/raw/mutex_2026-02-01T22-12-23-126Z_TokenRing_interactive_01_state.json`<br>trace: `TR-03_crash_recover/raw/mutex_2026-02-01T22-12-23-126Z_TokenRing_interactive_01_trace.txt`<br>png: `TR-03_crash_recover/raw/mutex_2026-02-01T22-12-23-126Z_TokenRing_interactive_01_preview.png` | Includes scripted scenario load plus interactive crash/recover snapshots. |

---

## Standard report sentence (recommended)

For RA-03 and RA-04, explicitly state:

> These cases intentionally demonstrate that under unreliable networks (message loss), Ricart–Agrawala may lose **liveness** (progress), while still preserving **safety** (mutual exclusion). The tool’s trace explains the stall by indicating which REPLY is missing.

---

## Completeness checklist

- [x] Each case has at least one evidence triplet (state/trace/png).
- [x] RA-03 and RA-04 include an explicit stall explanation in trace (“Stalled: … waiting for REPLY …”).
- [ ] (Optional) Re-capture evidence on `git checkout v0.3.2` to ensure the runtime baseline is exactly the tag during capture.
