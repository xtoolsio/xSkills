<img src="assets/xtools-logo.svg" alt="xTools" width="250">

# xSkills

Production-grade AI agent skills by [xTools](https://xtools.com). Compatible with Claude Code, Cursor, Windsurf, and any SKILL.md-compatible agent.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/xtoolsio/xSkills?style=social)](https://github.com/xtoolsio/xSkills/stargazers)
![Skills](https://img.shields.io/badge/Skills-1-brightgreen)
![Azure Resources](https://img.shields.io/badge/Azure_Resources-150%2B-0078D4)
![Agents](https://img.shields.io/badge/Agent-Claude_%7C_Cursor_%7C_Windsurf-blueviolet)
![IaC](https://img.shields.io/badge/IaC-Terraform_%7C_OpenTofu_%7C_Bicep_%7C_ARM_%7C_Pulumi-623CE4)

---

## Installation

```bash
npx skills add xtoolsio/xSkills
```

## Available Skills

| Skill | Description | Version |
|-------|-------------|---------|
| [azure-naming-convention](skills/azure-naming-convention/) | Azure resource naming convention based on Microsoft Cloud Adoption Framework (CAF). Covers 150+ resource types for Terraform, OpenTofu, Bicep, ARM templates, and Pulumi. | 0.1.0 |

## About

xSkills is a collection of specialized AI agent skills maintained by xTools. Each skill provides domain-specific knowledge, workflows, and reference material that AI agents can use to produce higher-quality output.

Skills follow the open [SKILL.md](https://github.com/anthropics/skills/blob/main/spec/agent-skills-spec.md) specification, making them compatible with any agent that supports the format.

## Contributing

Contributions are welcome. Each skill should:

1. Have a `SKILL.md` with valid YAML frontmatter (`name`, `description`)
2. Keep `SKILL.md` lean (under 2,000 words) with detailed content in `references/`
3. Include concrete examples and trigger phrases in the description
4. Follow imperative writing style

## License

MIT License - see [LICENSE](LICENSE) for details.
