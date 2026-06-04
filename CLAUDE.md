# CLAUDE.md — graphed-numpy

Defers to the root **`graphed-project/CLAUDE.md`**; the **project plan
(`graphed-project-plan-gated.md`) always wins.** This file distills **milestone M2**.

## What this repo is

`graphed-numpy`: a **trivial numpy backend** that proves the `graphed` backend seam. Operates on 1-D
numpy "bags": elementwise arithmetic, boolean `filter`, `sum` reduction, and opaque `map` callables.
**No HEP** — the real reference backend is `graphed-awkward` (M3).

> Guardrail: trivial seam-prover only. `external_payload` returns a descriptor for any wrapped
> opaque callable, flagged as a **preservation risk** (real content hashing is M9).

## M2 — implemented

- `NumpyForm` (dtype + kind) implementing the `graphed.Form` protocol.
- `NumpyBackend` implementing `graphed.Backend`: `op_form` (dtype promotion + ill-typed detection —
  arithmetic needs numeric, `filter` needs a boolean mask, `sum` needs numeric), `eval_stage`
  (numpy ops), `boundary_ops`, `project` (M5 stub), `external_payload`.
- `from_array(session, name, values)` helper to create a source `Array`.

## Dependencies

`graphed` (the frontend / Backend protocol) + `graphed-core` (the store, for `PayloadDescriptor`) +
`numpy`. In CI these install from their GitHub repos; locally use editable installs.

## Gates

`ruff` + `ruff format` · `mypy --strict` (numpy treated as untyped at the boundary) ·
`pytest tests/frozen/m2 --cov=graphed_numpy` (100%) · `sphinx -W`.

Status: see `.graphed/state.json`.
