# git rebase

`git rebase` was probably one of the Git commands that confused me the most when I first came across it.

At first, I mostly used `git merge` whenever I needed to bring changes from `dev` into my branch. It worked, but over time I started seeing branches with lots of merge commits and a history that was harder to follow.

That's when I started learning about `rebase`.

The basic idea finally clicked for me:

```bash
git rebase dev
```

It takes my changes and puts them **on top of the latest `dev`**, giving me a cleaner and more linear history.

For example:

```text
Before:

dev:     A---B---C
              \
feature:       D---E
```

After rebasing:

```text
dev:     A---B---C
                  \
feature:           D'---E'
```

The commits are replayed on top of the latest `dev`.

### Where I use it

I mainly use `rebase` on my own feature branches when I want to bring them up to date with `dev` before opening a PR or merging them.

It helps me deal with conflicts before the final merge and keeps the history easier to follow.

## When Rebase Can Be Dangerous and should be avoided

One thing I learned pretty quickly is that **rebase rewrites history**.

That means I need to be careful about where I use it. If other developers are already working on the same branch, rebasing it can cause problems because the commit history they have locally may no longer match mine.

So my general rule is:

> **Rebase my own feature branches. Be very careful with shared branches.**

I avoid rebasing branches like `dev`, `staging`, or `main` when other developers are already using them.

Rebase is great for keeping my own branch clean and up to date, but once a branch is shared, rewriting its history can create unnecessary problems for everyone else.

In begining I always found the command confusing, 
For example following command:

```bash

git fetch
git rebase origin/dev

```

Here I am not rebasing the dev branch. The important part is which branch I am currently on.

If I am on:

```bash
feature/login

then:

git rebase origin/dev
```
means:

"Take my feature/login commits and replay them on top of origin/dev."

I am rebasing my feature branch, using dev as the base.


Before:

```text
dev:           A---B---C---D
                    \
feature/login:       E---F

```
I am currently on feature/login.

I run:

```bash
git rebase origin/dev
```
After:

```text
dev:           A---B---C---D
                            \
feature/login:              E'---F'
```

My dev branch hasn't been changed at all.

What I should NOT do

If I am actually on dev:

```bash
git switch dev
git rebase origin/main
```
Now I am rebasing dev.

That's the distinction I learned.

So:

## On my feature branch
git rebase origin/dev

✅ Rebase my branch onto dev.

## On dev (Usually avoid unless branching strategy is different)
git rebase origin/main

⚠️ Rebase dev onto main.