# How we work

This applies to every repository in the `magnamining` org. Individual repos can add to
it, but shouldn't contradict it.

We're a small team. These conventions exist so that code written by one person is
readable by the next person — including you, eight months from now, at 2am, when the
sync job has stopped running and you don't remember writing any of this.

## Branches

`main` is always deployable. Work happens on short-lived branches off `main`.

```
<type>/<short-description>

feat/topvu-shift-mapping
fix/calendar-sync-timezone-drift
chore/bump-node-22
docs/working-alone-runbook
```

Types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`.

Keep branches short-lived. A branch that has been open for three weeks is a merge
conflict wearing a trench coat.

## Commits

Write the message for the person running `git log` to figure out when something broke.

```
Fix timezone drift in Avanti calendar sync

Avanti returns shift times in local time without an offset. We were
parsing them as UTC, so every shift after the DST change landed an
hour early.

Parse with the site's timezone explicitly rather than relying on the
runtime default.
```

Subject line in the imperative mood, under ~72 characters, no trailing period. If the
change needs explaining, explain **why** in the body — the diff already shows what.

## Pull requests

Every change to `main` goes through a PR. Yes, including small ones. Yes, even when
you're the only person who touched that repo.

The point isn't ceremony — it's that PRs give you a searchable record of why a change
happened, a place for CI to run before the change is live, and a diff someone can read
later without reconstructing it from commits.

**Before you open it:**
- CI passes locally if you can run it
- No secrets, tokens, connection strings, or internal hostnames in the diff
- The README still matches reality if you changed how the thing runs

**Reviews.** One approval is required on `main`. If you're genuinely the only person
available — the other dev is away and it can't wait — an org admin can bypass. Use that
for real urgency, not to skip a review that would have been mildly inconvenient.

If you bypass, say so in the PR description and get a post-hoc read when someone's back.

**Reviewing.** Be direct about problems and quick about approving. Distinguish
"this is broken" from "I'd have done it differently" — the second one is a comment,
not a blocker. A PR sitting unreviewed for two days is worse for the codebase than
almost anything that could be in it.

## Merging

**Squash and merge** is the default. One PR, one commit on `main`, clean history.

Use a merge commit only when the individual commits genuinely tell a story worth
keeping. Rebase-and-merge if you've curated the commits deliberately.

Delete the branch after merging. The button does it for you.

## Every repo needs a README that answers

1. **What does this do, and what breaks if it stops?**
2. **How do I run it locally?** Exact commands. Assume a fresh laptop.
3. **How does it get deployed, and where does it run?**
4. **What config and secrets does it need, and where do they come from?** Names only,
   never values.
5. **Who owns it, and what's the first thing to check when it's broken?**

A repo without this is a repo only one person can maintain.

## Secrets

Never commit them. Push protection will block you, which is the system working
correctly.

If you push a secret and it gets through: **rotate it first, then clean the history.**
Rotation is the fix. Removing it from git is cleanup. Deleting the commit does not
un-leak a credential that was on GitHub's servers.

Config that varies by environment goes in environment variables. Commit a
`.env.example` with the keys and dummy values so the next person knows what to set.

## Dependencies

Commit your lockfile — `package-lock.json`, `requirements.txt` pinned, `composer.lock`.
Reproducible builds matter more than being on the newest version.

Dependabot opens patch PRs. Triage them weekly. Security advisories get handled the
week they appear; everything else can batch.

## Naming repositories

```
magna-<system>-<function>     integrations       magna-topvu-vend-sync
<department>-<thing>          internal apps      helpdesk-form
ops-<area>                    operations tooling ops-ad-automation
```

Lowercase, hyphens, no underscores, no spaces. Descriptive over clever — you will
search for these by name in two years.
