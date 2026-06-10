# M11 attempts — graphed-numpy (dask.array parity P0: shape-aware forms + full elementwise tier)

## Iteration 0 — TEST_AUTHORING/TEST_SANITY — 2026-06-10 (freeze-M11-0)

- frozen suite authored (~115 parametrized cases); verified NON-VACUOUS against pre-M11 code;
  pre-freeze sanity fix (recorded): the reflected-scalar direction of `ldexp` is invalid in numpy
  itself, excluded. Implemented and locally green.

## DESIGN REVIEW — human-directed re-factorization — 2026-06-10 (freeze-M11-1 supersedes)

- The numpy calling idiom moves OFF the shared `graphed.Array` onto `graphed_numpy.NumpyArray`
  (`array.py`), supplied to Sessions via `NumpyBackend.array_type` (new M11 frontend hook).
  Forms split into `forms.py`. Original commits preserved on `backup/m11-m12-numpy-flavored`
  (nothing pushed). Suite re-frozen with `test_numpy_array_idiom.py` pinning that every builder
  returns the proxy and that no idiomatic member leaks onto the base class.

## Iteration 0 — IMPLEMENTING/REVIEW — 2026-06-10

- `NumpyForm` (forms.py) gains `shape` (axis 0 = None) + `ndim`; `describe()` unchanged for
  records and 1-D vectors (M2 pin holds). `_UNARY`/`_BINARY` op->callable tables shared by
  `op_form` (zero-length meta arrays: numpy's own promotion/broadcasting/type errors at record
  time) and `eval_stage` — inference and evaluation cannot drift.
- gates: frozen_tests 156/156 PASS · coverage 92% (>=90, branch) · ruff+format clean ·
  mypy --strict clean · determinism green (frozen byte-equality test) · sphinx -W clean.
