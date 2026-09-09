[← Back to Main README](../README.md)

# Creating Custom Sub-Agents


> **Built-in agents are like off-the-shelf tools. They work for most tasks. But sometimes, you need a custom tool built specifically for your needs.**

This is where custom sub-agents come in.

---

## The First Thing I Learned: Why Custom Sub-Agents Are Necessary

Imagine I want to perform a security audit on my codebase. I want to check for SQL injection vulnerabilities, exposed API keys, and other security issues.

I could use a built-in agent for this. It would do a reasonable job.

But here's the problem: My company has a specific security checklist. We follow particular guidelines. We have our own definition of what a "secure" codebase looks like.

**The built-in agent doesn't know any of this.**

It has generic knowledge about security audits. But it doesn't know my company's specific checklist. It doesn't know our custom requirements. It doesn't follow our particular guidelines.

This is exactly where custom sub-agents become essential.

> **Custom sub-agents let me create tailor-made assistants that follow my specific rules, check my custom requirements, and work exactly the way I need them to work.**

I can give them:
- My own system prompt
- Only the tools they need
- A specific model (Opus for complex tasks, Sonnet for simpler ones)
- My custom skills and hooks
- Project-specific memory

The result is a sub-agent that does exactly what I want, exactly how I want it.

The same logic applies to testing. I could use a generic testing agent. But if I want tests written in a specific style, following my team's conventions, with my project's specific testing framework—I need a custom agent.

---

## How to Create Custom Sub-Agents

The process is surprisingly simple.

A custom sub-agent is nothing but a **Markdown file with YAML front matter**. Just like everything else in Claude Code.

Here's what the structure looks like:

### The YAML Front Matter

This is where I define the agent's configuration:

**Name:**
The agent's name. This is how I reference it.

**Description:**
A one-line description of what the agent does. This helps Claude understand when to automatically trigger the agent.

**Tools:**
Which tools does this agent have access to?
- Read-only tools for exploration
- Write/edit tools for code changes
- Web search tools for research
- Only what it needs, nothing more

**Model:**
Which model should this agent use?
- Opus for complex reasoning tasks
- Sonnet for balanced performance
- Haiku for simple, fast tasks

**Skills:**
Which skills can this agent use?

**Hooks:**
Which hooks does the agent have access to?

**Memory:**
Does the agent need persistent memory?

**Effort Level:**
How many tokens can the agent spend on thinking?
- Low, Medium, High, or Max

**Color:**
A visual color for the agent in the interface

### The Main Body

This is where the real instructions live. I write detailed guidance about:

- What the agent's specific purpose is
- How it should approach its tasks
- What steps it should follow
- What it should check for
- What it should avoid
- How it should format its output

It's essentially a **system prompt** that I can customize for my specific needs.

---

## Two Ways to Create Custom Agents

I discovered two different approaches:

### Method 1: Using the Slash Agent Command

This is the easier approach, especially for beginners.

I simply type `/agents` and hit Enter. Claude Code shows me:
- How many agents I currently have
- What agents exist in my library
- An option to create a new agent

When I click "Create New Agent," I'm asked:

**Step 1: Project or Personal?**
- Project-level → Available only in my current project
- Personal → Available across all my projects

**Step 2: Auto-Generate or Manual Configure?**
- Auto-generate: Claude creates the agent file for me
- Manual: I configure everything myself

**Step 3: Provide a Description**
I write a brief description of what my agent should do.

**Step 4: Select Tools**
I choose which tools the agent needs.

**Step 5: Select Model**
I choose which model to use.

**Step 6: Select Color**
I pick a visual color.

**Step 7: Add Memory (Optional)**
I decide if the agent needs memory.

Claude then generates a complete Markdown file with YAML front matter and a detailed system prompt.

### Method 2: Manual Creation

Once I understood the structure, I started creating agents manually.

I simply:
1. Go to `.claude/agents/` in my project
2. Create a new Markdown file
3. Write the YAML front matter
4. Write the detailed instructions
5. Save the file

That's it. The agent is ready to use.

This gives me complete control over every detail.

---

## How Custom Sub-Agents Get Triggered

I learned there are two ways to trigger custom sub-agents:

### Implicitly (Automatic)

Claude reads the description field of my custom agents. When it encounters a task that matches a description, it automatically triggers that agent.

The description acts like a signal. It tells Claude: "Use me for tasks like this."

### Explicitly (Manual)

I can directly tell Claude to use a specific sub-agent. Either by:
- Describing what I want and mentioning the agent
- Creating a custom slash command that triggers the agent

Most developers I saw prefer manual triggering. It gives them more control over the workflow.

---

## What I Learned About Agent Design

### 1. Specialization Matters

Each agent should have one clear purpose:
- Test Writer → Write tests
- Test Runner → Run tests
- Security Reviewer → Check security
- Quality Reviewer → Check quality

Don't create one agent that tries to do everything. That defeats the purpose of sub-agents.

### 2. Tool Access Should Be Minimal

Give agents only the tools they need:
- Test Writer → Read + Edit (needs to write files)
- Test Runner → Read only (only needs to read and execute)
- Security Reviewer → Read + Grep + Glob (only needs to analyze)

**Principle:** Least privilege access. An agent can't cause harm with tools it doesn't have.

### 3. Models Should Match Task Complexity

- Security Reviewer → Opus (complex reasoning required)
- Test Writer → Sonnet (moderate complexity)
- Quality Reviewer → Sonnet (moderate complexity)
- Test Runner → Sonnet (straightforward execution)

**Principle:** Use the right tool for the job. Don't waste tokens on Opus for simple tasks.

### 4. Descriptions Are Important

The description field helps Claude understand when to automatically trigger the agent:

> "Use this agent to write pytest test cases for Spendly features. Invoke after implementing any feature to generate tests based on feature specs, not implementation."

**Principle:** Write clear, actionable descriptions that tell Claude exactly when to use the agent.

### 5. System Prompts Drive Behavior

The main body of the agent file acts as a system prompt. It defines:
- What the agent should do
- How it should do it
- What it should check
- What it should avoid
- How it should format output

**Principle:** Be specific. The more detailed the instructions, the better the results.

---


## My Final Mental Model for Custom Sub-Agents

Today, I think about custom sub-agents as a system of specialized workers:

```
                ┌─────────────────────────────────┐
                │       Main Agent               │
                │   (Orchestrator/Manager)       │
                └─────────────┬───────────────────┘
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
            ▼                 ▼                 ▼
    ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
    │   Testing   │   │  Code       │   │   Future    │
    │  Workflow   │   │  Review     │   │  Workflows  │
    └──────┬──────┘   └──────┬──────┘   └─────────────┘
           │                 │
    ┌──────┴──────┐   ┌──────┴──────┐
    ▼             ▼   ▼             ▼
┌────────┐  ┌────────┐ ┌────────┐  ┌────────┐
│ Test   │  │ Test   │ │Security│  │Quality │
│ Writer │  │ Runner │ │Reviewer│  │Reviewer│
└────────┘  └────────┘ └────────┘  └────────┘
```

Each agent has:
- Its own configuration (name, description, tools, model)
- Its own system prompt (instructions for behavior)
- Its own responsibility (one clear purpose)
- Its own output (results that feed back to the main agent)

And together, they form a complete system for building, testing, and reviewing code.

---


[← Back to Main README](../README.md)