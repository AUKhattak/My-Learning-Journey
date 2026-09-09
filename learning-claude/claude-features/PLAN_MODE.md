[← Back to Main README](../README.md)

# My Learning Journey: Planning Mode and Database Setup with Claude Code

When I first started building features with Claude Code, I thought the process was simple: give Claude a prompt, get code back, and move on.

I was wrong.

The more I worked on actual features, the more I realized that jumping straight into code was causing more problems than it solved. I'd get code that technically worked, but it didn't always fit my project structure. It missed edge cases. It sometimes created more work than it saved.

That's when I discovered something that completely changed my workflow: **Plan Mode**.

---

## The First Thing I Learned: Plan Mode Isn't Just About Planning

My first understanding of Plan Mode was quite basic. I thought it was just a way to get Claude to think before writing code.

But as I used it more, I realized its real value is much deeper.

**Plan Mode is actually a way to separate thinking from doing.**

When I activate Plan Mode, Claude Code spawns multiple agents that explore my entire codebase. They read files. They understand the architecture. They identify existing patterns.

And here's the most important thing I learned:

> **During Plan Mode, Claude can only read files. It cannot write or change anything.**

This was a game-changer for me.

I finally had a way to get Claude to deeply understand my project before making any changes. No more guessing. No more assumptions. Just pure analysis and understanding.

---

## How I Learned to Activate Plan Mode

I discovered there are two ways to enter Plan Mode:

**Method 1: The Keyboard Shortcut**
- Simply press Shift+Tab twice
- A notification appears: "Plan Mode On"

**Method 2: The Slash Command**
- Type `/plan` and hit Enter
- Same result, different approach

I found myself using the keyboard shortcut more often because it was faster, but both methods work exactly the same way.

---

## What I Learned About the Planning Process

The first time I used Plan Mode, I was honestly amazed at what happened next.

When I entered my prompt and hit Enter, I watched Claude Code start exploring. It was reading files across my entire project. It was understanding the structure. It was identifying patterns.

I could see it thinking:

> "I need to understand the existing database setup."
> "Let me check the current db.py file."
> "I should see what's already in app.py."
> "Let me understand the project architecture."

And then, after all that exploration, it would generate a detailed implementation plan.

The plan included:
- Exactly which files needed to change
- What specific code needed to be added
- Where dependencies existed
- Potential edge cases to handle
- How the new code would fit into the existing architecture

This wasn't just a rough outline. This was a **detailed, executable roadmap**.

---

## The Critical Constraint I Had to Remember

One of the most important things I learned about Plan Mode is this:

**During Plan Mode, I cannot perform any write operations.**

This means:
- No changing files
- No creating new files
- No saving the plan automatically (I had to save it manually)

At first, this felt restrictive. Why can't Claude just write the plan for me?

But then I understood the genius behind this design.

By preventing any writes during planning, Claude Code forces me to **review and validate** before implementation. I can't just let Claude run wild and make changes without oversight.

This saved me countless times. I caught mistakes in the plan that would have been much harder to fix after the code was already written.

---

## What I Learned About the Plan Review Process

After Claude generated the plan, I learned that I shouldn't just blindly implement it.

Instead, I needed to:

**1. Read the entire plan carefully**
I learned to go through every section. Understand what would change and why.

**2. Validate against my requirements**
I learned to check if the plan actually solved what I needed. Did it address all the acceptance criteria? Did it handle edge cases?

**3. Look for potential issues**
I learned to spot problems before they became code. Did the plan suggest changing files that shouldn't be touched? Did it introduce unnecessary complexity?

**4. Ask for clarification when needed**
I learned that I could go back and ask Claude to explain parts of the plan I didn't understand. Sometimes the plan was good, but I needed more context.

This review process became one of the most valuable parts of my workflow.

---

## Then I Discovered: Model Selection Matters for Planning

As I got more comfortable with Plan Mode, I learned that the model I used made a big difference in plan quality.

Claude Code offers models such as:

- **Haiku**: Fast and cheap, but less capable for complex planning
- **Sonnet**: The balanced option, good for most tasks
- **Opus**: The most powerful, best for complex planning

