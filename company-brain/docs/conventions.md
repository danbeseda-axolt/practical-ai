# Conventions

**As of:** 2026-09-10

The architecture is five conventions. The agents are downstream of them. A team
that adopts only this file and none of the code still gets most of the benefit.

## 1. Date anything that decays

Every file that can go stale opens with:

```
**As of:** YYYY-MM-DD · **Owner:** [name]
```

Prices, headcount, supplier terms, market figures and anything with a number in
it decay. Conventions and decisions do not. An agent relying on a file older
than six months has to say so out loud.

The owner matters as much as the date. An unowned file is nobody's job to
refresh, and it will not be refreshed.

## 2. Update the series, do not restate it

New information appends to the file that already covers the subject. It does not
start a second file on the same subject.

This is the convention people break first and it is the one that kills knowledge
bases. Three files on pricing means nobody knows which is true, so everybody
asks a person instead, and the repo is now overhead rather than infrastructure.

If a file has genuinely been superseded, say so in the file and link forward.
Never leave two live answers to the same question.

## 3. Separate collection from analysis

Three layers, not two.

| Layer | Cadence | What it does |
|---|---|---|
| **Pull** | Scheduled | Fetch, redact, append to the raw corpus. Report what needs a human today. **No interpretation.** |
| **Corpus** | Standing | Raw, dated, deduped, identity-stripped. Generated, never hand-edited. |
| **Analysis** | On demand | Reads the corpus, never the API |

The expensive part is not the analysis, it is the collection. Throwing away the
raw material every morning means a question you did not think to ask at pull
time can never be asked. Keeping a dated corpus means old data answers new
questions.

It also means the analysis is reproducible. Anyone can re-run it over the same
input and get the same result, which is not true of anything that hits a live
API.

## 4. Redact before writing, never after

The full contract is in [`../customers/corpus-rules.md`](../customers/corpus-rules.md).
The short version: nothing unredacted touches the disk, and when a message
cannot be confidently classified, the message is dropped rather than guessed at.

## 5. Decisions are files

A decision file carries context, the options considered, the decision, why, what
was traded away, and when to revisit. Template in
[`../templates/decision.md`](../templates/decision.md).

Two reasons this earns its cost. An agent that can read the decisions log will
not quietly contradict a decision it does not know about. And "when to revisit"
turns a decision into something with an expiry rather than a permanent
constraint nobody remembers agreeing to.

Write the ones that were hard. A log of obvious decisions is noise.

## 6. Agents own a domain, not a tool

An agent is defined by the questions it answers, not by the API it can reach.
"The person who researches the market" is a domain. "The agent that can browse"
is a tool, and organising by tool produces agents that are vague about
everything.

Most agents here are corpus readers with `Read, Grep, Glob` and should stay that
way. Grant the web deliberately, per agent, and only where the domain needs it.

## 7. Generated files say so

Any file a script writes carries a line saying which script wrote it and when.
Hand-editing a generated file is how the next run silently destroys someone's
work.
