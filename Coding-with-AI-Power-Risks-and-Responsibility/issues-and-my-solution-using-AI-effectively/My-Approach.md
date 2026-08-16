[← Back to Main page](../Readme.md)

# 1. I would establish the engineering context before writing the application

I would not create a huge documentation project.

I would create a small number of documents that answer the questions AI otherwise has to guess about.

At minimum, I would probably start with:

```text
AGENTS.md
ARCHITECTURE.md
SECURITY.md
DECISIONS.md
```

Depending on the project, I might also have documents for:

```text
API_CONTRACTS.md
OPERATIONS.md
DOMAIN.md
```

The important thing is not the filenames.

The important thing is that the repository contains a **persistent source of truth for engineering decisions**.

A conversation with AI is temporary.

The architecture of the system is not.

---

# 2. `AGENTS.md`: define how AI is allowed to work

I would use `AGENTS.md` or an equivalent file specifically for AI-assisted development.

It would describe things such as:

* repository structure
* important commands
* testing conventions
* coding conventions
* dependency rules
* architectural boundaries
* security-sensitive areas
* files AI should not modify without approval
* generated code rules
* migration rules
* required validation before completing a task

But I would avoid writing vague rules such as:

> “Write clean code.”

That gives AI almost nothing useful.

Instead, I would define **observable constraints**.

For example:

> **Controllers are responsible for transport concerns only. They may authenticate the request context, validate transport-level input, invoke application use cases, and map results to responses. Business invariants must not be implemented in controllers.**

Or:

> **Do not introduce an interface merely to make a class easier to mock. Introduce abstractions at genuine architectural boundaries, such as external systems or stable application/domain ports.**

Or:

> **Do not introduce a new dependency when the existing platform or project already provides equivalent functionality. If a new dependency appears necessary, stop and explain the requirement, alternatives considered, and operational/security implications.**

Or:

> **Do not modify authentication, authorization, tenancy, encryption, payment processing, or secret-management code without explicitly identifying the security and architectural impact.**

These rules are much more useful than generic instructions because they describe **how decisions are supposed to be made**.

---

# 3. I would document dependency direction, not just architecture diagrams

An architecture diagram such as:

```text
API
 ↓
Application
 ↓
Domain
 ↓
Infrastructure
```

is useful, but it is not enough.

The important part of architecture is not the boxes.

It is the **rules governing the relationships between the boxes**.

For example, I might define:

```text
API
  ↓
Application
  ↓
Domain

Infrastructure ─────→ Application/Domain abstractions
```

And then explicitly state:

* The domain has no dependency on infrastructure.
* API handlers do not access repositories directly.
* Infrastructure implementations do not leak into application contracts.
* Persistence models are not automatically public API models.
* External SDK types must not cross application boundaries.
* Application use cases own orchestration.
* Domain objects own domain invariants.
* Infrastructure owns technical concerns such as persistence and external integrations.

This matters because AI is very good at making a local change that looks reasonable while violating a global architectural constraint.

For example, AI might discover that a controller needs some database information and write:

```text
Controller → Repository
```

The code may work.

The tests may even pass.

But the architecture has now changed.

That is exactly the sort of mistake I want the repository's rules to make difficult.

---

# 4. I would define architectural invariants

This is one of the most important things I would add to a new AI-assisted project.

I don't just want to describe the architecture.

I want to describe the things that **must remain true as the system evolves**.

For example:

> * Domain code must not depend on infrastructure packages.
> * API handlers must not directly access persistence.
> * Database entities must not become public API contracts by accident.
> * External provider SDK types must not leak into domain code.
> * Authorization decisions must be enforced consistently regardless of whether a request originates from HTTP, a background job, or a message consumer.
> * Data owned by another bounded context must not be modified through direct database access.
> * Public API changes must be evaluated for backward compatibility.
> * Domain invariants must remain valid regardless of the transport mechanism.

These are **architectural invariants**.

They are much more useful to AI than saying:

> “Please follow our architecture.”

They also give me something I can potentially enforce automatically through:

* static analysis
* dependency rules
* architecture tests
* CI
* schema validation
* linters
* code review

That is an important principle for me:

> **If a rule matters enough to repeat to AI, I should ask whether I can make the system enforce it automatically.**

I would rather have CI reject an architectural violation than repeatedly tell an AI agent not to make the same mistake.

---

