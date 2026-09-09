[← Back to Main README](../README.md)

# Skills in Claude Code

LLMs like Claude are general-purpose language models. They can reason across different domains. They can write code, analyze data, create documents.

But there's a gap between general capability and reliable high-quality output for specialized tasks.

Imagine you work in a marketing team and you need to create PowerPoint presentations regularly for senior management.

You could use Claude to generate PowerPoints. Claude knows what a PowerPoint presentation is. It knows how to structure slides. It knows which Python libraries to use.

**But Claude won't create a great PowerPoint for you.**

Why? Because Claude doesn't know:
- Your company's layout preferences
- Which fonts to use
- Where to put graphs versus charts versus tables
- Your organization's specific design guidelines

Claude has general capability for creating PowerPoints. But it lacks the specialized skill for creating PowerPoints **your way**.

### The Same Problem Everywhere

This problem shows up everywhere:

**Web Development**
Claude is great at general reasoning about code. But it doesn't know your company's design system or preferred UI patterns.

**Data Analysis**
Claude can perform general data analysis. But it doesn't know your specific data science workflows or preferred analysis methods.

**Document Writing**
Claude can write documents. But it doesn't know your writing style or how you prefer to structure information.

**Code Review**
Claude can review code. But it doesn't know your specific code review style or what you look for.

**The core problem:** LLMs are good at general reasoning but don't do well on specialized tasks.

---

## Why Detailed Prompts Aren't the Answer

When I first realized this problem, I thought: "I'll just write a very detailed prompt with all my instructions."

And that works for one task.

But here's what I discovered:

### Problem 1: Repetition
If I need to do the task repeatedly (like creating PowerPoints every day), I have to retype the same detailed instructions every time. Mistakes happen. Consistency suffers.

### Problem 2: Context Window Waste
If I put those detailed instructions in a system prompt, they sit in my context window all the time—even when I'm not using them. They eat up valuable space.

### Problem 3: No Resource Bundling
I can't attach reference images, design files, or code scripts to a prompt. If my PowerPoint skill needs specific design templates, I can't bundle them.

### Problem 4: No Sharing or Versioning
Prompts are personal. I can't easily share them with teammates. I can't version them. I can't improve them collaboratively.

### Problem 5: No Composability
What if I need to chain multiple specialized tasks together? For example: read a PDF → extract tables → create a PowerPoint. I can't compose prompts to do this reliably.

These five problems made me realize: **prompts are instructions for one-off tasks. Skills are reusable knowledge that loads on demand.**

---

## What Skills Actually Are

> **Skills are reusable file-based resources that provide Claude with domain-specific expertise—such as workflows, context, and best practices—that transforms general-purpose agents into specialists.**

In simple terms: A skill is a folder in my project that contains files which help Claude understand how to execute a specialized task.

### The Best Part: Skills Load on Demand

Unlike system prompts that sit in context memory all the time, skills only load when they're needed.

This is called **progressive disclosure**. Information is only presented at the moment it's needed.

**Level 1:** The skill's description (name + description) is always loaded. This is tiny.

**Level 2:** When Claude recognizes a task matches a skill description, it loads the full SKILL.md file.

**Level 3:** If the skill references other resources (scripts, templates, images), those load when needed.

This means I can have dozens of skills without bloating my context window. Skills are just-in-time knowledge.

---

## The Fourth Thing I Learned: The Structure of a Skill

A skill is just a folder. Inside that folder, there are two main components:

### The SKILL.md File (Required)

This file has two parts:

**YAML Front Matter:**
- `name`: The skill's name
- `description`: This is crucial. Claude reads this to know when to load the skill.

**Markdown Body:**
- Detailed instructions on how to execute the specialized task
- Code patterns and best practices
- Validation steps
- Links to supporting files

### Supporting Resources (Optional)

If the skill needs additional resources:
- `scripts/` folder for Python scripts
- `templates/` folder for reference images or design files
- Any other files the skill needs

### The Folder Hierarchy

```
project/
└── .claude/
    └── skills/
        └── skill-name/
            ├── SKILL.md
            ├── scripts/
            │   └── helper.py
            └── templates/
                └── design.png
```

---

## Types of Skills

### Personal Skills

These are stored in `~/.claude/skills/` (my home directory). They're available across ALL my projects.

**Use cases:**
- Personal coding style
- Personal writing style
- Personal design preferences
- Any workflow I want consistent everywhere

### Project Skills

These are stored in `.claude/skills/` inside my project. They're only available for that specific project.

**Use cases:**
- Team-specific workflows
- Project-specific conventions
- Company-specific guidelines
- Things I want to share with teammates

---

## How to Create a Skill

There are three ways to create a skill:

### 1. Manual Creation

Create a folder, create SKILL.md, write everything manually. This works but I don't recommend it for beginners.

### 2. Using the Skill Creator (Recommended)

Claude has a built-in "Skill Creator" skill. I can use it to create new skills interactively.

**The workflow:**
1. Open Claude Chat
2. Click the plus icon → Skills → Skill Creator
3. Tell the Skill Creator what I want
4. Answer a few questions
5. It generates the complete skill for me

The Skill Creator asks:
- What will the skill do?
- When should it trigger?
- What should the output look like?

It even analyzes my codebase to understand the context.

### 3. Community Skills

Other people have created skills too. I can find them on platforms like:
- The official Anthropic GitHub repository
- Community skill marketplaces

**Important warning:** Always review community skills before using them. They could contain security vulnerabilities or expose sensitive information.

---

## Skills vs Commands

This was a recent development that confused me at first.

**Commands** were custom slash commands I created to execute workflows. I triggered them manually.

**Skills** are specialized knowledge that Claude loads automatically when needed.

Now, Anthropic has merged these two concepts. Going forward:

- **No separate commands folder**
- **Everything goes in the skills folder**
- **Skills are accessible via slash commands**

To make a skill behave like a command (so Claude never auto-invokes it), I add this to the YAML front matter:

```yaml
disable_model_invocation: true
```

This means:
- I can still trigger it with `/skill-name`
- Claude won't load it automatically
- It behaves like a command but uses the same structure

**The reason for merging:** Both skills and commands use the same structure (folder + SKILL.md + optional resources). Keeping them separate was unnecessary.

---

## My Final Mental Model for Skills

Today, I think about skills as a system of specialized knowledge:

```
                ┌─────────────────────────────────┐
                │      General-Purpose LLM       │
                │   (Good at reasoning, broad)   │
                └─────────────┬───────────────────┘
                              │
                              ▼
                ┌─────────────────────────────────┐
                │         Skills Layer           │
                │   (Domain-specific expertise)  │
                └─────────────┬───────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│  Frontend     │    │  Data         │    │  Writing      │
│  Design Skill │    │  Analysis     │    │  Style Skill  │
│               │    │  Skill        │    │               │
│ → UI patterns │    │ → EDA steps   │    │ → Tone        │
│ → Component   │    │ → Feature     │    │ → Structure   │
│   guidelines  │    │   engineering │    │ → Formatting  │
│ → Design      │    │ → Validation  │    │ → Templates   │
│   principles  │    │   checks      │    │               │
└───────────────┘    └───────────────┘    └───────────────┘
```

Each layer adds capability:
- The LLM provides general reasoning
- Skills provide specialized domain knowledge
- Together, they create a powerful specialist

---


[← Back to Main README](../README.md)