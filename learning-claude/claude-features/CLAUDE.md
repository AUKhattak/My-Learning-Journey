[← Back to Main README](../README.md)

# CLAUDE.md And a Structured AI Development Workflow

When I first started exploring Claude Code, I thought the main challenge was simply learning how to give better prompts.

I quickly realized that I was looking at the problem from the wrong angle.

The more I worked with Claude Code, the more I noticed that I was repeatedly explaining the same things:

* How my project is structured
* Which technologies we use
* How we write code
* Which commands should be used
* How testing should be done
* What Claude should avoid changing
* What conventions the project follows
* How certain parts of the codebase are supposed to work

At first, I treated these as individual prompts.

But after repeating the same instructions again and again, I started asking myself:

> **Why am I explaining the same context to Claude every time?**

That question is what led me to `CLAUDE.md` when I started studying about using claude code.

And from there, I started discovering that `CLAUDE.md` was only the beginning.

---

## The First Thing I Learned: `CLAUDE.md` Is Not Just Another Markdown File

My first understanding of `CLAUDE.md` was quite simple:

**It is a place where I can write instructions for Claude.**

But as I used it more, I realized that its real value is much bigger.

I can use `CLAUDE.md` to provide persistent context about my project so that I don't have to repeat the same instructions in every conversation or session.

For example, instead of repeatedly saying:

> "This project uses TypeScript, follow our existing architecture, don't introduce unnecessary dependencies, run tests after making changes, and follow our coding conventions."

I can put those instructions into the project's `CLAUDE.md`.

Now Claude has that context whenever it works inside the project.

That changed the way I thought about AI-assisted development.

I stopped thinking only in terms of:

**"How do I prompt Claude better?"**

and started thinking:

**"How do I design the environment in which Claude operates?"**

That was an important shift in my learning.

---

# So What Should Actually Go Inside `CLAUDE.md`?

As I experimented, I realized that the most useful content in `CLAUDE.md` is not a huge explanation of the entire project.

It should contain information that actually changes how Claude should work.

For example:

### Project architecture

I can explain the important architectural decisions:

* Where the frontend lives
* Where the backend lives
* How APIs are organized
* Where business logic belongs
* Where database-related code lives
* Which layers should not directly communicate with each other

### Development conventions

I can define things such as:

* Naming conventions
* Preferred patterns
* File organization
* Error-handling approaches
* Component conventions
* Coding practices

### Commands

I can document important commands:

* How to install dependencies
* How to start the application
* How to run tests
* How to run linting
* How to build the project

### Constraints

This is one of the areas I found particularly useful.

I can tell Claude what **not** to do.

For example:

* Don't modify generated files
* Don't introduce a new dependency unless necessary
* Don't change database schemas without following the migration process
* Don't modify certain legacy modules
* Don't bypass existing abstractions

This type of information can prevent Claude from making technically valid but project-inappropriate changes.

---

# Then I Discovered That One `CLAUDE.md` Doesn't Have to Control Everything

As my projects became larger, another problem appeared.

A single root `CLAUDE.md` can become too large.

Imagine a project like this:

```text
project/
├── frontend/
├── backend/
├── database/
└── infrastructure/
```

The frontend developer doesn't need every detail about the database.

The database-related work doesn't necessarily need every frontend convention.

So I started thinking about **scope**.

Instead of putting every possible instruction into one file, I can keep instructions close to the part of the project they describe.

For example:

```text
project/
├── CLAUDE.md
├── frontend/
│   └── CLAUDE.md
├── backend/
│   └── CLAUDE.md
└── database/
    └── CLAUDE.md
```

Now the root `CLAUDE.md` can describe the project as a whole, while each subdirectory can contain more specific context.

This was another important lesson for me:

> **The closer an instruction is to the code it describes, the more useful and maintainable that instruction becomes.**

---

# Then Came `.claude`

At this point, I started noticing that Claude Code has a much broader configuration structure.

`.claude` isn't simply another place to put a `CLAUDE.md`.

It can become a toolbox for how Claude works with the project.

For example:

```text
.claude/
├── rules/
├── commands/
├── skills/
└── agents/
```

This is where my mental model started becoming much clearer.

I began separating different kinds of things instead of putting everything into one giant instruction file.

---

# Rules: When My Instructions Became More Specific

One thing I learned was that not every instruction needs to live inside the main `CLAUDE.md`.

Some instructions are very specific.