I learned that for simple features, Sonnet works perfectly fine. But for complex planning tasks, like when multiple files need to change or when I need deep architectural reasoning I should switch to Opus.

The trade-off is clear:

```
Haiku/Sonnet
→ Faster, cheaper
→ Good for simple plans

Opus
→ More capable, better reasoning
→ More expensive, consumes more tokens
→ Best for complex planning
```

I learned that many experienced developers use Opus specifically for planning and then switch to Sonnet for actual implementation. That's an approach I started adopting for complex features.

To switch models, I use the `/model` slash command.

---

## The Extended Thinking Revolution

This was one of the most fascinating concepts I learned.

Normally, when I ask Claude a question, it starts generating answers immediately—token by token, word by word. This works fine for simple questions.

But for complex planning tasks, this approach can fail.

I learned about **Extended Thinking Mode**, and it completely changed how I use Plan Mode.

Here's how I think about it:

**Normal Mode**
```
Question → Immediate Answer
(Claude starts talking right away)
```

**Extended Thinking Mode**
```
Question → Reasoning Phase → Answer
(Claude thinks first, then answers)
```

I learned that Extended Thinking Mode is like giving Claude a whiteboard. Before answering, Claude writes down its internal thoughts, explores different approaches, and only then produces the final answer.

This dramatically improves the quality of plans.

I learned to always enable Extended Thinking when using Plan Mode. The slight extra token cost was absolutely worth the improvement in plan quality.

To enable it, I use `/config` and toggle the thinking mode to `true`.

---

## The Effort Level Concept

Another important concept I learned about was **Effort Level**.

Effort Level controls how many tokens Claude can spend on its reasoning phase. It's directly related to Extended Thinking.

I learned about four effort levels:

**Low**
→ Very limited thinking time
→ 500-1000 tokens for reasoning
→ Fast but lower quality plans

**Medium**
→ Moderate thinking time
→ Balanced approach
→ Good for most planning tasks

**High**
→ Extensive thinking time
→ More tokens for reasoning
→ Higher quality plans

**Max**
→ Unlimited thinking tokens
→ Available only with Opus
→ Best for extremely complex planning

I learned that I should think of Effort Level as a budget for thinking. More thinking usually means better plans, but it also costs more tokens.

For most of my work, I found Medium to High is the sweet spot. Max is overkill unless I'm working on something genuinely complex.

I can adjust Effort Level using the `/effort` command, choosing from Low, Medium, High, Max, or Auto.

---

## The Ultra Plan Discovery

Just when I thought I understood Plan Mode completely, I discovered something even more powerful: **Ultra Plan**.

Ultra Plan is a new feature. And it's a game-changer.

When I trigger Ultra Plan with `/ultra-plan`, something amazing happens:

1. Anthropic starts a container in the cloud
2. An Opus 4.6 model instance runs inside that container
3. My plan is developed in the cloud environment
4. I get a rich web editing experience
5. I can edit and refine the plan there
6. When satisfied, I can "teleport" it back to my local machine

I learned that Ultra Plan is for when regular Plan Mode isn't giving me satisfactory results. It's more powerful, but also more expensive.

My rule of thumb became:

> **Use regular Plan Mode for most tasks. Switch to Ultra Plan only for genuinely complex features.**

The workflow I learned for Ultra Plan:

```
/ultra-plan [prompt]
       ↓
Cloud session starts
       ↓
Plan is developed in web interface
       ↓
Review and edit if needed
       ↓
"Teleport" back to local machine
       ↓
Implement using normal Plan Mode workflow
```

---

## What I Learned About the Complete Planning Workflow

After all this learning, I developed a complete workflow for planning and implementing features:

### Step 1: Create a Spec Document
```
Write requirements clearly
Define acceptance criteria
Create in .claude/specs/
```

### Step 2: Enter Plan Mode
```
Shift+Tab twice (or /plan)
Enable Extended Thinking
Set appropriate Effort Level
Choose model (Sonnet or Opus)
```

### Step 3: Generate the Plan
```
Provide prompt referencing the spec
Let Claude explore the codebase
Review the generated plan carefully
```

