# Security policy

## Reporting a vulnerability

**Do not open a public issue for a security problem.**

Report it through GitHub's [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
on the affected repository — the **Security** tab, then **Report a vulnerability** —
or contact an org owner directly.

Include what you found, how to reproduce it, and what an attacker could do with it.
You'll get an acknowledgement within two business days.

## If a credential leaks

Speed matters more than tidiness. In order:

1. **Rotate the credential.** Immediately, before anything else. A revoked secret is
   harmless no matter how many places it's been copied to.
2. **Check whether it was used.** Audit logs on the affected platform — GitHub,
   Microsoft 365, or the vendor system.
3. **Tell an org owner.** Even if you caught it yourself and rotated it in 30 seconds.
4. **Then clean up** the git history if it landed in a commit.

Nobody is in trouble for reporting a leaked credential quickly. Finding out late is
the expensive outcome.

## What's enabled org-wide

- **Secret scanning with push protection** — blocks commits containing recognized
  credential patterns before they reach the server.
- **Dependabot alerts and security updates** — vulnerable dependencies get flagged
  and patched automatically where possible.
- **Private vulnerability reporting** — on all repos.

## Handling sensitive material

Some repos in this org touch certificates, service accounts, and infrastructure config.

- Private keys and certificates never go in git, including in `.gitignore`d directories
  that might get force-added later. Use the platform's certificate store.
- Internal hostnames, IP ranges, and network topology stay out of public repos.
- Test fixtures use synthetic data. Never a snapshot of production.
