# CLAUDE.md

Instructions for Claude working in MagnaMining repositories. A repo can add its own rules
below these, but not contradict them.

## Before you start

- Read the README and CONTRIBUTING.md. Use the setup, lint and test commands they give.
- Work on a branch named `<type>/<short-description>`, using one of these types: feat, fix,
  chore, docs, refactor, test. Never commit to `main`.
- Only change what you were asked to. Don't rename, reformat or refactor other code.

## Commits

- Subject line: plain English that starts with a verb, under 72 characters, with no full stop.
  Say what the change does.
  Good: `Fix timezone drift in Avanti calendar sync`. Bad: `Update sync.py`, `fix: misc changes`.
- Body: leave it out unless the reason isn't obvious from the diff. If you add one, write a
  few lines on why. Don't list the files you changed.
- Don't add AI attribution: no `Co-Authored-By: Claude` line and no "Generated with Claude Code"
  line. The person who commits is the author.

## Pull requests

- The title follows the same rules as a commit subject. It becomes the commit on `main`.
- Fill in the PR template in a few lines. A reviewer should be able to read it in under a minute.
- Report test results honestly. If you didn't run something, say so.
- Don't add AI attribution to the PR description.

## Secrets and sensitive data

- Never commit secrets, keys, certificates, connection strings or `.env` files. Add each
  new config key to `.env.example` with a dummy value.
- Keep internal hostnames, IP addresses and network details out of code, comments, commits
  and PRs.
- Use made-up test data. Never copy production data into a test.
- If you find a secret in the repo, stop and tell the user. Don't try to clean up the
  history yourself.

## Changes that touch live systems

- New code that writes to an outside system (Entra, Exchange, AD or a vendor API) ships
  turned off, behind a setting that defaults to off. Until it's turned on, it logs what it
  would have done.
- Tests use mock services, never live ones. Don't run anything against production.
- Database migrations only move forward, and should add to the schema rather than rewrite it.
- Ask before you do anything that's hard to undo, such as deleting files or branches,
  force-pushing, running a migration or deploying.

## Before you say you're done

- Run the repo's lint and tests, and fix any failures you caused.
- Update the README if you changed how the project is set up, configured or deployed.
