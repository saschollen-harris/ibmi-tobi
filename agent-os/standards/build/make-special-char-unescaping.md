# Make-Side Special-Char Unescaping

Python escapes `#`/`$` to `HASHESCAPE_`/`DOLLARESCAPE_` before writing
names into generated Make files (see `special-char-escaping.md`).
`footer.mk` reverses this at the point a value is about to be used as a
real target/source name:

```make
define escape_specials
$(subst DOLLARESCAPE_,$$$$$$$,$(subst HASHESCAPE_,\#,$(1)))
endef
```

- `DOLLARESCAPE_` → `$$$$$$$` (7 dollar signs) — the repetition is
  required so a literal `$` survives multiple rounds of Make variable
  expansion before reaching the recipe
- `HASHESCAPE_` → `\#` — an escaped hash Make won't treat as a comment
- Applied via `escape_source`, and only to values under the current
  directory (`$(d)/%`) — object dependency names are left in their
  escaped form, since they're resolved elsewhere
- If you add a new place that emits a source/target name into a
  `Rules.mk`, run it through `escape_source`/`escape_specials` or `#`/`$`
  in that name will break the generated Makefile
