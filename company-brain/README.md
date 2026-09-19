# Company Brain

**A company's knowledge in plain Markdown, under version control, with agents on
top of it.**

Most companies know a great deal that is written down nowhere. It lives in the
founder's head, in Slack threads nobody can find, and in the tab order of one
person's browser. The usual answer is a retrieval project over the existing
silos: expensive, slow, and it leaves the company dependent on whoever built it.

This is the cheap, durable alternative. Knowledge is Markdown files in a git
repository the company owns outright. Agents read those files and do work with
them. Integrations pull from the tools the company already pays for. It diffs,
it reviews, it has a history, and any competent person can pick it up.

*In Czech: **firemní mozek**.*

---

## What is in here

```
CLAUDE.md          how an agent is expected to behave in this repository
.claude/agents/    agents that own a domain and answer from the corpus
.claude/skills/    repeatable procedures, including the integrations
.claude/commands/  the routines a human triggers
company/           who we are: org, entities, brand voice
customers/         what customers say, and the rules for storing it
operations/        how the work actually gets done
products/          what we sell
decisions/         what was decided, when, and why
templates/         the starting shape for new files
inbox/             documented input streams
tools/brain-map/   a scanner that inventories the corpus and flags stale files
docs/              conventions, getting started, a worked example
```

## The five conventions that make it work

Most of the value is not in the agents. It is in these.

**1. Date anything that decays.** Every file that can go stale carries an
`As of YYYY-MM-DD` line and a named owner. An agent relying on a file older than
six months has to say so.

**2. Update the series, do not restate it.** New information appends to the
existing file. A second file on the same subject is how a knowledge base dies.

**3. Separate collection from analysis.** Pullers fetch, redact and append to a
raw corpus. Analyses read the corpus, never the API. A question nobody thought
to ask at pull time can still be asked a year later.

**4. Redact before writing, never after.** Nothing unredacted touches the disk,
not even in a scratch file. There is no cleanup pass. See
[`customers/corpus-rules.md`](customers/corpus-rules.md).

**5. Decisions are files, not messages.** Context, options, the decision, why,
what was traded away, and when to revisit. An agent may not contradict one
without flagging it.

## Getting started

```bash
git clone https://github.com/danbeseda-axolt/practical-ai.git
cd practical-ai/company-brain
node tools/brain-map/brain-map.js
```

The scanner walks every knowledge file and reports its owner, what generates it,
and how stale it is. It exits non-zero when something is past its date, so it
can run in CI and fail the build on rotting knowledge.

Then read [`docs/getting-started.md`](docs/getting-started.md) and delete the
example content.

## Where this came from

This is the generic frame extracted from a working system: a live company brain
running 12 agents, 18 skills and 13 commands across roughly 43 knowledge files,
with integrations into a helpdesk, Slack, three survey tools and a subscription
platform, plus PII redaction rules and the staleness scanner included here.

That repository is private, because it is full of a real company's data. This
one is the architecture with the data taken out.

Built by an operator, not an engineer, which is the point. The people who need a
company brain are the ones running the company.

## Licence

MIT. Take it, change it, use it commercially.
