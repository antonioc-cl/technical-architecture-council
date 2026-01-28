# Technical Architecture Council Agent Skill

Expert software architecture council with 6 advisors for code design, architecture patterns, infrastructure, and development practices.

## Advisors

- **Martin Fowler** - Patterns, refactoring, architecture
- **Kent Beck** - TDD, emergent design, simplicity
- **Uncle Bob** - Clean code, SOLID
- **Sam Newman** - Microservices
- **Kelsey Hightower** - Infrastructure, cloud native
- **Gene Kim** - DevOps, flow

## Installation

### npx skills add (recommended)
```bash
npx skills add antonioc-cl/technical-architecture-council
```

### Claude Code
```bash
/plugin add https://github.com/antonioc-cl/technical-architecture-council
```

### Claude.ai
Upload the `technical-architecture-council` folder or add via skills panel.

## Usage

Activate when you need help with:

- Software architecture decisions
- Code design and structure
- Refactoring strategies
- Infrastructure and deployment
- Development practices (TDD, CI/CD)
- Technical debt management

### Example Prompts

```
Use technical-architecture-council to help me design my API
Use technical-architecture-council to refactor my monolith
Use technical-architecture-council to decide on microservices
Use technical-architecture-council for TDD best practices
```

## Structure

```
technical-architecture-council/
├── SKILL.md              # Main skill instructions
├── LICENSE               # MIT License
├── README.md             # This file
└── references/           # Detailed advisor profiles
    ├── fowler.md
    ├── beck.md
    ├── uncle-bob.md
    ├── newman.md
    ├── hightower.md
    └── kim.md
```

## License

MIT License - See LICENSE file for details.