### Step 4: Validate the Plan
```
Check against acceptance criteria
Look for missed edge cases
Verify it fits the architecture
Ask for clarifications if needed
```

### Step 5: Implement the Plan
```
Approve changes one by one
Review each file modification
Test as implementation proceeds
Commit after validation
```

### Step 6: Switch to Ultra Plan if Needed
```
If Plan Mode isn't sufficient
Use /ultra-plan for complex features
Review in web interface
Teleport back to local machine
Continue with normal workflow
```

---

## How My Workflow Changed

The biggest change for me wasn't learning the technical details of Plan Mode.

It was changing **how I think about building features**.

Initially, I thought:

> **"I need code, so let's start writing code."**

Then I moved toward:

> **"I need to plan before writing code."**

And eventually:

> **"I need to separate planning from implementation, use the right tools for each, and treat planning as a valuable phase of development."**

This separation of concerns made my development much more predictable and reliable.

I stopped wasting time on code that needed to be rewritten.

I stopped missing edge cases that only appeared later.

I stopped creating technical debt that had to be fixed.

Instead, I got clean, well-thought-out implementations that worked the first time.

---

## My Final Mental Model for Planning

Today, I think about planning with Claude Code as a layered process:

```
                ┌─────────────────────────┐
                │     Feature Request     │
                │   (What I want to do)   │
                └──────────┬──────────────┘
                           │
                           ▼
                ┌─────────────────────────┐
                │    Spec Document        │
                │ (Requirements & Criteria)│
                └──────────┬──────────────┘
                           │
                           ▼
                ┌─────────────────────────┐
                │    Plan Mode            │
                │  (Think, don't write)   │
                └──────────┬──────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
   ┌─────────┐      ┌─────────┐       ┌─────────┐
   │ Extended│      │  Model  │       │ Effort  │
   │Thinking │      │Selection│       │  Level  │
   └─────────┘      └─────────┘       └─────────┘
                           │
                           ▼
                ┌─────────────────────────┐
                │  Generated Plan         │
                │   (Review & Validate)   │
                └──────────┬──────────────┘
                           │
        ┌──────────────────┴──────────────────┐
        │                                      │
        ▼                                      ▼
┌─────────────────────┐              ┌─────────────────────┐
│  Satisfied?         │      No      │ Ultra Plan          │
│  Proceed to         │──────────────│(Cloud planning)     │
│  Implementation     │              │                     │
└─────────────────────┘              └─────────────────────┘
        │                                      │
        └──────────────────┬───────────────────┘
                           │
                           ▼
                ┌─────────────────────────┐
                │   Implementation        │
                │  (Write the code)       │
                └──────────┬──────────────┘
                           │
                           ▼
                ┌─────────────────────────┐
                │    Validation           │
                │  (Test & Verify)        │
                └──────────┬──────────────┘
                           │
                           ▼
                ┌─────────────────────────┐
                │    Commit & Merge       │
                │  (Share the feature)    │
                └─────────────────────────┘
```

---

## The Main Lessons I Took Away

If I had to summarize everything I learned about planning with Claude Code, here are the key principles:

### 1. Separate Planning from Implementation
Don't let Claude write code until it has a solid plan. This separation prevents mistakes and produces better results.

### 2. Enable Extended Thinking for Planning
Always enable Extended Thinking when using Plan Mode. The extra thinking time dramatically improves plan quality.

### 3. Choose the Right Model
Use Sonnet for simple planning. Use Opus for complex, multi-file features. The model choice matters.

### 4. Review and Validate Every Plan
Don't just blindly implement. Read the plan. Check it against your requirements. Catch mistakes early.

### 5. Use Ultra Plan for Complex Features
When regular Plan Mode isn't enough, Ultra Plan is there. But use it sparingly—it's more expensive.

### 6. Save Your Plans
I learned to save plans in `.claude/plans/` so I can reference them later. Documentation matters.

### 7. Iterate If Needed
Sometimes the first plan isn't perfect. I learned to iterate—refine the plan, run it again, get better results.

### 8. Plan Documents Are Part of the Project
I learned to treat spec documents and plans as project artifacts, not temporary files. They're valuable documentation for the future.

---

[← Back to Main README](../README.md)