# Password Cracking Using John the Ripper

A practical, permission-based guide to password recovery with John the Ripper (JtR) and its Johnny graphical front end.

## Documentation

- [Johnny GUI setup and first-use guide](docs/JOHNNY_SETUP.md) — what Johnny is, Linux installation, preparing an authorized PDF hash, running a limited wordlist test, and troubleshooting.
- [Walkthrough: cracking a locked PDF and reading the output](docs/WALKTHROUGH.md) — annotated screenshots of a completed lab run: Johnny reporting `1/1 cracked [format=PDF]`, the web cracker matching on guess 35 of 100, and the unlocked PDF with the captured flag.

## Screenshots

| # | Screenshot | What it shows |
| --- | --- | --- |
| 1 | [Johnny cracked a PDF hash](docs/images/01-johnny-pdf-cracked.png) | Johnny GUI: `100% (1/1: 1 cracked, 0 left) [format=PDF]` |
| 2 | [Web cracker match](docs/images/02-networkwalks-web-cracker-match.png) | Networkwalks cracker: `[+] MATCH` after 35 of 100 guesses |
| 3 | [Unlocked PDF](docs/images/03-pdf-unlocked-flag.png) | `My Locked PDF3.pdf` opened with the recovered password; flag captured |

See the [walkthrough](docs/WALKTHROUGH.md) for a full explanation of each output.

> Only test files and hashes that you own or are explicitly authorized to assess.
