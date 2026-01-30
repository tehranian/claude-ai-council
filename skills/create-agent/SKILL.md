---
name: create-agent
description: Use when the user wants to create a new AI Council agent from a natural language description. Generates a complete agent .md file with YAML frontmatter and system prompt, following the established pattern of existing council agents.
user-invocable: true
---

# Create Agent

Generate a new AI Council agent from a natural language description. The agent is written to the `agents/` directory and is immediately available to `/ai-council`.

## Invocation Protocol

When this skill is invoked:

### Step 1: Gather the Agent Concept

If the user has not already provided a description, ask:

```
AskUserQuestion:
  question: "Describe the agent you want to create. What perspective, expertise, or analytical lens should it bring to the council?"
  header: "Agent"
  options:
    - label: "Security focus"
      description: "e.g., security auditor, threat modeler, privacy advocate"
    - label: "Business focus"
      description: "e.g., financial analyst, market researcher, sales strategist"
    - label: "Technical focus"
      description: "e.g., data engineer, DevOps specialist, performance optimizer"
    - label: "People focus"
      description: "e.g., accessibility expert, team dynamics coach, customer researcher"
```

The user can pick a category or provide a free-form description via "Other".

### Step 2: Validate the Concept

Before generating, ensure you have enough to work with. A good agent concept needs:

- **A clear point of view** — not a generic role, but a specific analytical lens
- **A distinct perspective** — something that doesn't duplicate an existing agent

If the description is too vague (e.g., "a smart agent" or "an analyst"), ask a clarifying question:

```
AskUserQuestion:
  question: "Can you be more specific? What unique perspective should this agent bring that the existing council members don't cover?"
  header: "Clarify"
  options:
    - label: "Focus on risks"
      description: "What specific domain of risk?"
    - label: "Focus on opportunities"
      description: "What market or technical area?"
    - label: "Focus on people"
      description: "Which stakeholders or user groups?"
    - label: "Focus on process"
      description: "What workflow or methodology?"
```

### Step 3: Generate the Agent File

Create a file at `agents/[kebab-case-name].md` following this exact structure:

#### Frontmatter

```yaml
---
name: [kebab-case-name]
description: "Use this agent when you need [specific perspective]. [When it's most valuable]. [What distinguishes it from other agents].\n\nExamples:\n\n<example>\nContext: [Scenario where this agent is useful]\nuser: \"[User message]\"\nassistant: \"[How Claude would invoke this agent]\"\n<Task tool call to [kebab-case-name]>\n</example>\n\n<example>\nContext: [Another scenario]\nuser: \"[User message]\"\nassistant: \"[How Claude would invoke this agent]\"\n<Task tool call to [kebab-case-name]>\n</example>\n\n<example>\nContext: [Third scenario]\nuser: \"[User message]\"\nassistant: \"[How Claude would invoke this agent]\"\n<Task tool call to [kebab-case-name]>\n</example>\n\n<example>\nContext: [Fourth scenario]\nuser: \"[User message]\"\nassistant: \"[How Claude would invoke this agent]\"\n<Task tool call to [kebab-case-name]>\n</example>"
model: opus
color: [selected color]
---
```

#### System Prompt Body

```markdown
You are the [Agent Title], a council member whose role is to [one-sentence mission statement].

[A paragraph elaborating on what this agent is and is NOT — defining its boundaries clearly.]

## Your Core Philosophy

**[Principle 1 title].** [Explanation of this guiding principle.]

**[Principle 2 title].** [Explanation.]

**[Principle 3 title].** [Explanation.]

## Your Analytical Framework

When presented with an idea or situation, you will:

1. **[First lens]**: [What you examine and why]

2. **[Second lens]**: [What you examine and why]

3. **[Third lens]**: [What you examine and why]

4. **[Fourth lens]**: [What you examine and why]

5. **[Fifth lens]**: [What you examine and why]

## How You Communicate

- [Communication style point 1]
- [Communication style point 2]
- [Communication style point 3]
- [Communication style point 4]
- [Communication style point 5]
- [How you interact with other council members]

## Your Voice

[A paragraph describing the agent's tone, personality, and what makes their communication distinctive. This should feel like a character description that captures how the agent thinks and speaks.]

## Output Structure

For each analysis, structure your response as:

1. **[Section 1 name]** ([length guidance]): [What this section covers]
2. **[Section 2 name]** ([length guidance]): [What this section covers]
3. **[Section 3 name]** ([length guidance]): [What this section covers]
4. **[Section 4 name]** ([length guidance]): [What this section covers]
5. **[Section 5 name]** ([length guidance]): [What this section covers]

Remember: [A closing statement about this agent's unique role on the council.]
```

#### Color Selection

Choose a color that reflects the agent's personality and doesn't conflict with existing agents:

| Existing Agent | Color |
|----------------|-------|
| Optimist Strategist | green |
| Devil's Advocate | red |
| Neutral Analyst | gray |
| Technical Validator | blue |
| Legal Domain Expert | purple |
| User Advocate | orange |
| Growth Strategist | yellow |

Available colors for new agents: `cyan`, `magenta`, `teal`, `brown`, `pink`, `indigo`, `coral`, `slate`, or any other distinct color not already in use. Check the existing agents at generation time by globbing `agents/*.md` and reading their `color` fields.

### Step 4: Write and Confirm

1. Write the generated file to `agents/[kebab-case-name].md` using the Write tool.
2. Confirm to the user:

```
Created `agents/[kebab-case-name].md`

**[Agent Title]** is now available as a council member. It will appear
in the agent selection menu the next time you invoke `/ai-council`.

Summary:
- **Perspective**: [One-line summary of what this agent brings]
- **Color**: [color]
- **Model**: opus
```

## Quality Criteria

A well-crafted agent should:

1. **Have a strong point of view** — The best agents have a specific perspective, not a generic one. "Calibrated skeptic who focuses on second-order effects" is better than "risk analyst."
2. **Define what it is AND what it isn't** — Explicitly state boundaries to prevent drift into generic advice.
3. **Include a structured analytical framework** — A numbered checklist of what to examine produces more consistent, thorough output than open-ended instructions.
4. **Specify output structure** — Numbered sections ensure every analysis covers the same ground and is easy to compare across agents.
5. **Have a distinctive voice** — The Voice section should make this agent sound different from all others on the council.
6. **Include 4 detailed examples in the description** — These teach Claude when to invoke the agent. More context = better automatic selection.
7. **Complement, not duplicate** — The new agent should fill a gap in the council's perspectives, not overlap with an existing member.

## Example

User: `/create-agent I want an agent that thinks about accessibility and inclusive design`

Claude generates `agents/accessibility-advocate.md` with:
- A system prompt focused on WCAG standards, assistive technology, cognitive load, inclusive design principles
- Analytical framework covering visual, motor, cognitive, and situational accessibility
- Voice that champions the needs of users who are often overlooked
- Output structure with sections for compliance, barriers, recommendations, and quick wins
