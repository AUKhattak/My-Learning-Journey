[← Back to Main README](../Readme.md)

# git stash - git pop

`git stash` was another one of those commands that I didn't really understand at first.

Before I learned about it, if I was working on something and suddenly needed to switch to another branch, I had a problem. I either had to commit my unfinished changes, leave them sitting there, or do what I used to do before Git — make another copy of the project. 😄

Then I discovered `git stash`.

```bash
git stash
```

It basically gives me a clean working directory without having to commit unfinished work.

Later, when I'm ready to continue:

```bash
git pop
```

My changes come back, and I can carry on from where I left off.

This became really useful when I'm in the middle of something and suddenly need to switch branches to fix a bug or check something else.

Before `stash`, I would think:

> "I have unfinished code. Now what?" 😅

Now it's more like:

```text
I'm working
    ↓
Need to switch branches
    ↓
git stash
    ↓
Do what I need to do
    ↓
git pop
    ↓
Back to my unfinished work
```

It's a small thing, but `stash` has saved me from making a lot of unnecessary commits just to temporarily put my work somewhere.

## My Hard Rule for `git stash`

**`git stash` is for temporary work, not a replacement for commits.**

[← Back to Main README](../Readme.md)