# Contributing

This is **UA Skills** (`appeeky/ua-skills`) — paid user acquisition playbooks for TikTok, Meta, Apple Search Ads, creatives, and ROAS.

## Add a Skill

```
skills/your-skill-name/
└── SKILL.md           # Required, under 500 lines
└── references/        # Optional
```

Every `SKILL.md` needs YAML frontmatter:

```yaml
---
name: your-skill-name
description: When the user wants to [action]. Also use when the user mentions "[trigger phrases]". For [related task], see [other-skill].
metadata:
  version: 1.0.0
---
```

**Rules:**
- `name` must match the directory name exactly
- Lowercase, hyphens, 1-64 chars
- Description must include trigger phrases and scope boundaries
- Cross-reference related skills and aso-skills where relevant
- Reference Appeeky MCP tools when the skill uses live data

## Validate

```bash
bash validate-skills.sh
```

## Commits

`feat(skill-name): ...` / `fix(skill-name): ...` / `docs: ...`