For example:

```text
.claude/
└── rules/
    ├── coding-style.md
    ├── testing.md
    ├── security.md
    └── api.md
```

This gives me a way to organize detailed rules around particular topics.

Instead of having one massive file containing hundreds of lines of instructions, I can separate concerns.

For example:

**`coding-style.md`**

Can contain coding conventions.

**`testing.md`**

Can explain testing expectations.

**`security.md`**

Can contain security-related rules.

**`api.md`**

Can explain API-specific conventions.

This made me realize that the goal isn't to create the biggest possible instruction file.

The goal is to create a **clear instruction system**.

---

# The Most Important Lesson: Don't Turn `CLAUDE.md` Into a Project Wiki

This was probably one of the most important things I learned.

My first instinct was to keep adding information.

More context must mean better results, right?

Not necessarily.

If `CLAUDE.md` becomes hundreds or thousands of lines long, it becomes harder to maintain and harder to reason about.

The better approach is to keep the main `CLAUDE.md` focused on **high-signal information**.

I think about it this way:

> If a line doesn't materially change how Claude should work, I probably don't need it in the main `CLAUDE.md`.

The project documentation can explain the project.

The `CLAUDE.md` should explain **how Claude should operate within that project**.

That distinction helped me keep things much cleaner.

---

# `/init`: Where I Started Automating the Process

Another useful part of my learning was discovering `/init`.

Instead of starting with an empty `CLAUDE.md`, I can ask Claude Code to analyze the existing project and generate an initial version.

That gives me a starting point.

But I don't treat the generated file as the final answer.

I review it.

I remove unnecessary information.

I correct anything that isn't useful.

I add the project-specific rules that matter to me.

So my workflow became something like:

```text
Existing Project
      ↓
    /init
      ↓
Generated CLAUDE.md
      ↓
Review
      ↓
Remove noise
      ↓
Add important project rules
      ↓
Maintain over time
```

For me, the important part isn't the generation.

It's the **review and refinement** afterward.

---

# `CLAUDE.local.md`: When the Instructions Are Only for Me

Then I came across another useful distinction.

Sometimes I have instructions that are relevant to a project, but I don't necessarily want to share them with everyone working on that project.

That's where `CLAUDE.local.md` becomes useful.

I think of it as:

> **Project-specific instructions that are personal to me.**

For example, I might have personal preferences about how I want Claude to work while I'm developing locally.

Those shouldn't necessarily become team-wide project rules.

So my mental model became:

```text
CLAUDE.md
→ Shared project instructions

CLAUDE.local.md
→ My personal project-specific instructions
```

This distinction becomes especially useful when working in a team.

---

# Then I Started Thinking Globally

At some point, I realized that some preferences aren't actually project-specific at all.

There are things I want Claude to follow across multiple projects.

For example:

* My preferred coding style
* How I like explanations
* My general development preferences
* Personal workflows
* Tools or approaches I generally prefer

It doesn't make sense to copy those into every project's `CLAUDE.md`.

That's where the global configuration comes in:

```text
~/.claude/
└── CLAUDE.md
```

Now I can think in terms of three different levels:

```text
Global
   ↓
Project
   ↓
Specific Module
```

And this hierarchy helped me understand where an instruction belongs.

---

# My Rule of Thumb for Deciding Where Something Goes

After experimenting with all of this, I started using a simple mental model.

I ask myself:

### "Who needs this instruction?"

If the answer is:

**Every project I work on**

→ Put it in:

```text
~/.claude/CLAUDE.md
```

---

If the answer is:

**Everyone working on this project**

→ Put it in:

```text
project/CLAUDE.md
```

---

If the answer is:

**Only me, for this project**

→ Put it in:

```text
project/CLAUDE.local.md
```

---

If the answer is:

**Only this particular area of the project**

→ Put it closer to that area:

```text
frontend/CLAUDE.md
backend/CLAUDE.md
database/CLAUDE.md
```

---

If the answer is:

**This is a detailed rule about a specific topic**

→ Consider:

```text
.claude/rules/
```

---

If the answer is:

**This is not really an instruction, but a reusable workflow**

→ It may belong in:

```text
.claude/commands/
```

or

```text
.claude/skills/
```

This simple question—**"Who needs this?"**—became one of the easiest ways for me to decide where information should live.

---

# Commands, Skills and Agents

As I explored further, I realized that Claude Code isn't only about giving Claude instructions.

