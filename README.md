# gaic Skills Directory

A small public directory of general-purpose [Claude Code](https://claude.com/claude-code)
skills. One skill is bundled directly here and installable as a plugin; the
rest are links to other maintainers' work, credited and licensed on their own
terms.

## Install this marketplace

```
claude plugin marketplace add likwidmack/gaic-skills-directory
claude plugin install single-gpu-profile-switching@gaic-skills-directory
```

## Bundled here

| Skill | What it's for |
| ----- | -------------- |
| [infra-gpu-single-profile-switching](skills/infra-gpu-single-profile-switching/SKILL.md) | Safely switch between GPU-exclusive services in a Docker Compose stack on a single-GPU host. The plugin install name is still `single-gpu-profile-switching`. |

## Directory: other worthwhile skill sources

These are **not** vendored into this repo — install them from their own
marketplaces.

### Engineering

- [obra/superpowers](https://github.com/obra/superpowers) (MIT) — TDD,
  systematic debugging, brainstorming, plan-writing/execution, git worktrees,
  subagent dispatch, code-review handoffs.
  `claude plugin marketplace add obra/superpowers`
- [mattpocock/skills](https://github.com/mattpocock/skills) (MIT) — spec/
  ticket workflows, TDD, domain modeling, code review, "grilling" interview-
  style planning skills.
  `claude plugin marketplace add mattpocock/skills`

### Documents, design, and API reference

- [anthropics/skills](https://github.com/anthropics/skills) (Apache-2.0) —
  Word/Excel/PowerPoint/PDF document skills, MCP server building, algorithmic
  art, frontend design, and the Claude API reference skill.
  `claude plugin marketplace add anthropics/skills`
- [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
  (MIT) — searchable UI style, color palette, font pairing, and UX guideline
  databases across 22 tech stacks.
  `claude plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill`

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) to propose a new bundled skill or a
new directory link.

## License

MIT — see [LICENSE](LICENSE). Linked repositories carry their own licenses;
check each before use.
