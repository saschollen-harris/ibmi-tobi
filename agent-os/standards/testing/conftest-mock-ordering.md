# conftest.py Mock Ordering

`tests/unit/local/conftest.py` installs a mock into `sys.modules` at
**module load time**, before any test module in that directory is
imported:

```python
sys.modules['ibm_db_dbi'] = MockIbmDbDbi()
```

- `makei.ibm_job` does `import ibm_db_dbi` at the top of the file — on
  a machine without the real IBM i DB driver, that import fails outright
  unless something has already put a mock in `sys.modules` first
- pytest loads a directory's `conftest.py` before collecting/importing
  test files in that same directory, so this ordering happens
  automatically — you don't need to import anything from `conftest.py`
  yourself
- A new test that imports `makei` modules touching `ibm_db_dbi` must
  live under `tests/unit/local/` (not a new top-level test directory) to
  pick up this mock for free — putting it elsewhere means re-deriving
  the mock or the import breaking on non-IBM i machines
