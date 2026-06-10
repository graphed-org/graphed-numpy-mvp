# M11 attempts — graphed-numpy (dask.array parity P0: shape-aware forms + full elementwise tier)

## Iteration 0 — TEST_AUTHORING/TEST_SANITY — 2026-06-10

- frozen suite authored at `tests/frozen/m11/` (README + 3 test files; ~115 parametrized cases);
  verified NON-VACUOUS against pre-M11 code (every new op raises `unsupported op` at record time;
  no `.shape` metadata).
- pre-freeze sanity fix (recorded): the reflected-scalar direction of `ldexp` is invalid in numpy
  itself (`np.ldexp(int, float-array)` raises UFuncTypeError), so that single parametrized
  direction is excluded — the suite must only pin behavior numpy actually has.
- freeze: freeze-M11-0.

## Iteration 0 — IMPLEMENTING/REVIEW — 2026-06-10

- `NumpyForm` gains `shape` (axis 0 = None) + `ndim`; `describe()` unchanged for records and 1-D
  vectors (M2 pin holds). `_UNARY`/`_BINARY` canonical-op -> numpy-callable tables shared by
  `op_form` (zero-length meta arrays: numpy's own promotion/broadcasting/type errors at record
  time) and `eval_stage` — inference and evaluation cannot drift. `from_array` records trailing
  dims; field/filter/sum/map behavior unchanged.
- gates: frozen_tests 154/154 PASS · coverage 95% (>=90, branch) · ruff+format clean ·
  mypy --strict clean · determinism green (frozen byte-equality test) · sphinx -W clean.
