# claude_skill-correcthorsebatterystaple

A [Claude Code](https://claude.com/claude-code) skill that generates
xkcd-936-style passphrases on request, by shelling out to the `chbs`
CLI from the [correcthorsebatterystaple](https://github.com/CryptoJones/correcthorsebatterystaple)
Python package.

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg?logo=apache)](LICENSE)
[![Codeberg](https://img.shields.io/badge/Codeberg-CryptoJones%2Fclaude__skill--correcthorsebatterystaple-2185D0?logo=codeberg&logoColor=white)](https://codeberg.org/CryptoJones/claude_skill-correcthorsebatterystaple)
[![GitHub](https://img.shields.io/badge/GitHub-CryptoJones%2Fclaude__skill--correcthorsebatterystaple-181717?logo=github&logoColor=white)](https://github.com/CryptoJones/claude_skill-correcthorsebatterystaple)
[![Upstream](https://img.shields.io/badge/Upstream-correcthorsebatterystaple-007ec6)](https://github.com/CryptoJones/correcthorsebatterystaple)

> Mirrored on both [GitHub](https://github.com/CryptoJones/claude_skill-correcthorsebatterystaple) and
> [Codeberg](https://codeberg.org/CryptoJones/claude_skill-correcthorsebatterystaple). Issues filed on
> either are welcome; commits are pushed to both.

---

## What it does

When the operator asks for a passphrase, a memorable password, a
master password, an xkcd-936 password, or a diceware passphrase, this
skill fires and runs:

```bash
chbs --num-words 6 --show-entropy
```

…then renders the output as a code block plus a one-line entropy caption.

It does **not** re-implement passphrase generation in Python; it wraps
the canonical CLI so the wordlist + entropy math stay in one place.

See [`SKILL.md`](SKILL.md) for the full skill definition, including the
trigger phrases, the variant invocations (count > 1, capitalize, custom
separator), and the response format.

## Install

Two pieces:

1. **The skill itself** (this repo):
   ```bash
   git clone https://github.com/CryptoJones/claude_skill-correcthorsebatterystaple \
     ~/.claude/skills/correcthorsebatterystaple
   ```
2. **The underlying CLI** (the upstream package):
   ```bash
   pip install correcthorsebatterystaple
   # or from source: pip install -e ~/Source/repos/correcthorsebatterystaple
   ```

Verify with `command -v chbs` — it should resolve to the installed entry
point. If it's missing, the skill will tell the operator to install it
rather than falling back to an in-Python hack.

Restart Claude Code (or open `/hooks` once) for the skill to be picked up.

## Why a separate repo?

Two-layer mirroring:

- **Upstream** ([correcthorsebatterystaple](https://github.com/CryptoJones/correcthorsebatterystaple))
  — the Python package: the CLI, the bundled EFF wordlist, the test
  suite, the published wheel on PyPI (eventually).
- **This repo** — only the SKILL.md + install glue. Independently
  installable into Claude Code without dragging in the entire upstream
  package source.

Updating the SKILL.md doesn't require re-cutting the Python release.
Updating the CLI doesn't require touching this repo.

## Customization

The `frontmatter.description` in `SKILL.md` is what Claude matches
against to decide when to fire. Fork + edit if your trigger phrases
differ ("xkcd password" vs "memorable password" etc.).

## License

Apache 2.0. The bundled wordlist in the upstream package is CC BY 3.0
(EFF — see upstream README for attribution).

Proudly Made in Nebraska. Go Big Red! 🌽 https://xkcd.com/2347/