There are different ways of extending how Claude works.

I started thinking about the pieces like this:

### `CLAUDE.md`

**Context and instructions**

> "Here is how this project works and how you should behave."

### Rules

**Specific policies**

> "When working with APIs, follow these rules."

### Commands

**Reusable workflows**

> "When I run this command, perform this sequence of actions."

### Skills

**Reusable capabilities/workflows**

> "Here is a specialized way Claude can perform a particular type of task."

### Agents

**Specialized roles**

> "Handle this type of work with a particular focus."

This distinction made the whole Claude Code ecosystem easier for me to understand.

---

# And Then There Was Auto Memory

Another concept I found interesting was memory.

At first, I assumed that everything Claude remembers must be something I explicitly write in `CLAUDE.md`.

But that's not necessarily the case.

There is an important distinction between **instructions I intentionally provide** and **patterns Claude learns or records through its memory mechanism**.

I think about it like this:

```text
CLAUDE.md
→ What I explicitly tell Claude

Memory
→ Patterns and useful information Claude records from working with me
```

That distinction is important.

`CLAUDE.md` is something I intentionally maintain.

Memory is more about what Claude learns from ongoing work.

The `/memory` command can be used to inspect and manage that memory.

---

# The Structure I Would Use Today

After going through this learning process, this is roughly how I would organize a larger project:

```text
project/
│
├── CLAUDE.md
│
├── .claude/
│   ├── rules/
│   │   ├── coding-style.md
│   │   ├── testing.md
│   │   ├── security.md
│   │   └── api.md
│   │
│   ├── commands/
│   ├── skills/
│   └── agents/
│
├── frontend/
│   └── CLAUDE.md
│
├── backend/
│   └── CLAUDE.md
│
└── database/
    └── CLAUDE.md
```

And separately, at the user level:

```text
~/.claude/
└── CLAUDE.md
```

I wouldn't create all of these just because they exist.

I would introduce them only when the project actually needs that level of organization.

---

# What My Learning Journey Changed

The biggest change for me wasn't actually learning where to put `CLAUDE.md`.

It was changing how I think about AI-assisted development.

Initially, I thought:

> **Claude is an AI assistant, so I need to tell it what to do.**

Then I moved toward:

> **Claude is working inside my development environment, so I need to give it the right context.**

And eventually:

> **I can design the environment itself so that Claude understands how I want it to work.**

That's a much more powerful way of thinking.

Instead of repeatedly prompting Claude with the same information, I can turn recurring instructions into persistent project knowledge.

Instead of one giant instruction file, I can organize information according to scope.

Instead of explaining the same workflow repeatedly, I can turn it into reusable commands or skills.

Instead of relying entirely on explicit instructions, I can also make use of memory.

---

# My Final Mental Model

Today, I think about Claude Code configuration as a layered system:

```text
                 ┌─────────────────────┐
                 │     Global Rules    │
                 │ ~/.claude/CLAUDE.md  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Project Context   │
                 │     CLAUDE.md       │
                 └──────────┬──────────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │ Frontend │  │ Backend  │  │ Database │
        │CLAUDE.md │  │CLAUDE.md │  │CLAUDE.md │
        └──────────┘  └──────────┘  └──────────┘

                    Project Toolbox
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
           Rules        Skills        Agents
             │
             ▼
        Topic-specific
          guidance

                    Personal Layer
                           │
                           ▼
                    CLAUDE.local.md

                    Learned Layer
                           │
                           ▼
                       Memory
```

I don't see these as competing features anymore.

I see them as **different layers of context and behavior**.

---

# The Main Lesson I Took Away

If I had to summarize everything I learned into one principle, it would be this:

> **Don't keep telling Claude the same thing. Turn recurring knowledge into structure.**

Put global preferences at the global level.

Put shared project knowledge in the project.

Put module-specific knowledge near the module.

Put detailed topic-specific guidance into rules.

Put reusable workflows into commands or skills.

Keep personal project preferences separate.

And let memory handle the patterns that are learned over time.

Most importantly, I don't want my `CLAUDE.md` to become a giant documentation dump.

I want it to be **small, intentional, high-signal, and alive**.

Because the best `CLAUDE.md` isn't the one with the most information.

It's the one that contains the **right information at the right scope**.

And that, for me, was the real journey from simply using Claude Code to actually thinking about how to build a better environment for working with it.


[← Back to Main README](../README.md)