# 5. `ARCHITECTURE.md` would explain responsibilities and boundaries

I would use `ARCHITECTURE.md` to document the significant architectural decisions and boundaries.

Not every class.

Not every function.

The important questions are:

* What are the major components?
* What does each component own?
* What does each component explicitly not own?
* What dependencies are allowed?
* Where are transactions controlled?
* Where are domain invariants enforced?
* Where does authorization happen?
* Where does integration with external systems happen?
* Which component owns each piece of data?
* Which interactions are synchronous?
* Which are asynchronous?
* What consistency guarantees exist?

For example, if the system contains Orders and Payments, I would want AI to know whether:

```text
Order owns order state.
Payment owns payment state.
```

and whether:

```text
Order Service → Payment Service
```

is a synchronous API call, an asynchronous message, or something else.

I would also explicitly document what AI must **not** do.

For example:

> **The Order component must not update Payment-owned data directly in the database. Cross-component state changes must use the published application or messaging boundary.**

That single rule can prevent a surprising amount of architectural decay.

---

# 6. I would document data ownership

This becomes particularly important as a system grows.

For every significant piece of data, I want to know:

> **Who owns it?**

For example:

```text
User
  owned by Identity

Order
  owned by Ordering

Payment
  owned by Payments

Invoice
  owned by Billing
```

Then I would document:

* authoritative source
* allowed writers
* consumers
* replication/caching rules
* consistency expectations
* deletion/retention requirements

Otherwise AI will eventually encounter a situation where it needs information from another subsystem and make the obvious implementation choice:

> “I'll just query that table.”

That may be technically easy and architecturally disastrous.

A useful rule would therefore be:

> **Ease of access does not imply ownership. Do not bypass an architectural boundary merely because the underlying database is technically accessible.**

---

# 7. `SECURITY.md` would describe the security model, not just security best practices

I would not want a security document that simply says:

* don't log passwords
* use HTTPS
* scan dependencies

Those are useful, but they don't describe the application's security architecture.

I would document things such as:

### Authentication

How identity is established.

For example:

```text
Request
  ↓
Authentication
  ↓
Authenticated principal
```

### Authorization

Then separately:

```text
Authenticated principal
  ↓
Authorization policy
  ↓
Allowed operation on resource
```

I would explicitly distinguish authentication from authorization.

Authentication answers:

> Who are you?

Authorization answers:

> Are you allowed to perform this operation on this resource?

That distinction matters enormously in AI-generated code.

For example, a controller checking:

```text
if user.isAuthenticated()
```

does not necessarily mean the user is authorized to modify:

```text
/order/123
```

The authorization model might instead require:

* role checks
* ownership checks
* tenant membership
* resource policies
* administrative privileges

I want AI to understand that model before it changes security-sensitive code.

---

# 8. I would define trust boundaries

For security-sensitive systems, I would explicitly document trust boundaries.

For example:

```text
Internet
   ↓
API Gateway
   ↓
Application
   ↓
Database
```

and:

```text
Application
   ↓
External Payment Provider
```

The interesting questions are:

* Which inputs are untrusted?
* Which identities are trusted?
* Where is validation performed?
* Where is authorization performed?
* What information may cross each boundary?
* What happens when an external system lies, fails, or behaves unexpectedly?

This gives AI much more useful context than:

> “Remember security.”

---

# 9. I would keep secrets and sensitive data out of the AI workflow

This would be non-negotiable.

I would never paste:

* production credentials
* private keys
* access tokens
* customer data
* real payment information
* sensitive production logs
* internal secrets

into a prompt simply because it makes debugging easier.

If AI needs configuration information, I would provide a sanitized version:

```text
DATABASE_URL=<redacted>
API_KEY=<redacted>
TENANT_ID=<example>
```

The same applies to logs.

Instead of giving AI:

```text
customer@example.com
account_id=847293
token=eyJ...
```

I would give it:

```text
user=<redacted>
account_id=<example>
token=<redacted>
```

The principle is simple:

> **Give AI the information it needs to reason about the problem, not information it does not need.**

---

# 10. `DECISIONS.md` would preserve architectural reasoning

I would also keep lightweight architectural decision records.

Not every decision deserves documentation.

But decisions that affect the future shape of the system do.

For example:

* database selection
* messaging architecture
* authentication strategy
* tenancy model
* consistency model
* API versioning
* storage strategy
* major integration patterns

