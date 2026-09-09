[← Back to Main README](../README.md)

# What Is a Slash Command?

**A slash command is a shortcut that triggers a predefined action or workflow inside a Claude Code session.**

Instead of writing a complete prompt describing what I want to happen, I can use a short command beginning with `/`.

For example:

```text
/exit
```

Instead of explaining:

> "I want to close this Claude Code session."

I simply type `/exit`.

This might sound like a small convenience, but I realized that there is a much bigger idea behind it.

The people building Claude Code noticed that developers tend to repeat certain patterns.

If a particular action is performed repeatedly, it makes sense to turn that action into a reusable command.

So instead of:

```text
Long prompt
      ↓
Explain what I want
      ↓
Claude interprets it
      ↓
Workflow happens
```

I can have:

```text
/command
    ↓
Predefined workflow
    ↓
Action
```

That small change can make the development workflow much faster and more consistent.

---

# Built-in Commands vs Custom Commands

As I explored slash commands, I learned that there are two broad categories.

## 1. Built-in Slash Commands

These are commands that come with Claude Code.

They are available by default when using Claude Code.

For example:

```text
/exit
/resume
/rename
/usage
/config
/permissions
```

These commands provide functionality that is already built into the Claude Code workflow.

---

## 2. Custom Slash Commands

The second category is much more interesting from a developer's perspective.

These are commands that **I can create for my own workflows**.

Imagine that I have a particular project where I repeatedly perform the same sequence of actions.

Instead of explaining that process to Claude every time, I can turn the recurring workflow into a reusable command.

That was an important shift in my thinking.

I started realizing that slash commands aren't just shortcuts.

They are a way of **encoding repeatable developer workflows**.

---

# Before Understanding Commands, I Had to Understand Sessions

One thing I noticed while learning slash commands was that the concept of a **session** keeps coming up.

So I had to understand what a session actually means in Claude Code.

For me, the simplest definition is:

> **A session is one conversation with Claude Code.**

When I start Claude Code, I begin a conversation.

Everything that happens during that conversation becomes part of that session.

That includes:

* My messages
* Claude's responses
* Tool interactions
* The work Claude performs
* The context built during the conversation

A session therefore isn't just a single prompt.

It represents an ongoing piece of work.

---

# Why Session Management Became Important to Me

Once I understood sessions, I started realizing that I shouldn't treat Claude Code like one giant conversation.

If I'm building a website, for example, I might have several features:

```text
Login
Registration
Dashboard
Payments
Profile
Notifications
```

My first instinct might be to build everything inside one session.

But that can quickly become messy.

The context for the login feature gets mixed with the context for payments.

Then I start working on the dashboard.

Then I ask an unrelated question.

Over time, the conversation becomes difficult to reason about.

So one of the most useful practices I learned was:

> **One meaningful task or feature should ideally have its own session.**

For example:

```text
Session 1 → Login
Session 2 → Registration
Session 3 → Dashboard
Session 4 → Payments
```

This gives me a much cleaner separation of context.

It also makes it easier to understand what happened in a particular session later.

---

# Resuming Previous Sessions

This is where sessions became even more useful.

A session doesn't necessarily disappear just because I close my terminal.

If I worked on something today and want to continue tomorrow, I can resume that previous conversation.

For example:

```text
claude -r
```

This allows me to look through previous conversations and select the one I want to continue.

That changed how I thought about my workflow.

I don't necessarily have to finish an entire feature in one sitting.

I can work on it today, close the terminal, and come back later.

The important thing is that I can return to the existing context rather than starting from scratch.

---

# Moving Between Sessions

I also learned that there is a difference between **starting Claude Code again** and **switching between existing sessions**.

Inside Claude Code, I can use:

```text
/resume
```

to move to another existing session.

This is useful when I'm already working but realize that another task belongs to a different conversation.

Instead of mixing the contexts together, I can switch to the appropriate session.

