# OpenSpec for DIC3 Project Refactor

This directory contains the **OpenSpec specification scaffold and active changes** for DietSense.

## Complete installation & CLI Usage

OpenSpec upstream: https://github.com/Fission-AI/OpenSpec

Requires Node.js 22.12.0+ for npm installation:

```bash
npm install -g @fission-ai/openspec@latest
openspec init
openspec --version
```

## Planned workflow

1. Explore: `/opsx:explore`
2. Propose a change: `/opsx:propose <change-name>`
3. Review `proposal.md`, `design.md`, `specs/`, and `tasks.md`
4. Apply only after approval: `/opsx:apply`
5. Verify, then archive: `/opsx:archive`

## Repository policy

- Keep existing `myplan/` records unchanged as the historical ground truth.
- Treat DietSense capabilities as proposed targets (Expected Outcomes), not proven benefits.
- Avoid publishing user-specific health logs or secrets in a public repository.
- Require empirical evidence for accuracy, safety, and effectiveness claims.
