# Security Policy

## Scope

V3CT0R-CTF intentionally contains vulnerable challenges. **Vulnerabilities inside a challenge that are part of its intended solution are not security issues** and should not be reported here.

This policy covers **unintended** vulnerabilities, including:

* Flaws in deployment configs, scripts, or tooling in this repository that could affect hosts running them (for example container escapes, or a challenge that can reach beyond its sandbox)
* Unintended solutions that bypass a challenge's design, whether you found them during an event or afterward
* Exposed secrets, credentials, or personal data committed to the repository
* Vulnerabilities in event infrastructure operated by CTF-NGP

Out of scope: social engineering of organizers or participants, physical attacks, denial-of-service testing, and attacks on third-party services.

## Supported Versions

| Version | Supported |
|---------|-----------|
| `main` branch | Yes |
| Current event edition (2026) | Yes |
| Previous editions | Best effort |

## Reporting a Vulnerability

**Please do not open a public issue or pull request for security reports.**

Use one of the following private channels:

1. **GitHub private vulnerability reporting:** go to the repository's **Security** tab and select **Report a vulnerability**.
2. **Email:** **[support@cybernexus.live](mailto:support@cybernexus.live)**

Please include:

* A description of the issue and its impact
* The affected challenge, file, or component
* Steps to reproduce or a proof of concept
* Any suggested fix

### During a Live Event

If you find an unintended solution or platform bug while a competition is running, report it privately to the organizers right away instead of exploiting it or sharing it with other teams. Responsible reports may be acknowledged or rewarded at the organizers' discretion, and exploiting platform bugs for points may lead to disqualification under our [Code of Conduct](CODE_OF_CONDUCT.md).

## What to Expect

* **Acknowledgement** within 5 business days
* **Initial assessment** within 10 business days
* Regular updates until the issue is resolved
* Credit in release notes or advisories if you want it

## Safe Harbor

We will not pursue action against researchers who act in good faith, who test only against the in-scope targets listed above, who avoid privacy violations and service disruption, and who give us reasonable time to respond before any public disclosure.

## Disclosure

We ask that you keep reports confidential until a fix is available or 90 days have passed, whichever comes first. We will coordinate public disclosure with you.

## Running Challenges Safely

Challenges are untrusted code. Always run them in an isolated VM or network, never alongside production workloads, and never with sensitive credentials or data present.