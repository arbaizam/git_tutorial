# Git Tutorial — Plan

Working doc for building an interactive git tutorial for our team.

## Decisions

| Area | Decision |
|---|---|
| Audience | Our team: strong in scientific computing; git experience ranges from none to basic commit/push (lead engineer excepted). |
| Format | Interactive simulator (safe, resettable) **+** a real sandbox repo in ADO for hands-on practice. Knowledge checks per scenario; no completion tracking. |
| Tech | Plain HTML/CSS/JS. No build step, no npm, no CDN (all libraries vendored). Must work by double-clicking `index.html` (`file://`), so no ES modules and no `fetch()`. |
| Remote | Azure DevOps Repos. Built on GitHub, then mirrored to ADO via a second remote (this doubles as an exercise). ADO org/project names stay out of this repo because it is **public**; use `{org}/{project}` placeholders. |
| Simulator layout | Three working panes (**Databricks Git folder / VSCode Source Control / terminal**), an **ADO Repos** screen for PRs, and a live commit graph showing local vs. remote. |
| Content | Scenarios live in a separate data file (`scenarios.js`), so anyone can add one via PR without touching the engine. |
| Examples | Repo `rules_engine`, data `mpa`, pipelines `rules_engine - staging release` / `rules_engine - prod release`. |

## Our environment (from answers)

- Windows laptops. VSCode: Source Control panel + terminal, mixed.
- Azure Databricks Git folders, authenticated with a PAT. One Git folder per person (never shared), so every person and every Git folder is its own clone.
- Notebooks are currently `.ipynb`; we want `.py` source format.
- Databricks Asset Bundles, deployed by ADO Pipelines. All prod code lives in `main`.
- Policy: no direct commits to `main`. Every change goes through a PR with specific required reviewers (the lead engineer and the plan author). PRs complete with **squash**.
- No ADO Boards linking. No code is shared between VSCode and Databricks today. Only convention: the default branch is `main`.

## Proposed scenarios (awaiting approval)

**0. Mental model**
- 0.1 Snapshots, commits, and the graph. Your clone vs. ADO; each Databricks Git folder is a separate clone.

**1. The daily loop**
- 1.1 Clone `rules_engine` into Databricks and into VSCode.
- 1.2 Branch, change, commit, push. Covers the staging area, which Databricks hides.
- 1.3 Open a PR in ADO. Required reviewers approve, then complete with squash and delete the branch.
- 1.4 After the squash: update local `main` and start a fresh branch. Shows why reusing a completed branch causes phantom conflicts.
- 1.5 Get a teammate's changes (fetch vs. pull) in Databricks and in VSCode.
- 1.6 A pull is refused because of uncommitted changes: commit, stash, or discard.
- 1.7 "I committed on local `main`": ADO rejects the push, so move the work onto a branch.

**2. Working together**
- 2.1 Merge conflict on the same notebook: how two clones create one, how to resolve it in VSCode, and why `.ipynb` conflicts are painful.
- 2.2 Reviewing a PR: read the diff, comment, then approve or wait for the author.
- 2.3 Move work between VSCode and Databricks: push from one, pull in the other.

**3. Fixing things**
- 3.1 A completed PR broke `rules_engine - prod release`: ADO's **Revert** button creates a revert PR, then the pipeline redeploys. Squash makes this one clean commit to revert.
- 3.2 Clean diffs: convert notebooks from `.ipynb` to `.py` source format and add a `.gitignore` for data and output files. Doubles as a real cleanup PR.

**4. Words you'll hear**
- 4.1 Rebase vs. merge vs. squash (diagrams).
- 4.2 Cherry-pick: copy one commit onto another branch.

**Appendix**
- PAT errors: what an expired or invalid token looks like and where to renew it.
- Windows notes: Git for Windows and line endings.

**Not covered:** committing secrets (only touched on in 3.2's `.gitignore`), hotfix while a feature is in flight. A PR blocked by policy is folded into 1.3.

## Open follow-ups

1. **Branch policies:** In ADO, go to Repos → Branches, hover over `main`, then ⋯ → Branch policies. Report which are on: minimum number of reviewers, build validation, limit merge types, comment resolution.
2. **Staging:** Does `rules_engine - staging release` also deploy from `main`? Does it run automatically when a PR completes, or is it started by hand?
3. **Public repo:** OK for `rules_engine` / `mpa` names to appear in this public repo? If not, either make the repo private or use stand-in names.
4. **Scenario list:** Approve, cut, or add.
