# The raw corpus — rules for every source

**As of:** 2026-09-10 · **Owner:** [name]

The contract every puller obeys before it writes a customer's words to this
repo. Decided in
[`../decisions/0001-separate-collection-from-analysis.md`](../decisions/0001-separate-collection-from-analysis.md).

## Summary

One raw corpus per source, all generated and never hand-edited:

| Folder | Source | Puller | State |
|---|---|---|---|
| `reviews/` | review platform | `pull-reviews` | example |
| `tickets/` | helpdesk | `pull-helpdesk` | example |
| `survey/` | survey tool | `pull-survey` | example |

Analyses read these files. **Analyses do not call the APIs.** That is the whole
point: a question nobody thought to ask at pull time can still be asked later.

## The redaction contract

This repo is shared with the whole team and pushed to a remote. `CLAUDE.md`
forbids customer personal data. A raw corpus of ticket bodies is the most likely
place for that rule to break, so it is spelled out here.

**Redact before writing, never after.** Nothing unredacted touches the disk, not
even briefly, not even in a scratch file. There is no cleanup pass.

**Strip, always:**

- Names — the customer's, and any third party they mention
- Email addresses, phone numbers, social handles
- Street addresses, postcodes, anything that locates a person
- Order numbers, tracking numbers, subscription IDs, payment references
- Any detail specific enough to identify someone, especially anything about
  health, finances or employment

**Keep:**

- The ticket or response ID, so a line can be traced back to its source
- The date, the channel, the language
- The customer's own words, with the above removed
- Country or region, where it is coarse enough not to identify anyone

**When in doubt, drop the whole message.** Not the field, the message. If a
sentence cannot be redacted without destroying its meaning, it does not go in. A
smaller corpus is an acceptable price; a leaked address is not.

**Never re-add by hand.** If an analysis needs an identity to make sense, the
analysis is wrong. Quoting a name into a chat is fine. Committing it is not.

## Verify before every commit

```bash
node .claude/skills/corpus-check/check-redaction.js customers/*/all-*.md
```

Exits non-zero on email addresses, phone numbers, digit runs of six or more, and
anything else matching the strip list. Wire it into a pre-commit hook or CI so
the check is not optional.

## Why this file exists at all

Writing raw customer messages to disk is the single most dangerous thing in this
architecture, and it is hard to walk back once pushed. Everything else here can
be fixed with an edit. This cannot.
