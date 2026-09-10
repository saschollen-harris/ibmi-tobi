# Non-Recursive Directory-Stack Build

The whole project tree is built in **one** `make` invocation — this is
the classic "recursive make considered harmful" avoidance pattern, not
per-directory sub-makes.

- `d` holds the directory currently being processed (its `Rules.mk`)
- `include_subdir_rules` (skel.mk) does the traversal:
  1. push `d` onto `dir_stack`, set `d` to the subdirectory
  2. `$(eval $(value HEADER))` — reset per-directory target vars
  3. `include <subdir>/.Rules.mk.build`
  4. `$(eval $(value FOOTER))` — wire up targets/clean/subdir recursion
     for that directory
  5. pop `d` back off `dir_stack`
- Never invoke `make` recursively per-directory, and never set `d`
  outside this push/pop macro — sibling directories rely on `d` being
  restored correctly when their `Rules.mk` is processed, and breaking
  that silently misattributes targets/objects to the wrong directory
