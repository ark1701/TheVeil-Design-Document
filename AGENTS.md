# Working on The Veil
Start with README.md, design/current.md and the current row in production/roadmap.md. Arlen makes gameplay decisions. Either assistant can implement or review a scoped task. Use one implementer per branch and have the next reviewer read the same commit.

1. Open one issue describing a player-visible result or a reproducible failure. Include the source rule, affected files, a pass condition and what requires a headset.
2. Create a short-lived branch for that issue. Make one change a learner can explain and roll back.
3. Record the editor/build/device actually tested. Distinguish confirmed design, provisional balance, proposed engineering choices and observed behavior.
4. Open a pull request with what changed, how to reproduce the check and what is still unverified. Link the issue.
5. End the session with the branch, commit, changed paths, result, blocker and one next action. Update the design only when Arlen has resolved a decision. Keep tool-specific conversations out of the rules.

For Arlen: an issue names a piece of work. A branch gives that work a separate copy of the project history. A pull request lets you inspect the changes before they enter main.
