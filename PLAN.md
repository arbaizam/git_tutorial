# Git Tutorial — Plan

Working doc for building an interactive git tutorial for our team. Decisions are settled; open questions need answers before scenarios are written.

## Decisions

| Area | Decision |
|---|---|
| Audience | Our team: strong in scientific computing, new to git and team development workflows. |
| Format | Interactive simulator (safe, resettable) **+** a real sandbox repo for hands-on practice in VSCode and Databricks. |
| Tech | Plain HTML/CSS/JS. No build step, no npm, no CDN — all libraries vendored. Must work by double-clicking `index.html` (`file://`), so no ES modules and no `fetch()`. |
| Remote | Azure DevOps Repos (matches production). Built on GitHub, then mirrored to ADO via a second remote (doubles as an exercise). |
| Simulator layout | Three working panes — **Databricks Git folder / VSCode Source Control / terminal** — plus an **ADO Repos** screen for PRs, and a live commit graph showing local vs. remote. |
| Content | Scenarios live in a separate data file (`scenarios.js`), so anyone can add one via PR without touching the engine. |
| Progress | Saved in each person's browser (`localStorage`). No central completion tracking unless Q18 says otherwise. |
| Setup chapter | ADO authentication (Entra ID / PAT in Databricks; Git Credential Manager in VSCode) and clone URLs (`dev.azure.com/{org}/{project}/_git/{repo}`). |

## Open questions

Reply by number. "Skip" or "don't know" is fine.

### Environment
1. Azure Databricks? Do Git folders authenticate to ADO with Entra ID or a PAT?
2. Git folders: one per person, or shared by several people?
3. Notebooks in the repo: `.py` source format or `.ipynb`?
4. Do you use Databricks Asset Bundles or ADO Pipelines? Which?
5. How does code reach prod? (Job points at a branch/tag, bundle deploy, manual copy, other)
6. VSCode: Source Control panel, terminal, or GitLens? Databricks extension or Databricks Connect?
7. Work laptops: Windows or Mac?

### Workflow
8. Branching today: commit straight to `main`, feature branches + PRs, or `dev`/`prod` branches? If nothing is settled, should the tutorial propose a model?
9. Which ADO branch policies are on? (minimum reviewers, build validation, linked work items, comment resolution)
10. PR completion merge type: merge, squash, rebase, or semi-linear?
11. Do you link ADO Boards work items to commits/PRs?
12. Is any Python code shared between VSCode and Databricks (e.g., a package imported in notebooks)? Same repo or separate?
13. Conventions for branch names, commit messages, or ticket IDs? Give one example of each if so.

### Scenarios
14. Mark the ones that happen to you, and add your own:
    - a. Two people edit the same notebook → merge conflict
    - b. Committed to `main` by accident
    - c. A commit broke a scheduled job and must be undone
    - d. Getting a teammate's changes into your Databricks Git folder
    - e. Pull fails because of uncommitted changes
    - f. Committed a secret, token, or data file
    - g. Notebook outputs or large CSVs make diffs unreadable
    - h. Urgent prod fix while a feature is half done
    - i. Start work in VSCode, finish in Databricks (or the reverse)
    - j. Reviewing and approving a teammate's PR
    - k. PR blocked by a branch policy
15. In one non-confidential line, what data and pipelines does the team work on? (Used to give examples realistic names.)

### Audience and scope
16. Range of git experience on the team? (none / basic commit-push / some branching)
17. Depth: essentials only (clone, branch, commit, pull, push, PR, conflicts), or also `revert`, `reset`, `stash`, `rebase`, `cherry-pick`?
18. Do you want quizzes or knowledge checks? Do you need to know who has completed it? (Tracking that changes the design.)

### Practice repo
19. Can you create a sandbox repo in ADO? Who can set branch policies on it?
20. Where in ADO should the tutorial be mirrored? (org/project/repo name)
