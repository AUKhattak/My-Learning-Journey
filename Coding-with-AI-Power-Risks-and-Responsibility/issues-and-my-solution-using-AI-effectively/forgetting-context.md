[← Back to Main page](../Readme.md)

# Forgetting Context and Better Approach for Context Preservation

For long-running coding projects, **context and knowledge management is one of the biggest practical limitations of using Claude (or any LLM coding agent)** as the project grows. But I wouldn't describe it simply as "Claude forgets." The deeper problem is **maintaining the right context, state, decisions, and architecture across many sessions**.

### What happens as a project grows

Imagine a project starts like this:

```text
Day 1
Claude sees:
- 20 files
- simple architecture
- 1 feature
- few decisions

→ Easy
```

Six months later:

```text
Project
├── 800 source files
├── 150 tests
├── 40 database migrations
├── 30 architectural decisions
├── 100 issues
├── 20 integrations
├── old/deprecated code
└── lots of implicit assumptions
```

Claude **cannot meaningfully keep all of that in its active context at once**.

So the problem becomes:

> **How do we give the agent the right subset of the project's accumulated knowledge at the right moment?**

That's the real challenge.

---

## There are actually 4 different "memory" problems

### 1. Session memory

You work with Claude for 3 hours.

You discuss:

> "Don't implement this using Redis because we tried it and it caused X."

Tomorrow you start a new session.

If that decision wasn't persisted somewhere, the new session may have no idea why Redis was rejected.

This is the most obvious form of "forgetting."

---

### 2. Architectural memory

This is much more important.

Suppose your project has this rule:

```text
All business logic goes through the Service layer.
Controllers must never access repositories directly.
```

You might have discussed this decision 4 months ago.

But a new agent session sees:

```text
Controller → Repository
```

and thinks:

> "That's perfectly reasonable."

It can write technically correct code that is **architecturally wrong for your project**.

That's extremely common in large codebases.

---

### 3. Decision memory

This is even more subtle.

Imagine:

> "We deliberately don't use library X."

Why?

Because six months ago you discovered a performance problem.

If that knowledge isn't recorded, a future agent might "improve" the project by introducing X again.

So you need persistent records of:

```text
Decision
Why
Alternatives considered
Consequences
Date
```

This is basically an **Architecture Decision Record (ADR)**.

---

### 4. Project-state memory

The agent also needs to know:

```text
What are we doing?
What is finished?
What's broken?
What's next?
What are we currently investigating?
What assumptions are temporary?
```

This changes constantly.

A huge codebase can be perfectly documented but still confusing if the **current state** isn't maintained.
Then Claude isn't expected to remember everything.

## My Approach to Preserving Context and Quality for AI

I approach the context problem by treating project knowledge as part of the codebase rather than something that should live only inside an AI conversation.

As a project grows, it becomes unrealistic to expect an AI agent to remember every discussion, architectural decision, constraint, and previous investigation. Instead, I make the important knowledge **persistent, structured, and easy for the agent to discover when it is relevant**.

The goal is not to give the AI the entire project every time. The goal is to make sure that, for any task, the AI can quickly identify the **right context, current state, architectural rules, and previous decisions** before making changes.

## 1. I maintain persistent project documentation

I keep important project knowledge in the repository itself. This includes things such as:

* High-level architecture
* Important conventions and coding rules
* System boundaries and responsibilities
* Integration details
* Development workflows
* Known constraints
* Important assumptions
* Current project state

This means the knowledge does not depend on a particular AI session or on me remembering to explain the same thing again.

The repository becomes the **source of truth**, while the AI conversation becomes temporary working context.

## 2. I record important architectural decisions

For decisions that have long-term consequences, I use Architecture Decision Records (ADRs).

Instead of only recording:

> "We don't use Redis."

I record the actual decision and its reasoning:

```text
Decision:
Do not use Redis for this use case.

Context:
...

Alternatives considered:
...

Reason:
...

Consequences:
...

Date:
...
```

This is important because the AI can see not only **what** the architecture looks like, but **why** it looks that way.

Without this information, an AI agent may see an existing solution and reasonably decide that another approach would be "better." The problem is that it may not know about the historical constraints that led to the current design.

ADRs therefore protect the project from repeatedly revisiting already-settled decisions or accidentally reintroducing rejected technologies and patterns.

## 3. I maintain current project state separately from permanent documentation

Architectural knowledge changes relatively slowly, while project state changes continuously.

Therefore, I keep track of things such as:

```text
Current objective
Completed work
Open problems
Known bugs
Active investigations
Next steps
Temporary assumptions
```

This gives the AI a snapshot of **where the project currently is**, rather than forcing it to reconstruct the state from hundreds of files, commits, issues, and previous conversations.

This distinction is important:

**Documentation explains how the system works.**

**Project state explains what is happening right now.**

