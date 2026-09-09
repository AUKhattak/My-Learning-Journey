[← Back to Main README](../README.md)

# LLMs Are Powerful But Need to Be Controlled

LLMs are incredibly powerful. They have vast knowledge and can understand complex tasks. But they also have problems. They're unpredictable, stateless, and sometimes hallucinate.

The same way a horse has raw power but needs a harness to become useful for pulling a carriage, an LLM needs a harness to become useful for software engineering.

**A raw horse** → Powerful but uncontrolled  
**A harness** → Controls and directs the power  
**A horse and carriage** → Useful and reliable

**A raw LLM** → Powerful but uncontrolled  
**A coding harness** → Controls and directs the power  
**A coding agent** → Useful and reliable

This is exactly what Claude Code is a **coding harness** built on top of Claude's LLM.

The harness handles:
- Reading the file system
- Executing commands
- Managing conversation history
- Handling tool calls
- Managing memory
- Spawning sub-agents
- And much more

The harness is **deterministic**—it always does exactly what it's told. The LLM is **probabilistic**—it can be unpredictable and make mistakes.

This combination a predictable harness and an unpredictable LLM is where the need for hooks arises.

---

## The Problem Hooks Solve

The relationship between the harness and the LLM is like a boss-employee relationship:

**The LLM is the boss** → Gives instructions  
**The harness is the employee** → Faithfully executes instructions

The harness always executes instructions exactly as given. But the LLM can sometimes give dangerous instructions.

I put safety rules in my CLAUDE.md file "don't delete files," "don't modify .env files," "always use parameterized queries." 98% of the time, the LLM follows these rules.

But there's a 2% chance. When the context window is full or the task is complex, the LLM might forget or override my instructions.

In that 2% scenario, the LLM might say: "Delete the database file" or "Modify the .env file." And the harness, being obedient, would execute these dangerous commands.

**This is the problem hooks solve.** They let me enforce rules at the harness level, not just at the instruction level.

---

## The Agent Loop and Session Lifecycle

To understand hooks, I needed to understand two more concepts:

### The Agent Loop

When I give Claude Code a task, it's rarely a one-shot operation. Instead, it's a multi-step process:

1. I submit a prompt
2. The LLM decides the next action (e.g., "I need to read app.py")
3. A tool call goes from the LLM to the harness
4. The harness executes the tool (reads the file)
5. The content goes back to the LLM
6. The LLM decides the next action
7. This continues until the task is complete

This entire sequence is the **agent loop**.

### The Session Lifecycle

On a higher level, there's another loop—the session lifecycle:

```
Session Start
   ↓
User Submits Prompt
   ↓
Agent Loop Starts
   ↓
Pre Tool Use Event
   ↓
Tool Execution
   ↓
Post Tool Use Event
   ↓
Agent Loop Ends
   ↓
Stop Event
   ↓
Session End
```

At each of these points, the harness generates events. And these events are exactly where hooks come in.

---

## What Hooks Actually Are

Finally, with all the context in place, I could understand the definition:

> **Hooks are custom scripts written by the programmer that the harness automatically executes at specific events during a session's lifecycle.**

In simple terms: Hooks are scripts I write. I connect them to specific events in the session lifecycle. When those events happen, the harness automatically runs my scripts.

### How Hooks Work

**1. Configure the Hook** → I create a `settings.json` file in `.claude/` and define my hooks.

**2. The Session Lifecycle Runs** → The harness goes through its normal flow.

**3. An Event Occurs** → At certain points (like "pre-tool-use"), the harness checks for hooks.

**4. The Hook Scripts Run** → If a hook is configured for that event, the harness executes the script.

**5. The Hook Returns a Response** → The script returns exit codes: 0 = continue, 2 = stop.

**6. The Harness Acts** → Based on the exit code, the harness either continues or stops the operation.

### The Structure of a Hook

A hook has three main parts:

**Event** → What event triggers this hook?
- `PreToolUse` - Before a tool is used
- `PostToolUse` - After a tool is used
- `SessionStart` - When a session starts
- `SessionEnd` - When a session ends
- `Stop` - When the LLM stops working

