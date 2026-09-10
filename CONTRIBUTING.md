# Contributing

## Adding a directory link

Open a PR adding a row to the relevant category table in `README.md`
(or a new category if none fits). Include: repo link, license, a one-line
description, and the exact `claude plugin marketplace add` command. Do not
vendor the linked repo's code into this repo.

## Proposing a new bundled skill

Bundled skills in this repo must be general-purpose — not specific to any
one project or company's internal tooling. Open a PR with:

1. `skills/<skill-name>/SKILL.md` following the existing skill's structure
   (frontmatter `name` + `description`, then the guidance body).
2. A new entry in `.claude-plugin/marketplace.json`'s `plugins` array and a
   matching `.claude-plugin/plugin.json` if the skill ships as its own
   plugin.
3. A row in `README.md`'s "Bundled here" table.

Skills that only make sense inside a specific proprietary codebase belong in
that codebase's own `.agents/` or `.claude/skills/` directory, not here.
