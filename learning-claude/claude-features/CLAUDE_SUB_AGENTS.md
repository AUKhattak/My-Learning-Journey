[← Back to Main README](../README.md)

# Sub-Agents and the Context Window Problem

Before I could understand sub-agents, I had to understand the problem they solve. And that problem goes all the way back to how LLMs work.

Here's what I learned:

When I interact with any LLM, I'm making API calls. I send a question through the API. The API feeds it to the LLM. The LLM generates a response. The response comes back through the API.

This is straightforward.

But there's a catch:

> **LLMs are stateless by nature. They have no memory of their own.**

This means if I ask an LLM a question today, it won't remember that question tomorrow. It won't remember the answer either.

Let me give you an example that helped me understand this:

**Turn 1:**
I ask: "What is the capital of France?"
LLM responds: "Paris"

**Turn 2 (immediately after):**
I ask: "What about Germany?"
LLM responds: "Germany is a federal parliamentary republic with 16 states, Berlin as its capital..."

The LLM has no idea I was asking about capitals. It doesn't remember the previous question or answer. It's completely stateless.

This seems like a huge problem, right? How can anyone build a chat application with an LLM if it doesn't remember previous conversations?

---

## The Solution That Developers Found

I learned that developers came up with a workaround for this statelessness problem.

The solution is simple but effective:

> **Instead of sending just the current question, send the entire conversation history with every API call.**

So when I ask "What about Germany?", I don't just send that question. I also send:

- "What is the capital of France?" (previous question)
- "Paris" (previous answer)
- "What about Germany?" (current question)

Now the LLM has the full context. It understands I'm asking about the capital of Germany. It responds with "Berlin."

This works great for chat applications.

But I learned that this approach fails spectacularly for coding agents.

---

## Why This Approach Fails for Coding Agents

This was the moment everything clicked for me.

Imagine I have a large codebase with around 15-20 files and roughly 30,000 tokens of code.

Now I want to add an authentication system. So I send this prompt:

> "Analyze my codebase and build an auth system."

Here's what happens:

**Turn 1:**
The coding agent needs to understand my existing codebase. What database am I using? What's my backend service? What are my API contracts?

So it loads all 30,000 tokens of my codebase into its context window. Plus my prompt. Total: ~32,000 tokens.

The LLM analyzes everything, creates a plan, and returns it.

**Turn 2:**
I say: "Now implement the JWT middleware based on the plan."

But because LLMs are stateless, I have to send the entire conversation history again. This means:

- The full 30,000 token codebase
- My previous message
- The plan that was returned
- My current message

Total: ~32,000+ tokens. And I'm still in the second turn!

**Turn 3:**
I say: "Add rate limiting and refresh token rotation."

Again, I have to send everything:
- The entire codebase (30,000 tokens)
- All previous conversation (growing larger)
- All generated code (growing larger)
- My current message

Total: ~39,000 tokens now.

**By Turn 8:**
I'm sending around 76,000 tokens per request. I've already spent over $1 just on this single conversation!

---

## The Two Big Problems

This is where I truly understood why sub-agents exist. There are two massive problems with this approach:

### Problem 1: Context Window Overflow

The more I work with the LLM, the faster my context window fills up. Every single turn requires sending the entire conversation history plus the full codebase.

My context window gets consumed at an alarming rate.

### Problem 2: Lost in the Middle Effect

I learned that when context windows get too full, LLMs tend to focus on the earliest and latest tokens. They forget the middle ones.

This is a well-studied phenomenon. And it means that even if I manage to fit everything into the context window, the LLM might not actually use all that information effectively.

The quality of responses degrades.

So here I was, spending more money and getting worse results. There had to be a better way.

---

## Sub-Agents

This is where sub-agents came into the picture.

I learned that sub-agents are:

> **Specialized AI assistants that run in their own isolated context windows, doing heavy lifting in a separate space and handing back only what matters.**

Here's how I think about it now:

When I'm chatting with Claude Code, I'm talking to the **main agent**. But the main agent can spawn completely new agents on its own or when I ask it to.

These new agents are **sub-agents**.

And here's the key insight I learned:

> **Each sub-agent gets its own fresh context window.**

---

## How Sub-Agents Actually Work

