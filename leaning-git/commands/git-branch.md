[← Back to Main README](../Readme.md)

# git branch

One of the first Git concepts that really changed the way I worked was **branches**.

Before Git, if I wanted to try something risky, my solution was to make a copy of the whole project:

```text
project/
project-backup/
project-backup-new/
project-final/
project-final-2/
```

Very professional version control system. 😄

With Git, I learned that I don't need to copy my entire project just to experiment with something. I can simply create a branch:

```bash
git branch experiment-1
```
And yes thats how i used to name a branch. 😄 Very descriptive. Very professional.

Over time, I started giving branches names that actually describe what I'm working on, for example:

```bash
git branch feature/user-login
git branch fix/navbar-mobile
git branch refactor/auth-service
```

Now I have a separate place to work on my changes without messing with the main branch.

At first, I mainly thought of branches as **"a place to develop a new feature."** But as I started working with shared repositories, I realized branching is useful for much more than that.

A repository can have different branches for different purposes, for example:

```text
main / production
        ↑
     staging
        ↑
      test
        ↑
       dev
```

The exact workflow depends on the team, but the idea is that different branches can represent different stages or purposes of the project.

For example:

* `dev` → where active development happens
* `test` → where changes can be tested
* `staging` → a final environment before production
* `main` / `production` → stable code used by real users

Branches can also be used for features, bug fixes, experiments, releases, and sometimes even temporary work.

What I like most about branching now is that it gives me the freedom to **try things without being afraid of breaking everything**. If my experiment turns into a disaster, it's just a branch. 😄

Coming from my old approach of making five copies of the same folder, this feels a lot cleaner. Once I understood that, branching stopped feeling complicated and became one of the Git features I use almost every day.

## When Branches Become a Headache 😅

Branches are great, but they can also become a headache if you don't manage them properly.

The biggest problem I've faced is letting branches live for too long. A branch that started as a small feature can easily turn into something that nobody remembers anymore. The longer I leave it, the more it can drift away from the main branch and the harder it becomes to merge.

I've also learned that a branch shouldn't be treated like a completely separate world. If I'm working on a branch for a long time without keeping it up to date, I can end up with a lot of conflicts when it's finally time to merge.

So there are a few things I take seriously when creating or working on a branch:

* **Meaningful names** — `feature/user-login` is much better than `experiment-1`.
* **Keep it focused** — I try to keep one branch focused on one feature, bug fix, or task.
* **Keep it short-lived** — Once the work is done and merged, I don't need the branch hanging around forever.
* **Stay up to date** — Especially when working with a team, I regularly bring relevant changes from the main development branch into my work.
* **Keep commits clean** — I try not to mix unrelated changes into the same branch.
* **Know the purpose** — Before creating a branch, I want to know what I'm actually creating it for.
* **Be careful with shared branches** — `main`, `staging`, and other shared branches aren't places where I want to experiment randomly. 😄

One thing I've learned is that **creating a branch is easy. Managing it properly is what matters.**

Branches give me the freedom to experiment and work safely, but too many old branches, unclear names, and branches that have drifted too far can quickly turn Git into a mess.

So nowadays, I try to treat branches as temporary workspaces with a clear purpose create one when I need it, keep it healthy, and get rid of it when I'm done.


[← Back to Main README](../Readme.md)