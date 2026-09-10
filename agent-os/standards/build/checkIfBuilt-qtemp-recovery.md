# checkIfBuilt QTEMP Recovery

Build success is verified by checking whether the object actually landed
at `/QSYS.LIB/<LIB>.LIB/<OBJ>` — not by trusting the compile command's
exit code alone. IBM i compile commands can succeed but resolve the
output library differently than expected via the library list.

- Object exists in the target library → build succeeded; if a stray
  copy also exists in `QTEMP`, delete it (cleanup)
- Object missing from the target library but present in `QTEMP` →
  the compile put it in the wrong place; move it into the target
  library so the build result matches what was requested
- Object missing from both → genuine build failure
