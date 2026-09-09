[← Back to Main README](../README.md)

# git rebase vs reset vs revert — choosing the right command

Once I started learning `git reset`, `git revert`, and `git rebase`, I realized that all three can feel like they're doing the same thing:

> "I don't like my current history. Let me fix it." 😄

But they solve very different problems.

The biggest question I ask now is:

**Am I rewriting history, creating a new history entry, or moving my commits somewhere else?**

That question usually tells me which command I need.

## The Simple Difference

This is the mental model I use:

```text
git reset
    ↓
Move the branch backward
Rewrite history

git revert
    ↓
Create a new commit that undoes another commit
Keep history

git rebase
    ↓
Move/replay my commits onto a new base
Rewrite history
```

Git's own documentation describes `reset` as moving the branch tip, `revert` as creating new commits that reverse earlier commits, and `rebase` as replaying commits onto another base.

That distinction became much more important once I started working with other people.

## `git reset` — "I Want to Go Back"

I mostly use `reset` when I'm working **locally** and realize that my recent history isn't what I want.

For example:

```text
A --- B --- C --- D
              ↑
            HEAD
```

Maybe `D` was a mistake.

I can do:

```bash
git reset HEAD~1
```

and move my branch back:

```text
A --- B --- C
          ↑
        HEAD
```

Depending on whether I use `--soft`, `--mixed`, or `--hard`, my changes can remain staged, remain unstaged, or be discarded.

The important part is that **the branch pointer moves backward**.

That's why I think of reset as a **local cleanup tool**.

If I haven't pushed my commits yet, I can use it to clean up mistakes without making everyone else deal with my messy history.

### The tradeoff

The nice thing:

> I can clean up my history.

The dangerous thing:

> **I am rewriting history.**

So if I've already pushed those commits and other people are working from them, reset can create a lot of confusion.

That's why I'm much more comfortable using:

```bash
git reset
```

on my own local branch than on a shared branch.

---

## `git revert` — "I Want to Undo It Without Rewriting History"

This is where `revert` feels very different.

Suppose I have:

```text
A --- B --- C
```

and `C` introduced a bug.

Instead of moving back to `B`, I can:

```bash
git revert C
```

Git creates a new commit:

```text
A --- B --- C --- D
              ↑
          revert C
```

`D` contains the changes needed to reverse `C`.

So the original commit stays in the history.

That's why I think of `revert` as:

**"I don't want this change anymore, but I still want the history to show that it happened."**

Git's documentation explicitly describes `revert` as creating new commits that reverse the effects of earlier commits.

### The tradeoff

The good thing:

> **It doesn't rewrite the existing history.**

That makes it much safer for commits that have already been shared.

The downside is that my history gets another commit.

So instead of hiding the mistake:

```text
A --- B
```

I have:

```text
A --- B --- C --- D
```

where `D` explains that `C` was undone.

Personally, I prefer that on shared branches because the history tells the story.

---

## `git rebase` — "I Want a Cleaner History"

Rebase was probably the one that confused me the most.

Suppose I have:

```text
A --- B --- C --- F
          \
           D --- E
```

My feature branch has `D` and `E`, but `main` has moved forward to `F`.

I can rebase my branch:

```bash
git rebase main
```

Git takes my commits and **replays them on top of the newer base**:

```text
A --- B --- C --- F --- D' --- E'
```

The original `D` and `E` are replaced by new commits `D'` and `E'`.

Git describes rebase as transplanting a series of commits onto another starting point and replaying those commits.

This can give me a much cleaner, more linear history.

### The tradeoff

The benefit:

**Cleaner and easier-to-follow history.**

The cost:

**It rewrites history.**

That's why I became very careful about rebasing commits that have already been shared.

If someone else has based their work on my original commits, rewriting them means we're no longer talking about exactly the same commit history.

So my general rule became:

**Rebase my own unpublished work. Don't casually rebase shared history.**

---

## So Which One Do I Use?

This is the little decision tree I use now:

```text
Did I make a local mistake?
        ↓
     reset

Did I already share the commit
and want to undo it?
        ↓
     revert

Do I want to clean up my own
branch history?
        ↓
     rebase
```

Or even shorter:

```text
reset  → move backward
revert → undo with a new commit
rebase → replay commits on a new base
```

## What Rewrites History?

This was one of the most important things for me to understand.

| Command      | Rewrites history? | Typical use                         |
| ------------ | ----------------- | ----------------------------------- |
| `git reset`  | **Yes**           | Local cleanup                       |
| `git revert` | **No**            | Undo shared changes                 |
| `git rebase` | **Yes**           | Clean up / reorganize local history |

So when I see:

```text
reset → rewrite
rebase → rewrite
revert → preserve
```

I immediately know which commands I need to be more careful with.

## My Best-Practice Rule

The simplest rule I've come to follow is:

**If the commits are only mine, I can rewrite them. If other people depend on them, I usually preserve them.**

So on my own feature branch:

```bash
git reset
git rebase
```

can be perfectly reasonable.

But once something is shared on:

```text
main
staging
team branches
```

I'm much more cautious about rewriting history.

If I need to undo something that has already been shared:

```bash
git revert
```

is usually the safer choice.

Of course, the exact workflow depends on the team's Git conventions, but this rule has helped me avoid a lot of unnecessary trouble.

## The Way I Think About Them Now

I don't really memorize the commands anymore.

I think about **what I want to happen to the history**.

If I want to say:

> "Forget that I made these commits."

I think:

```bash
git reset
```

If I want to say:

> "That commit happened, but I want to undo its changes."

I think:

```bash
git revert
```

And if I want to say:

> "Take my commits and put them on top of this newer base."

I think:

```bash
git rebase
```

The biggest lesson for me was that **rewriting history isn't automatically bad**.

It's incredibly useful when I'm cleaning up my own work.

The problem starts when I rewrite history that other people are already depending on.

So now, before using any of these commands, I ask myself one simple question:

**"Has anyone else already built their work on top of this history?"**

If the answer is no, I have much more freedom.

If the answer is yes, I slow down.


[← Back to Main README](../README.md)