# git fetch

One Git command I use quite often, but didn't really understand at first, is **`git fetch`**.

When I first started working with remote repositories, I mostly used:

```bash
git pull
```

Something changed on GitHub?

Just:

```bash
git pull
```

Problem solved. 😄

At least, that's what I thought.

Later, I learned that `git pull` is actually doing more than one thing.

It basically fetches the latest changes from the remote repository and then tries to integrate those changes into my current branch.

That's when I discovered:

```bash
git fetch
```

And I really liked the idea behind it.

**Fetch lets me see what changed on the remote without immediately changing my current work.**

## What Does `git fetch` Actually Do?

Let's say my local repository looks like this:

```text
A --- B --- C
          ↑
        main
```

But someone else has pushed a new commit to the remote:

```text
A --- B --- C --- D
                    ↑
                  origin/main
```

My local `main` doesn't know about `D` yet.

If I run:

```bash
git fetch
```

Git contacts the remote repository and downloads the information about the new commit.

Now my repository knows about:

```text
D
```

and my references are updated:

```text
A --- B --- C --- D
          ↑       ↑
        main   origin/main
```

But importantly:

**my local `main` has not moved.**

That's the part I didn't understand when I first started using Git.

## Fetch Doesn't Change My Current Work

This is probably the easiest way I remember it:

```text
git fetch
    ↓
"Tell me what's new."

git pull
    ↓
"Get what's new and integrate it."
```

When I run:

```bash
git fetch
```

Git doesn't suddenly merge someone else's changes into the branch I'm working on.

My files don't suddenly change because someone pushed something.

My current branch stays where it is.

That's why I started thinking of `fetch` as a **safe way to check what's happening on the remote**.

## What Is `origin/main`?

This is where `git fetch` helped me understand another Git concept.

I often saw things like:

```bash
origin/main
```

and initially wondered:

> "Is that another branch?"

It's better to think of it as a **remote-tracking reference**.

For example:

```text
main
origin/main
```

These can point to different commits.

Suppose I have:

```text
A --- B --- C
          ↑
        main
```

and the remote has:

```text
A --- B --- C --- D --- E
          ↑           ↑
        main      origin/main
```

After:

```bash
git fetch
```

Git updates my knowledge of `origin/main`.

Now I can see:

```text
A --- B --- C --- D --- E
          ↑           ↑
        main      origin/main
```

This tells me:

> "The remote branch is two commits ahead of my local branch."

But Git hasn't automatically put those commits into my local `main`.

I still get to decide what to do next.

## Checking What Changed

This is where I started finding `fetch` really useful.

After:

```bash
git fetch
```

I can compare my local branch with the remote:

```bash
git log main..origin/main
```

This shows commits that are on `origin/main` but not on my local `main`.

I can also look at the differences:

```bash
git diff main..origin/main
```

Now I can actually inspect what changed before deciding whether I want to merge, rebase, or do something else.

That feels much safer than blindly running:

```bash
git pull
```

and discovering afterward that things changed.

## A Situation Where Fetch Became Useful

Imagine I'm about to start working on something.

Before making changes, I can do:

```bash
git fetch
```

Then:

```bash
git status
```

and check where my branch is compared to the remote.

Or I can inspect the remote changes:

```bash
git log --oneline HEAD..origin/main
```

Now I know what other people have pushed.

If everything looks good, I can decide how I want to bring those changes into my branch.

For example:

```bash
git merge origin/main
```

or, depending on my workflow:

```bash
git rebase origin/main
```

The important thing is that **fetch and integration are separate steps**.

That separation gives me more control.

## Fetch vs Pull

This is probably the comparison that helped me understand it the most.

### `git fetch`

```bash
git fetch
```

Means:

> "Download the latest information from the remote, but don't change my current branch."

### `git pull`

```bash
git pull
```

Means roughly:

> "Fetch the latest information and then integrate it into my current branch."

So conceptually:

```text
git fetch
    ↓
remote → local repository
```

while:

```text
git pull
    ↓
fetch
    +
merge
```

The exact integration behavior of `git pull` depends on the configuration and options I'm using, but the important idea is that `pull` goes further than `fetch`.

## Why I Sometimes Prefer Fetch

When I'm working on a shared project, I don't always want Git to immediately change my branch.

Sometimes I just want to know:

> "What's happened since I last checked?"

So I run:

```bash
git fetch
```

Then I can inspect the situation.

Maybe someone pushed a fix.

Maybe the branch moved ahead several commits.

Maybe there are changes that will conflict with what I'm working on.

Maybe nothing happened at all.

Either way, **I get to look before I act.**

That small difference has saved me from a few surprises. 😄

## Fetching a Specific Remote

Most of the time, I just use:

```bash
git fetch
```

But I can also specify a remote:

```bash
git fetch origin
```

If I have multiple remotes, this can be useful because I can choose which remote I want to update information from.

For example:

```bash
git remote -v
```

might show:

```text
origin
upstream
```

Then I can fetch from a specific one:

```bash
git fetch upstream
```

Again, this doesn't mean I'm merging anything.

I'm simply updating my local knowledge of what exists on that remote.

## Fetch Doesn't Mean "Download Everything Into My Files"

This was another thing I had to get used to.

When I run:

```bash
git fetch
```

I don't suddenly see other people's changes in my working directory.

For example, if someone changes:

```text
src/login.js
```

on the remote, running:

```bash
git fetch
```

doesn't overwrite my local `src/login.js`.

That's because Git is updating the repository's remote-tracking information, not checking those changes out into my current working tree.

This is exactly why fetch is so useful when I just want to inspect what's happening.

## The Way I Think About It Now

The simplest way I remember `git fetch` is:

```text
git fetch

"Go check the remote for me."

↓
Download new commits
↓
Update origin/main
↓
Don't touch my current branch
↓
Let me decide what to do next
```

So if I have:

```text
local:

A --- B --- C
          ↑
        main


remote:

A --- B --- C --- D --- E
```

I can run:

```bash
git fetch
```

and then:

```text
A --- B --- C --- D --- E
          ↑           ↑
        main      origin/main
```

Now I know that the remote is ahead.

I haven't merged anything.

I haven't changed my files.

I just know more than I did before.

And that's probably the biggest thing I learned about `git fetch`:

> **Fetching is about updating my knowledge of the remote repository, not immediately changing my work.**

Once I understood that, `git fetch` stopped feeling like some unnecessary extra Git command.

