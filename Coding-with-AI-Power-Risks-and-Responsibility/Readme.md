[← Back to Main page](../README.md)

# The Evolution of My AI Coding Workflow 

I started my journey with AI-assisted coding in a very simple way: I asked ChatGPT to write an authentication script for me. Then I took the code, understood it, and decided where it should live in the project, how it should connect to the rest of the system, and which dependencies I actually needed.

Over time, that changed.

Instead of asking AI for a piece of code, I started asking it to make larger changes. Eventually, I reached the point where a single prompt could make changes across the project, manage dependencies, create files, connect features, and implement an entire flow almost by itself.

Honestly, it is crazy how far this has come.

But using it blindly is just as dangerous as it is powerful.

AI can write code that looks correct while completely missing the bigger picture of your project. Some of the main risks I have noticed are:

* **Forgetting context:** AI can lose track of decisions, assumptions, or requirements made earlier.
* **Losing the architecture:** It may not fully understand why your project is structured a certain way.
* **Bypassing design rules:** AI can introduce code that works but violates your architecture, patterns, or conventions.
* **Security risks:** Especially in web applications, blindly accepting generated authentication, authorization, validation, or API code can introduce serious vulnerabilities.
* **Dependency problems:** AI may add unnecessary packages, use outdated libraries, or create conflicting dependencies.
* **Over engineering:** A simple feature can suddenly become a much larger and more complicated implementation.
* **Hidden bugs:** Generated code can pass a quick test while failing in edge cases you never considered.
* **False confidence:** Code that looks clean and professional is not necessarily correct.
* **Loss of understanding:** If AI does everything, you can slowly stop understanding your own codebase.

The goal, at least for me, is not to stop using AI. It is to get better at using it.

AI should be the tool that helps me move faster, not the person making every engineering decision for me.


## Why These Problems Happen — and How I Deal With Them

Here is my current understanding of why these problems happen and, more importantly, how I try to deal with them.

Most of these issues come down to one thing: **AI does not truly understand a project the way a developer does.** It works from the context it has been given, the code it can see, and the instructions in the current task. As the project grows, that context becomes harder to maintain.

I have found that the best approach is not to avoid AI, but to put boundaries around it. I want AI to handle the repetitive and time-consuming work while I remain responsible for the architecture, security, dependencies, and important technical decisions.

Below is a breakdown of the main problems I have encountered and how I approach each one.

### Issues

1. [Forgetting Context and My Approach to Handle this issue](./issues-and-my-solution-using-AI-effectively/forgetting-context.md)
2. [Losing the Project Architecture and How I manage this Issue](./issues-and-my-solution-using-AI-effectively/Losing-Architecture.md)
3. [How AI Introduces Security Risks and Vulnerability and My Approach to Handle this](./issues-and-my-solution-using-AI-effectively/Security-Risks.md)





# If I Started a New Project Tomorrow, How Would I Use AI?

If I had to start a new project tomorrow, I would use AI from day one.

But I would not start by asking AI to build the application.

That is probably the biggest change in my thinking after working with AI for a while.

I would start by defining the **engineering environment in which AI is allowed to operate**.

The reason is simple:

> **AI can make implementation extremely fast. It cannot be allowed to silently make architectural decisions on my behalf.**

The more capable AI becomes, the more important this distinction gets.

I don't want AI to replace engineering discipline. I want AI to operate inside that discipline and dramatically increase how much work I can do.

My goal would therefore be:

> **Give AI high autonomy for implementation, but low autonomy for consequential architectural decisions.**

That means establishing the architecture, constraints, security model, development workflow, and decision history before asking AI to start generating large amounts of code.


