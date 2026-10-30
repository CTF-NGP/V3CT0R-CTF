# Contributing to V3CT0R-CTF

Thanks for helping make V3CT0R-CTF better. This guide explains how to contribute challenges, fixes, and documentation.

By participating, you agree to follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## Ways to Contribute

* **Submit a challenge** for a current or future edition
* **Fix a bug** in a challenge, deployment config, or script
* **Improve documentation**, including challenge descriptions and READMEs
* **Add a write-up** for a challenge from a finished event
* **Report problems** by opening an issue

> **Found a security vulnerability in our infrastructure or tooling?** Do not open a public issue. Follow [SECURITY.md](SECURITY.md) instead.

## Getting Started

1. Fork the repository and clone your fork:
   ```bash
   git clone https://github.com/<your-username>/V3CT0R-CTF.git
   cd V3CT0R-CTF
   ```
2. Create a branch from `main`:
   ```bash
   git checkout -b add-<category>-<challenge-name>
   ```
3. Make your changes, commit, and push.
4. Open a pull request against `main`.

## Challenge Submission Guidelines

Place each challenge at `<year>/<category>/<challenge-name>/` with this structure:

```
<challenge-name>/
├── README.md             # Required: description, hints, author, difficulty
├── dist/                 # Files given to players
├── src/                  # Source and Dockerfile (hosted challenges)
└── solution/             # Required: writeup and working solve script
```

### Requirements

* **Original work.** Challenges must be your own or properly credited and licensed for reuse under Apache-2.0 compatible terms.
* **Flag format:** `V3CT0R{...}`. Flags must be unique and must not appear in `dist/`.
* **Solvable and tested.** Include a solve script and confirm it works against a fresh deployment.
* **Reproducible.** Hosted challenges must build and run from the provided Dockerfile or Compose file with no manual steps.
* **Contained.** No challenge may access the network beyond what it needs, persist outside its container, or harm the host or other players.
* **Fair.** Avoid guessing, excessive brute force, and reliance on unrelated external services.
* **No real secrets.** Never commit real credentials, personal data, or production keys.
* **Difficulty label:** Mark each challenge as Easy, Medium, Hard, or Insane in its README.

### Challenge README Template

```markdown
# <Challenge Name>

**Category:** Web | Pwn | Reverse | Crypto | Forensics | OSINT | Misc
**Difficulty:** Easy | Medium | Hard | Insane
**Author:** <your handle>

## Description
<Text shown to players>

## Hints
1. <Optional hint>

## Deployment
<How to build and run, ports used>

## Solution
See `solution/`.
```

## Pull Request Guidelines

* Keep each PR focused on a single challenge or fix.
* Use a clear title, such as `Add web/jwt-confusion challenge` or `Fix pwn/ret2win Dockerfile`.
* Describe what the change does and how you tested it.
* Link related issues with `Closes #123`.
* Do not commit large binaries unnecessarily, and never commit build artifacts or secrets (check `.gitignore`).
* Be responsive to review feedback.

## Commit Messages

Write short, descriptive messages in the imperative mood:

```
Add crypto/weak-rsa challenge
Fix port mapping in web/sqli compose file
Update README with deployment steps
```

## Reporting Issues

When opening an issue, include:

* A clear description of the problem
* Steps to reproduce
* Expected and actual behavior
* Your environment (OS, Docker version) where relevant

## Spoilers

Keep solutions in `solution/` and do not post flags or solutions in public issues, pull request descriptions, or comments for an ongoing event.

## License

By contributing, you agree that your contributions will be licensed under the [Apache License 2.0](LICENSE), the same license as this project.

## Questions

Open a discussion or issue, or contact the maintainers at **[support@cybernexus.live](mailto:support@cybernexus.live)**.