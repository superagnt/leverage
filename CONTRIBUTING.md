# Contributing

Community skills live in `skills/community/<your-skill>/` — one directory per
skill, carrying at minimum a `SKILL.md` in the
[agentskills.io](https://agentskills.io) format (YAML frontmatter with `name`
matching the directory, a `description`, then the instructions). Supporting
`references/` and `scripts/` directories are welcome.

## The bar

- **It works cold.** A fresh agent with only your SKILL.md and (if needed) a
  `SUPERAGNT_API_KEY` must be able to complete the workflow. Say every
  prerequisite in the frontmatter (`required_environment_variables`) and in
  the body.
- **Scannable and honest.** Skills here are indexed and security-scanned by
  third-party registries; anything resembling hidden instructions, credential
  harvesting, or undeclared side effects is rejected outright.
- **No secrets, ever.** Not in examples, not in comments.
- **Real claims only.** If your skill's copy cites numbers, they come from a
  run you actually did.

## Process

1. Fork, add `skills/community/<your-skill>/`, open a PR.
2. CI runs format + scan checks; a maintainer reviews the instructions.
3. Merged contributions are credited in the next release's notes.

`skills/core/` and `skills/packaged/` are CI-generated from the superagnt
monorepo and owned by CODEOWNERS — please don't PR against them; open an
issue instead.