**Matcher** → Acts like a filter. Narrow down when the hook should run:
- Only for `bash` commands
- Only for `write` operations
- Only for `read` operations

**Action** → What should happen when the hook triggers?
- Run a Python script
- Run a shell command
- Perform some validation

---

## The Use Cases I Learned

### 1. Auto-Formatting and Linting

When Claude writes code, formatting might not be consistent across different sessions. In one session, it might add spaces around equals signs. In another, it might not.

**The solution:** Set up a hook on `PostToolUse` that runs a formatter (like `black` for Python) every time Claude finishes editing a file.

```json
{
  "Event": "PostToolUse",
  "Matcher": "write|edit",
  "Action": "Run 'black {file_path}'"
}
```

Now every time Claude writes code, it's automatically formatted consistently.

**Linting** catches actual problems:
- Unused imports
- Undefined variables
- Unreachable code
- Bare except clauses

### 2. Blocking Dangerous Shell Commands

What if the LLM decides to delete my database file? Or modify my `.env` file?

**The solution:** Set up a hook on `PreToolUse` that checks for dangerous commands.

```json
{
  "Event": "PreToolUse",
  "Matcher": "bash",
  "Action": "Run a script that checks the command"
}
```

If the command tries to delete the database, the script returns exit code 2, and the harness blocks the operation.

### 3. Protecting Sensitive Files

I can protect specific sensitive files:
- `.env` files (containing API keys)
- Database files (containing user data)
- Migration files (critical for database structure)

**The solution:** A `PreToolUse` hook that checks if the command targets any sensitive file and blocks it.

### 4. Notifications

If I'm running a 10-minute task, I don't want to stare at the terminal waiting.

**The solution:** A hook on the `Stop` event that sends me a push notification.

```json
{
  "Event": "Stop",
  "Action": "Send a notification via a service"
}
```

Now I get a notification on my phone when Claude finishes its work.

---


### Creating the Hook

I created two hooks in my `settings.json` file inside `.claude/`:

**Hook 1: Auto-Formatting**

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "write|edit",
        "hooks": [
          {
            "type": "command",
            "command": "black {file_path}"
          }
        ]
      }
    ]
  }
}
```

**Hook 2: Protecting Sensitive Files**

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "bash",
        "hooks": [
          {
            "type": "command",
            "command": "python .claude/block_dangerous.py"
          }
        ]
      }
    ]
  }
}
```


### The Difference Between Instructions and Enforcement

**CLAUDE.md instructions** are like telling a child: "Don't run in the street." 98% of the time, the child listens. 2% of the time, the child gets excited and runs anyway.

**Hooks** are like building a fence. The child can't run in the street even if they want to.

---

## My Final Mental Model for Hooks

Today, I think about hooks as safety and automation layers:

```
                ┌─────────────────────────────────┐
                │      Session Lifecycle         │
                │  (Events happen in sequence)   │
                └─────────────┬───────────────────┘
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
            ▼                 ▼                 ▼
    ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
    │  Session    │   │ Pre-Tool    │   │ Post-Tool   │
    │   Start     │   │    Use      │   │    Use      │
    └──────┬──────┘   └──────┬──────┘   └──────┬──────┘
           │                 │                 │
           ▼                 ▼                 ▼
    ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
    │   Hooks     │   │   Hooks     │   │   Hooks     │
    │  Execute    │   │  Execute    │   │  Execute    │
    └─────────────┘   └─────────────┘   └─────────────┘
                              │
                              ▼
                ┌─────────────────────────────────┐
                │     Safety & Automation         │
                │                                 │
                │ ┌─────────────────────────────┐ │
                │ │ Block dangerous commands    │ │
                │ │ Auto-format code           │ │
                │ │ Lint code                  │ │
                │ │ Send notifications         │ │
                │ │ Generate session summaries │ │
                │ │ Capture telemetry          │ │
                │ └─────────────────────────────┘ │
                └─────────────────────────────────┘
```


---


[← Back to Main README](../README.md)