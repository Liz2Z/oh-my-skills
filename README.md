# Oh My CC Skills

A curated collection of Claude Code skills for enhanced productivity and development workflows.

## About

This marketplace plugin provides useful skills that extend Claude Code's capabilities. Skills are specialized knowledge files that Claude can invoke to handle specific tasks more effectively.

## Installation

Add this repository to your Claude Code environment:

```bash
/plugin marketplace add Liz2Z/oh-my-skills
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

### 5W Explainer (5W 解释器)
Systematic explanation framework using the 5W structure (What, Who, When, Where, Why).
- Explains any concept, technology, product, or process
- Structured approach for comprehensive understanding
- Clear definitions and context
- User-centric explanations

**Trigger:** Use when user asks "what is X", "explain X", "help me understand X", or needs systematic explanation of any topic.

### Life OS
Personal knowledge management system for organizing thoughts and insights.
- Record thoughts and insights
- Track AI conversations
- Manage personal principles and methodologies
- Semantic search capabilities

**Trigger:** Use when user asks to record/search thoughts, conversations, principles, or update personal knowledge.

### Biz Explainer (业务梳理)
Business logic visualization using Mermaid diagrams for non-technical stakeholders.
- Deep analysis of business logic from code
- Mermaid diagram generation
- Comprehensive documentation creation
- Non-technical friendly explanations

**Trigger:** Use when user wants to analyze business processes, visualize workflows with Mermaid, or generate business documentation.

## Usage

Once installed, Claude Code will automatically invoke these skills when relevant to your task. You can also explicitly reference a skill by name:

```
"Use the git-wizard skill to help resolve this merge conflict"
"Let me use code-explainer to understand this function"
"Help me organize my tasks with todo-master"
"Use 5w-explainer to help me understand this concept"
"I want to record this thought using life-os"
"Analyze the business logic and create Mermaid diagrams using biz-explainer"
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

Created by @Liz2Z

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [Skill Creation Guide](https://github.com/anthropics/claude-code/tree/main/skills)
- [Marketplace Plugins](https://github.com/topics/claude-code-marketplace)