I would record more than:

> “We chose PostgreSQL.”

I would capture something closer to:

```text
Decision:
Use PostgreSQL as the primary transactional datastore.

Context:
The domain requires transactional consistency across several related
entities. Access patterns are predominantly relational.

Alternatives considered:
PostgreSQL
MongoDB
DynamoDB

Decision:
Use PostgreSQL.

Consequences:
We accept relational schema management and migration requirements.
We gain strong transactional semantics and familiar query capabilities.

Rejected alternatives:
MongoDB would not materially simplify the current domain.
DynamoDB would introduce unnecessary access-pattern constraints
for the current requirements.
```

The important part is not the decision itself.

It is the **reasoning behind the decision**.

Without that history, AI sees an existing architecture and may assume it is accidental.

Six months later it may confidently suggest:

> “We could simplify this by replacing PostgreSQL with X.”

The ADR tells it:

> We already considered that.

---

# 11. I would distinguish requirements from architectural decisions

This is another rule I would explicitly teach AI.

Suppose the requirement is:

> Checkout should not block while confirmation is being processed.

That is a business/system requirement.

It does not automatically mean:

> Introduce Kafka.

The architecture might eventually be:

```text
Checkout
   ↓
Durable message
   ↓
Confirmation worker
```

But there are many possible implementations.

I want AI to distinguish:

```text
Requirement
    ↓
Architectural decision
    ↓
Implementation choice
```

Rather than:

```text
Requirement
    ↓
AI chooses technology
```

So I might tell it:

> **Do not turn an implementation preference into an architectural requirement. If the task implies a significant architectural change, explain the alternatives and consequences before implementation.**

This is especially important because AI has enormous exposure to popular technologies and patterns.

The fact that something is common does not mean it belongs in this system.

---

# 12. I would classify changes by architectural blast radius

I would not give AI the same degree of autonomy for every task.

A useful model would be:

### Level 1 — Local change

Examples:

* isolated bug fix
* test improvement
* validation change
* small refactoring

AI can usually implement these with relatively high autonomy.

### Level 2 — Cross-component change

Examples:

* new application use case
* persistence changes
* API contract changes
* integration with an existing subsystem

AI should first explain the affected boundaries and provide an implementation plan.

### Level 3 — Architectural change

Examples:

* introducing a message broker
* changing database technology
* changing consistency guarantees
* changing the tenancy model
* introducing a new bounded context
* changing authentication architecture

AI should stop and explain the architectural consequences before implementation.

### Level 4 — Security-critical change

Examples:

* authorization
* identity
* cryptography
* secrets
* payment flows
* sensitive data handling

AI can assist, but I would require explicit human review.

The principle is:

> **The greater the architectural blast radius, the lower the autonomous authority I give AI.**

---

# 13. How I would actually use AI during development

Once the foundation is established, AI becomes part of the normal development workflow.

But I would not start every task with:

> “Implement this.”

For a non-trivial change, my workflow would be:

```text
Understand
    ↓
Identify constraints
    ↓
Plan
    ↓
Review plan
    ↓
Implement
    ↓
Test
    ↓
Challenge
    ↓
Human review
    ↓
Merge
```

The distinction is important.

AI is involved throughout the process.

But AI does not own the process.

---

# 14. First, I would ask AI to understand the change

For a meaningful feature, I might start with:

> Read `AGENTS.md`, `ARCHITECTURE.md`, `SECURITY.md`, the relevant ADRs, and the existing implementation.
>
> Do not modify anything.
>
> Explain:
>
> 1. Which architectural boundary this change belongs to.
> 2. Which existing components are relevant.
> 3. Which domain rules apply.
> 4. Which security rules apply.
> 5. Which dependencies may be affected.
> 6. Whether the change alters an existing contract or consistency guarantee.
> 7. Any ambiguity that must be resolved before implementation.
>
> If the repository does not contain enough information to determine the correct architectural behavior, identify the missing information instead of inventing a design.

That last instruction is important.

I don't want AI filling architectural gaps with plausible guesses.

I want it to **surface ambiguity**.

---

# 15. Then I would ask for a plan

Once AI understands the problem, I would ask:

> Based on the existing architecture, propose the smallest implementation plan.
>
> Reuse existing patterns where possible.
>
> Identify:
>
> * files/components that will change
> * new abstractions, if any
> * database changes
> * API contract changes
> * security implications
> * transaction/consistency implications
> * failure modes
> * testing strategy
>
> Do not introduce a new dependency or architectural component without explaining why the existing system cannot satisfy the requirement.

