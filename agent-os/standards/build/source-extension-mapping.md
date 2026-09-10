# Source-Extension → Object-Type Mapping

`src/makei/const.py` defines three dicts that map IBM i source file
extensions to build targets. They serve different lookup directions —
keep all three in sync when adding a new source type:

- `FILE_TARGET_MAPPING` — extension → target name(s), e.g.
  `"RPGLE": {"MODULE", "PGM"}`
- `FILE_TARGETGROUPS_MAPPING` — extension → target group(s)
- `TARGET_TARGETGROUPS_MAPPING` — target name → target group

Notes:

- `FILE_MAX_EXT_LENGTH` is derived automatically from
  `FILE_TARGET_MAPPING`'s keys (max dot-separated parts) — this is what
  lets multi-part extensions like `PGM.RPGLE` be recognized over `RPGLE`
  in `decompose_filename`. No manual update needed there.
- Adding a new extension to only one or two of the three dicts causes
  inconsistent behavior (e.g. build succeeds but target-group lookups
  fail) rather than an obvious error — update all three together.
