[← Back to Main README](../Readme.md)

# How I Secure Environment Variables

One of my first lessons about Git security actually started with something much less serious — `node_modules`. 😄

The first time I pushed one of my Node.js projects, I somehow managed to push **all of `node_modules`** into the repository.

Thousands of files later, I learned an important lesson:

> **Not everything in my project folder belongs in Git.**

That was what introduced me to `.gitignore`.

Once I started working with real projects, I realized this wasn't only about keeping `node_modules` out of the repository. I also had to make sure that sensitive configuration and secrets never got committed.

## How I Handle Environment Variables

I don't want things like:

```text
API keys
database passwords
access tokens
private keys
cloud credentials
```
sitting directly in my repository.

For my GitHub Actions workflows, I use **GitHub Secrets** to store sensitive values instead of putting them directly into workflow files or source code.

For example:

```text
env:

  API_KEY: ${{ secrets.API_KEY }}
```

I can also keep secrets specific to different environments, such as `test` and `production`.

That means I don't need to expose production credentials to workflows that only need access to the test environment.

## What I Never Commit

I also make sure sensitive files don't accidentally get pushed:


```text
.env
.env.*
*.pem
*.key
credentials.json
service-account.json
```

I normally add the appropriate files to `.gitignore`:

```text
node_modules/
.env
.env.*
*.pem
*.key
credentials.json
service-account.json
```


But I don't rely only on `.gitignore`.

Before pushing, I still pay attention to what I'm actually committing.

The important rule for me is:

> **If a file contains a secret, it doesn't belong in Git.**

Even if the repository is private, I don't treat Git as a secret vault.

## My Rule of Best Practice

```text
Sensitive value?
        ↓
GitHub Secret / proper secret manager


Configuration?
        ↓
Environment variable


Dependency files?
        ↓
.gitignore


Code?
        ↓
Never hardcode credentials
```


And if I ever accidentally commit a real secret, I don't just delete the file and assume everything is fine.

I **rotate or revoke the exposed credential first**, because once a secret has been committed, I have to assume it may have been exposed.

Then I clean up the repository history if necessary.

My biggest lesson from accidentally pushing `node_modules` was simple:

> **Git should contain the source code and files needed to build the project — not every file sitting on my machine. 😄**

[← Back to Main README](../Readme.md)