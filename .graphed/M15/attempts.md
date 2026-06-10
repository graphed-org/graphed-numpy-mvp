# M15 attempts — graphed-numpy (rectilinear parquet I/O, parity plan M15.3)

## Iteration 0 — TEST_AUTHORING/TEST_SANITY — 2026-06-10 (freeze-M15-0)

- frozen suite authored (tests/frozen/m15: 11 tests); NON-VACUOUS (collection fails on the
  missing graphed_numpy.io). This milestone AMENDS the R16.7 deferral by user decision
  (2026-06-10): parquet I/O alone enters the numpy MVP; the rest of P3.9/P4 stays Phase 2.

## Iteration 0/1 — IMPLEMENTING/REVIEW — 2026-06-10

- io.py specializes graphed.parquet, RECTILINEAR ONLY: schema columns must be fixed-width
  primitives (numeric/bool); jagged/nested columns refused at construction naming the column and
  pointing at graphed-awkward. from_parquet records a record source (schema -> NumpyForm
  fields); read_parquet_partition resolves blind partitions with column projection; to_parquet
  compiles once (R7.8), reads the M5 field-touch projection's columns (exact for flat data —
  no structure-only needs exist), writes single-column parts, rejects multi-source arrays and
  non-1-D outputs; disabled/enabled write consistency (R15.4); in-memory record AND flat sources
  write by steps.
- Iteration 1: flat in-memory sources were handed to the evaluator as a {"" : array} dict —
  the chunk for a FLAT source must be the array itself; fixed (the frozen 2-D-refusal test
  caught it).
- gates: frozen_tests 333/333 PASS · coverage 95% (>=90, branch) · ruff+format clean ·
  mypy --strict clean · determinism green (frozen IR byte-equality test) · sphinx -W clean.

## Iteration 2 — IMPLEMENTING — 2026-06-10 (CI finding)

- Remote CI failed on EVERY matrix cell while the local suite was green: pyarrow's
  ChunkedArray.to_numpy AND DataType.to_pandas_dtype route through pyarrow's pandas SHIM —
  pandas is not a dependency of this package, but the shared dev venv (a sibling repo's deps)
  masked it locally. Fixed pandas-free: column conversion via combine_chunks().to_numpy(), schema
  translation via an explicit arrow-name -> numpy-dtype map. tests/extra/m15/test_pandas_free.py
  re-runs the whole parquet path with pandas imports BLOCKED so the masking cannot recur.