Then I review the plan.

This is where I can catch bad architectural direction **before AI writes hundreds of lines of code**.

---

# 16. My prompts would become engineering instructions

I wouldn't generally write:

> “Create a user management API.”

I'd provide context, intent, constraints, and a definition of done.

For example:

> **Context**
>
> Read `AGENTS.md`, `ARCHITECTURE.md`, `SECURITY.md`, and the existing user-management implementation.
>
> **Requirement**
>
> Administrators must be able to deactivate a user.
>
> **Before implementation**
>
> Determine what “deactivated” means in the existing domain model. Identify whether deactivation affects authentication, existing sessions, authorization, API keys, background processing, or downstream consumers.
>
> **Constraints**
>
> * Do not introduce a new dependency.
> * Do not modify the authentication flow without explicit approval.
> * Reuse the existing authorization mechanism.
> * Keep domain invariants outside the API layer.
> * Use the existing persistence abstraction.
> * Do not access another bounded context's database directly.
> * Follow the existing error-handling and audit-event patterns.
> * Do not modify unrelated code.
>
> **Architectural analysis**
>
> Before coding, identify:
>
> 1. The domain invariant being introduced.
> 2. The application use case responsible for the state transition.
> 3. The authorization boundary.
> 4. The persistence boundary.
> 5. The transaction/consistency requirements.
> 6. The authentication/session implications.
> 7. The audit requirements.
> 8. The concurrency and idempotency behavior.
>
> **Implementation**
>
> Make the smallest change that satisfies the requirement and preserves the existing architectural invariants.
>
> **Validation**
>
> Run the relevant tests, static analysis, formatting, and security checks.
>
> Then report:
>
> * what changed
> * what assumptions were made
> * which architectural invariants were affected
> * which failure cases were tested
> * any remaining concerns requiring human review

That is very different from asking AI to generate code.

I'm giving it **context, intent, constraints, boundaries, and a definition of done**.

---

# 17. I would make AI explicitly analyze failure modes

One of the most useful things AI can do is challenge the happy path.

I don't just want:

> “What happens when everything works?”

I want:

> **How can this fail?**

For an important feature, I would ask AI to consider at least:

### Domain failures

* invalid state transitions
* violated invariants
* duplicate commands
* conflicting operations

### Concurrency failures

* lost updates
* race conditions
* duplicate requests
* stale reads
* concurrent state transitions

### Distributed-system failures

* timeouts
* retries
* partial success
* duplicate messages
* out-of-order messages
* downstream outages

### Security failures

* privilege escalation
* broken object-level authorization
* tenant isolation failures
* replay
* unauthorized resource access
* sensitive-data exposure

### Operational failures

* database unavailable
* external dependency unavailable
* queue backlog
* deployment mismatch
* migration failure
* schema/version incompatibility

This changes the development conversation from:

> “Make it work.”

to:

> **“Define how it behaves when the system does not work normally.”**

That is a much more important engineering question.

---

# 18. I would pay particular attention to idempotency and concurrency

AI-generated code tends to focus heavily on sequential execution.

Real systems don't behave that way.

If an endpoint says:

```text
POST /payments
```

I immediately want to know:

> What happens if the client sends the same request twice?

If a message consumer receives:

```text
PaymentCompleted
```

twice, I want to know:

> Is processing idempotent?

If two administrators modify the same resource simultaneously:

> Which update wins?

If a request times out after the server commits the transaction:

> What happens when the client retries?

These are not implementation details.

They are part of the system's behavioral contract.

I would therefore explicitly ask AI to identify:

* idempotency requirements
* concurrency assumptions
* transaction boundaries
* retry behavior
* failure recovery

before implementing important workflows.

---

# 19. I would separate implementation from review

After AI implements something, I would use it again—but in a different mode.

I would not ask:

> “Is this code correct?”

That invites confirmation.

I would ask:

> **Assume this implementation is wrong. Try to find concrete scenarios in which it violates the requirements, architectural invariants, security model, consistency guarantees, or existing contracts.**
>
> For every concern, identify the exact code path and explain the failure scenario.
>
> Do not produce generic recommendations. Focus on concrete, testable problems.