This reinforced an idea that became increasingly important to me:

> **Context management is a major part of working effectively with AI.**

---

# Naming Sessions Properly

Another small thing that turned out to be surprisingly useful was naming sessions.

Claude Code can generate a session name automatically.

But if the name is based on the first question I asked, it may not be very meaningful later.

So instead of allowing something generic to happen, I can explicitly rename the session:

```text
/rename login-feature
```

Now, when I look through my previous sessions, I immediately know what that conversation was about.

For me, this became a simple habit:

> **If I know what I'm working on, I should name the session accordingly.**

For example:

```text
login-feature
registration-feature
database-refactor
payment-integration
api-cleanup
```

This becomes especially valuable after working on a project for several weeks.

---

# A Small Habit That Changed My Workflow: Commit at Milestones

While working with sessions, I also started thinking more carefully about Git.

One practice I found useful was creating commits whenever I reached an important milestone.

For example:

```text
Start Login Feature
       ↓
Implement Authentication
       ↓
Commit
       ↓
Add Validation
       ↓
Commit
       ↓
Add Error Handling
       ↓
Commit
```

The reason I like this workflow is that Claude Code can make substantial changes.

Having meaningful commits gives me checkpoints.

If something goes wrong later, I have a clear history of where the project was at each milestone.

So my session workflow gradually became:

```text
Plan
  ↓
Implement
  ↓
Reach milestone
  ↓
Commit
  ↓
Continue
  ↓
Commit
```

---

# One of My Favorite Commands: `/btw`

One of the commands I found particularly interesting was:

```text
/btw
```

I think of this as a way to ask Claude a **side question without making that question part of the main conversation context**.

This solves a very common problem.

Imagine Claude is currently working on a login feature.

While watching the work, I suddenly wonder:

> "What exactly is Jinja templating in Flask?"

That question is useful to me, but it isn't necessarily part of the login implementation.

If I simply ask the question normally, it becomes part of the main conversation.

Over time, unrelated questions can accumulate and pollute the context.

With `/btw`, I can treat it more like a temporary side conversation.

Conceptually:

```text
Main Task
   │
   ├── Claude works on Login
   │
   └── /btw → "What is Jinja templating?"
                 ↓
              Answer
                 ↓
          Main context stays clean
```

This became one of the commands I personally found very useful.

It taught me another important lesson:

> **Not every question I ask Claude needs to become part of the context of the task I'm working on.**

---

# Exporting a Conversation

Another workflow I found useful was exporting an important session.

This becomes especially relevant before a major refactoring.

Suppose I have an important conversation where Claude and I discussed:

* Why the architecture was designed a certain way
* What decisions we made
* Why certain approaches were rejected
* How a feature was implemented

That conversation contains valuable context.

Instead of leaving it only inside Claude Code's session history, I can export it:

```text
/export filename.md
```

Now the conversation can be stored as a file inside the project.

This gives me another useful workflow:

```text
Important Session
       ↓
     Export
       ↓
     Markdown
       ↓
Future Refactoring
       ↓
Use as Context
```

For me, this is particularly useful when doing large refactors.

The previous conversation can provide historical context about **why the code looks the way it does**, rather than only showing what the current code looks like.

---

# Authentication and Switching Accounts

Not every slash command is related to coding workflows.

Some are about managing Claude Code itself.

For example:

```text
/logout
```

allows me to log out of the current Claude Code account.

This becomes useful when I have different accounts—for example, a personal account and an account provided by a company.

I can log out and authenticate again with the appropriate account.

There is also a login flow that allows me to authenticate again when necessary.

I don't use these commands constantly, but understanding them is useful because Claude Code isn't just an AI model—it is also a tool that has its own authentication and configuration environment.

---

# Switching Between Models

Another important part of my learning was understanding that Claude Code isn't tied to only one model.

There are different model options with different trade-offs.

The way I think about them is roughly:

