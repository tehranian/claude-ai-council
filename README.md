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

The council is designed to be extensible. You can create your own specialized agents to bring any perspective to the table -- a security auditor, a data engineer, an accessibility expert, whatever your team needs.

### 1. Create the agent file

Add a new `.md` file in the `agents/` directory. The file has two parts: YAML frontmatter and the system prompt.

**Frontmatter fields:**

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Kebab-case identifier (e.g. `security-auditor`) |
| `description` | Yes | When Claude should use this agent, with examples |
| `model` | Yes | Model to use (`opus` recommended for best analysis) |
| `color` | No | Display color for the agent |

**System prompt structure:**

The body of the file is a system prompt that defines the agent's persona. Follow this pattern:

```markdown
---
name: security-auditor
description: "Use this agent when you need security analysis..."
model: opus
color: red
---

You are the Security Auditor, a council member whose role is to...

## Your Core Philosophy

[2-4 guiding principles that shape this agent's perspective]

## Your Analytical Framework

[Numbered list of what the agent examines when analyzing a proposal]

## How You Communicate

[Bullet points defining the agent's communication style]

## Your Voice

[A paragraph describing the agent's tone and personality]

## Output Structure

[Numbered sections the agent should include in every response]

## When the Council Gathers

[The agent's unique role during multi-agent deliberations]
```

### 2. Register the agent in SKILL.md

Add your agent to two places in `SKILL.md`:

**The agent selection prompt** (Step 1):
```yaml
- label: "Security Auditor"
  description: "Identifies vulnerabilities, threat models, and security risks"
```

**The bundled agents table:**
```markdown
| Security Auditor | `agents/security-auditor.md` | Vulnerability analysis, threat modeling |
```

### Tips for writing good agents

- **Give it a clear point of view.** The best agents have a strong perspective, not a generic one. "Calibrated skeptic" is better than "general analyst."
- **Define what it is AND what it isn't.** Explicitly stating what the agent does *not* do prevents it from drifting into generic advice.
- **Include an analytical framework.** A structured checklist of what to examine produces more consistent, thorough output than open-ended instructions.
- **Specify output structure.** Numbered sections ensure every analysis covers the same ground and is easy to compare across agents.
- **Write detailed examples in the description.** The `description` field in the frontmatter tells Claude *when* to use this agent. More examples = better automatic selection.

## License

MIT