That's a much better adversarial review.

But I would not treat that as independent verification.

The AI that produced the implementation may share the same assumptions that produced the bug.

For important changes, I still want normal human review.

---

# 20. I would make contracts explicit

As systems grow, architecture isn't just about internal code structure.

There are contracts everywhere:

* HTTP APIs
* events
* messages
* database schemas
* configuration
* integrations
* authentication claims

I would therefore teach AI to ask:

> **Who consumes this contract?**

For example, if an API currently exposes:

```json
{
  "id": "123",
  "name": "John",
  "status": "active"
}
```

AI might decide that:

```json
status
```

should become:

```json
state
```

because `state` is "cleaner."

That may be a perfectly reasonable internal refactoring and a breaking public API change.

The correct instruction is:

> **Do not change public contracts merely to make the internal design cleaner. Identify consumers, compatibility requirements, versioning strategy, and migration implications first.**

That is the kind of distinction that becomes increasingly important as AI accelerates development.

---

# 21. I would treat observability as part of the architecture

I would also establish observability conventions early.

Not simply:

> “Add logging.”

I'd define:

* structured logging
* correlation/request IDs
* metrics
* distributed tracing where appropriate
* audit events
* PII restrictions
* error classification
* operational versus business events

For example:

> An administrator deactivated a user

is a different type of information from:

> Database connection failed after three retries.

The first may belong in an audit trail.

The second belongs in operational telemetry.

Those are different concerns and should not automatically be treated as the same kind of log entry.

Again, I want AI to understand the architectural distinction rather than merely produce logging statements.

---

# 22. I would regularly ask AI to challenge the architecture itself

As the system grows, I would periodically stop asking AI to implement features and instead ask it to inspect the system.

For example:

> Review the current repository against `ARCHITECTURE.md` and our documented architectural invariants.
>
> Identify:
>
> * dependency-boundary violations
> * responsibilities leaking between layers
> * infrastructure concerns appearing in domain code
> * duplicated business rules
> * abstractions without a clear architectural purpose
> * direct access to data owned by another component
> * inconsistent integration patterns
> * public contract changes that may create compatibility problems
>
> For each finding, identify the concrete code path and explain why it violates the documented architecture.

And separately:

> Review the system from a security perspective.
>
> Identify trust boundaries, authorization assumptions, tenant-isolation risks, sensitive-data exposure, dependency risks, and areas that require manual security review.
>
> Do not assume that existing code is correct simply because tests pass.

At this point AI becomes more than a code generator.

It becomes another set of eyes over a system that is increasingly difficult for one person to hold entirely in their head.

---

# 23. The key distinction: local decisions versus global decisions

This is probably the most important rule I would establish.

> **AI should be allowed to make local implementation decisions, but it should not silently make global architectural decisions.**

For example:

### Local decision

Should this method be extracted into a helper?

AI can probably decide.

### Global decision

Should we introduce Kafka?

AI should explain the alternatives and consequences before doing it.

---

### Local decision

How should this validation be structured?

AI can decide within existing conventions.

### Global decision

Should authorization move from the application layer into an API gateway?

That is an architectural decision.

---

### Local decision

Which existing repository method should be reused?

AI can choose.

### Global decision

Should this service access another service's database directly?

That requires an explicit architectural decision.

This gives me a useful operating principle:

> **AI can implement decisions. It should not quietly invent consequential ones.**

---

# 24. What I would automate

I would also look for every important rule that can be enforced mechanically.

For example:

```text
Architecture rule
      ↓
Can a tool enforce it?
      ↓
Yes → automate it
No  → document + review
```

Examples might include:

* dependency direction
* formatting
* linting
* test execution
* dependency vulnerability scanning
* secret detection
* API schema compatibility
* migration checks
* generated-code validation
* forbidden imports
* coverage thresholds where meaningful

This is one of the biggest lessons I would apply to AI-assisted development:

> **Don't rely on AI remembering a rule that the engineering system can enforce.**

Prompts are useful.

Automated constraints are better.

---

# 25. The workflow I would use

If I had to summarize my development process tomorrow, it would look something like this:

```text
Define the problem
       ↓
Separate requirements from implementation choices
       ↓
Define architecture and boundaries
       ↓
Define security model and data ownership
       ↓
Record important architectural decisions
       ↓
Automate enforceable constraints
       ↓
Ask AI to understand the change
       ↓
Identify ambiguity and architectural impact
       ↓
Review the implementation plan
       ↓
Let AI implement
       ↓
Run automated checks
       ↓
Ask AI to attack the implementation
       ↓
Human review
       ↓
Merge
       ↓
Update architectural knowledge when the system changes
```