```text
More capability
     ↑
     │
   Opus
     │
   Sonnet
     │
    Haiku
     │
     ↓
Faster / cheaper
```

The exact capabilities and pricing can change over time, so I don't think of these as permanent characteristics.

What mattered more to me was learning **when to use which type of model**.

---

# The Planning vs Implementation Pattern

One workflow that stood out to me was separating **planning** from **implementation**.

When I'm solving a complex programming problem, there are really two different activities.

### Phase 1 — Planning

I'm thinking about:

* Architecture
* Design decisions
* Trade-offs
* Specifications
* Implementation strategy

For this type of work, I want a model that is strong at reasoning through complex problems.

### Phase 2 — Implementation

Once the plan is clear, the problem changes.

Now I need reliable code generation and execution based on an already-established plan.

So the workflow becomes:

```text
Complex Problem
      ↓
Planning
      ↓
Architecture
      ↓
Specification
      ↓
Implementation Plan
      ↓
Code Generation
```

This distinction was useful because it stopped me from thinking:

> "I need to use the most powerful model for everything."

Instead, I started thinking:

> **"What kind of work am I doing right now?"**

That is a much better question.

---

# `/model`: Switching Models

Claude Code provides a slash command for switching models:

```text
/model
```

This allows me to select the model I want to use for the current work.

The practical lesson I took from this wasn't simply how to change models.

It was:

> **Model selection should depend on the task, not just on which model sounds most powerful.**

For complex planning, I may want stronger reasoning.

For everyday implementation, a balanced model may be sufficient.

For simpler repetitive tasks, a faster and less expensive model may be enough.

---

# `/usage`: Knowing How Much I'm Using

One thing I learned relatively quickly is that AI usage isn't infinite.

Claude Code usage can involve different limits, including session-level and weekly usage considerations depending on the plan and setup.

So there is a command that helps me inspect my current usage:

```text
/usage
```

I find this useful because it gives me visibility into how much I'm consuming.

This becomes especially important when using more expensive or capable models.

If I continuously use the most expensive model for every small task, I can burn through my available usage much faster.

So now I think about usage as another development resource.

Just like I wouldn't unnecessarily consume CPU, memory, or cloud resources, I don't want to unnecessarily consume model usage.

---

# Extra Usage

There can also be situations where I reach my available usage limit.

Depending on the account and plan, additional usage may be available as a paid top-up.

The important lesson for me isn't the command itself.

It's understanding that:

> **My workflow should be designed with usage constraints in mind.**

For a small project, this might never become a problem.

For a large project with extensive planning, many features, and heavy model usage, it can matter much more.

---

# `/stats`: Understanding My Own Usage

Another interesting command is:

```text
/stats
```

This gives me statistics about how I've been using Claude Code.

Things like:

* Token usage
* Models used
* Number of sessions
* Active days
* Longest session
* Usage streaks

At first, this might seem like a fun dashboard.

But after using Claude Code for a while, it becomes more useful.

It allows me to look at my own behavior.

For example:

> Am I using one model too much?

> Are my sessions becoming unnecessarily long?

> How often am I actually using Claude Code?

It turns my usage into something I can analyze.

---

# `/insights`: Learning From My Own Workflow

This was another feature I found particularly interesting.

The `/insights` command can generate a detailed report about how I have been using Claude Code.

Instead of just showing raw numbers, it gives me a more detailed picture of my workflow.

It can help me understand:

* What I'm doing well
* Where I may be inefficient
* What workflows I'm using
* What patterns appear across my sessions
* What I might improve

The report is generated as an HTML file.

I wouldn't necessarily run this after every session.

But after I've completed a reasonable number of sessions, it can be useful to step back and ask:

> **"Am I actually using Claude Code effectively?"**

That is a question I think is easy to forget.

Sometimes we learn a tool's features but never evaluate whether our own workflow has improved.

---

# `/config`: Controlling the Environment

