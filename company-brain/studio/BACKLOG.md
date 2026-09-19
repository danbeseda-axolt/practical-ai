# Backlog — the build queue

**As of:** 2026-09-18 · **Owner:** Dan Beseda

The chief takes the first `[ ]` item whose dependencies are done. One per
session. Dan's items are never reordered or deleted by an agent; discovered
work is appended at the end of its phase.

---

## Phase 0 — Foundation

- [ ] **0.1 Brand foundation** · `builder-brand`
  Name, one-paragraph story, positioning against three real competitors, voice
  rules, and `company/facts.md` as the canonical fact sheet.
  *Done when:* facts.md exists with name, founding, location, markets, channels,
  team (fictional, roles only), and the compliance line on health claims.
- [ ] **0.2 Catalogue and bill of materials** · `builder-brand` · after 0.1
  Four SKUs, one bundle, one starter kit. Price, COGS, pack size, supplier
  (fictional), supplier lead time, MOQ. Starter kit BOM for kitting.
  *Done when:* `products/catalogue.md` and `products/catalogue.json` agree, and
  the kit BOM names every component with its own SKU.
- [ ] **0.3 Simulation engine v1** · `builder-data` · after 0.2
  Seeded Node generator in `sim/`: 180 days of Shopify-shaped orders, 3PL
  shipments, stock movements and purchase orders, respecting lead times.
  Deterministic from a seed. `--check` validates internal consistency.
  *Done when:* same seed → byte-identical output; stock never goes negative
  without a recorded stockout; every order is either shipped, pending, or
  cancelled with a reason.
- [ ] **0.4 Port the brain-map scanner** · `builder-data`
  Generic version of the staleness scanner in `tools/brain-map/`.
  *Done when:* it runs on this repo, lists every knowledge file with owner and
  age, and exits 1 on a stale file.

## Phase 1 — Operations

- [ ] **1.1 Operations agent and runbook** · `builder-ops`
  `.claude/agents/operations.md` and `operations/daily-ops-runbook.md`: the
  checks, thresholds, report format. Read-only on every system.
- [ ] **1.2 Skill: pull-orders** · `builder-ops` · after 0.3
  Reads Shopify-shaped orders (from `sim/` in demo mode; documents the real
  Shopify connector call it stands in for).
- [ ] **1.3 Skill: check-fulfilment** · `builder-ops`
  Reconciles orders against 3PL shipments. Flags stuck, unshipped past SLA,
  address holds, mismatched line items.
- [ ] **1.4 Skill: stock-and-lead-times** · `builder-ops`
  Weeks of cover per SKU from sell-through, incoming POs, lead times. Projects
  forward 12 weeks. **Alerts on any week below safety stock**, and says when the
  reorder must be placed to avoid it.
- [ ] **1.5 Skill: kitting-work-order** · `builder-ops`
  Drafts a kitting work order for the starter kit from the BOM and component stock.
- [ ] **1.6 Support tickets in the simulation** · `builder-data`
  Synthetic tickets tied to real simulated orders (late delivery, damaged,
  subscription change, taste, questions that tempt a health claim).
- [ ] **1.7 Customer-service agent and reply skill** · `builder-ops`
  `support-playbook.md`, a `draft-replies` skill, the compliance boundary on
  health questions. Drafts only, never sends.
- [ ] **1.8 Operations reports** · `builder-ops`
  Generated into `reports/operations/`: daily ops, tickets, orders in/out,
  stock with lead-time alerts. Markdown plus JSON for the site.
- [ ] **1.9 /daily-ops command** · `builder-ops`

## Phase 2 — Site v1: the brain explorer

- [ ] **2.1 Site skeleton** · `builder-site`
  Static site in `site/`, generated from the repo at build time — never a
  hand-copied duplicate. GitHub Pages ready.
- [ ] **2.2 The brain map** · `builder-site`
  Departments as clickable areas → agents, skills, daily/weekly routines, reports.
- [ ] **2.3 How it works** · `builder-site`
  Diagram: Markdown in git → agents → connectors → the tools. Connector list
  with what each pulls and which department uses it (`company/connectors.md`,
  built from a production brain's connector list plus suggestions).
- [ ] **2.4 Browse the brain** · `builder-site`
  Rendered, navigable view of the knowledge files.
- [ ] **2.5 Reports pages** · `builder-site`
- [ ] **2.6 Build log page** · `builder-site`
- [ ] **2.7 Link to and from Dan's resume** · `builder-site`
  The resume lives in the same repo, at `/resume` (outside `company-brain/`).

## Phase 3 — Founder desk

- [ ] **3.1 Synthetic founder inbox** · `builder-data`
  Emails from suppliers, retailers, creators, investors, customers — plus what
  the founder actually sent back.
- [ ] **3.2 Founder-desk agent and three daily runs** · `builder-founder-desk`
  Morning, midday, end of day. Drafts, never sends.
- [ ] **3.3 Learning from edits** · `builder-founder-desk`
  Draft vs sent diff → `founder-desk/style-rules.md`, dated, with the example
  that taught each rule. Show later drafts improving.
- [ ] **3.4 Founder desk on the site** · `builder-site`

## Phase 4 — The brain's intake

- [ ] **4.1 Intake map** · `builder-knowledge`
  Every stream: Google Meet transcripts, Slack, email, surveys, reviews, brand
  manual. For each: puller, redaction, where it lands, who reads it.
- [ ] **4.2 Synthetic meeting transcripts and Slack** · `builder-data`
- [ ] **4.3 Pull skills: meetings and Slack** · `builder-knowledge`
  Collection → redaction → corpus, per `customers/corpus-rules.md`.
- [ ] **4.4 Brand manual as a skill** · `builder-brand`
- [ ] **4.5 Intake on the site** · `builder-site`

## Phase 5 — Marketing

- [ ] **5.1 Marketing structure** · `builder-marketing` · BLOCKED: Dan to send the Parker marketing map
- [ ] **5.2 Marketing lead agent and department agents** · `builder-marketing` · after 5.1
- [ ] **5.3 Marketing data in the simulation** · `builder-data`
  Ad spend by channel, new vs returning, subscriptions, cohorts, creators and
  their sources.
- [ ] **5.4 Daily competitor research** · `builder-marketing`
  Real competitors, public information only, one dated report a day.
- [ ] **5.5 Metrics overview** · `builder-marketing`
  CAC, LTV, LTV:CAC, MER, AOV, repeat rate, churn — computed from `sim/`.
- [ ] **5.6 Creator pipeline** · `builder-marketing`
  Stages, owner, source for every creator, conversion by source.
- [ ] **5.7 Marketing on the site** · `builder-site`

## Phase 6 — Marketing operations

- [ ] **6.1 Marketing-ops structure** · `builder-marketing` · after 5.1

## Phase 7 — The brand's own storefront

- [ ] **7.1 Visual identity** · `builder-brand`
- [ ] **7.2 Storefront website for the fictional brand** · `builder-site` + `builder-brand`

## Phase 8 — Ask the brain

- [ ] **8.1 Chat on the site** · `builder-site` · BLOCKED: Dan to decide hosting and who pays for the API calls
