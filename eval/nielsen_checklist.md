# Nielsen Heuristic Evaluation Checklist — Distributed Mutual Exclusion Explorer

**Artefact:** Distributed Mutual Exclusion Explorer  
**Version:** v0.3.2 (Token Ring + Ricart–Agrawala; faults: token loss, crash/recover, message loss; evidence export)  
**Evaluator:** Self (developer)  
**Date:** 2026-02-06  
**Environment:** Browser/OS/device (fill in)

---

## Scope

This checklist covers the **end-to-end UI** for the Distributed Mutual Exclusion Explorer, including:

- Configuration (algorithm selection, process count, apply/reset)
- Core controls (step/run/pause/reset; speed)
- Per-process actions (request/release; crash/recover)
- Trace panel and safety indicator (mutual exclusion invariant)
- Preview canvas and PNG export
- Evidence export (state JSON + trace TXT + preview PNG)
- JSON import/export and scripted scenario loading (Token Ring + RA demos)
- **Ricart–Agrawala (RA)** network/message UI:
  - Message queue table
  - Message-fault controls (drop-next-send; drop-next-in-flight message)
  - Stalled-state explanation in trace (“waiting for REPLY from …”)

---

## Evidence

Add screenshots to `report/figures/` and reference them here.

Recommended set for v0.3.2 (curated report figures):
- `report/figures/fig_tr_token_lost.png` (Token Ring: token lost / recovery context)
- `report/figures/fig_tr_crash_recover.png` (Token Ring: crash/recover state)
- `report/figures/fig_ra_tiebreak.png` (RA: tie-break example, scripted)
- `report/figures/fig_ra_drop_inflight_stall.png` (RA: drop next in-flight message → stall)
- `report/figures/fig_ra_drop_next_send_stall.png` (RA: drop-next-send → stall)
- `report/figures/fig_ui_overview.png`

---

## Severity scale (Nielsen-style)

Use the following optional severity scale to prioritise changes:

- **0 — Cosmetic:** does not need fixing unless time permits
- **1 — Minor:** low priority; small improvement
- **2 — Moderate:** important; should be fixed
- **3 — Major:** high priority; significantly impacts usability
- **4 — Critical:** usability catastrophe; must fix

---

## Notes on evaluation method (limitations)

- Single evaluator and also the developer; potential bias.
- Focus is on **teaching clarity and demo repeatability**, not production-grade UX.
- If time permits, a short peer review (1–2 students) would strengthen validity.

---

## (1) Visibility of system status

**What to check:** Users can quickly see the current state and understand what just happened.

**Findings (v0.3.2):**
- Trace panel provides step-by-step events and warnings.
- Safety label indicates whether the mutual exclusion invariant holds.
- Preview canvas labels critical section owner; Token Ring also shows token holder.
- Mode pill indicates `interactive` vs `script`.

**Issues / actions:**
- **Issue (S1):** During stalls, the primary indicator is in the trace; some users may miss it.
  - **Action:** Consider a small “Status” line near controls that mirrors the latest stall reason (optional post-freeze improvement).

**Evidence:** (add screenshot references)

---

## (2) Match between system and the real world

**What to check:** Terminology and behaviours match mutual exclusion concepts used in lectures/books.

**Findings (v0.3.2):**
- Uses domain terms: token, critical section, request/release, crash/recover, message loss.
- RA fault demos naturally illustrate “safety vs liveness” trade-off.

**Issues / actions:**
- **Issue (S2):** New learners may not know the difference between Token Ring and RA at first glance.
  - **Action:** Add a short algorithm description panel (1 paragraph each) and “What to observe” bullets (optional post-freeze).

**Evidence:** (add screenshot references)

---

## (3) User control and freedom

**What to check:** Users can recover from mistakes and stop unwanted actions.

**Findings (v0.3.2):**
- Reset, Clear trace, and Exit script mode are available.
- Fault recovery actions exist (Regenerate token; Recover process).

**Issues / actions:**
- **Issue (S1):** Reset can discard state without confirmation.
  - **Action:** Optional confirmation dialog (“Reset simulation?”) or an “Are you sure?” for destructive actions (post-freeze).

**Evidence:** (add screenshot references)

---