Another command I learned was:

```text
/config
```

This allows me to modify Claude Code configuration settings.

Depending on the available options, I can change things such as:

* Thinking-related settings
* Verbosity
* Terminal display behavior
* Language
* Other configuration preferences

This helped me understand that Claude Code isn't only about prompts and models.

There is an entire environment around the model that I can configure according to how I work.

---

# `/permissions`: Giving Claude the Right Level of Access

This was one of the commands where I became much more cautious.

Claude Code can interact with tools.

For example, it may have tools that allow it to:

* Read files
* Write files
* Execute shell commands
* Search the web
* Interact with other systems

These tools are what make Claude Code much more powerful than a normal chatbot.

But power also means permissions matter.

Claude Code can ask for permission before using certain tools.

For example:

```text
Claude wants to execute a command.
        ↓
      Ask me
        ↓
     I approve
```

But if Claude asks me the same thing repeatedly, that can become frustrating.

So permissions can be configured.

---

# The Permission Model I Learned

The permission interface gives me different ways to control tools.

Conceptually, I can think of them as:

```text
ALLOW
→ Let Claude use this without asking

ASK
→ Ask me before using it

DENY
→ Never allow it
```

This is powerful, but it also means I need to be careful.

For example, I might decide that a harmless tool can always be used.

But I should think carefully before automatically allowing shell commands.

There is a big difference between:

```text
Safe repetitive operation
```

and:

```text
Potentially destructive command
```

So I don't want to blindly add everything to `allow`.

---

# Local, Project, and User-Level Permissions

Another thing that helped me understand Claude Code configuration was the concept of **scope**.

When configuring permissions, I can think about whether the setting applies:

### Locally

Only to me and this particular project.

### Project-wide

To the project and potentially other people working with the same repository configuration.

### User-wide

Across the projects I work on from my machine.

This is very similar to the way I started thinking about `CLAUDE.md`.

Again, the question becomes:

> **Who should this rule affect?**

That simple question makes configuration decisions much easier.

---

# Why Permissions Made Me Think Differently About Claude Code

Before using Claude Code deeply, I mostly thought of an AI assistant as something that generates text.

But Claude Code can actually interact with my development environment.

It can inspect files.

It can modify files.

It can execute commands.

It can retrieve information.

It can use additional tools.

That means I'm not simply giving an AI a prompt anymore.

I'm giving an AI **controlled access to a working environment**.

And once I realized that, permissions stopped feeling like a minor configuration option.

They became part of responsible AI-assisted development.

---

# `/theme`: Making the Environment Comfortable

Some commands are simply about personal experience.

For example:

```text
/theme
```

allows me to change the appearance of Claude Code.

There are different themes and display options available.

This isn't a productivity-changing feature for me, but I like understanding that the environment can be adapted to my preferences.

---

# `/voice`: Moving Beyond Typing

Another interesting feature I explored was voice mode.

With:

```text
/voice
```

I can enable voice interaction.

Instead of typing everything, I can speak my prompt.

For example:

> "Explain the project structure to me."

The idea is simple, but it introduces another way of interacting with Claude Code.

This made me realize that the interface doesn't necessarily have to be limited to typing commands and prompts.

---

# I Don't Need to Memorize Every Slash Command

When I first started learning about slash commands, seeing the number of available commands could feel overwhelming.

But I eventually realized something important:

> **I don't need to memorize everything.**

Claude Code itself provides a list of available commands.

I can type:

```text
/
```

and browse through the available options.

Each command also has a description.

So rather than trying to memorize an entire command reference, I prefer to learn commands as I encounter real problems.

For example:

```text
Need to resume something?
→ /resume

Need to rename a session?
→ /rename

Need a side question?
→ /btw

Need to check usage?
→ /usage

Need to change model?
→ /model

Need to configure permissions?
→ /permissions
```

The command becomes memorable because I associate it with an actual problem I had.

