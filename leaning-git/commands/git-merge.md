[← Back to Main README](../Readme.md)

# git merge

After learning about `git fetch`, the next command that started making much more sense to me was **`git merge`**.

Before I really understood Git, I thought merging was basically:

> "Take two branches and smash them together." 😄

And honestly, sometimes it feels exactly like that when there are conflicts.

But the basic idea is actually pretty simple.

**Merging is how I bring the changes from one branch into another branch.**

## My First Understanding

Let's say I have a `main` branch:

```text
A --- B --- C
          ↑
        main
```

Then I create a feature branch:

```text
A --- B --- C
          \
           D --- E
                ↑
             feature
```

I've finished the feature, and now I want those changes in `main`.

First, I switch to the branch that should receive the changes:

```bash
git switch main
```

Then I merge the feature branch:

```bash
git merge feature
```

Git takes the changes from `feature` and brings them into `main`.

Depending on the history, Git may create a merge commit, or it may simply move the branch forward.

That's something I didn't understand at first.

**A merge doesn't always create a merge commit.**

## The Simple Case: Fast-Forward

Suppose my history looks like this:

```text
A --- B --- C
          \
           D --- E
                ↑
             feature
```

And `main` hasn't changed since I created the feature branch.

When I run:

```bash
git switch main
git merge feature
```

Git can simply move `main` forward:

```text
A --- B --- C --- D --- E
                    ↑
                  main
```

This is called a **fast-forward merge**.

There isn't really anything complicated for Git to combine.

`main` was simply behind `feature`, so Git can move the pointer forward.

When I first saw this, I thought:

> "Wait... where is the merge commit?"

There isn't one. 😄

Git didn't need one.

## When Both Branches Have Changed

Now let's say `main` continued moving while I was working on the feature:

```text
A --- B --- C --- F
          \
           D --- E
```

Now `main` and `feature` have both moved forward.

If I run:

```bash
git switch main
git merge feature
```

Git needs to combine the two lines of development.

The history may become:

```text
A --- B --- C --- F ------- M
          \               /
           D --- E -------
```

Where `M` is a **merge commit**.

This commit represents the point where the two branches were brought together.

That's when I started to see why Git branches aren't just folders or copies.

They're really different lines of development that can eventually be combined.

## What Branch Am I Merging Into?

This was one of the most important things I learned about `git merge`.

When I run:

```bash
git merge feature
```

Git merges `feature` **into the branch I'm currently on**.

So if I'm on:

```bash
git switch main
```

then:

```bash
git merge feature
```

means:

> "Bring `feature` into `main`."

But if I'm on:

```bash
git switch staging
```

and run:

```bash
git merge feature
```

then I'm bringing `feature` into `staging`.

The command doesn't mean:

> "Merge these two branches."

It's more like:

> **"Take this branch and merge it into where I currently am."**

That little detail is easy to forget and can cause some very confusing moments. 😄

## Merge After Fetch

This is also where `git fetch` started connecting with everything else I've learned.

Suppose someone pushed new commits to `origin/main`.

I can first run:

```bash
git fetch
```

Now I know about the latest remote changes.

If I'm working on another branch and want those changes:

```bash
git merge origin/main
```

Git brings the latest `origin/main` changes into my current branch.

So the workflow can look like:

```bash
git fetch
git merge origin/main
```

I like this approach because I can separate:

```text
fetch
↓
"What's new?"

merge
↓
"Okay, now bring those changes into my branch."
```

It gives me a little more control than blindly pulling everything.

## Merge Conflicts 😅

Of course, everything sounds nice until Git says:

```text
CONFLICT
```

This usually happens when both branches have changed the same part of a file in incompatible ways.

For example, on one branch:

```text
Hello from feature
```

and on another:

```text
Hello from main
```

Git can't confidently decide which version I want.

So it stops the merge and asks me to resolve the conflict.

My job is then to open the affected file and decide what the final version should look like.

After fixing the conflict, I stage the file:

```bash
git add <file>
```

Then I complete the merge:

```bash
git commit
```

Depending on the situation and Git version, Git may also provide commands such as:

```bash
git merge --continue
```

The important thing I learned is:

> **A merge conflict isn't Git failing. It's Git asking me to make a decision that it can't safely make for me.**

Once I started thinking about conflicts that way, they became a lot less scary.

## Aborting a Merge

Sometimes I start a merge and realize:

> "Nope. Not dealing with this right now." 😄

If the merge is still in progress, I can abort it:

```bash
git merge --abort
```

Git attempts to return my working tree and index to the state they were in before the merge started.

This is one of those commands I'm very happy exists.

Instead of manually trying to undo a half-completed merge, I can simply say:

```bash
git merge --abort
```

and step back.

## Merge vs Rebase

Once I learned about merging, I eventually ran into **rebase**.

That's where things started getting interesting.

Both can be used to integrate changes, but they approach history differently.

With merge:

```text
A --- B --- C --- F
          \       \
           D --- E --- M
```

I keep the existing branch history and create a merge point when necessary.

With rebase, Git can move my commits onto the newer base:

```text
A --- B --- C --- F --- D' --- E'
```

So I started thinking about it like this:

```text
merge
→ combine histories

rebase
→ rewrite my commits on top of another base
```

I'm not saying one is always better than the other.

Different teams have different workflows.

For me, the important thing is understanding what each operation actually does before running it.

## Merge Doesn't Delete the Other Branch

Another thing that confused me at first was what happens to the feature branch after a merge.

Suppose I have:

```text
main
A --- B --- C --- D
                    ↑
                  main
                    \
                  feature
```

After merging, I might think:

> "Okay, Git merged the branch, so the branch is gone."

Not quite.

The branch still exists unless I delete it.

For example:

```bash
git branch -d feature
```

The merge incorporates the changes into `main`, but the branch itself is still a reference until I remove it.

That's why I often clean up finished branches after a successful merge.

## A Typical Feature Workflow

The workflow I've settled into looks something like this:

```bash
git switch main
git pull

git switch -c feature/user-login

# work, work, work...

git add .
git commit -m "add user login"

git switch main
git pull

git merge feature/user-login
```

Then, once everything is tested:

```bash
git branch -d feature/user-login
```

The exact workflow can obviously be different depending on the team and whether pull requests are involved.

But the basic idea stays the same:

```text
create branch
     ↓
do the work
     ↓
finish the work
     ↓
merge it
     ↓
delete the finished branch
```

## What I Learned About Merge

The biggest thing I learned is that **merging is not really about combining folders or copying files**.

It's about combining two lines of development.

For example:

```text
main
A --- B --- C --- F
          \
           D --- E
                ↑
             feature
```

After the merge:

```text
A --- B --- C --- F ------- M
          \               /
           D --- E -------
```

Git is preserving the history of both branches and recording the point where they came together.

That history can actually be useful later when I'm trying to understand:

> "Why did this change end up here?"

## The Way I Think About It Now

The easiest way I remember `git merge` is:

> **"I'm on one branch, and I want to bring another branch's changes into it."**

So:

```bash
git switch main
git merge feature
```

means:

```text
feature
   ↓
bring these changes
   ↓
into main
```

And when combined with what I learned from `git fetch`:

```text
git fetch
    ↓
see what changed remotely

git merge
    ↓
bring the changes I want into my branch
```

That made the whole branch workflow feel much more connected.

Branches let me work separately.

Fetch lets me see what changed elsewhere.

Merge lets me bring those lines of development back together.

And after all the confusion I had with branches in the beginning, I actually like that Git keeps the history visible instead of pretending everything happened in one straight line. 😄


[← Back to Main README](../Readme.md)