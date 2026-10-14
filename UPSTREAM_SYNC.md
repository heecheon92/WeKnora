# Maintaining this fork

This fork keeps upstream updates separate from personal configuration:

- `main` mirrors [`Tencent/WeKnora`'s `main`](https://github.com/Tencent/WeKnora/tree/main). Do not commit customizations directly to it.
- `personal` contains this guide and an English `.env.example` with `DEFAULT_LOCALE=ko-KR` and `WEKNORA_LANGUAGE=ko-KR`. Use this branch for local work.
- `.env` stays local and untracked. Never commit credentials or overwrite your existing `.env` when syncing.

## Sync with upstream

Run these commands from the clone's root. Check `git status` first and commit or stash any unfinished work before switching branches.
The commands assume `origin` points to `https://github.com/heecheon92/WeKnora.git` and `upstream` points to `https://github.com/Tencent/WeKnora.git`; verify with `git remote -v`.

First, fast-forward the upstream mirror and push it to the fork:

```bash
git fetch upstream main &&
git switch main &&
git merge --ff-only upstream/main &&
git push origin main
```

After that succeeds, merge the updated mirror into the customization branch:

```bash
git switch personal &&
git merge main &&
git push origin personal
```

This is a manual sync process; no scheduled synchronization is configured. Merging into `personal` preserves published history, so routine updates do not require rebasing or force-pushing.

If `main` cannot fast-forward or a push is rejected, stop and inspect the branch history rather than resetting or force-pushing. If the merge into `personal` conflicts, resolve the affected files, preserving new upstream settings alongside the English descriptions and Korean defaults. Stage only the resolved files, run `git merge --continue`, and then `git push origin personal`. To cancel an in-progress merge instead, use `git merge --abort`.

After a successful sync, `git status` should show a clean working tree on `personal`. You can compare the mirror with the fetched upstream revision and inspect the remaining customizations:

```bash
git rev-parse main upstream/main
git diff --stat main...personal
```

The two revision hashes should match. These commands update source code only; they do not update running containers or migrate local `.env` settings.
