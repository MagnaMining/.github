# How we work

This applies to every repository in the `magnamining` org. A repo can add its own rules,
but shouldn't contradict these.

These rules exist so the next person can read and fix what you wrote, even if that person
is you in eight months.

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

Merge or close a branch within days, not weeks. Long-lived branches cause merge conflicts.

## Commits

Write for the person reading `git log` to find out when something broke.

- **Subject line:** plain English that starts with a verb, under 72 characters, with no full
  stop. Say what the change does.
- **Body:** optional. Add one only when the reason isn't obvious from the diff, and keep it
  to a few lines on *why*. The diff already shows what changed.
- **One change per commit.** Don't mix a fix with unrelated cleanup.
- **No AI attribution.** Whoever commits is the author, whatever tool helped.

```
Fix timezone drift in Avanti calendar sync

Avanti sends shift times without a UTC offset and we were reading
them as UTC, so shifts after the DST change landed an hour early.
```

Not: `Update sync.py`, `fixes`, `WIP`, `feat(sync): misc changes`.

## Pull requests

Every change to `main` goes through a PR, including small ones and repos only you work on.
The PR is the record of why a change was made, and where CI runs before it goes live.

- **Title:** same rules as a commit subject. It becomes the commit on `main`.
- **Description:** fill in the template in a few lines. Aim for something a reviewer can
  read in under a minute. Leave out detail the diff already shows.
- **Before you open it:** CI passes, there are no secrets or internal hostnames in the
  diff, and the README still matches how the thing runs.
- **Merge:** squash and merge. That's the only method allowed, and the branch is deleted
  automatically.
- **Reviews:** none are required while the team is one person. Once there's a second
  developer, one approval will be required. Until then, ask someone to look at anything
  risky.

**Reviewing.** Be direct about problems and quick to approve. Say whether something is
broken or just not how you'd have done it. Only the first is a reason to block. Review
within two days.

## Claude

Claude is the only AI coding tool we use. Every repo has a `CLAUDE.md` at its root, copied
from the [org standard](https://github.com/magnamining/.github/blob/main/CLAUDE.md). Add
repo-specific rules below the org ones. Don't change the org rules in a single repo; change
them here.

## Every repo needs a README that answers

1. **What does this do, and what breaks if it stops?**
2. **How do I run it locally?** Exact commands. Assume a fresh laptop.
3. **How does it get deployed, and where does it run?**
4. **What config and secrets does it need, and where do they come from?** Names only,
   never values.
5. **Who owns it, and what's the first thing to check when it's broken?**

Without these answers, only one person can maintain the repo.

## Secrets

Never commit them. Push protection blocks known credential patterns, and a ruleset blocks
key, certificate and `.env` files.

If a secret gets through: **rotate it first, then clean the history.** Rotating it is the
fix. Removing it from git is cleanup. Deleting the commit doesn't help once a credential
has been on GitHub's servers.

Config that varies by environment goes in environment variables. Commit a `.env.example`
with the keys and dummy values so the next person knows what to set.

## Dependencies

Commit your lockfile: `package-lock.json`, a pinned `requirements.txt`, or `composer.lock`.
Builds you can reproduce matter more than being on the newest version.

Dependabot opens patch PRs. Triage them weekly. Handle security advisories the week they
appear; everything else can be batched.

## Naming repositories

```
magna-<system>-<function>     integrations       magna-topvu-vend-sync
<department>-<thing>          internal apps      helpdesk-form
ops-<area>                    operations tooling ops-ad-automation
```

Lowercase, hyphens, no underscores, no spaces. Choose a descriptive name over a clever one.
You'll be searching for it by name in two years.
