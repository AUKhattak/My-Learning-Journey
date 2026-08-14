# git cherry-pick

Another Git command that took me a little while to understand was **`git cherry-pick`**.

The name itself didn't help me much.

When I first heard "cherry-pick", I had no idea what that was supposed to mean in Git. 😄

Eventually, I learned that the idea is actually pretty simple:

> **Take a specific commit from somewhere else and apply it to my current branch.**

That became useful when I had a change on one branch that I wanted on another branch, but I didn't want to merge the entire branch.

## My First Understanding

Let's say I have two branches:

```text
main
  A --- B --- C

feature
       \
        D --- E
```

Commit `D` contains a small bug fix that I really want in `main`.

But `E` contains other work that isn't ready yet.

If I merge the whole `feature` branch, I'd get both:

```text
D
E
```

But that's not what I want.

I only want the change from `D`.

That's where `git cherry-pick` comes in.

I can switch to `main` and run:

```bash
git cherry-pick D
```

Git takes the changes introduced by `D` and creates a new commit on my current branch.

The history becomes something like:

```text
main
  A --- B --- C --- D'
                    ↑
                  main

feature
       \
        D --- E
```

The important part is that `D'` is **not the exact same commit** as `D`.

Git applies the changes from `D` and creates a new commit on `main`.

So I now have the same change in both branches, but represented by different commits.

## Why Would I Need This?

At first, I thought:

> "Why wouldn't I just merge the branch?"

And sometimes, I absolutely should merge the branch.

But sometimes a branch contains several changes and I only need one of them.

For example:

```text
feature
   |
   ├── add login page
   ├── fix login validation
   ├── redesign dashboard
   └── experiment with animations
```

Maybe the login validation fix is important, but the rest of the feature isn't ready.

Instead of merging everything, I can cherry-pick just the relevant commit.

```bash
git cherry-pick <commit>
```

Now I can get the fix without bringing the unfinished work with it.

That's when the command started making sense to me.

## A Realistic Example

Imagine I'm working on a feature branch:

```text
feature/payment
```

And I make several commits:

```text
a12f3d add payment form
b82c91 fix payment validation
c71a20 redesign checkout page
d45e12 experiment with new animation
```

Then someone discovers that the payment validation bug also exists on `main`.

The complete payment feature isn't ready to go into `main`.

But the validation fix is.

So I can switch to `main`:

```bash
git switch main
```

and cherry-pick the specific commit:

```bash
git cherry-pick b82c91
```

Now `main` gets that fix without getting the other three commits.

That's a pretty powerful idea.

## Cherry-Pick Is Not Copying a Commit

This was another small detail that took me some time to understand.

When I run:

```bash
git cherry-pick b82c91
```

Git isn't literally moving commit `b82c91` from one branch to another.

Instead, Git looks at what that commit changed and applies those changes to my current branch.

Then Git creates a new commit.

So conceptually:

```text
Original branch:

A --- B --- C
          \
           D


Current branch:

A --- B --- E
```

After cherry-picking `D`:

```text
A --- B --- E --- D'
```

`D'` contains the changes from `D`, but it is a new commit.

This is why the commit hash will be different.

## Cherry-Pick vs Merge

This is where I found it useful to compare the two.

With a merge, I'm basically saying:

> **"Bring this branch's work into my branch."**

With cherry-pick, I'm saying:

> **"I only want this particular commit."**

For example:

```text
MERGE

feature
   D --- E --- F
  /
main
A --- B
```

A merge brings the branch history together.

But with cherry-pick:

```text
feature
   D --- E --- F

main
A --- B --- D'
```

I can take only `D`.

That's why I think of cherry-pick as a **selective way of moving changes between branches**.

## Cherry-Picking Multiple Commits

I'm not limited to one commit either.

I can cherry-pick multiple commits:

```bash
git cherry-pick abc123 def456
```

Or, depending on the situation, a range of commits can be selected.

But this is where I started becoming a little more careful.

If the commits depend heavily on each other, cherry-picking them individually can get complicated.

For example:

```text
commit A → creates something
commit B → modifies it
commit C → depends on B
```

Cherry-picking only `C` might not make much sense because the changes from `A` and `B` aren't there.

So I learned that cherry-pick is most useful when I understand **what the commit actually contains and what it depends on**.

## Cherry-Pick Can Have Conflicts

Just like merging and reverting, cherry-picking can also cause conflicts.

For example:

```bash
git cherry-pick abc123
```

Git might try to apply the changes and discover that the same part of the code has changed differently on my current branch.

Then I have to resolve the conflict.

After fixing the files, I can continue:

```bash
git add .
git cherry-pick --continue
```

Or if I realize I shouldn't have started the cherry-pick in the first place:

```bash
git cherry-pick --abort
```

That gives me a way to back out of the operation if things become messy.

## One Situation Where I Really Like Cherry-Pick

One of the most useful situations I've found is when a **small bug fix needs to go into multiple branches**.

Imagine we have:

```text
main
staging
feature/new-version
```

And I make a small fix on one branch:

```text
fix: handle empty username
```

That fix might be needed in `main` and `staging`, even though the rest of the branch isn't ready.

Instead of merging a whole branch just to get one small fix, I can cherry-pick that commit where I need it.

For example:

```bash
git switch main
git cherry-pick abc123
```

Then:

```bash
git switch staging
git cherry-pick abc123
```

Now both branches have the fix.

That's one of those situations where the command suddenly feels very practical.

## But I Don't Want to Cherry-Pick Everything

Once I learned about cherry-pick, it was tempting to use it whenever I wanted changes from another branch.

But I've learned that it shouldn't become my default way of moving work around.

If an entire feature branch should become part of `main`, a merge or rebase-based workflow usually makes more sense.

Cherry-pick is more useful when I need **specific changes**.

For me, the question is basically:

> "Do I want this whole branch, or do I only want this particular commit?"

If I want the whole branch:

```text
merge / rebase
```

If I only want one specific change:

```text
cherry-pick
```

That mental model makes the decision much easier.

## The Way I Think About It Now

The easiest way I remember `git cherry-pick` is:

```text
Another branch has something useful.

I don't want the whole branch.

I just want this one commit.

→ cherry-pick it.
```

So if my history looks like:

```text
feature
   D --- E --- F

main
A --- B --- C
```

and I only want `E`, I can do:

```bash
git switch main
git cherry-pick E
```

and Git gives me:

```text
feature
   D --- E --- F

main
A --- B --- C --- E'
```

The change from `E` is now part of `main`, without bringing `D` and `F` along with it.

That's what finally made the name **cherry-pick** make sense to me.

I'm basically standing in one branch, looking at a bunch of commits, and saying:

> **"I don't want the whole basket. I'll just take this one."** 🍒

And honestly, that's a pretty good description of what the command does.
