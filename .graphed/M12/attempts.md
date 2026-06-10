# M12 attempts — graphed-numpy (dask.array parity P1: reductions, creation, random, monoids)

## Iteration 0 — TEST_AUTHORING/TEST_SANITY — 2026-06-10 (freeze-M12-1)

- frozen suite authored (reductions x axis x keepdims vs numpy; creation; random; monoids);
  verified NON-VACUOUS (124 failures against pre-M12 code). Suite text is UNCHANGED from the
  backed-out freeze-M12-0 authoring — it pins the numpy idiom, which now lives on NumpyArray per
  the M11 factorization (see .graphed/M11/attempts.md), so the re-freeze is a relabel only.

## Iteration 0 — IMPLEMENTING/REVIEW — 2026-06-10

- NumpyArray gains the numpy METHOD idiom (sum/prod/mean/std/var/min/max/any/all/argmin/argmax,
  cumsum/cumprod) over the base _reduction/_scan infrastructure, plus the full
  __array_function__ reduction/scan handler table (incl. nan-variants, amin/amax aliases).
- Backend: _REDUCERS/_SCANS tables shared by record-time inference (LENGTH-ONE unit metas;
  argmin/mean reject empty) and eval_stage; the M2 "sum requires numeric" pin generalized to all
  reducers (numeric-or-bool — numpy 2 string ufuncs would otherwise concatenate).
- creation.py: zeros/ones/full/empty/arange/linspace as deterministically NAMED sources (identical
  creations intern to one node); *_like as fusible ops; empty==zeros (determinism by design).
- random.py: GraphedRNG — sources seeded AND named by (seed, draw); same seed -> identical values
  and identical IR bytes. reductions.py: monoid(kind) quadruples (moment sums for mean/var/std);
  median refused (not tree-reducible).
- gates: frozen_tests 280/280 PASS · coverage 93% (>=90, branch) · ruff+format clean ·
  mypy --strict clean · determinism green · sphinx -W clean.