Let me walk through the example.

I ask my main agent: "Add auth to my Express app."

Here's what happens step by step:

### Step 1: Main Agent Recognizes the Task
The main agent is intelligent enough to realize: "If I load the entire codebase into my context window, I'll have to carry it through the whole conversation. That's inefficient."

### Step 2: Main Agent Spawns a Sub-Agent
Instead of doing the analysis itself, the main agent creates a new, isolated sub-agent specifically for analysis.

### Step 3: Sub-Agent Gets Its Own Context
This sub-agent gets a fresh context window. It receives two things:
1. Command to load the entire codebase
2. Prompt: "Analyze this codebase and come up with an implementation plan"

### Step 4: Sub-Agent Does the Heavy Lifting
The sub-agent loads all 30,000 tokens of the codebase. It analyzes everything. It understands the architecture, the database, the API contracts.

### Step 5: Sub-Agent Returns Only What Matters
The sub-agent doesn't return the entire codebase. It returns a small summary:

> "Found 20 files. Express + Prisma + Redis setup. 12 routes. No existing authentication. Redis already configured for caching."

Just 500 tokens of pure insight.

### Step 6: Sub-Agent's Context Is Destroyed
Once the sub-agent returns its results, its entire context window is destroyed. The 30,000 tokens of codebase are gone. They never enter the main conversation.

---

## The Beautiful Result

This was the moment I truly appreciated the power of sub-agents.

**Without Sub-Agents:**
Every conversation turn sends 30,000+ tokens of codebase. Context window fills fast. Money burns quickly.

**With Sub-Agents:**
The main conversation only contains the 500-token plan. Every subsequent turn, I'm sending just the plan plus the conversation history.

The savings are enormous:

```
Without sub-agents:
30,000 tokens × every turn = Massive cost

With sub-agents:
500 tokens × every turn = Massive savings
```

Two benefits emerge:

1. **My context window lasts much longer** - I can have longer conversations without hitting limits
2. **I save money on every turn** - Less tokens sent, less money spent

---

## The Analogy That Helped Me Understand

I thought about it this way:

In programming, when I call a function, I don't care what code is written inside it. I just provide inputs and get outputs. The internal implementation is abstracted away.

Sub-agents work exactly the same way.

I give a sub-agent a task (input). It works in its own isolated environment. It returns the result (output). I use that output to continue my work.

I don't need to know every file it read or every thought process it had. I just need the result.

This abstraction is what makes sub-agents so powerful.

---

## The Advantages I Learned About

After understanding how sub-agents work, I learned about their four main advantages:

### 1. Context Isolation

This was the primary benefit I already understood. Each sub-agent gets a completely fresh context window where it can do any amount of analysis-heavy work.

### 2. Specialization

This was a game-changer for me.

I learned that I can create specialized sub-agents:
- A research sub-agent
- A code-writing sub-agent
- A security auditor sub-agent

And the best part? Each agent can have:

- **Its own system prompt** - Tailored to its specific role
- **Its own tools** - Only the tools it needs, nothing more
- **Its own model** - Opus for complex reasoning, Sonnet for simpler tasks

I could design each agent exactly how I wanted it to work.

### 3. Modularity

This flowed naturally from specialization. I could break down the entire software development lifecycle into specialized sub-agents:

- One sub-agent for code analysis
- One sub-agent for planning
- One sub-agent for implementation
- One sub-agent for code review
- One sub-agent for testing

Each agent handles its specific part of the workflow. Together, they form a complete development pipeline.

### 4. Parallelism

This was the most exciting advantage I discovered.

Since each sub-agent has its own isolated context window, they can work independently and simultaneously.

Here's an example:

Let's say I need to perform Exploratory Data Analysis (EDA) on three different datasets.

**Without sub-agents:**
I would do them sequentially. Dataset 1 → Dataset 2 → Dataset 3. Slow and inefficient.

**With sub-agents:**
I can spawn three instances of my EDA sub-agent. Each one works on a different dataset simultaneously. All three complete at roughly the same time.

This is true parallelism. And it's only possible because sub-agents run in their own isolated contexts.

Another example I thought of:
- Build an auth service → spawn sub-agent 1
- Build a payment service → spawn sub-agent 2
- Build a user service → spawn sub-agent 3