## (4) Consistency and standards

**What to check:** Consistent naming, layout, and interaction patterns.

**Findings (v0.3.2):**
- Per-process action verbs are consistent (Request CS, Release CS, Crash, Recover).
- Controls are grouped logically (configuration, run controls, fault controls, export controls).

**Issues / actions:**
- **Issue (S1):** Terminology may vary between “CS” and “critical section” across UI/trace.
  - **Action:** Standardise wording across trace/UI and documentation.

**Evidence:** (add screenshot references)

---

## (5) Error prevention

**What to check:** Invalid operations are prevented rather than only reported.

**Findings (v0.3.2):**
- Buttons are disabled when actions do not apply (e.g., Release only when in CS; Recover only when crashed).
- RA “Drop next in-flight message” should be disabled when the queue is empty.
- Script mode limits manual interference (exit script mode returns to interactive).

**Issues / actions:**
- **Issue (S1):** Some edge cases may still allow confusing sequences (e.g., repeated exports without clearing trace).
  - **Action:** Optional small UI hints (e.g., toast “Exported evidence snapshot”)—post-freeze.

**Evidence:** (add screenshot references)

---

## (6) Recognition rather than recall

**What to check:** Users can read state directly; do not need to remember it.

**Findings (v0.3.2):**
- Process table shows per-process status and token presence (Token Ring).
- Message queue table shows message IDs/type/from/to/ts (RA).
- Preview provides a compact summary for quick demos.

**Issues / actions:**
- **Issue (S1):** Symbol meanings (token dot / request marker / crash cross / CS double circle) may require explanation.
  - **Action:** Add a small legend (3–5 lines) near the preview or below it.

**Evidence:** (add screenshot references)

---

## (7) Flexibility and efficiency of use

**What to check:** Efficient for repeated demos and fast iteration.

**Findings (v0.3.2):**
- Run mode supports automatic playback with adjustable speed.
- Scripted demos support repeatable scenarios (Token Ring crash demo; RA tie-break demo).

**Issues / actions:**
- **Issue (S1):** No keyboard shortcuts; step-through demos rely on mouse clicks.
  - **Action:** Optional shortcuts (Step/Run/Reset; Ctrl+Enter etc.) post-freeze.

**Evidence:** (add screenshot references)

---

## (8) Aesthetic and minimalist design

**What to check:** Interface is not cluttered; information is relevant.

**Findings (v0.3.2):**
- Clear grouping of panels: controls, tables, trace, preview.
- Trace is contained and scrollable.

**Issues / actions:**
- **Issue (S1):** With many controls (especially RA faults + exports), the control area can feel dense.
  - **Action:** Optional “Advanced” toggle for fault/export controls post-freeze.

**Evidence:** (add screenshot references)

---

## (9) Help users recognize, diagnose, and recover from errors

**What to check:** When things go wrong (fault injection), the UI explains what happened and what recovery is possible.

**Findings (v0.3.2):**
- Token Ring: token loss produces “no progress” trace lines and recovery via “Regenerate token”.
- RA: stall states can be explained with explicit trace messages (e.g., waiting for a missing REPLY due to message loss).

**Issues / actions:**
- **Issue (S1):** Recovery hints are primarily in the trace.
  - **Action:** Optional UI prompt banner (“No progress — token lost. Use Regenerate token.”) post-freeze.

**Evidence:** (add screenshot references)

---

## (10) Help and documentation

**What to check:** Minimal help is available without leaving the tool.

**Findings (v0.3.2):**
- README provides setup and usage instructions (outside the UI).

**Issues / actions:**
- **Issue (S2):** In-app help is minimal.
  - **Action:** Add a short “How to use” panel (3–6 bullets) and “What to observe” section (safety vs liveness) post-freeze.

**Evidence:** (add screenshot references)

---

## Summary of key actions (post-v0.3.2 improvements)

- Add a small legend for preview symbols (token, request, crash, CS).
- Add concise algorithm help text (Token Ring vs RA) and “what to observe” guidance.
- Optional: make stall reasons visible beyond the trace (status line / banner).
- Optional: keyboard shortcuts for Step/Run/Reset.
- Refresh evidence links in this checklist and keep curated figures in `report/figures/`.