Both are necessary for an AI agent to work effectively.

## 4. I keep context close to the code it describes

I try to avoid putting all project knowledge into one enormous document.

Instead, I keep context close to the relevant part of the system whenever possible.

For example:

```text
project/
├── docs/
│   ├── architecture/
│   ├── decisions/
│   └── project-state/
│
├── services/
│   ├── payments/
│   │   └── README.md
│   ├── authentication/
│   │   └── README.md
│   └── notifications/
│       └── README.md
```

This makes the context more discoverable.

If an AI is working on payments, it should not need to consume the documentation for the entire application. It should be able to find the architectural and business context relevant to payments.

This is much more scalable than continuously expanding a single global context document.

## 5. I use the AI to discover context before changing code

I don't want the agent to immediately start implementing a task based only on the user's prompt.

For non-trivial changes, I expect the agent to first understand:

1. What part of the system is affected?
2. What architectural rules apply?
3. What previous decisions are relevant?
4. What existing implementations follow the same pattern?
5. What is the current project state?
6. What tests and integrations could be affected?

Only after that should implementation begin.

This creates a workflow closer to:

```text
Task
  ↓
Discover relevant context
  ↓
Understand architecture
  ↓
Check previous decisions
  ↓
Inspect existing implementation
  ↓
Plan change
  ↓
Implement
  ↓
Validate
  ↓
Update project knowledge if necessary
```

The important part is that **context discovery becomes part of the development process**, rather than something I hope the model remembers.

## 6. I preserve context at the point where knowledge is created

One of the biggest problems with AI-assisted development is that valuable knowledge is often created during a conversation and then disappears when the session ends.

For example, during an investigation the AI and I might discover:

> "The reason this service cannot use the shared transaction is because the external provider commits asynchronously."

That is valuable architectural knowledge.

I therefore try to capture important discoveries in the project documentation, ADRs, code comments, tests, or issue/task state depending on what kind of knowledge it is.

This creates a feedback loop:

```text
AI investigation
      ↓
New knowledge discovered
      ↓
Knowledge persisted
      ↓
Future AI sessions can retrieve it
      ↓
Better decisions
      ↓
More knowledge discovered
```

Over time, the project becomes easier for both humans and AI agents to understand.

## 7. I use code and tests as part of the context system

Documentation alone is not enough.

I also treat the existing codebase and test suite as persistent context.

For example, an architectural rule may be documented as:

> "Business logic belongs in the service layer."

But the code should reinforce that rule, and tests should protect important behavior.

This gives the AI multiple sources of truth:

```text
Documentation → explains intent
ADRs           → explain decisions
Code           → shows implementation
Tests          → define expected behavior
Project state  → explains current work
Git history    → provides historical context
```

The AI can then reason across these sources instead of relying on a single document.

## 8. I keep temporary context separate from permanent knowledge

Not everything discussed with an AI should become permanent documentation.

For example:

```text
"Let's try approach A and see if it works."
```

is temporary working context.

But:

```text
"Approach A was rejected because it creates race conditions."
```

may be permanent project knowledge.

I therefore distinguish between:

**Temporary context**

* Current conversation
* Investigation notes
* Hypotheses
* Short-lived implementation plans

and:

**Persistent context**

* Architecture
* Decisions
* Constraints
* Invariants
* Important discoveries
* Current project state

This prevents the documentation from becoming unnecessarily large while ensuring that important knowledge survives.

## Why I believe this is a better approach

I don't try to solve the problem by giving the AI more and more context.

That approach eventually becomes inefficient because more context does not necessarily mean better understanding. Large amounts of irrelevant information can make it harder for the AI to identify what actually matters.

Instead, I optimize for **context quality and retrieval**.

The principle is:

> **The AI does not need to remember the entire history of the project. It needs a reliable way to recover the relevant history when it matters.**

This approach also reduces dependency on a specific AI model or session. If I change from Claude to another coding agent, or start a completely new session, the important knowledge remains in the project.

Most importantly, it protects against **architectural drift**.

Without persistent context, an AI agent tends to optimize for the local problem:

```text
"This implementation works."
```

With persistent architectural context, the question becomes:

```text
"This implementation works,
but does it follow the decisions,
constraints, and architecture of this project?"
```

That distinction becomes increasingly important as the codebase grows.

## The overall principle

My approach is therefore to build a **context system around the codebase**, rather than depending on AI memory.

I want the project to contain enough structured information that a new AI session can go from:

```text
"I don't know this project."
```

to:

```text
"I understand the relevant architecture,
I know the important constraints,
I know why previous decisions were made,
I understand the current state,
and I know what needs to be changed."
```

That makes AI-assisted development much more reliable because the model is no longer expected to be the long-term storage for project knowledge.

**The AI provides reasoning and implementation capability; the project repository provides persistent knowledge and truth.**


[← Back to Main page](../Readme.md)