All three services are independent. All three can be developed in parallel.

This completely changed how I think about AI-assisted development.

---

## Where I Learned Sub-Agents Are Used Most

I discovered several primary use cases where sub-agents are extensively used:

### 1. Codebase Exploration

This is the most common use case. I learned that whenever I ask Claude to explore or analyze a codebase, it automatically uses the Explore sub-agent behind the scenes.

The beauty is that I don't even need to tell it to. Claude is intelligent enough to recognize that exploration tasks need a sub-agent and spawns one automatically.

### 2. Code Review

This was a fascinating insight. I learned that the agent that writes code isn't always the best agent to review it.

Why? Because the agent that wrote the code knows exactly what it did. It knows which trade-offs were considered. It knows which approaches were rejected. It has an inherent bias.

When the same agent reviews its own code, the review isn't unbiased.

The solution? Use a different sub-agent for code review. A fresh perspective. No bias. Better reviews.

### 3. Testing

Same logic applies here. The agent that wrote the code shouldn't be the one testing it. A separate testing sub-agent produces better test cases and finds more bugs.

### 4. Multi-Stage Pipelines

Sometimes tasks are connected: the output of task 1 becomes input for task 2, and the output of task 2 becomes input for task 3.

For these scenarios, I learned that sub-agents can handle the handoffs perfectly:

- Sub-agent 1: Writes the API contract
- Sub-agent 2: Implements the actual code
- Sub-agent 3: Tests the implementation

Each agent does its part and passes the result to the next.

### 5. Parallel Independent Tasks

As I already mentioned, when tasks are independent and don't depend on each other, sub-agents can work in parallel.

### 6. Security Audits

Again, a separate security auditor sub-agent is better than having the same agent that wrote the code audit itself. Less bias, better security findings.

---

## The Types of Sub-Agents I Learned About

Claude Code offers two main types of sub-agents:

### Built-in Sub-Agents

These come pre-configured. I don't need to create them. There are three primary ones:

**1. Explore Sub-Agent**
- Triggered automatically when Claude needs to explore a codebase
- Reads, analyzes, and understands the code
- Returns a summary

**2. Plan Sub-Agent**
- Triggered when I'm in Plan Mode
- Handles the heavy lifting of creating implementation plans from specs
- Returns a detailed plan

**3. General Purpose Sub-Agent**
- Used for both read and write tasks
- Triggered when Claude needs a sub-agent for any general purpose task

### How They Get Triggered

I learned there are two ways sub-agents get triggered:

**Implicitly (Automatic):**
Claude recognizes that a task needs a sub-agent and delegates on its own. I don't need to tell it anything. It just happens.

**Explicitly (Manual):**
I can explicitly tell Claude that I want a sub-agent for a specific task.

---

## Custom Sub-Agents

Beyond the built-in ones, I can create my own custom sub-agents.

There are two levels:

**User-Level Sub-Agents:**
- Available on my machine
- Can be used across all projects
- Stored in `~/.claude/`

**Project-Level Sub-Agents:**
- Available only for the current project
- Stored in `.claude/` within the project
- Not accessible from other projects

### What I Can Configure

When creating custom sub-agents, I can configure:

**1. Tools Access**
- Which tools can the sub-agent use?
- Explore agent → Read tools
- Code agent → Write tools
- Research agent → Web search tools

**2. System Prompt**
- What is this sub-agent's specific purpose?
- How should it approach its tasks?

**3. Model**
- Which model should this sub-agent use?
- Opus for complex reasoning
- Sonnet for moderate tasks
- Haiku for simple tasks

**4. Permissions**
- What can this sub-agent do?
- What is it restricted from doing?

**5. Hooks**
- I can also configure hooks access.

**6. Skills**
- Which skills can the sub-agent use?

Here's an example configuration for a Security Reviewer sub-agent:

```
Tools: Read, Grep, Glob
Model: Opus
Prompt: "Review for injection, authorization bypass, and data exposure vulnerabilities"
```

And for a Research sub-agent:

```
Tools: Read, Grep, Web Search, Web Fetch
Model: Sonnet
Prompt: "Research and gather information on given topics"
```

---

## The Practical Demonstration

