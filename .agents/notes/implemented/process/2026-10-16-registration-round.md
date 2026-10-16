# Agent Note: The 2026-10-16 registration round

Status: implemented

## Problem

Eleven index pull requests were open at once, all appending one object to the
`community.json` array. Every one of them needs the mandatory usefulness,
stability and compatibility assessment, and because they all touch the same
array tail, merging any single one invalidates the rest. The round also had to
separate entries that are simply awaiting the required check from entries that
have not yet earned it.

## Decision

**#37 merged as `b37cc4ce`** (dsh-workspace-activity) and **#28 merged as
`2e2800c0`** (dsh-breadboard). Both cleared all three properties on evidence: the
upstream repositories are public, MIT or BSD-3-Clause, unarchived, and their
published artifacts match their `main` byte for byte; `#37`'s slot geometry,
colour tokens and row anchors were checked against the installed host, and its
install was reproduced under an isolated `DSH_HOME` (`dsh plugin --profile web add`
exits 0 and the bundle lands in the profile); `#28`'s install was reproduced in a
profile-shaped pnpm workspace (exit 0, no `--allow-build` needed), which also
retires the earlier `allowBuilds` objection. Neither duplicates an existing
entry: #37 is the only workspace-level running indicator, #28 the only hardware
simulation surface.

**Three more were reviewed as acceptable and are blocked only on the array
tail**: #39 (dsh-streamfold), #33 (dsh-vitals) and #32 (dsh-restart-anywhere). Each
got an approving review that names what is still required from the author - a
branch update - rather than a re-review. #32 additionally records that the restart
path swaps the process image on a port-close signal rather than on teardown
completion, which can drop still-queued session records, and that the execve path
has no rollback.

**Four were held on evidence**: #38 (smart-compaction) must show an end-to-end run
in a standard preset, because the mount script it asks the user to run writes to
`$DSH_HOME/.agent-presets/`, which is empty on a standard install while the host
disables its own compaction rows in favour of per-preset isolated ones; #36
(dsh-version-guard) claims upstream tests that do not exist in its repository and
keeps a disable mechanism that does not recognise the official plugin manager's
`disabled: true` marker; #27 (cute-fat-fish-pet) still owes host-load evidence for a
browser half outside the desktop shell; and #18 (dsh-media-dock) must clear two
hard-coded Windows paths that already shipped in 0.1.1. #34 (the Trae2api rename)
waits on a description correction that the 2026-10-13 comment already requested.
#26 (dsh-clear-usage) conflicts and awaits that same push.

**All of these stay in this catalog.** #27 was checked specifically: its upstream
has no `pet.json` and no `petManifestVersion`, so it is a cordis plugin and not a
pet asset pack, and `docs/plugins.md` plus `market-inputs.lock.json` put only
`pet.json` v2 packs on the pet path.

## Alternatives considered

**Merge several entries in one pass.** GitHub refuses: they append at the same
array tail, so after the first merge the rest report `CONFLICTING`, and
`update-branch` returns 422 rather than merging the base in. Merging is one entry
per round unless an author rebases.

**Hold the whole batch until every author rebases.** Two entries were mergeable
and fully assessed; withholding them would delay content that had already earned
its place for the sake of the others.

**Land the reviewed entries with a maintainer commit on `main` and close the
pull requests.** It would clear the backlog in one commit, at the cost of the
contributors' authorship and of the review trail the catalog depends on.

## Consequences

The catalog grew from 137 to 139 entries and the Workshop picked both up. The
tail-append contention is now a standing property of this catalog rather than an
accident of one round: contributors have to merge `main` before their entry can
land, and the required check has to be re-approved after each such push because
the pull requests come from forks. Reviews that accept an entry but demand a
rebase say so explicitly, so the next round can tell "needs a rebase" apart from
"needs a decision".
