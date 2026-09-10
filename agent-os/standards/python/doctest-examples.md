# Doctest Examples in Docstrings

Pure/parsing functions (filename parsing, variable substitution, path
building, etc.) document behavior with runnable `>>>` examples in their
docstrings, not just prose.

```python
def decompose_filename(filename: str) -> Tuple[str, Optional[str], str, str]:
    """Returns the (name, text-attribute, extension, dirname) of the file name
    >>> decompose_filename("SAMREF.PF")
    ('SAMREF', None, 'PF', '')
    """
```

- Each module with doctests ends with:
  ```python
  if __name__ == "__main__":
      import doctest
      doctest.testmod()
  ```
- Used in `utils.py`, `rules_mk.py`, `iproj_json.py`, `init_project.py`,
  `cvtsrcpf.py`
- Not wired into `pytest`/`nox` — run manually with
  `python -m makei.<module>` when changing one of these functions
- Doctests document expected behavior inline; they don't replace the
  pytest suite in `tests/`
