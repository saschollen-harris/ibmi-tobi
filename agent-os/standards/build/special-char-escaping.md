# Special-Char Escaping (# and $)

IBM i library/object names can legally contain `#` and `$`, but Make treats
`#` as a comment start and `$` as a variable reference. Any name flowing
into a generated Makefile or build-vars file must be escaped first.

- Escape before writing to Make: `#` → `HASHESCAPE_`, `$` → `DOLLARESCAPE_`
  (see `_escape_special_chars_internal` / `escape_special_chars` in
  `build.py` / `utils.py`)
- Unescape before printing to the user or comparing against real names
  (see `_unescape_special_chars`)
- Never pass a raw name containing `#`/`$` into a Make command, target
  list, or `.Rules.mk.build` var file
