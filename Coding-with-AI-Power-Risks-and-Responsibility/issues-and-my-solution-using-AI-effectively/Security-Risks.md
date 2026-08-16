[← Back to Main page](../README.md)

## How AI Introduces Security Risks and Vulnerabilities — and How I Handle Them

Security is one area where I am particularly careful with AI-assisted coding.

The problem is not that AI always writes insecure code. The problem is that **AI can produce code that looks secure, follows familiar patterns, passes basic tests, and is still vulnerable in ways that are easy to miss.**

I have experienced this myself.

The more code AI writes, the easier it becomes to assume that security has already been handled because the implementation looks professional.

That assumption is dangerous.

### The real problem: AI solves the coding problem, not necessarily the security problem

When I ask AI:

> “Add authentication to this API.”

AI can give me a working authentication flow very quickly.

But authentication is not the same as security.

There are many questions behind that seemingly simple request:

* Where are credentials stored?
* How are passwords hashed?
* How long are sessions or tokens valid?
* Can tokens be replayed?
* How are refresh tokens rotated?
* What happens after logout?
* How is authorization enforced?
* Can a user access another user's resources by changing an ID?
* What happens when an account is disabled?
* Are sensitive values appearing in logs?
* What happens when an external service fails?

AI may solve the first 80% of the implementation while missing an important security assumption in the remaining 20%.

And that 20% is often where the vulnerability lives.

---

## Example 1: Authorization that looks correct

One issue I pay particular attention to is authorization.

Imagine an API endpoint:

```text
GET /api/documents/{documentId}
```

AI might correctly check that the user is authenticated:

```text
if (!user.isAuthenticated()) {
    return unauthorized();
}
```

Everything looks fine.

But authentication only answers:

> “Who are you?”

It doesn't answer:

> “Are you allowed to access this document?”

If the code retrieves a document only by `documentId`, a user might change:

```text
/api/documents/1001
```

to:

```text
/api/documents/1002
```

and access somebody else's document.

The code is authenticated.

The endpoint works.

The tests may pass.

But the authorization boundary is missing.

### How I handle it

When AI generates security-sensitive code, I don't ask only:

> “Does authentication work?”

I explicitly ask:

> “What prevents a user from accessing a resource they don't own? Show me where authorization is enforced and what happens if the resource belongs to another user.”

I also test the **negative path**, not just the happy path.

For me, security testing is often about proving that something **cannot** happen.

That is a much better approach than simply asking AI to “make this endpoint secure.”

---

## Example 2: AI-generated file handling

File uploads are another area where I have seen AI produce code that looks perfectly reasonable.

Suppose I ask:

> “Add an endpoint for uploading profile images.”

AI may generate code that:

1. accepts the uploaded file,
2. checks the extension,
3. saves it,
4. returns the URL.

That sounds reasonable.

But checking `.jpg` or `.png` is not enough to establish that the uploaded content is actually what we expect.

There are also questions around:

* file size limits,
* content validation,
* storage location,
* executable content,
* filename handling,
* path traversal,
* access control,
* serving uploaded content safely.

The dangerous part is that the generated code may look complete.

### How I handle it

For security-sensitive features, I don't let the prompt be the security specification.

I define the security requirements first.

For example:

> “Uploads must have a strict size limit, must not be stored in an executable location, filenames supplied by users must never become filesystem paths, content must be validated, and downloads must respect authorization.”

Then I ask AI to implement **those requirements**.

This is an important difference in my workflow.

I don't ask AI:

> “Make file upload secure.”

I give AI the security constraints and ask it to implement them.

---

## Example 3: Secrets and credentials

AI can also introduce a much simpler but very common problem: secrets.

For example, while integrating an external API, AI might suggest:

```text
API_KEY = "..."
```

inside configuration or, worse, directly inside source code.

It may even generate examples using realistic-looking credentials and developers can accidentally leave them there.

I've also seen another variation: logging request objects or configuration values during debugging, without realizing that those objects contain tokens or credentials.

### How I handle it

I establish a hard rule:

> **Secrets never belong in source code, logs, prompts, or committed configuration.**

I use the appropriate secret-management mechanism for the environment, and I ask AI to work with references to secrets rather than the actual values.

I also review logging whenever AI touches authentication, external integrations, or request/response handling.

This is an area where a simple architectural rule is much more reliable than asking AI to remember to be secure every time.

---

## Example 4: SQL injection is not the only injection problem

Most developers know about SQL injection, so AI will often correctly use parameterized queries.

But injection problems are broader than SQL.

For example, if AI generates an administrative search feature, it may safely parameterize the database query but then place the returned value directly into an HTML response.

Now the database query is safe, but the application may still have an XSS problem.

The lesson for me is important:

> **Security cannot be checked at only one layer.**

A value can be safe for one context and dangerous in another.

### How I handle it

I ask AI to identify the **trust boundary** of the data.

For any user-controlled value, I want to know:

* Where does it enter the system?
* Where is it stored?
* Where is it transformed?
* Where is it rendered or executed?
* What validation or encoding is appropriate at that boundary?

This is more useful than simply asking:

> “Is this input sanitized?”

Because there isn't one universal form of “sanitized.”

The correct treatment depends on where the data is going.

---

## I don't use AI as my security approval process

This is probably the most important part of my approach.

I don't ask AI:

> “Is this code secure?”

And then accept:

> “Yes, this implementation follows security best practices.”

That is not a security review.

AI can help me find potential problems, but I don't use its own confidence as evidence that the code is secure.

Instead, I use AI as a **security assistant**.

For example:

> “Review this implementation as an attacker. Identify possible authorization bypasses, injection points, insecure data exposure, trust-boundary violations, credential handling problems, and failure scenarios.”

Then I investigate the findings myself.

For important systems, I still rely on established security practices, automated scanning, dependency auditing, code review, penetration testing, and security expertise where appropriate.

AI is another layer in that process, not the final authority.

---

## My approach: define security before asking AI to code

Over time, I have found that the most effective pattern is:

**Requirements → Security constraints → AI implementation → Security review → Tests**

Not:

**Prompt → AI code → “Looks secure” → Ship**

Before I ask AI to implement a security-sensitive feature, I try to define the threats and constraints first.

For example:

> “Users must only access resources belonging to their organization. Authentication is handled by the existing identity provider. Authorization must be checked at the resource boundary. Do not trust IDs supplied by the client. Do not log tokens or personal data.”

Now AI has something concrete to implement.

Afterward, I ask it to challenge the implementation:

> “Assume the client is malicious. How could they bypass this?”

That second question is often more valuable than asking AI to write the original code again.

---

## Why I prefer this approach

I could simply tell AI:

> “Write secure code.”

But that is too vague.

“Secure” depends on the application, the threat model, the data, the environment, and the consequences of failure.

My approach is better because **I define what security means for the specific feature before AI starts implementing it.**

That also keeps responsibility where it belongs.

AI can help me write the authentication middleware, validation, tests, database queries, and security checks.

But I decide:

* What needs to be protected.
* Who is allowed to access it.
* What the trust boundaries are.
* What happens when something fails.
* What risks are acceptable.
* What security requirements cannot be compromised.

After years, I've learned that security is rarely one clever piece of code.

It is a collection of decisions made across the system.

AI can help me make those decisions faster **once I know what questions need to be asked**.

That is why my rule with AI-assisted security is simple:

> **I don't ask AI to make my code secure. I define the security requirements, ask AI to implement them, and then try to break what it built.**

That keeps AI in the role where I find it most useful: **a very fast implementation and review assistant, but not the owner of the security model.**


[← Back to Main page](../README.md)