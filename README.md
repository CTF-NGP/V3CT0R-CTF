# V3CT0R-CTF

Challenge source, infrastructure, and write-ups for the **V3CT0R Capture The Flag** competition, run by [Cybernexus CTFd Team](https://github.com/CTF-NGP).

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)

## About

V3CT0R-CTF is a Capture The Flag competition covering the classic security categories. This repository holds everything needed to understand, run, and extend the event: challenge files, deployment configs, and (after the event) solutions.

Each year's material lives in its own folder, starting with [`2026/`](2026/).

## Repository Structure

```
V3CT0R-CTF/
├── 2026/                 # Challenges and materials for the 2026 edition
├── .gitignore
├── LICENSE               # Apache-2.0
├── README.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
└── SECURITY.md
```

Inside each year folder, challenges are expected to follow this layout:

```
<year>/<category>/<challenge-name>/
├── README.md             # Description, hints, author, difficulty
├── dist/                 # Files handed to players
├── src/                  # Source and Dockerfile (hosted challenges)
└── solution/             # Writeup and solve script (kept private until the event ends)
```

## Challenge Categories

| Category | Description |
|----------|-------------|
| Web | Vulnerabilities in web applications and APIs |
| Pwn | Binary exploitation |
| Reverse | Reverse engineering of binaries and firmware |
| Crypto | Cryptographic weaknesses and implementations |
| Forensics | Disk, memory, network, and file analysis |
| OSINT | Open-source intelligence gathering |
| Misc | Everything else |

## Getting Started

### For Players

1. Read the rules and scoring information on the event platform.
2. Download challenge files from the platform (or from the relevant `dist/` folder after the event).
3. Capture flags, which use the format `V3CT0R{...}`, and submit them.

### For Organizers and Self-Hosters

Prerequisites: [Git](https://git-scm.com/) and [Docker](https://docs.docker.com/get-docker/) with Docker Compose.

```bash
git clone https://github.com/CTF-NGP/V3CT0R-CTF.git
cd V3CT0R-CTF/2026/<category>/<challenge-name>
docker compose up --build
```

Run hosted challenges in an isolated environment, such as a dedicated VM or network. Many challenges are intentionally vulnerable.

## Safety Notice

The challenges in this repository contain **deliberate vulnerabilities and may include malicious-looking behavior**. Never run them on a machine or network you care about, and never deploy them alongside production systems.

## Contributing

We welcome new challenges, fixes, and write-ups. Please read [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code of Conduct](CODE_OF_CONDUCT.md) first.

## Security

To report an unintended vulnerability in our infrastructure or tooling, see [SECURITY.md](SECURITY.md). Do not open a public issue for these.

## License

Released under the [Apache License 2.0](LICENSE).

## Acknowledgements

Thanks to all authors, testers, and participants who make V3CT0R-CTF possible.