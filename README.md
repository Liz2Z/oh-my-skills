# Oh My CC Skills

A curated collection of Claude Code skills for enhanced productivity and development workflows.

## About

This marketplace plugin provides useful skills that extend Claude Code's capabilities. Skills are specialized knowledge files that Claude can invoke to handle specific tasks more effectively.

## Installation

Add this repository to your Claude Code environment:

```bash
/plugin marketplace add zan/oh-my-cc-skills
```

## Included Skills

### Git Wizard
Advanced Git operations specialist for complex version control workflows.
- Complex merge/rebase operations
- Conflict resolution
- History manipulation
- Git recovery techniques

**Trigger:** Use when working with Git operations, branching, merging, or conflict resolution.

### Code Explainer
Expert at breaking down complex code into clear, understandable explanations.
- Code walkthroughs
- Architecture explanations
- Pattern identification
- Algorithm analysis

**Trigger:** Use when needing to explain code, understand implementations, or document logic.

### TODO Master
Task management and productivity specialist for organizing work effectively.
- Task creation and breakdown
- Prioritization frameworks
- Progress tracking
- Workflow organization

**Trigger:** Use when managing tasks, planning work, or organizing TODOs.

## Usage

Once installed, Claude Code will automatically invoke these skills when relevant to your task. You can also explicitly reference a skill by name:

```
"Use the git-wizard skill to help resolve this merge conflict"
"Let me use code-explainer to understand this function"
"Help me organize my tasks with todo-master"
```

## Development

### Adding New Skills

1. Create a new directory in `skills/<skill-name>/`
2. Add a `SKILL.md` file with YAML frontmatter:

```yaml
---
name: skill-name
description: When to trigger this skill
license: MIT
---

# Skill Title

Your skill content here...
```

3. The `description` field is crucial - it's used to auto-trigger your skill

### Skill Structure

```
skills/
└── your-skill/
    ├── SKILL.md          # Required: skill definition
    └── references/       # Optional: supporting files
        └── example.md
```

## Contributing

Contributions welcome! Feel free to:
- Add new skills
- Improve existing skills
- Fix bugs or issues
- Suggest enhancements

## License

MIT License - See [LICENSE](LICENSE) file for details.

## Author

Created by @zan

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [Skill Creation Guide](https://github.com/anthropics/claude-code/tree/main/skills)
- [Marketplace Plugins](https://github.com/topics/claude-code-marketplace)
