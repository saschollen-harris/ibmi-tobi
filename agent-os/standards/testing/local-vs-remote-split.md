# local vs remote Test Split

`nox -s test` (the CI/standard session) only runs `tests/unit/local`.
`tests/unit/remote` is never exercised by CI.

- `tests/unit/remote` tests import `makei.ibm_job`, which does
  module-level `import ibm_db_dbi` and `import fcntl` — both require a
  real IBM i/Unix environment just to **collect** the test file
  (`fcntl` isn't available on Windows; `ibm_db_dbi` isn't installed off
  IBM i). There's no CI runner for this, so these tests only run when
  manually invoked on such a system.
- `tests/unit/local/conftest.py` mocks `ibm_db_dbi` into `sys.modules`
  so local tests can still exercise most logic without the real driver
  — see `conftest-mock-ordering.md`.
- When adding tests for `ibm_job.py`-dependent code: if the mock in
  `conftest.py` is enough, put the test in `unit/local` so it's actually
  covered by CI. Only put it in `unit/remote` if it truly needs the real
  driver/`fcntl` behavior — and know that CI will not catch regressions
  there.
