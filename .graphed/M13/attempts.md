# M13 attempts — graphed-numpy (dask.array parity P2: manipulation + indexing)

## Iteration 0 — TEST_AUTHORING/TEST_SANITY — 2026-06-10 (freeze-M13-0)

- frozen suite authored (indexing, manipulation, combination/aggregates — 30 tests, all compared
  against numpy directly); verified NON-VACUOUS (28/30 fail against pre-M13 code).

## Iteration 0/1 — IMPLEMENTING/REVIEW — 2026-06-10

- NumpyArray: tuple subscripts (`a[:, inner...]`, injective spec param, partition-local fusible;
  indexing axis 0 inside a tuple refused), manipulation methods (reshape/ravel/squeeze/transpose/
  T/swapaxes/astype/clip/round/take), and ~24 new __array_function__ handlers (where with scalar
  branches, concatenate/hstack, diff/isin/searchsorted, unique/bincount, histogram/2d/dd keeping
  numpy's (counts, record-time-concrete edges) contract, stack/vstack refused).
- Backend: `_manip_eval` shared by record-time inference (zero-length metas; unit meta for
  `index`) and eval_stage; `_check_manip_geometry` pins the axis-0 rules (reshape leading -1,
  no transpose/swapaxes/expand_dims/squeeze of axis 0); `take` gets an explicit form rule (the
  gathered extent is the partitioned index length — None at the gathered axis).
- Boundary decisions: slice/index (base Array), take(axis None/0), concatenate(axis 0), and the
  whole-axis analytics record reduction nodes; inner-axis variants stay fusible.
- Iteration 1 (recorded in the graphed repo too): int __getitem__ made Array infinitely iterable
  via the legacy protocol — np.concatenate(a) looped forever; fixed with an explicit __iter__
  TypeError + regression test. Handler argument-error branches covered by tests/extra/m13.
- gates: frozen_tests 315/315 PASS · coverage 95% (>=90, branch) · ruff+format clean ·
  mypy --strict clean · determinism green (frozen byte-equality test) · sphinx -W clean.
