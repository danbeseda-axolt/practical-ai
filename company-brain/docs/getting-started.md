# Getting started

**As of:** 2026-09-10

Standing up a company brain from this frame. Half a day to something useful, a
few weeks to something the team relies on.

## Day one

**1. Clone it and strip the examples.**

```bash
git clone https://github.com/danbeseda-axolt/practical-ai.git
cd practical-ai/company-brain
rm decisions/0001-separate-collection-from-analysis.md
```

Keep `templates/`, `docs/` and `customers/corpus-rules.md`. Delete the rest of
the example content as you replace it.

**2. Write `CLAUDE.md` first.** Replace the placeholder line with one sentence
naming the company and what it does. This file is what makes every agent behave
consistently, and it is the highest-leverage twenty minutes in the whole setup.

**3. Write three files, not thirty.** The instinct is to document everything.
Resist it. Start with the three things people ask about most often, which in
practice are usually:

- `company/org.md` — who does what
- `operations/[the process everyone asks about].md`
- `products/[what you sell].md`

A brain with three accurate files gets used. A brain with thirty stale ones gets
abandoned in a month.

**4. Run the scanner.**

```bash
node tools/brain-map/brain-map.js
```

It reports every knowledge file with its owner, its generator and its staleness,
and exits non-zero on anything past its date.

## Week one

**Add the first agent.** One that answers questions from the corpus, with
`Read, Grep, Glob` and nothing else. Resist giving it the web. See convention 6
in [conventions.md](conventions.md).

**Write the first decision.** Pick an argument the team has already had and
settled. The value shows up the first time somebody tries to reopen it.

**Put the scanner in CI.** Staleness that nothing enforces is staleness that
compounds.

## Week two and beyond

**Add an integration only when a person is doing the pull by hand.** Automating
a job nobody does is how these projects die. Follow the three-layer rule in
convention 3: the puller fetches, redacts and appends. It does not interpret.

**Watch for the second file on the same subject.** It is the first symptom of
the whole thing rotting. When you see one, merge it and say so in the file.

## What not to do

- **Do not migrate the wiki.** Importing five hundred stale pages produces a
  repository of five hundred stale pages. Start empty and let real questions
  pull content in.
- **Do not put anything secret in here.** Keys, passwords, customer personal
  data. See `CLAUDE.md` and `customers/corpus-rules.md`.
- **Do not build agents before there is anything to read.** An agent over an
  empty corpus is a chatbot with extra steps.
- **Do not skip the owner line.** An unowned file is nobody's job to refresh.
