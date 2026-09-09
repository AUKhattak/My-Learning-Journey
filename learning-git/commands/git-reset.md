[← Back to Main README](../README.md)

# git reset

Another Git command that took me a while to really understand was **`git reset`**.

When I first started using Git, I thought every commit was basically permanent. Once I committed something, I assumed I had to live with it forever.

And honestly, sometimes my commit history looked like this:

```text
add login
fix login
fix login again
actually fix login
oops
final fix
final-final fix
please work
```

Very clean history. 😄

Eventually, I learned about `git reset`.

At a high level, `git reset` is a way of telling Git:

> **"I want to move my branch back to an earlier point."**

For example, suppose my history looks like this:

```text
A --- B --- C --- D
              ↑
            HEAD
```

If I realize that `D` was a mistake and I want to go back to `C`, I can use:

```bash
git reset C
```

Or, more commonly, I can use something like:

```bash
git reset HEAD~1
```

That means:

> Move back by one commit.

This was one of those Git concepts that sounded scary at first because the word **reset** makes it sound like I'm about to destroy everything. 😄

But `git reset` doesn't always mean "delete everything."

The important part is **which reset mode I use**.

## The Three Reset Modes

The three modes I use most often are:

```bash
git reset --soft
git reset --mixed
git reset --hard
```

They all move `HEAD`, but they treat my changes differently.

That's the part that confused me at first.

### `git reset --soft`

A soft reset moves the branch back to an earlier commit, but keeps my changes **staged**.

For example:

```bash
git reset --soft HEAD~1
```

If I just made a commit and realize:

> "Wait, I forgot to include one more file."

I can reset the commit while keeping all the changes staged.

Then I can add the missing file and create a new commit.

So instead of:

```text
commit 1
commit 2
```

I can essentially go back to:

```text
changes → staged → new commit
```

I think of `--soft` as:

> **"Undo the commit, but keep everything ready to commit again."**

---

### `git reset --mixed`

This is actually the default behavior of `git reset`.

For example:

```bash
git reset HEAD~1
```

With a mixed reset, Git moves the branch back and **unstages the changes**, but the changes themselves remain in my working directory.

So my files aren't simply thrown away.

This is useful when I realize:

> "I committed too early, but I still want all these changes. I just don't want them committed yet."

After the reset, I can see my changes as unstaged modifications and decide what I actually want to commit.

I think of `--mixed` as:

> **"Undo the commit and unstage my changes, but keep the work."**

This is probably the reset mode I use when I want to reorganize a commit.

---

### `git reset --hard`

And then there is the one that deserves a little more respect:

```bash
git reset --hard HEAD~1
```

A hard reset moves the branch back **and changes the working tree to match that commit**.

In other words, the changes from the commits I'm resetting away from can disappear from my working directory.

For example:

```text
A --- B --- C
          ↑
        HEAD
```

If I run:

```bash
git reset --hard HEAD~1
```

I end up at:

```text
A --- B
      ↑
    HEAD
```

And the changes introduced by `C` are no longer present in my working directory.

This is why I treat `--hard` very carefully.

It is basically me telling Git:

> **"I don't want these changes anymore. Make my files look exactly like the target commit."**

Sometimes that's exactly what I want.

Sometimes it's a terrible idea. 😄

## The Difference I Finally Memorized

The easiest way I've found to remember the three modes is:

```text
--soft   → move commit, keep changes staged
--mixed  → move commit, keep changes unstaged
--hard   → move commit, throw away working changes
```

Or even shorter:

```text
soft   → keep staged
mixed  → keep unstaged
hard   → discard
```

That little mental model made `git reset` much less intimidating.

## A Common Example

One situation where I often find `reset` useful is when I accidentally create a commit that I don't really want.

For example:

```bash
git add .
git commit -m "add user profile"
```

Then five seconds later:

> "Oh... that commit contains way too much stuff." 😅

If I haven't pushed it yet, I can undo the commit while keeping the changes:

```bash
git reset HEAD~1
```

Now the commit is gone, but my files are still there as unstaged changes.

I can then decide what actually belongs in the commit:

```bash
git add src/profile.js
git add src/profile.css
git commit -m "add user profile"
```

Now my history is cleaner, and I didn't have to delete my work and start again.

## Reset vs Delete

One of the biggest things I learned is that `git reset` isn't really about deleting files.

It's more about **moving the branch pointer and deciding what should happen to the changes around it**.

That's an important distinction.

Git's history might look like:

```text
A --- B --- C --- D
```

If I reset from `D` back to `B`:

```text
A --- B
      ↑
    HEAD
```

the branch is now pointing at `B`.

The commits `C` and `D` are no longer part of the current branch history.

But Git may still be able to find those commits for a while, which is something I learned later through `git reflog`.

That was a comforting discovery because it meant that accidentally resetting something didn't necessarily mean:

> "Everything is gone forever."

Still, I don't use that as an excuse to run `git reset --hard` carelessly. 😄

## One Important Rule

The biggest rule I've learned with `git reset` is:

> **Be much more careful with commits that have already been pushed.**

If I'm working locally and haven't shared my commits with anyone, rewriting my history is usually much less problematic.

But if I've already pushed commits to a shared branch, resetting and force-pushing can cause problems for other people who are working from the same history.

So before running something like:

```bash
git reset --hard
git push --force
```

I stop and think.

Especially if I'm on:

```text
main
staging
```

or another shared branch.

That's where a simple local cleanup can turn into a team problem very quickly. 😅

## What I Use `git reset` For

Nowadays, I mostly think about `git reset` as a tool for **fixing my local history**.

Some common situations are:

* I committed too early.
* I want to combine changes into a cleaner commit.
* I accidentally staged files I didn't want to commit.
* I want to undo my last commit but keep the work.
* I want to completely throw away some local changes.
* I want to move my branch back to an earlier commit.

The command itself isn't particularly complicated.

The part that took time for me was understanding what happens to my **commit, staging area, and working directory** after the reset.

Once I understood that, `git reset` stopped feeling like a dangerous Git command and became another tool in my everyday workflow.

But I still look twice before typing:

```bash
git reset --hard
```

Because unlike my old project-backup strategy, there isn't a folder called `project-backup-final-final` waiting to save me. 😄


[← Back to Main README](../README.md)