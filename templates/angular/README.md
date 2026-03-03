# Angular Template

A ready-to-use `.agents` folder for **Angular** projects, including all Angular skills, shared skills, all rules, and the full agent library.

## Included

- **Agents** — full set (committer, implementer, linter, planner, release-notes-writer, researcher, reviewer, unit-tester)
- **Rules** — code-style, file-moves
- **Skills** — angular (all), code-review, commit-and-pr, css, orchestrate-dev-team, rxjs, typescript

## Install

Run the following command from your **project root** to install this template into your `.agents` folder without overwriting any files you already have:

```bash
git clone https://github.com/rensjaspers/agents.git /tmp/agents-library && \
  rsync -av --copy-links --ignore-existing /tmp/agents-library/templates/angular/ .agents/ && \
  rm -rf /tmp/agents-library
```

> **Note:** `--ignore-existing` ensures that any skills, rules, or agents you have already customised are not overwritten.
