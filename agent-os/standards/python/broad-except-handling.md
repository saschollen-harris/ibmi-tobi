# Broad-Except Error Handling

Code that shells out, hits the DB, or runs a CL command wraps the call in
`except Exception:` (with `# pylint: disable=broad-except`) and always
prints a `[FAILED] ...`-style message before deciding how to proceed.
Never let a bare exception surface without that message — a CLI user
needs to see what failed, even if the exception then propagates.

Behavior after the message varies by call site — match the existing
pattern at the layer you're touching, don't invent a new one:

- CLI-facing helpers with no caller to hand the error to: print then
  `sys.exit(1)` (e.g. `utils.parse_variable`)
- Validation helpers: catch and return `False` (e.g. `utils.validate_ccsid`)
- DB/CL wrappers used inside a larger flow: print then re-raise, unless
  an explicit `ignore_errors` flag says to swallow it
  (e.g. `IBMJob.run_cl`, `IBMJob.run_sql`)
