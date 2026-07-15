# AI Agent Skills

A curated repository of reusable **agent skills** that can be shared across different AI agents and providers. It also includes examples of global directives used to better suit agent and model experience and guide as efficiently and precisely as possible the agent in a general/agnostic way

## Overview

This project is intended to:

- centralize reusable skill definitions
- make skills easier to discover and maintain
- provide a consistent structure for creating and documenting new skills
- enable cross-agent portability
- bring real examples of how to use them properly

## Repository Structure

```text
.
├── README.md
├── global-directives/
    └── codex/
    └── ...
└── skills/
    └── example-skill/
        └── SKILL.MD
```

## Getting Started

### 1) Clone the repository

```bash
git clone https://github.com/gabrielrovesti/ai-agent-skills.git
cd ai-agent-skills
```

### 2) Browse existing skills

See the [`skills/`](./skills) directory for available skills and their documentation.

### 3) Add a new skill

1. Create a dedicated folder under `skills/`
2. Add a clear `README.md` for the skill
3. Describe:
   - purpose
   - expected inputs
   - expected outputs
   - examples
   - limitations
4. Keep naming and formatting consistent with existing skills

## Skill Documentation Guidelines

Each skill should include:

- **Name**
- **Purpose**
- **When to use it**
- **Inputs**
- **Outputs**
- **Usage examples**
- **Edge cases / limitations**
- **Dependencies (if any)**

## Quality Standards

When contributing, aim for:

- clear and concise language
- deterministic behavior where possible
- explicit assumptions and constraints
- portable usage across providers
- safe defaults and security-aware behavior

## Roadmap (Initial)

- [ ] Add first production-ready skill set
- [ ] Add examples for multiple providers
- [ ] Add validation checklist for skill quality
- [ ] Add CI checks as repository grows

## License

No license has been defined yet.

If you plan to open-source usage broadly, add a repository license in a follow-up update.
