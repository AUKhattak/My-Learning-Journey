[← Back to Main README](../README.md)

# merge conflict and how do I manage it

One of the Git messages that scared me the most when I first started was:

```text
CONFLICT
```

My first reaction was basically:

> "Okay... what did I break?" 😅

But after dealing with a few conflicts, I realized that a merge conflict isn't necessarily something going wrong.

It usually means **Git found two changes that it can't safely combine automatically, so it needs me to decide what the final version should be.**

## How a Conflict Happens

Imagine I have two branches:

```text
main
  A --- B --- C
         \
          D --- E
               ↑
            feature
```

Maybe both branches changed the same part of a file.

On `main`:

```text
Hello from main
```

On `feature`:

```text
Hello from feature
```

Now I try:

```bash
git switch main
git merge feature
```

Git looks at the changes and basically says:

> "Both of you changed this. I'm not choosing." 😄

The merge stops and Git marks the file as conflicted.

## What the File Looks Like

Git adds conflict markers:

```text
<<<<<<< HEAD
Hello from main
=======
Hello from feature
>>>>>>> feature
```

The top part is my current branch:

```text
<<<<<<< HEAD
Hello from main
```

The bottom part is the branch I'm trying to merge:

```text
Hello from feature
>>>>>>> feature
```

Now it's my job to decide what the final code should be.

Maybe I want:

```text
Hello from main and feature
```

So I edit the file and remove the conflict markers.

## Finishing the Merge

Once I've resolved the conflict, I tell Git that the file is fixed:

```bash
git add <file>
```

Then I complete the merge:

```bash
git commit
```

The general workflow becomes:

```text
git merge
    ↓
CONFLICT
    ↓
open conflicted files
    ↓
decide what the code should be
    ↓
remove conflict markers
    ↓
git add
    ↓
git commit
```

That's really all a merge conflict is.

The difficult part isn't the commands.

It's deciding **which changes should actually survive**.

## What If I Don't Want the Merge?

Sometimes I start a merge and realize the conflicts are bigger than I expected.

I can stop the merge with:

```bash
git merge --abort
```

Git will try to return me to the state from before the merge.

This is useful when I realize:

> "I'm not ready to deal with this right now." 

I can then investigate the branches, update my work, or try a different approach.

## How I Try to Avoid Conflicts

After dealing with enough conflicts, I learned that the best conflict resolution is sometimes **avoiding the conflict in the first place**.

A few things help:

* Keep branches reasonably up to date.
* Keep branches focused on one task.
* Avoid having multiple people constantly changing the same large files.
* Merge or rebase regularly when appropriate for the team's workflow.
* Keep commits small and focused.

For example, a branch that lives for three months without integrating changes from `main` can eventually become:

```text
main
  A --- B --- C --- D --- E --- F
         \
          X --- Y --- Z
```

At that point, merging `feature` back into `main` can be much harder than if I had kept the branch reasonably up to date.

## Don't Just Pick "Mine" or "Theirs"

One mistake I made early on was treating conflict resolution like a simple choice:

```text
keep mine
or
keep theirs
```

Sometimes that's correct.

But sometimes **both changes are important**.

For example:

```text
main:
add validation

feature:
change error message
```

The correct solution might be to keep both.

So when resolving a conflict, I try to think:

> **"What should the final code look like?"**

rather than:

> "Which side should I click?"

That small change in thinking makes conflict resolution much easier.

## The Way I Think About Conflicts Now

I don't see a merge conflict as Git failing anymore.

I see it as Git saying:

> **"I found something that requires a human decision."**

The workflow is basically:

```text
Git can combine it
        ↓
     merge

Git can't decide
        ↓
     conflict
        ↓
I decide
        ↓
git add
        ↓
complete the merge
```

And honestly, I'd rather Git stop and ask me than silently choose the wrong code.

So my biggest lesson with merge conflicts is:

> **Don't panic, don't blindly choose "ours" or "theirs", and don't rush. Understand both changes and decide what the final code should be.**

[← Back to Main README](../README.md)