- **[My Approach](./issues-and-my-solution-using-AI-effectively/My-Approach.md)**
  - [Engineering context](./issues-and-my-solution-using-AI-effectively/My-Approach.md#1-i-would-establish-the-engineering-context-before-writing-the-application)
  - [AI working rules](./issues-and-my-solution-using-AI-effectively/My-Approach.md#2-agentsmd-define-how-ai-is-allowed-to-work)
  - [Architecture & dependency direction](./issues-and-my-solution-using-AI-effectively/My-Approach.md#3-i-would-document-dependency-direction-not-just-architecture-diagrams)
  - [Architectural invariants](./issues-and-my-solution-using-AI-effectively/My-Approach.md#4-i-would-define-architectural-invariants)
  - [Responsibilities & boundaries](./issues-and-my-solution-using-AI-effectively/My-Approach.md#5-architecturemd-would-explain-responsibilities-and-boundaries)
  - [Data ownership](./issues-and-my-solution-using-AI-effectively/My-Approach.md#6-i-would-document-data-ownership)
  - [Security model](./issues-and-my-solution-using-AI-effectively/My-Approach.md#7-securitymd-would-describe-the-security-model-not-just-security-best-practices)
  - [Trust boundaries](./issues-and-my-solution-using-AI-effectively/My-Approach.md#8-i-would-define-trust-boundaries)
  - [Sensitive data & secrets](./issues-and-my-solution-using-AI-effectively/My-Approach.md#9-i-would-keep-secrets-and-sensitive-data-out-of-the-ai-workflow)
  - [Architectural decision records](./issues-and-my-solution-using-AI-effectively/My-Approach.md#10-decisionsmd-would-preserve-architectural-reasoning)
  - [Requirements vs. implementation](./issues-and-my-solution-using-AI-effectively/My-Approach.md#11-i-would-distinguish-requirements-from-architectural-decisions)
  - [Architectural blast radius](./issues-and-my-solution-using-AI-effectively/My-Approach.md#12-i-would-classify-changes-by-architectural-blast-radius)
  - [Understand before implementing](./issues-and-my-solution-using-AI-effectively/My-Approach.md#14-first-i-would-ask-ai-to-understand-the-change)
  - [Plan before coding](./issues-and-my-solution-using-AI-effectively/My-Approach.md#15-then-i-would-ask-for-a-plan)
  - [Engineering prompts](./issues-and-my-solution-using-AI-effectively/My-Approach.md#16-my-prompts-would-become-engineering-instructions)
  - [Failure modes & edge cases](./issues-and-my-solution-using-AI-effectively/My-Approach.md#17-i-would-make-ai-explicitly-analyze-failure-modes)
  - [Concurrency & idempotency](./issues-and-my-solution-using-AI-effectively/My-Approach.md#18-i-would-pay-particular-attention-to-idempotency-and-concurrency)
  - [Adversarial AI review](./issues-and-my-solution-using-AI-effectively/My-Approach.md#19-i-would-separate-implementation-from-review)
  - [Contracts & compatibility](./issues-and-my-solution-using-AI-effectively/My-Approach.md#20-i-would-make-contracts-explicit)
  - [Observability](./issues-and-my-solution-using-AI-effectively/My-Approach.md#21-i-would-treat-observability-as-part-of-the-architecture)
  - [Continuously challenge the architecture](./issues-and-my-solution-using-AI-effectively/My-Approach.md#22-i-would-regularly-ask-ai-to-challenge-the-architecture-itself)
  - [Local vs. global decisions](./issues-and-my-solution-using-AI-effectively/My-Approach.md#23-the-key-distinction-local-decisions-versus-global-decisions)
  - [Automated guardrails](./issues-and-my-solution-using-AI-effectively/My-Approach.md#24-what-i-would-automate)
  - [AI-assisted engineering workflow](./issues-and-my-solution-using-AI-effectively/My-Approach.md#25-the-workflow-i-would-use)
  - [Evolving architectural knowledge](./issues-and-my-solution-using-AI-effectively/My-Approach.md#26-i-would-also-make-the-documentation-evolve-with-the-code)
  - [The changing role of the developer](./issues-and-my-solution-using-AI-effectively/My-Approach.md#27-the-role-of-the-developer-changes)
  - [What I would give AI](./issues-and-my-solution-using-AI-effectively/My-Approach.md#28-so-what-would-i-actually-give-ai)
  - [The real promise of AI-assisted development](./issues-and-my-solution-using-AI-effectively/My-Approach.md#29-the-real-promise-of-ai-assisted-development)


[← Back to Main page](../README.md)