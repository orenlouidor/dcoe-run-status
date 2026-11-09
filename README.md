# dcoe-scheduler

The sweep queue and the status page of the DCOE project's experiment sweeps. The scheduler
(`scripts/exec/scheduler.py` of the DCOE source repository) runs on the Athena login node, drives
Athena and IUCC, reads the queue from this repository every 10 seconds and pushes the status back
every 15 minutes.

**Status page:** https://orenlouidor.github.io/dcoe-scheduler/ (`index.html`; `STATUS.md` and
`status.json` hold the same tables).

## The queue: `sweeps.txt`

One sweep per line, in priority order (curations are submitted top to bottom). Edit it here on
GitHub, or clone, edit and push from anywhere; the scheduler acts within about ten seconds.

```
frames_v3_cv5_echo                  # the spec configs/experiments/frames_v3_cv5_echo.yaml, from the scheduler checkout's HEAD (main)
frames_v3_cv5_ctx_all @ feat/ctx    # the same, frozen from the branch (or tag, or sha) feat/ctx of the DCOE source repository
- frames_v3_cv5_top10               # cancel this sweep: its jobs are cancelled, its rows read Cancelled; remove the "-" to resume
```

- The ref after `@` must be pushed to `origin` of the DCOE source repository: both clusters fetch it from there.
  A sweep is frozen at the commit the ref names when it is first seen; the branch moving later does not move it.
- A cancellation names the sweep exactly; whatever follows an `@` on a `-` line is ignored.
- A finished sweep stays listed so its results stay on the page. `#` starts a comment.
- The queue is public: sweep names and refs only, never a path with a credential.

## Refresh now

A file named `refresh` (any content) pushed to this repository makes the scheduler run a cycle at once and
rewrite the page; the publish that answers it removes the file. The page's **Refresh now** button opens
GitHub's new-file form for it. The scheduler commits the status as `dcoe-scheduler`; queue edits and refresh
requests carry their author.
