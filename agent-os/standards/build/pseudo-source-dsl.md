# Pseudo-Source DSL Syntax

`extractAndLaunch` parses a small custom line-based format for CL/QMQRY-
style pseudo-source files before dispatching each command to `launch`:

- Line ends with `+` → continuation; strip the trailing `+`, append the
  next line to the same command instead of running it yet
- Line starts with `!` → strip the `!`, run the command but suppress
  failure (don't abort the script if this one command fails)
- Line starts with `/*` → comment, skipped
- Blank line → skipped
- `&O` / `&N` in the assembled command → substituted with `objlib` /
  `objname` before the command runs

Any new pseudo-source consumer must implement all five rules — skipping
continuation or comment handling will misparse existing pseudo-source
files rather than fail loudly.
