# AI Council - Claude Code Skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that summons a council of specialized AI advisors to deliberate on your ideas, proposals, or decisions from multiple perspectives.

## What It Does

The AI Council launches parallel sub-agents, each with a distinct analytical lens. Council members analyze independently, document their reasoning to a persistent file (`AI-COUNCIL.md`), and can optionally debate to surface the strongest arguments.

## Council Members

| Agent | Role |
|-------|------|
| **Optimist Strategist** | Explores best-case scenarios and maps success pathways |
| **Devil's Advocate** | Stress-tests assumptions and surfaces hidden risks |
| **Neutral Analyst** | Provides objective synthesis and trade-off analysis |
| **Technical Validator** | Assesses engineering feasibility and implementation reality |
| **Legal Domain Expert** | Analyzes legal risks, regulatory requirements, and compliance |
| **User Advocate** | Champions user needs, identifies friction, validates UX |
| **Growth Strategist** | Examines viral loops, acquisition channels, and scaling |

## Installation

### Option 1: Add as a skill directory

Clone this repo into your Claude Code skills directory:

```bash
git clone https://github.com/tehranian/claude-ai-council.git ~/.claude/skills/ai-council
```

### Option 2: Add to a project

Clone into your project and reference it in your `.claude/settings.json`:

```bash
cd your-project
git clone https://github.com/tehranian/claude-ai-council.git .claude/skills/ai-council
```

## Usage

In Claude Code, invoke the skill:

```
/ai-council
```

You'll be prompted to:
1. **Select council members** - Choose which perspectives you want
2. **Describe your proposal** - What idea or decision needs analysis
3. **Review results** - Each agent provides structured analysis
4. **Optional debate** - Have agents challenge each other's positions

Results are saved to `AI-COUNCIL.md` in your working directory.

## Common Patterns

| Pattern | Agents |
|---------|--------|
| **Full Council** | All 7 agents |
| **Technical Review** | Technical Validator + Devil's Advocate + Neutral Analyst |
| **Strategic Review** | Optimist Strategist + Growth Strategist + Devil's Advocate |
| **Quick Feasibility** | Technical Validator + Neutral Analyst |

## Adding Custom Agents

Create a new `.md` file in the `agents/` directory following the existing agent format. Each agent file needs:

- YAML frontmatter with `name`, `description`, `model`, and `color`
- A system prompt defining the agent's persona and analytical framework
- An output structure section

Then update `SKILL.md` to include the new agent in the selection options and agent table.

## License

MIT
