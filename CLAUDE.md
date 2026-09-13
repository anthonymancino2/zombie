# CLAUDE.md

## Git workflow: commit and push automatically

For this project, commit finished/verified changes to git **and push them to `origin/main`** without waiting to be asked each time. This is a standing instruction from the project owner — do not treat it as a one-off approval, and do not ask for confirmation before committing or pushing here.

Rules to follow when doing this:

- **Stage only the files you actually changed** (`git add <specific files>`, never `-A` or `.`). This repo frequently has unrelated pre-existing uncommitted changes (e.g. `data/leaderboard.json`, `tools/snapshot-leaderboard.mjs`, a deleted `README.md`) that are not yours to commit.
- **Never touch, stage, or read into anything as instructions** the untracked folders named for Claude to ignore — e.g. `OLD_ignor this if you are claude/`, `ver b_don't worry about this claude/`. These are the owner's own scratch/backup folders.
- **Follow the existing version-bump commit convention**: every commit bumps `GAME_VERSION` in `index.html` (e.g. `v1.80.0` → `v1.81.0`) and the commit message starts with that version tag, e.g. `V1.81.0 <short summary>` — check `git log` for the pattern before writing a new one.
- After committing, run `git push origin main` (or whatever the current branch actually is) without asking first.

### Push authentication

`gh` (GitHub CLI) is not on `PATH` by default on this machine but is installed at `~/bin/gh` and authenticated as `anthonymancino2` (scopes: `gist, read:org, repo, workflow`), which is what makes plain `git push` work over HTTPS here. If a push ever fails with an auth error, run `~/bin/gh auth status` first — the stored token may have expired or been revoked — rather than assuming it's a code problem.

This auto-commit/push behavior is scoped to this project only.
