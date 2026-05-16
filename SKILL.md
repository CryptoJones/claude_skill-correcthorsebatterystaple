---
name: correcthorsebatterystaple
description: Generate xkcd-936-style passphrases (four random common words from the EFF Long Wordlist). Use when Aaron asks for "a passphrase", "a memorable password", "a master password", "something I can speak over the phone", "xkcd-936 password", or "diceware passphrase". Wraps the `chbs` CLI from the `correcthorsebatterystaple` Python package.
---

# correcthorsebatterystaple — passphrase skill

When Aaron wants a passphrase he can actually remember, this skill shells out to the `chbs` CLI (`correcthorsebatterystaple` package). The CLI bundles the EFF Long Wordlist (7776 hand-curated words, ~12.92 bits of entropy per word) and uses `secrets.SystemRandom` for entropy — never the un-seeded `random` module.

## Trigger phrases (frontmatter description is the trigger; this is the operator-facing reminder)

Examples of asks that should fire this skill:

- "Generate a passphrase / strong password / master password"
- "Give me an xkcd-936 password"
- "I need something I can remember"
- "Something memorable for my password manager's master key"
- "A diceware passphrase"
- "A speakable / phone-friendly password"

Don't fire on:

- "Generate a random 32-char API token" → use `openssl rand -hex 32` instead; passphrases are for human-memorable secrets.
- "Generate a Bearer / OAuth / JWT" → those have specific formats and aren't passphrases.

## What the CLI does

```
chbs                              # 4 words, default separator "-"
chbs --num-words 6                # 6 words (~77.5 bits, modern recommendation)
chbs --separator ' '              # spaces between words
chbs --capitalize                 # Title-Case each word
chbs --count 5                    # generate 5 options
chbs --show-entropy               # print entropy estimate on stderr
chbs --wordlist /path/to/list     # use a custom wordlist
chbs --help                       # full reference
```

## How to invoke

Default response: one 6-word passphrase with the entropy estimate. 6 words is the modern recommendation (51.7 bits at 4 words feels comfortable but doesn't hold up against a serious offline attack much past 2030).

```bash
chbs --num-words 6 --show-entropy
```

If Aaron asks for more options to pick from, generate 5 at the same length:

```bash
chbs --num-words 6 --count 5 --show-entropy
```

If he asks for "the classic" or specifically "xkcd 936", use 4 words:

```bash
chbs --num-words 4 --show-entropy
```

If he asks for one he can dictate over the phone, use spaces and capitalize so the word boundaries are unambiguous:

```bash
chbs --num-words 6 --capitalize --separator ' '
```

## Install prerequisite

The skill assumes `chbs` is on PATH. To install if missing:

```bash
pip install correcthorsebatterystaple
# or, from source:
git clone https://github.com/CryptoJones/correcthorsebatterystaple ~/Source/repos/correcthorsebatterystaple
cd ~/Source/repos/correcthorsebatterystaple && pip install -e .
```

Check with `command -v chbs`. If missing, suggest the install command and stop — don't try to generate a passphrase by hand in Python (the inline `secrets.choice` shortcut bypasses the bundled wordlist + entropy reporting).

## Response shape

Render the output as a code block with the entropy estimate as a one-line caption afterwards. Example reply:

```
foundling-quasar-twitch-flogging-eldercare-unrigged
```

> Estimated entropy: 77.5 bits. Six words from the EFF Long Wordlist (7776
> entries, ~12.92 bits each). If you store this in a password manager, the
> manager's master password should be at least this strong.

If `--count > 1`, render each option on its own line in the same block.

## Why this skill exists (and the underlying project)

The `correcthorsebatterystaple` CLI lives at:

- **GitHub**: https://github.com/CryptoJones/correcthorsebatterystaple
- **Codeberg**: https://codeberg.org/CryptoJones/correcthorsebatterystaple

Stdlib-only, Apache 2.0, EFF wordlist bundled (CC BY 3.0 attribution in the source). The skill wraps the CLI rather than re-implementing the generation logic so the wordlist + entropy math stay in one place — fixing a bug in the CLI fixes both the command-line invocation and every Claude-skill invocation at once.
