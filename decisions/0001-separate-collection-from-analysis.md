# Decision: separate collecting customer data from analysing it

- **Date:** 2026-09-10
- **Owner:** [name]
- **Status:** accepted

*A worked example. This is a real decision from the system this frame was
extracted from, with the company's specifics removed. It is here to show the
shape, and to be deleted once you have your own.*

## Context

A daily routine pulled four customer sources every morning and reported what
changed. It worked, but it was doing two different jobs at once, and they want
different things.

The first job is **voice**: how customers talk, the words they use, the words
they never use. It feeds anything that writes.

The second job is **insight**: who these people are, what they want, what stops
them buying. It feeds positioning, product and pricing.

Two problems followed from running them as one thing.

**Only one source kept its raw material.** Reviews went API to raw file to
analysis. The other three wrote an analysis straight from the API, so a question
nobody thought to ask at pull time could not be asked later. The data was gone,
and one of the sources could not be re-pulled at all.

**Every analysis was source-shaped.** There was a reviews view, a tickets view
and a survey view, and nothing that read across them. But those are three
different populations who do not agree. Reviews are written by people who
stayed. Surveys reach people who bought. Tickets are the only place the
frustrated and the undecided appear.

## Options considered

1. **Leave it.** One routine, one analysis per source, refreshed daily.
2. **Split the analysis in two, keep pulling as-is.** A voice pass and an
   insight pass, both re-pulling from the APIs each time.
3. **Split collection from analysis.** Pull writes a raw, anonymised corpus.
   Analyses read the corpus, not the APIs. *(chosen)*

## Decision

Three layers, not two.

| Layer | Cadence | What it does |
|---|---|---|
| **Pull** | Daily | Fetch, redact, append to the corpus. Report what needs a human today. **No interpretation.** |
| **Corpus** | Standing | Raw, dated, deduped, identity-stripped. Generated, never hand-edited. |
| **Analysis** | On demand | Two agents, both reading the corpus rather than the APIs |

The daily run stops writing standing notes entirely. It appends and reports.
Analyses are run deliberately, over accumulated data.

## Why

**Raw first, because the question changes.** The expensive part is the
collection, and it was being thrown away every morning. A dated, anonymised
corpus means a new question can be asked of old data without another pull.

**Two analysts, because the outputs have different readers.** Voice is consumed
by anything that writes. Insight is consumed by decisions about what to sell and
to whom. Merging them produces a document that serves neither.

**Daily analysis was always the wrong cadence.** Most days are quiet.
Segmentation over 24 hours of data is noise. Voice and insight both move slowly.
The exception queue is the only genuinely daily thing.

### What we traded away

**A daily read on themes.** New themes now surface on the analysis run rather
than the morning after they appear. Accepted, because the daily exception queue
still catches anything urgent, and a theme that is real on Tuesday is still real
on Friday.

**Simplicity.** Three moving parts where there was one. Justified only because
the corpus is reusable. If nobody re-interrogates it, this was overhead.

### The risk that matters

**Redaction at ingest.** Reviews were easy: public, and names stripped at the
source. Ticket bodies are not. They carry names, emails, order numbers,
addresses and occasionally health information, all of which `CLAUDE.md` forbids
in a repo the whole team can read.

Writing raw tickets to disk is the single most dangerous thing in this design
and it is hard to walk back once pushed. The contract is in
[`../customers/corpus-rules.md`](../customers/corpus-rules.md) and it is
deliberately strict.

## Revisit when

- Nobody has run an analysis over the corpus in two months. The raw layer is
  then costing storage and redaction risk for nothing.
- The corpus passes roughly 2,000 messages and one agent can no longer read it
  in a single context. Analyses then need chunking.
- A new source is added with a different personal-data shape.
