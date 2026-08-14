# git stash vs cherry-pick

When I first learned `git stash` and `git cherry-pick`, they both felt like ways of **moving work around**.

But they solve completely different problems.

The easiest way I remember them now is:

```text
git stash
    ↓
Move my unfinished local work temporarily

git cherry-pick
    ↓
Take an existing commit and apply it to another branch
```

## The Difference

Suppose I'm working on a feature:

```text
main
  A --- B
         \
          C --- D
               ↑
            feature
```

I have some **uncommitted changes**:

```text
feature
    ↓
unfinished work
```

If I need to switch branches temporarily:

```bash
git stash
```

My working directory becomes clean.

Later:

```bash
git stash pop
```

My unfinished changes come back.

That's what `stash` is for:

**Temporary, uncommitted work.**

Cherry-pick is different.

Suppose I already have a commit:

```text
feature
  C --- D --- E
            ↑
        bug-fix
```

and I want the changes from `D` on `main`, but I don't want the rest of the feature branch.

I can do:

```bash
git switch main
git cherry-pick D
```

Now I have:

```text
feature
  C --- D --- E

main
  A --- B --- D'
```

That's what `cherry-pick` is for:

**Taking a specific existing commit and applying it somewhere else.**

## The Tradeoff

|                   | `git stash`                | `git cherry-pick`                  |
| ----------------- | -------------------------- | ---------------------------------- |
| Works with        | Uncommitted changes        | Existing commits                   |
| Main purpose      | Temporarily put work aside | Move a specific change             |
| Creates commit?   | No                         | Yes, a new commit                  |
| Rewrites history? | No                         | No, but duplicates a change        |
| Best for          | Temporary interruptions    | Selective changes between branches |

The important difference for me is:

```text
stash      → "I haven't committed this yet."

cherry-pick → "I already committed this, but I want it over there too."
```

## When I Use Each One

I use **stash** when:

```text
I'm working
    ↓
Something interrupts me
    ↓
I need a clean branch
    ↓
git stash
```

I use **cherry-pick** when:

```text
A useful commit exists on another branch
    ↓
I don't want the whole branch
    ↓
git cherry-pick <commit>
```

## When I Need to Be Careful

With `stash`, my biggest concern is forgetting unfinished work in a growing list of old stashes. 😄

With `cherry-pick`, my biggest concern is duplicating the same logical change across different branches.

So I try not to use either just because I can.

## My Rule of Best Practice

**`stash` for temporary unfinished work, `cherry-pick` for a specific finished commit.**

```text
Not committed? → git stash

Committed and need that specific change elsewhere? → git cherry-pick
```

I remember it as: **stash is temporary storage, cherry-pick is history.**


