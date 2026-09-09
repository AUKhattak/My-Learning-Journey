[← Back to Main page](../Readme.md)

# AI Can Write the Code. But Who Is Protecting the Architecture?

After years of software development, I’ve spent a lot of time thinking about where AI fits into software engineering.

AI has changed how quickly we can build software. I can describe a feature, get a working implementation, refactor a service, write tests, and move on in a fraction of the time it used to take.

But there is a problem I’ve noticed while working this way.

**AI is very good at solving the problem in front of it. It is not always good at protecting the design of the system around that problem.**

And if I’m not careful, the architecture can slowly drift.

## How architecture starts to drift

AI doesn't necessarily forget the architecture. The problem is that **architectural context becomes less important as the conversation and codebase evolve**.

At the beginning of a project, I might have clearly defined boundaries:

* Controllers handle transport concerns.
* Business rules belong in the domain.
* Infrastructure details stay outside the domain.
* External systems are accessed through integration interfaces.
* Dependencies should point in a specific direction.

Then I start asking AI to implement features.

One request becomes ten. Ten become fifty.

The AI sees the current task and the surrounding code, and naturally tries to find the simplest way to make that task work.

That is where small deviations begin.

A piece of business logic goes into a controller because it is convenient.

An external API is called directly from a service because it is faster.

A new helper is created instead of using an existing abstraction.

A dependency is introduced because it solves today's problem.

Individually, none of these changes looks serious.

**The problem is what happens after hundreds of them.**

The system still works, but the architecture I originally designed is no longer the architecture I have.

## AI makes this problem bigger

This isn't only an AI problem. Developers have always introduced technical debt.

But AI changes the scale.

A developer might introduce one questionable abstraction.

AI can introduce twenty of them in an afternoon.

A developer might put business logic in the wrong layer once.

AI can repeat the same mistake across multiple features because it sees an existing implementation and treats it as a valid pattern.

This is where experience matters.

A less experienced developer may look at generated code and ask:

> “Does this work?”

I tend to ask a different question:

> **“Does this belong here?”**

That question is often more important.

Working code is not necessarily good code. Passing tests doesn't automatically mean the design is healthy.

## A simple example

Imagine I have a document-processing system where external services must always be accessed through an integration layer.

The intended design is:

```text
Application
    ↓
Integration Layer
    ↓
External Service
```

I ask AI:

> “Add support for sending notifications to Slack.”

AI might implement the Slack API call directly inside an application service because that is the most straightforward solution.

The code works.

But now I have:

```text
Application
    ↓
Slack API
```

The architectural boundary has been bypassed.

Later, I ask AI to add Microsoft Teams support. It may follow the same approach.

Now vendor-specific logic is spreading through the application.

Nothing failed. There is no obvious bug.

**The design has simply started moving in the wrong direction.**

## The solution I use

I don't expect AI to remember my entire architecture from a long conversation.

I give it a **source of truth**.

I maintain a concise architecture document that describes things such as:

* System boundaries
* Responsibilities of each layer
* Dependency rules
* Important design patterns
* Architectural decisions
* Things that are explicitly not allowed

It doesn't need to be a 100-page document.

It needs to answer one important question:

> **“What rules must the code follow?”**

Then, before I ask AI to make a significant change, I give it the relevant architectural context.

For example:

> “Before implementing this change, review the architecture rules. Tell me which layer this functionality belongs to, which existing components should be changed or reused, and whether the proposed solution introduces any architectural violations.”

That changes the interaction.

I'm no longer asking AI only:

> “Write this feature.”

I'm asking:

> **“Write this feature within these boundaries.”**

## I don't let AI decide where things belong

This is another habit I've developed.

When AI proposes putting business logic in a controller, I don't simply move it myself.

I ask:

> “Why does this logic belong in the controller? Check this against our architecture and explain where this responsibility should live.”

When AI introduces another abstraction, I ask:

> “What problem does this abstraction solve? Do we already have something that handles this responsibility?”

When AI adds a new dependency, I ask:

> “Is this dependency allowed at this architectural boundary?”

These questions force the AI to reason about the design instead of simply producing code.

## I also use AI to find architectural drift

This is one of the most useful parts of my workflow.

After several rounds of AI-assisted development, I ask it to review the implementation:

> “Review the current code against the architecture document. Look for layer violations, unexpected dependencies, duplicated responsibilities, unnecessary abstractions, and places where the implementation has started to drift.”

AI is actually quite useful for this.

It can scan a large amount of code and point me toward areas that deserve attention.

But I still make the final decision.

I don't outsource architectural ownership to AI.

## The role of experience becomes more important, not less

I don't think the value of a senior developer is simply being able to write code faster.

AI has changed that equation.

The value is increasingly in knowing **why** the code should be written a certain way.

Where should this responsibility live?

What should depend on what?

Is this abstraction actually necessary?

Is this a local optimization that will create a larger problem later?

Is this change consistent with the way the system was designed?

Those are architectural questions.

AI can help me answer them, but I don't blindly accept the answer.

After years of experience, I've learned that a system rarely becomes difficult to maintain because of one terrible piece of code.

It usually happens because of **hundreds of small decisions that seemed reasonable at the time**.

AI can make those decisions much faster.

That's why I believe architectural discipline becomes even more important in AI-assisted development.

> **AI writes the code. I own the architecture.**

That is the balance I try to maintain: let AI give me speed, but don't let that speed slowly redefine the system I'm building.


[← Back to Main page](../Readme.md)