[← Back to Main Page](../Readme.md)

# My Git Journey

This repo is where I’m documenting my journey with Git, how I learned it, what I struggled with, and how I became more comfortable using it.

Before Git, my version control strategy was basically: *“This version works, so I better not touch it.”* 😄 I used to copy my project into another folder before making changes, just in case I broke something. So I had things like `project-final`, `project-final-2`, and, of course, `project-final-2-new`.

No more `project-final`, `project-final-2`, or `project-final-2-new`. Now I’m confident making changes because I know I can always go back, see what happened, and recover my work if something goes wrong. Git has basically become my safety net.


Here, I want to go back and document those parts properly especially the commands I found difficult, the mistakes I made, and what helped me finally understand them.

This isn’t meant to be a generic Git guide. It’s just my own learning journey and how my understanding of Git changed over time.

## Basic Git Commands

The first commands I learned were the usual ones: `git init`, `git add`, and `git commit`. Like most developers learning Git for the first time, these were pretty straightforward, and I quickly got used to using them.

But there were other commands that I found much more confusing at the beginning. Some of them didn’t make much sense to me, and I often had to look up what they actually did.

Interestingly, those same commands have now become some of my go-to commands. They make my workflow much easier and give me more confidence when working with my code, and even more importantly, when working on shared repositories.

## The Commands That Made My Life Easier

This is the second phase of my Git journey, the commands I use the most today and the ones that have made working with Git much easier for me.

This was also the phase where there was no ChatGPT to come to the rescue. 😄 When I got stuck, my options were pretty simple: use stackoverflow see for relevant commands that can help me and eventually read **official Git documentation** for detailed understanding of trade-off.

These are some of hand picked commands which I found confusing at first, and then how I learned them, and how they became part of my everyday Git workflow.

Click on any command to see how I learned it, what confused me, and how I use it today.



* [**git branch**](./commands/git-branch.md)
* [**git fetch**](./commands/git-fetch.md)
* [**git merge**](./commands/git-merge.md)
* [**git pull**](./commands/git-pull.md)
* [**git pull VS [git fetch + git merge]- Trade-offs and where avoid git pull**](./commands/git-pull-vs-git-fetch+git-merge.md)
* [**Merge Conflict and How do I manage it**](./commands/merge-conflict-handle.md)

---

* [**git rebase (How it rewrites history)**](./commands/git-rebase.md)
* [**git reset**](./commands/git-reset.md)
* [**git revert**](./commands/git-revert.md)
* [**Git Rebase vs Reset vs Revert — Choosing the Right Command**](./commands/git-rebase-vs-reset-vs-revert.md)

---

* [**git stash and git pop**](./commands/git-stash.md)
* [**git cherry-pick**](./commands/git-cherry-pick.md)
* [**Git Cherry-Pick VS git stash pop — Choosing the Right Command**](./commands/git-cherry-pick-vs-merge-vs-rebase.md)



## My Approach for Choosing a Branching Strategy for My Real Industry Project

I started by understanding the Git concepts one by one, branches, merge, rebase, cherry-pick, fetch, pull, stash, conflicts, and branch protection. As I understood how each one worked and the trade-offs involved, I started thinking about what actually made sense for my project.

Eventually, I chose a strategy based on how my team worked, how our environments were deployed, and how much protection each branch needed.

* [**Branching Strategy I Chose**](./commands/git-branching-strategy.md)
* [**Never push these files in github**](./commands/git-ignore.md)



## What I Do If I Accidentally Push a Secret to GitHub

Even with `.gitignore` and GitHub Secrets, mistakes can happen. If I accidentally push an API key, password, token, private key, or another sensitive value, I don't treat it as a simple Git cleanup problem.

My first priority is to **invalidate or rotate the secret**, because deleting the file from the repository does not make the exposed credential safe again.

After that, I clean the secret from the Git history if necessary and check where else the credential may have been used.

The main lesson for me is:

> **Once a secret is pushed, assume it is compromised first and clean up the Git history second.**

* [**What to Do If Secrets Are Accidentally Pushed**](./commands/git-secret-pushed.md)




## My approach using chatGPT for Git commands I didnot use before

These days, when I run into a Git situation where I’m completely unsure what to do, I also use AI to help me figure it out.

But one thing I’ve learned is that using AI doesn’t mean blindly following whatever it suggests. Especially with Git, that can be risky. A command might solve the immediate problem while creating a much bigger one somewhere else.

For example, if I’m stuck with a complicated Git situation, I might ask ChatGPT or Claude to explain what is happening and suggest a few ways to handle it. But before running anything, I try to understand the situation myself and look at the trade-offs between the different options.

I pay particular attention to whether a command will **rewrite history**, potentially affect other people, or make it harder to recover from a mistake. There is a big difference between a command that safely moves me forward and one that can fundamentally change the history of a repository.

I also do not let Claude or any other AI tool run Git commands blindly on my behalf. I want to know **what command is being executed, why it is being suggested, and what could happen if it goes wrong**.

Sometimes the safest solution isn't necessarily the shortest one. I might choose a slightly more complicated approach because it preserves history or gives me an easier way to recover if something goes wrong.

So AI has become another tool in my Git learning process, but not a replacement for understanding Git. When I encounter a command I haven't used before, I use AI to help me understand it, compare the alternatives, and think through the risks. **Only after I understand what I'm about to do do I run the command.**


[← Back to Main page](../Readme.md)