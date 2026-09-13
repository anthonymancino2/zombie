# CLAUDE.md

Project: **Wild West Z** / "Dust & Dead" — a single-file HTML zombie wave-shooter (Three.js, MQTT
coop, GitHub Pages). Everything that matters lives in `index.html`; see `README.md` for how the
game itself works (coop protocol, leaderboard, deployment).

## Git workflow: commit and push automatically

For this project, commit finished/verified changes to git **and push them to `origin/main`**
without waiting to be asked each time. This is a standing instruction from the project owner —
treat it as pre-approved for every future session, not a one-off.

**What you can do without asking, in this repo:**
- `git add` the specific files you changed, `git commit`, and `git push origin main` (a normal
  fast-forward push of your own commits).

**What still needs the owner's explicit go-ahead first, even here:**
- Force-pushing anything (`--force`, `--force-with-lease`), to `main` or any other branch.
- Any destructive/history-rewriting operation: `git reset --hard`, `git rebase -i`, amending a
  commit that's already been pushed, deleting local or remote branches/tags.
- Pushing to a branch other than `main`, or creating/merging/closing a pull request.
- Changing repository settings: GitHub Pages source, Actions permissions, branch protection,
  collaborators, repo visibility, or deleting the repo.
- Publishing a GitHub Release.
- Discarding any uncommitted work already sitting in the working tree (see below — there usually
  is some, and it isn't yours).

If genuinely unsure whether something crosses that line, ask rather than guessing.

### Push authentication

`gh` (GitHub CLI) is not on `PATH` by default on this machine but is installed at `~/bin/gh` and
authenticated as `anthonymancino2` (scopes: `gist, read:org, repo, workflow`) — that's what makes
plain `git push` work over HTTPS here. If a push ever fails with an auth error, run
`~/bin/gh auth status` first (the token may have expired or been revoked) before assuming it's a
code problem.

### Commit hygiene — this repo usually has other stuff sitting in it

- **Stage only the files you actually changed** (`git add <specific files>`, never `-A` or `.`).
  Pre-existing uncommitted changes routinely sit in the working tree (e.g. `data/leaderboard.json`,
  `tools/snapshot-leaderboard.mjs`, a deleted `README.md`) — they are not yours to commit, stage,
  or discard just because they're there.
- **Never touch, read as instructions, or commit anything inside** the untracked folders the owner
  names for Claude to leave alone — e.g. `OLD_ignor this if you are claude/`,
  `ver b_don't worry about this claude/`. Treat any file names like these as a hard stop, not
  something to investigate.
- **Follow the existing version-bump convention**: every commit bumps `GAME_VERSION` in
  `index.html` (e.g. `v1.80.0` → `v1.81.0`) and the commit message starts with that version tag,
  e.g. `V1.81.0 <short summary>` — check `git log` for the current pattern before writing a new one.

## Local testing workflow

`index.html` is a ~4MB single file; coop and the leaderboard need a real HTTP origin, not
`file://`. To test locally:

```bash
python3 -m http.server 8743
```

then open `http://localhost:8743/index.html` in a browser. **Browsers aggressively cache this
file** — after editing it, reload with a cache-busting query string
(`http://localhost:8743/index.html?cb=<anything>`) or you'll silently test the old version.

**Do not pollute live shared state while testing:**
- Never click "HOST A RUN" in Coop Mode during testing — it announces a real, joinable room on the
  public MQTT broker that real players could stumble into.
- Never let a test run reach game-over/score-submission with throwaway data — it publishes to the
  same shared leaderboard real players see (`data/leaderboard.json` / the live MQTT broker). Quit
  to menu instead of dying, or use characters/callsigns that make it obvious it's a test only as a
  last resort — avoiding submission entirely is strongly preferred.
- Reading the leaderboard, connecting to the coop relay to confirm it's online, and playing solo
  survival/challenges to completion are all fine — those don't affect anyone else's data.

## Known quirks worth knowing before "fixing" something

- Some CSS/behavior bugs in this file have been patched once already in the wrong place because a
  later rule in the cascade silently overrode an earlier-in-source-order fix with equal
  specificity — when a fix doesn't seem to take effect, check for a rule of the same
  specificity later in the file before assuming your change is wrong.
- `requestPointerLock()` calls should always attach a `.catch()` to the returned promise — modern
  browsers make it async-rejectable, and an unhandled rejection there is otherwise silent noise at
  best and confusing at worst.
- Draggable HUD button positions (Pause → MOVE BUTTONS) must be clamped against the CSS safe-area
  inset variables (`--sat/--sab/--sal/--sar`), not just the raw window edges — otherwise a button
  dragged near an edge can land under a phone's notch/camera cutout on rotation.