Notice what is deliberately missing:

```text
Give AI a blank repository
       ↓
"Build the application"
```

That is not the workflow I want.

---

# 26. I would also make the documentation evolve with the code

There is a danger in treating these documents as static configuration.

They aren't.

If the architecture changes, the documentation has to change with it.

If we introduce asynchronous processing, I expect:

```text
ARCHITECTURE.md
DECISIONS.md
OPERATIONS.md
```

to potentially change.

If we change authorization, I expect:

```text
SECURITY.md
ARCHITECTURE.md
```

to change.

If we introduce a new public API contract, I expect the relevant contract documentation and compatibility rules to change.

I would therefore treat architectural documentation as part of the system itself.

Not bureaucracy.

**Architecture as code-adjacent knowledge.**

---

# 27. The role of the developer changes

This is where I think the biggest shift is happening.

When AI can generate implementation extremely quickly, the scarce resource is no longer simply:

> “Who can type the code fastest?”

The scarce resources become:

* understanding the problem
* making good architectural decisions
* recognizing hidden constraints
* defining invariants
* understanding failure modes
* evaluating trade-offs
* validating security assumptions
* understanding operational consequences
* knowing what should not be changed

That makes engineering judgment more valuable, not less.

AI can write a repository implementation.

It cannot be allowed to decide, without oversight:

> “This system should now have a completely different consistency model.”

That's my job.

---

# 28. So what would I actually give AI?

Not a blank repository.

I would give it something more like this:

```text
                    ENGINEERING INTENT

                         ↓

              Architecture & Boundaries
                         ↓
              Security & Trust Model
                         ↓
                Data Ownership Rules
                         ↓
               Architectural Decisions
                         ↓
                Automated Guardrails
                         ↓
                 AI Agent / Developer
                         ↓
          Implementation within boundaries
                         ↓
              Automated Verification
                         ↓
                  Adversarial Review
                         ↓
                   Human Judgment
```

The important part is that AI operates **inside** the engineering system.

It isn't the engineering system.

---

# 29. The real promise of AI-assisted development

After 15+ years of software development, I don't think the interesting future is:

> “AI can write code.”

We already know that.

The more interesting question is:

> **What happens when an experienced engineer can delegate a large percentage of implementation work while retaining control over architecture, constraints, security, and system behavior?**

That is where the leverage becomes significant.

I can spend less time writing repetitive implementation code and more time thinking about:

* whether the domain model is correct
* whether the boundaries are right
* whether the consistency guarantees are appropriate
* whether the security model is sound
* what happens when dependencies fail
* how the system evolves
* what decisions we are going to regret later

And AI can help me investigate, implement, test, challenge, and document those decisions much faster.

But I still own the decisions.

---

# The principle I would take into a new project

If I had to reduce everything above to one idea, it would be this:

> **Give AI high autonomy where the blast radius is small and the constraints are explicit. Give AI low autonomy where the decision changes architecture, security, data ownership, contracts, or system behavior.**

The goal isn't to keep AI away from the important parts of engineering.

Quite the opposite.

I want AI involved in those parts.

I want it to analyze the architecture.

Challenge my assumptions.

Find inconsistencies.

Explore alternatives.

Identify failure modes.

Review the implementation.

Suggest improvements.

But I don't want it quietly turning those suggestions into architectural decisions.

That distinction matters because AI changes the economics of implementation.

It does not remove the need for engineering intent.

In fact, I would argue that the opposite is true.

**The faster AI can change a system, the more important it becomes to know exactly which parts of that system are allowed to change—and which parts are not.**

So if I started a project tomorrow, I wouldn't give AI a blank repository and say:

> “Build this for me.”

I'd give it a **well-defined engineering environment, explicit architectural boundaries, documented decisions, automated guardrails, and a clear definition of what it is allowed to decide.**

Then I would let it move extremely fast.

Because that, to me, is the real promise of AI-assisted development:

> **Not AI replacing the developer, but an experienced engineer using AI to dramatically increase implementation capacity while retaining ownership of the architecture and the system's behavior.**


[← Back to Main page](../README.md)