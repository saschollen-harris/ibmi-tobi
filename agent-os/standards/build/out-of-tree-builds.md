# Out-of-Tree Builds via TOP_BUILD_DIR

By default, compiled objects for a `Rules.mk` directory land in
`OBJDIR` alongside its source (`OBJPATH = $(call real_to_build_dir,$(d))`).

- Setting `TOP_BUILD_DIR` (must be an absolute path) redirects **all**
  compiled objects for the whole tree outside the project directory
  instead — mirroring the source tree structure under that path
- This changes `dist_clean` behavior:
  - `TOP_BUILD_DIR` set: `skel.mk` defines a single top-level
    `dist_clean : rm -rf $(TOP_BUILD_DIR)` — no per-directory clean needed
  - `TOP_BUILD_DIR` unset: `footer.mk` wires `dist_clean` through each
    directory's `dist_clean_$(d)` target instead
- Don't assume `OBJDIR` is always relative to the source directory —
  code that resolves object paths should go through
  `real_to_build_dir`/`build_to_real_dir`, not string-join `$(d)` and
  `OBJDIR` directly