To really understand how built-in sub-agents work, I did a practical experiment in one of my project.

### Setting Up Observability

First, I installed a library called `agents-observe` that provides real-time observability. It shows me exactly when sub-agents are triggered and what they're doing.

The library uses hooks to provide this visibility. But the result is amazing: a real-time dashboard showing every sub-agent action.

### Demonstrating the Explore Sub-Agent

I started by writing a prompt:

> "Explore the codebase and tell me what this is all about. Also tell me what features have been developed and what features are yet to be developed."

Before running it, I checked my context window: ~147.5K tokens free.

Then I ran the prompt.

**What happened in the dashboard:**
- A new sub-agent appeared: "Claude Explore"
- It started triggering read tools to explore files
- It was analyzing and understanding the codebase
- It completed its task
- The sub-agent stopped
- Control returned to the main agent

**The result:**
- The main agent received a comprehensive summary
- My context window still had ~142K tokens free
- The full codebase never entered the main conversation

The difference was dramatic. Without the sub-agent, my context window would have dropped significantly. With the sub-agent, it barely changed.

### Demonstrating the Plan Sub-Agent

Next, I created a spec for a new feature: "Backend Routes for Profile Page."

Then I went into Plan Mode with a prompt:

> "Read this spec file and come up with an implementation plan for adding the backend routes."


**What happened in the dashboard:**
- Two Explore sub-agents spawned first (to analyze different parts of the codebase)
- They explored and returned findings
- A Plan sub-agent spawned and created the implementation plan
- Three General Purpose sub-agents spawned in parallel
- All three sub-agents worked simultaneously on their assigned parts
- They completed their tasks
- The main agent integrated everything

I could see all of this happening in real-time on the dashboard. Three sub-agents, working in parallel, completing their tasks at roughly the same time.

---


This separation of concerns made everything better:

- My context windows lasted longer
- I spent less money on tokens
- I could work in parallel on independent tasks
- I got more specialized, higher-quality outputs
- I could use different models for different types of work

---

## My Final Mental Model for Sub-Agents

Today, I think about sub-agents as a layered system:

```
                ┌─────────────────────────┐
                │      Main Agent         │
                │  (Orchestrator/Manager) │
                └──────────┬──────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│   Explore     │  │     Plan      │  │ General       │
│  Sub-Agent    │  │   Sub-Agent   │  │  Purpose      │
│               │  │               │  │  Sub-Agent    │
│ Fresh Context │  │ Fresh Context │  │ Fresh Context │
│ Isolated      │  │ Isolated      │  │ Isolated      │
│ Returns       │  │ Returns       │  │ Returns       │
│ Summary       │  │   Plan        │  │   Code        │
└───────────────┘  └───────────────┘  └───────────────┘
                           │
                           ▼
                ┌─────────────────────────┐
                │    Custom Sub-Agents    │
                │                         │
                │ ┌─────────────────────┐ │
                │ │ Security Reviewer   │ │
                │ │ Research Agent      │ │
                │ │ Code Writer         │ │
                │ │ Test Generator      │ │
                │ └─────────────────────┘ │
                └─────────────────────────┘
```

Each layer has its purpose. Each sub-agent has its own context. Each returns only what matters.

---


If I had to summarize everything I learned about sub-agents, here are the key principles:

### 1. Sub-Agents Solve the Context Window Problem
By running in isolated contexts, sub-agents prevent the main conversation from being flooded with unnecessary codebase information.

### 2. Sub-Agents Enable Specialization
Different agents for different tasks. Each with its own prompt, tools, and model.

### 3. Sub-Agents Save Money
Less tokens sent per conversation turn means lower costs. The savings compound over long conversations.

### 4. Sub-Agents Enable Parallelism
Independent tasks can run simultaneously. This is impossible with a single main agent.

### 5. Sub-Agents Reduce Bias
The agent that writes code shouldn't review it. A fresh sub-agent provides unbiased reviews.

### 6. Sub-Agents Are Hierarchical
Main agent → Sub-agents. They don't talk to each other directly. All coordination happens through the main agent.

### 7. Sub-Agents Are Disposable
Once a sub-agent completes its task, its context is destroyed. No lingering memory. Clean and efficient.

---


[← Back to Main README](../README.md)