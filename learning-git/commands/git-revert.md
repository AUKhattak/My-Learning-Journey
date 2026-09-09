[← Back to Main README](../README.md)

# git revert

After learning `git reset`, the next Git command that made a lot more sense to me was **`git revert`**.

At first, I honestly thought:

> "Why do I need another command to undo something?"

I already had:

```bash
git reset
```

So when I first saw:

```bash
git revert
```

I assumed they were basically the same thing.

They're not.

The biggest difference I learned is that **`git reset` moves history backward, while `git revert` creates a new commit that undoes an earlier commit.**

That small difference becomes really important when working with shared repositories.

## My First Understanding of Revert

Let's say my Git history looks like this:

```text
A --- B --- C
```

And commit `C` introduced a bug.

With `reset`, I could move my branch back:

```text
A --- B
      ↑
    HEAD
```

But with `revert`, Git does something different.

It creates another commit:

```text
A --- B --- C --- D
              ↑
          "undo C"
```

Commit `D` contains the changes needed to undo what happened in `C`.

So the history stays intact.

That's the part that made `git revert` click for me.

I'm not deleting the mistake from history.

I'm basically telling Git:

> **"I know this commit happened, but I want to undo its changes."**

## A Simple Example

Suppose I have:

```bash
git log --oneline
```

and see:

```text
8a21abc add new payment flow
4c52def update navbar
91f3a12 add user profile
```

Then I realize:

> "That payment flow is broken and needs to be removed."

Instead of resetting the branch, I can run:

```bash
git revert 8a21abc
```

Git creates a new commit that reverses the changes introduced by `8a21abc`.

My history becomes something like:

```text
91f3a12 add user profile
4c52def update navbar
8a21abc add new payment flow
b71c9de Revert "add new payment flow"
```

Nothing disappeared from the history.

Anyone looking at the repository can still see that the payment flow was added and then reverted.

And honestly, I like this because the history tells the actual story.

Someone can look at it later and understand:

> "We added this, something went wrong, and then we reverted it."

## Why Revert Is Useful on Shared Branches

This is where `git revert` became especially useful to me.

Imagine I have already pushed my changes to `main`.

```text
A --- B --- C
          ↑
        main
```

Other people may already have pulled `C`.

If I use `git reset` and move `main` back to `B`, I'm changing the history that other people already have.

That can become messy.

But with:

```bash
git revert C
```

I get:

```text
A --- B --- C --- D
              ↑
            main
```

Now everyone can continue working with the same history.

The bad changes from `C` are undone by `D`, but the commit itself remains part of the history.

That's one of the main reasons I think of `revert` as the safer choice for **already-shared commits**.

## Revert Doesn't Mean "Erase"

This was probably the most important thing I learned.

When I run:

```bash
git revert <commit>
```

Git isn't going back in time and pretending the commit never happened.

Instead, Git creates a new commit.

So if I have:

```text
A --- B --- C
```

and revert `C`, I get:

```text
A --- B --- C --- D
```

where `D` is effectively the opposite of `C`.

That means `git revert` is a form of **undoing through a new commit**, rather than rewriting the existing history.

## What If I Revert an Older Commit?

This is where things can get a little more interesting.

Suppose I have:

```text
A --- B --- C --- D --- E
```

and I decide that `B` introduced something I don't want anymore.

I can still run:

```bash
git revert B
```

Git will try to create a new commit that reverses the changes from `B`.

But now Git has to apply that reversal on top of `E`.

If later commits depend on the changes from `B`, conflicts can happen.

So `git revert` doesn't magically remove a commit without consequences.

It creates the inverse changes and tries to apply them to the current state.

Sometimes that works perfectly.

Sometimes Git basically looks at me and says:

> "You're going to have to figure this one out yourself." 😄

## Revert Can Also Have Conflicts

Just like merging, reverting can result in conflicts.

For example:

```bash
git revert abc123
```

might produce a conflict if the code changed significantly after the commit I'm trying to revert.

At that point, I need to resolve the conflict, stage the resolved files, and continue the revert.

The exact workflow can vary, but the important thing for me is understanding that:

> **Revert creates changes. It doesn't simply move a pointer backward.**

That makes it easier to understand why conflicts are possible.

## Reset vs Revert

This is how I remember the difference now:

```text
git reset
    ↓
Move the branch backward

git revert
    ↓
Create a new commit that undoes an old commit
```

For example:

```text
RESET

A --- B --- C
          ↓
       reset
          ↓
A --- B
```

While:

```text
REVERT

A --- B --- C
              ↓
           revert C
              ↓
A --- B --- C --- D
```

That visual difference helped me understand the commands much better than simply memorizing their definitions.

## When I Usually Use Revert

I mostly think about `git revert` when the commit has **already been shared**.

For example:

* A bad commit was pushed to `main`.
* A feature caused a production problem.
* A change needs to be temporarily undone.
* I want to undo a commit without rewriting the existing history.
* Other developers already have the commits I'm trying to undo.

In those situations, creating a new commit is usually much safer than rewriting history.

## Reverting Doesn't Mean the Work Is Lost

Another thing I like about `git revert` is that the original commit is still there.

For example:

```text
A --- B --- C --- D
```

where:

```text
C = add new feature
D = revert new feature
```

The feature isn't necessarily gone forever.

Later, I might decide that the original feature was actually useful after all.

I can make the necessary changes and introduce it again, or potentially revert the revert.

Git doesn't forget what happened.

It keeps the history and records the decision to undo it.

## The Way I Think About It Now

If I'm working locally and haven't shared my commits yet, I might use:

```bash
git reset
```

because I may want to clean up my history.

If a commit has already been pushed and other people may depend on that history, I usually think:

```bash
git revert
```

because I don't want to rewrite something everyone else is already using.

So the simple rule I keep in my head is:

> **Reset changes the history. Revert adds to the history.**

Once I understood that difference, `git revert` stopped feeling like another confusing Git command.

It became a much more natural way of saying:

> "This change was already part of our history. I don't want its effects anymore, so let's create a new commit that undoes it."

And unlike my old habit of making another copy of the project whenever something went wrong, Git gives me a proper record of what happened. 😄


[← Back to Main README](../README.md)