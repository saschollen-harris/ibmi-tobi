# Shell-Side Special-Char Unescaping

The `#`/`$` escaping chain (see `special-char-escaping.md` and
`make-special-char-unescaping.md`) has a third leg: shell scripts invoked
during the build must also unescape before using a name.

`extractAndLaunch` does this on its own arguments before use:

```bash
pseudoSrcFile="${pseudoSrcFile//\\#/#}"
pseudoSrcFile="${pseudoSrcFile//DOLLARESCAPE_/$}"
objname="${objname//DOLLARESCAPE_/$}"
```

- The escaped forms (`\#`, `DOLLARESCAPE_`) only exist to survive the
  Make layer — once a value reaches a shell script as an argument, it's
  no longer going through Make expansion, so it must be converted back
  to the literal character before being used in a path, `sed`, or command
- Any new script invoked from the Makefile that receives a source
  filename or object name as an argument needs this same unescaping
  before using that value
