# What I Would Do If I Accidentally Pushed a Secret to GitHub

If something like this happened to me, I wouldn't just delete the file and push another commit.

I would treat the secret as **compromised** and follow a simple process:

```text
Secret accidentally pushed
        ↓
Revoke / rotate the secret
        ↓
Check if it was used
        ↓
Remove it from Git history
        ↓
Store the new secret securely
        ↓
Prevent it from happening again
```

## 1. Revoke or Rotate the Secret First

My first priority would be making the exposed secret useless.

For example:

- old API key → revoke
- new API key → create
- new API key → GitHub Secret

I wouldn't start by cleaning Git history because even after removing the commit, the old secret may already have been copied or accessed.

## 2. Check Whether It Was Used

If the secret had access to a database, cloud account, API, or other important service, I would check the relevant logs and audit history.

I'd want to know:

- Was the secret used?
- When was it used?
- What did it access?
- Does anything suspicious need to be investigated?

## 3. Remove the Secret From Git History

After the credential is no longer valid, I would clean the secret from the repository history if necessary.

For example, if I accidentally committed `.env`, I could use `git-filter-repo` to remove the file from the repository history.

I would be careful with this because rewriting history changes commit hashes and can affect other developers.

I wouldn't use:

- `git revert`

as the solution.

A revert removes the change from the current version of the project, but the secret can still exist in the previous Git history.

## 4. Move the New Secret to the Right Place

After rotating the credential, I would make sure the new value isn't going back into the repository.

For example:

- API key → GitHub Secret
- Database password → GitHub Secret / Secret Manager
- `.env` → `.gitignore`

## 5. Prevent the Same Mistake

Finally, I would look at why the secret was pushed in the first place.

I would make sure I have:

- `.gitignore`
- Secret Scanning
- Push Protection
- GitHub Secrets
- Branch protection
- Code review

The goal wouldn't just be to fix my mistake.

It would be to make the same mistake harder to make again.

## My Rule of Best Practice

If I accidentally push a secret, I assume it is compromised. I revoke it first, investigate it, clean the history if necessary, and then improve the process so it doesn't happen again.