---

# The Bigger Lesson I Took From Slash Commands

After learning all of these commands, I realized that the important part isn't actually remembering the commands.

The important part is understanding **why they exist**.

Claude Code is trying to turn common developer actions into reusable workflows.

And that connects directly with what I learned earlier about `CLAUDE.md`.

With `CLAUDE.md`, I learned:

> **Don't repeatedly explain the same context. Store it as persistent instructions.**

With slash commands, I learned:

> **Don't repeatedly describe the same action. Turn it into a reusable workflow.**

That distinction became very powerful for me.

---

# My Mental Model Started Changing

Initially, I thought about Claude Code like this:

```text
Me
 ↓
Prompt
 ↓
Claude
 ↓
Answer
```

Now I think about it more like this:

```text
                    Claude Code
                         │
        ┌────────────────┼────────────────┐
        │                │                │
     Context          Commands          Tools
        │                │                │
   CLAUDE.md         /resume          File access
   Rules             /rename          Shell
   Memory            /btw             Web
                     /model            MCP
                     /usage
                     /config
```

And all of these pieces work together.

The model is only one part of the system.

The real productivity comes from designing the whole workflow around it.

---

# The Workflow I Would Follow Today

After learning these concepts, my development workflow would look something like this:

```text
Start a task
     ↓
Create / use a dedicated session
     ↓
Rename the session
     ↓
Plan the work
     ↓
Implement the feature
     ↓
Use /btw for unrelated questions
     ↓
Reach a meaningful milestone
     ↓
Create a Git commit
     ↓
Check /usage when needed
     ↓
Continue or finish
     ↓
Resume later if necessary
     ↓
Export important conversations
```

And for larger tasks:

```text
Complex Feature
      ↓
Planning
      ↓
Architecture
      ↓
Specification
      ↓
Implementation
      ↓
Testing
      ↓
Commit
      ↓
Next Milestone
```

This is much more structured than simply opening Claude and starting to type prompts.

---

# What I Learned Beyond the Commands

Looking back, I don't think the biggest takeaway from this learning process was:

> "Here are 20 slash commands."

The bigger lesson was about **workflow design**.

I learned to separate:

* Sessions
* Tasks
* Context
* Models
* Usage
* Permissions
* Configuration
* Reusable workflows

And I learned that each of these exists for a reason.

---

# My Final Mental Model

Today, I think about Claude Code as something closer to a development operating environment than a simple chatbot.

I have:

```text
                    CLAUDE CODE
                         │
        ┌────────────────┼────────────────┐
        │                │                │
     Context          Workflow          Execution
        │                │                │
   CLAUDE.md        Slash Commands      Tools
   Rules            Custom Commands     Bash
   Memory           Skills              Files
   Sessions         Agents              Web
                                      MCP
```

Then around all of that, I have:

```text
Configuration
     │
     ├── Models
     ├── Permissions
     ├── Usage
     ├── Theme
     └── Preferences
```

This is the point where Claude Code started making much more sense to me.

---

# The Main Lesson I Took Away

If I had to summarize my entire learning journey around slash commands in one sentence, it would be:

> **When I notice myself repeatedly explaining or performing the same thing, I should look for a way to turn that repetition into a reusable workflow.**

Sometimes that means a slash command.

Sometimes it means `CLAUDE.md`.

Sometimes it means a rule.

Sometimes it means a skill or an agent.

And sometimes it simply means organizing my sessions better.

The real skill isn't memorizing every Claude Code command.

The real skill is recognizing **patterns in my own development workflow** and then finding a way to make those patterns reusable.

That is what made slash commands much more interesting to me than just a collection of shortcuts.

They represent a larger idea:

> **The more repetitive my workflow becomes, the more opportunities I have to automate it.**

And that is probably one of the biggest reasons Claude Code becomes more powerful the longer I use it.


[← Back to Main README](../README.md)