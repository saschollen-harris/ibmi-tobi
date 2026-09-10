# launch Library-List Setup

`launch` builds the job's library list before executing the compile
command:

- `preUsrlibl` entries are added with `liblist -af` **in reverse order**
  — each `-af` prepends to the front of the library list, so inserting
  the array backwards produces the intended final left-to-right order
- `postUsrlibl` entries are added with `liblist -al` (append) in their
  original order — no reversal needed since append preserves order
- Before running the actual command, `cl "CHGJOB LOG(4 00 *SECLVL)"` is
  set so the joblog captures full second-level error text — without
  this, `getJobLog`/`getJoblog_for_job` would only see truncated message
  text for failures

If you add another library-list source, check whether `liblist` prepends
or appends before deciding insertion order — reversing the wrong one
silently changes resolution order for `*LIBL`-qualified objects.
