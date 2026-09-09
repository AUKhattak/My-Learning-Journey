[← Back to Main README](../README.md)

# Branching Strategy for My Project

For my last project at my company, I wanted to keep the branching strategy simple.

I didn't want to create five or six branches just because Git made it possible. 

The project had two important environments:

```text
test
production
```

So I decided to have two long-lived branches that represented those environments:

```text
test
  ↓
production
```

I used:

```text
test
production
```

as the names because they made the purpose immediately obvious.

The idea was simple:

```text
test branch
    ↓
GitHub Actions
    ↓
Test Environment


production branch
    ↓
GitHub Actions
    ↓
Production Environment
```

Every time changes reached `test`, the CI/CD workflow for the test environment would run.

Every time changes reached `production`, the production CI/CD workflow would run.

That gave me a very clear relationship between **Git branches and deployment environments**.

## Why I Didn't Add a Permanent `dev` Branch

At first, I wondered if I should have:

```text
dev
 ↓
test
 ↓
production
```

because that's a common setup.

But for this project, I didn't feel I needed a permanent `dev` branch.

Developers could create short-lived feature branches from `test`:

```text
test
  |
  ├── feature/login
  ├── feature/payment
  └── fix/navbar
```

They could work freely there and push whatever they needed.

Once the work was ready, it could be reviewed and merged into `test`.

So my workflow was more like:

```text
feature branch
      ↓
    test
      ↓
 production
```

rather than:

```text
feature
   ↓
 dev
   ↓
 test
   ↓
 production
```

For a small or medium-sized project, I found the first approach easier to understand and maintain.

I didn't want a branch to exist simply because "that's how branching strategies are usually done."

I wanted every long-lived branch to have a clear purpose.

## My `test` Branch

The `test` branch was my integration point for changes that were ready to be tested.

A typical workflow looked like:

```text
feature/login
      ↓
Pull Request
      ↓
test
      ↓
GitHub Actions
      ↓
Test Environment
```

Once something was merged into `test`, the CI/CD pipeline would automatically deploy it to the test environment.

This gave me a useful rule:

> **If it's in `test`, it should be something the team is comfortable testing.**

It didn't necessarily mean the code was production-ready.

It meant:

> "This is the current version we want to test."

## My `production` Branch

The `production` branch was much more protected.

The workflow was:

```text
test
  ↓
Pull Request
  ↓
production
  ↓
GitHub Actions
  ↓
Production
```

I didn't want someone to accidentally run:

```bash
git push origin production
```

and suddenly deploy something to real users. 😅

So `production` was intentionally harder to change.

## Protecting the Important Branches

This is where GitHub branch protection became really important for me.

I protected both:

```text
test
production
```

so developers couldn't simply push directly into them.

For example, I enabled rules such as:

* **Require a pull request before merging**
* **Require approvals/review before merging**
* **Require required status checks to pass**
* **Require branches to be up to date before merging**, where appropriate
* **Block force pushes**
* **Block branch deletion**

The exact rules depend on the project, but the principle was:

> **Important branches should not depend on someone remembering to be careful. The repository should enforce the rules.**

That was especially important for `production`.

## Reviews for `test` and `production`

I also wanted changes going into both important branches to be reviewed manually.

So instead of:

```text
developer
   ↓
git push
   ↓
test
```

the workflow became:

```text
developer
   ↓
feature branch
   ↓
Pull Request
   ↓
automated checks
   ↓
manual review
   ↓
test
```

And for production:

```text
test
   ↓
Pull Request
   ↓
automated checks
   ↓
manual review
   ↓
production
```

That gave me two different types of protection.

**Automated protection:**

```text
tests
linting
build
security checks
CI status
```

**Human protection:**

```text
code review
approval
```

I liked having both because passing CI doesn't necessarily mean that the change is a good engineering decision.

And a reviewer might notice something that a test can't.

## What About `dev`?

This is where I think the answer is:

> **No, I don't automatically need a permanent `dev` branch.**

A `dev` branch can be useful when the project actually needs another integration stage.

For example:

```text
feature
   ↓
dev
   ↓
test
   ↓
production
```

might make sense if:

* many features are being integrated continuously
* `dev` represents a genuinely different environment
* the team needs an unstable integration area
* testing happens in multiple stages
* releases are coordinated separately from development

But if `dev` is just another branch between feature branches and `test`, it can add complexity without adding much value.

In my project, I preferred:

```text
feature branches
       ↓
      test
       ↓
   production
```

because every branch had a clear job.

## What About Developers Pushing to `dev`?

If I did have a `dev` branch, I would be more comfortable making it less restrictive than `test` or `production`.

For example:

```text
dev
 ↓
developers can push
```

while:

```text
test
 ↓
PR + review + CI checks
```

and:

```text
production
 ↓
PR + review + CI checks
```

That can be a reasonable setup when `dev` is deliberately an integration playground.

But I wouldn't make that decision just because "dev branches are supposed to be open."

The important question is:

> **What damage can happen if someone pushes directly to this branch?**

If `dev` only affects an isolated development environment, the risk may be acceptable.

If it triggers an important deployment or is used by many people, I'd protect it too.

## Why This Strategy Worked for Me

What I liked most about this setup was that I could look at the repository and immediately understand what was happening:

```text
feature/login
      ↓
     test
      ↓
 production
```

And the deployment relationship was equally clear:

```text
test branch
     ↓
test environment


production branch
     ↓
production environment
```

I didn't have to remember what a mysterious `dev2`, `release`, or `integration` branch was supposed to represent.

The branch itself told me its purpose.

It also gave me a nice balance between **developer freedom and protection**.

Developers could work freely on their feature branches.

The important branches had automated checks and human review.

And GitHub Actions handled deployment after the changes reached the appropriate branch.

## My Rule of Best Practice

The main lesson I took from this project is:

> **Don't create branches just because a branching strategy says you should. Create long-lived branches when they represent a real workflow or environment.**

For this project, that meant:

```text
feature/*
    ↓
   test
    ↓
production
```

with:

```text
feature/*
→ developer freedom

test
→ CI checks + review + test deployment

production
→ CI checks + review + production deployment
```

That approach suited me because it was simple enough to understand, but still had enough protection to prevent accidental deployments.

And more importantly, the repository itself enforced the rules.

I didn't have to rely on someone remembering:

> "Please don't push directly to production." 

The branch protection, required checks, reviews, and CI/CD pipeline made the safe path the normal path.


[← Back to Main README](../README.md)