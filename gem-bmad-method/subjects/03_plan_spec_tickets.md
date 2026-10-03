# BMad Method :: 03 - Fase Plan: PRD, UX, Arquitetura, Spec e Tickets

Fonte: gem-bmad-method (Repositório BMAD-METHOD)

---

## File: docs/plan/break-work-into-stories-and-track-it.md

`````markdown
---
title: 'Break Work into Stories and Track It'
description: Turn intent, a spec, or a PRD into ticket entries, build them directly, and track progress in joined plans.
sidebar:
  order: 7
---

Use `bmad-ticket` to split and track work. It accepts described intent, a spec, or a PRD. One small story or bug can go straight to [Build](../build/build-a-change.md) without ticketing.

## Plan the Work

For several epics, create an initiative and ask the skill to slice it. Each epic gets an envelope with requirements and done-when checks. Incept one epic to propose its stories and bugs in build order, with requirement coverage, dependencies, and verification. Review the breakdown before accepting it.

The initiative's `tickets.toml` lists epics. Each epic's `tickets.toml` lists entries with stable numeric ids. A planned entry needs no story file. Standalone tracked stories and bugs have files in `backlog/`; they do not need an invented epic.

See [Set Up the Ticket Tree](./set-up-the-ticket-tree.md) for store and tracker configuration.

## Build an Entry

Say “build story 1.2” to `bmad-build`. It reads the entry and epic, plus an existing refined leaf file, and writes acceptance criteria into its plan. Refinement before building is optional unless the work needs it.

The plan sits beside `tickets.toml` as `story-<slug>-plan.md`. Its numeric `ticket` joins the entry. A backlog plan uses its leaf file stem instead. The plan owns status and records the baseline before changes.

For unattended work, explicitly dispatch a ticket to `bmad-build-auto`, one invocation per ticket. It does not select the next ticket itself. Read [Autonomous Development Loops](../build/autonomous-development-loops.md) before wiring a runner.

## Track Progress

Ask `bmad-ticket` “what's next?” or “show status.” Plans carry build progress. A build finishes at `built`, shown in the review column; the user or orchestrator decides when to mark it `done`. A tracker card's status remains separate from build status.

Keep completed plans. Deleting one removes the state and evidence later builds, review, and retrospective read.

## Review and Close

`bmad-code-review` reads a ticket's plan and baseline and appends a dated `Code Review` block. It never changes ticket status. When the epic is finished, run [Retrospective](../build/finish-an-epic.md) with its folder, id, or slug. Retrospective writes its evidence and verdict directly in the epic folder; `bmad-ticket` handles confirmed closure.

## Correct Course

Run `bmad-correct-course` when a requirement, architecture choice, or dependency changes significantly. It requires a PRD and your description of the affected work and dependencies. For standalone spec work without a PRD, update the spec with `bmad-spec` instead. Correct-course assesses the available planning documents and writes its proposal as `change-<slug>/change-<slug>.md` in the active initiative's folder, or in the output folder when none is active, with the edits and a `bmad-ticket` handoff. It does not read or edit the ticket tree. Apply the approved changes through the owning skills, then use `bmad-ticket` to revise the remaining breakdown.
`````

---

## File: docs/plan/choose-a-planning-path.md

`````markdown
---
title: 'Choose a Planning Path'
description: Choose the smallest BMad path that safely fits a software change, from a trivial edit to a multi-epic project.
sidebar:
  order: 1
---

Use this page to decide how much planning a change needs. The answer turns on
one question: is the intent already well defined? If it is, feed it to
`bmad-spec`, which shapes it to the size of the work, and build. If it is not,
the other pages in this chapter are how you get a defined intent. If the work
belongs to an organization, with a PRD other people must approve and several
engineers building in parallel, read
[Plan Inside an Organization](./plan-inside-an-organization.md) first; it
says how this chapter fits the process you already have.

## Start from the Intent

A well-defined intent says what should be true when the work is done, what
must not change, and what is out of scope: complete enough that someone else
could build it without guessing, and no longer than that. Where it came from
does not matter: a sentence, an issue, a forged idea, a research report, a
PRD.

Keep the input short. `bmad-spec` reads everything you give it in one pass,
and the practical ceiling is a few tens of thousands of tokens, roughly a
40-page document. Hand it a pile of raw documents several times that size and
it silently loses the parts that mattered; condense them first. If the spec
says the input is too thin, you are not done on this chapter yet.

- **Well-defined intent**: run `bmad-spec` with it. A spec that fits one Build
  session goes straight to `bmad-build`; an epic-sized one goes to
  `bmad-ticket` for stories, then a Build per story. See
  [Define Requirements and a Specification](./define-requirements-and-a-specification.md).
- **Anything else**: the intent is not ready yet. Use the pages below until it
  is, then run `bmad-spec`. The spec skill writes the contract; it does not
  help you figure out what you want.

If the change fits one implementation session, you are on the Build page's
territory, not this chapter's: [Build a Change](../build/build-a-change.md)
covers sizing a session and whether a small change needs BMad at all.

:::note[Prerequisites]
Install BMad before using Build or another BMad workflow. You don't need BMad
for an obvious, low-risk edit.
:::

## Get to a Well-Defined Intent

These are independent tools, not stages. Pick the ones the gap calls for, in
any order. None of them build anything. Condense what they produce and hand
`bmad-spec` the result, not the raw pile.

| The intent is missing                                            | Do this                                                                                                      |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| A clear idea at all, or confidence the idea is good              | [Explore and Validate an Idea](./explore-and-validate-an-idea.md)                                            |
| Evidence a decision should rest on                               | [Research a Decision](./research-a-decision.md)                                                              |
| A written account of what the product is, for a PRD or a pitch   | A brief or PRFAQ: [Define Requirements and a Specification](./define-requirements-and-a-specification.md)    |
| Shared decisions several epics or agents must follow             | [Design UX and Architecture](./design-ux-and-architecture.md)                                                |
| Agreement, ownership, and sign-off among several people or teams | A PRD as the document the organization owns: [Plan Inside an Organization](./plan-inside-an-organization.md) |

A short list of decisions is often enough on its own. You need a PRD when more
than one person must agree on what the product is, or more than one epic must
not diverge; otherwise skip it. A multi-epic product runs `bmad-spec` once per
epic with those documents as sources.

## Planning Skills and What They Produce

Every skill in this chapter writes a document you can hand on. The table runs
from analysis through planning to solutioning; each chapter page is linked
from the first skill it covers and explains when its skills fit. In an
installed project, `bmad` recommends the next one. Each document lands in
its own `<type>-<slug>/` folder inside the active initiative's folder, or
directly in the output folder (`_bmad-output` by default) when no initiative
is active.

| Skill                           | Purpose                                                                                                                                        | Produces                                                                            |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `bmad-brainstorming`            | Generate ideas with a facilitated session ([Explore and Validate an Idea](./explore-and-validate-an-idea.md))                                  | `brainstorm.html` keepsake plus an optional `brainstorm-<topic>.md`                 |
| `bmad-forge-idea`               | Pressure-test an idea until it hardens, proves out, or dies cheaply                                                                            | `forge-report.html` every run; `forge-<slug>.md` when the idea hardens              |
| `bmad-deep-recon`               | Research a subject to support a decision ([Research a Decision](./research-a-decision.md))                                                     | Cited `research-<topic>.md` plus an optional HTML briefing                          |
| `bmad-product-brief`            | Capture the product vision when the concept is clear ([Define Requirements and a Specification](./define-requirements-and-a-specification.md)) | `brief-<slug>.md` + `addendum.md`                                                   |
| `bmad-prfaq`                    | Stress-test a product concept customer-first, working backwards from the press release                                                         | `prfaq-<slug>.md` + a distillate                                                    |
| `bmad-prd`                      | Create, update, or validate a PRD                                                                                                              | Create/update: `prd-<slug>.md`, `addendum.md`, `.memlog.md`; validate: HTML + `.md` report |
| `bmad-ux`                       | Record how the product looks and behaves ([Design UX and Architecture](./design-ux-and-architecture.md))                                       | `DESIGN.md`, `EXPERIENCE.md`, `ux-<slug>.md`, `.memlog.md`                          |
| `bmad-spec`                     | Condense any intent into a short contract; hand it to `bmad-ticket` for stories on request                                          | `spec-<slug>.md` + companions                                                       |
| `bmad-architecture`             | Make the technical decisions that keep separately built parts consistent                                                                       | `architecture-<slug>.md` by default                                                 |
| `bmad-ticket` | [Plan and track entries](./break-work-into-stories-and-track-it.md) | Epic envelopes, ordered `tickets.toml`, and optional leaf files |

`bmad-prd` has three intents, create, update, and validate; say which one you
want when you invoke it, or it will ask. `bmad-product-brief` feeds `bmad-prd`,
which reads the brief during discovery, but neither requires the other.

![Three columns of planning skills and the files each writes: analysis (brainstorming, forge idea, deep recon, product brief, PRFAQ), planning (PRD, UX, spec), and solutioning (architecture and ticket), all handing off to bmad-build, one session per unit](/diagrams/planning-skills.svg)

## Size Follows the Intent

The size of the intent decides how many Build sessions follow. One coherent
outcome that needs several sessions is an epic. Work that spans several epics,
or likely needs roughly 20 or more sessions, is a project. Scope is only one
signal: use more planning when the work has high risk, unclear requirements,
broad architectural reach, cross-system effects, or coordination between
people or teams.

![Four nested paths reuse the same unit: edit directly, run one Build, repeat Build across an epic, or repeat epic paths across a project](/diagrams/development-paths.svg)

Every path uses the same implementation unit. Larger work adds shared context
around that unit and repeats it; it does not switch to a separate delivery
system.

## Run the Path

### 1. Start Epic-Sized Work

Use this path when the work needs several Build sessions but still has one
coherent outcome.

**Define and divide the epic**

1. Run `bmad-spec` with the epic intent. See
   [Define Requirements and a Specification](./define-requirements-and-a-specification.md)
   for what a spec contains and when it is enough on its own.
2. Run `bmad-ticket` with the spec folder. It plans the epic with
   you and records the stories in build order in the epic's `tickets.toml`.
3. Review the proposed order and decide which stories need a checkpoint.
4. Build each story from its entry when you are ready; no story file is
   needed. Refine a story first only when its entry says `refine = true`, the
   ticket has no epic, or you want the acceptance criteria written before
   build.

The breakdown is an execution plan, not a promise that nothing will change.
Update the spec and re-slice the remaining stories when earlier work reveals a
missing constraint, a better division, or a conflict between stories.

**Establish the implementation pattern**

Implement important, risky, or foundational stories with `bmad-build`. Early
stories often settle the architecture, initial project structure, and repeated
patterns that later stories will follow. Give those decisions human attention
before automating repetitions of them.

Run Build once per story, naming the story. Build writes its plan beside the
epic's `tickets.toml` and leaves the plan at `built` until you mark it done
through `bmad-ticket`. To run stories unattended instead, give
`bmad-build-auto` the story as its intent, one run per story; see
[Autonomous Development Loops](../build/autonomous-development-loops.md).

**Finish the epic**

Verify the stories together, then run `bmad-retrospective` with the epic folder, id, or slug. It reads entries and joined plans and judges the combined result against the epic's requirements. Keep those plans after closure.
See [Finish an Epic](../build/finish-an-epic.md).

### 2. Start Project-Sized Work

Use the full BMad flow for a greenfield product, a multi-epic initiative, or
work likely to need roughly 20 or more implementation sessions.

Prepare only the planning the project actually needs from the table above
([Plan Inside an Organization](./plan-inside-an-organization.md) covers who
owns which document and where sign-off happens). Then run `bmad-spec` per epic,
track the stories with
[Break Work into Stories and Track It](./break-work-into-stories-and-track-it.md),
and close each epic with [Finish an Epic](../build/finish-an-epic.md).

These documents coordinate implementation. They do not replace Build. Each
epic still becomes a sequence of one-session units. Independent epic streams
can proceed in parallel when their boundaries are explicit. Each stream needs
an owner, and all streams stay accountable to the same product intent and
architecture. Run integration checks and a retrospective at each epic
boundary.

Dividing work can lose information: a requirement weakens, a constraint
disappears, or two correct stories fail when combined. The PRD, architecture,
and specs exist so later sessions can still see the whole.

## After Decisions Stabilize

`bmad-build-auto` runs one session without waiting for human input. It does
not choose the next story or own the backlog. Use it after the important
implementation decisions are stable. For the worker contract, see
[Autonomous Development Loops](../build/autonomous-development-loops.md).

## What You Get

A path sized to the work: a spec and stories for an epic, or shared product
documents plus one spec per epic for a project — each still implemented one
Build session at a time.
`````

---

## File: docs/plan/define-requirements-and-a-specification.md

`````markdown
---
title: 'Define Requirements and a Specification'
description: Choose between a succinct spec and the full product-planning path — product brief or PRFAQ, then a PRD — and know what each produces.
sidebar:
  order: 5
---

Use this page to pick the requirements skill for the work in front of you: a
product brief or a PRFAQ, then a PRD, for a product; or `bmad-spec` for the
contract implementation reads.

## Which Path Applies

[Choose a Planning Path](./choose-a-planning-path.md) decides the size. Most
work goes straight to `bmad-spec` from whatever defined the intent: a forged
idea, a PRFAQ summary, a brainstorm intent, an issue. A PRD becomes necessary
when several people must agree on what the product is or several epics must
stay aligned; [Plan Inside an Organization](./plan-inside-an-organization.md)
covers that setting. This page covers what each document is for and which
skill writes it. None of them replaces the spec: every epic still ends up as a
`spec-<slug>.md` that Build reads, and when there is a PRD, that is where the spec's
answers come from.

The brief, PRD, and UX skills steer the same way. Each opens with a brain
dump: tell it everything and point it at any files you have. Then you choose a
working mode. **Fast path** batches the remaining gaps into a question or two
and drafts the whole document with `[ASSUMPTION]` tags for you to correct.
**Coaching path** pulls the thinking out of you section by section and pushes
back where an answer is thin. Sessions can be paused and resumed.

## Product Brief: Capture Conviction

Run `bmad-product-brief` to create, update, or validate a brief: a one- to
two-page account of the product concept, right-sized to its purpose. A passion
project does not get investor-grade rigor; a pitch input does. The coach reads
the stakes early and calibrates how hard it pushes.

Use it when your concept is relatively clear and you want it written down
before a PRD. It produces `brief-<slug>.md` plus `addendum.md`, which holds the depth
that belongs later rather than in the brief: rejected alternatives, options
considered, technical constraints, sizing data. `bmad-prd` reads both. For
serious market sizing or competitor teardowns it hands you to
[Deep Recon](./research-a-decision.md).

## PRFAQ: Working Backwards

Run `bmad-prfaq` for Amazon's Working Backwards method as a challenge. You
write the press release announcing the finished product before anything is
built, then answer the hardest questions customers and stakeholders would
ask. Solution-first and technology-first openings get redirected to the
customer's problem. Vague answers get challenged. When you are stuck it offers
concrete reframings rather than repeating the question.

A run moves through five stages: customer, problem, stakes, and concept; the
press release; the customer FAQ; the internal FAQ on feasibility and
trade-offs; and a verdict on the concept's strength. Claims about the market
and competitors are checked against current research, not assumed. If after a
few exchanges you cannot name a customer or a problem, it sends you back to
brainstorming or Forge Idea instead of forcing it.

Use it when you want the concept stress-tested before committing resources.
If you cannot write a compelling press release, the product is not ready. It
produces the PRFAQ document plus a short summary a PRD or spec can read, and
it accepts `-H` for an unattended first draft when you supply the customer,
problem, stakes, and concept up front.

Brief and PRFAQ both feed a PRD, and the PRFAQ summary can feed `bmad-spec`
directly when no PRD is needed. Choose by how much challenge you want: the
brief is collaborative discovery, the PRFAQ is the harder path. Neither is
required; `bmad-prd` starts from a brain dump on its own.

## PRD: Agree on What and Why

Run `bmad-prd` to create, update, or validate a Product Requirements Document.

- **Create** runs discovery, then drafts. On the Coaching path you also pick
  an entry point: **Vision + Features** for capability-first products and
  internal tools, or **Journey-led** for consumer and multi-stakeholder
  products, where user journeys are told with a named protagonist.
- **Update** reconciles the PRD with a change signal, surfacing conflicts with
  earlier decisions before applying anything.
- **Validate** critiques without changing and produces a findings report.

The PRD describes capabilities, not implementation: features grouped, with
functional requirements under stable IDs and non-functional requirements in
their own section. Technical choices go to `addendum.md`. Length scales with
stakes, from about two pages for a hobby project to as long as the
requirements need for a launch.

It answers "what should we build and why." It does not say how; that is
[Design UX and Architecture](./design-ux-and-architecture.md). It does not
divide work into stories; that is
[Break Work into Stories and Track It](./break-work-into-stories-and-track-it.md).
If you open it with a one-pager in mind or an idea to vet, it points you at
the brief or the PRFAQ instead.

## Spec: The Contract Implementation Reads

Run `bmad-spec` to turn an intent into a short contract that Build reads.
`spec-<slug>.md` has five fields: Why, Capabilities (each with an intent and a
success condition), Constraints, Non-goals, and Success signal. Tables,
diagrams, glossaries, and documents other skills already wrote sit beside it;
the spec points at them rather than copying them.

The spec writes the contract; it does not help you figure out what you want.
Rich input is extracted with no questions. Sparse input gets a choice: a
best-effort draft where every gap becomes an open question, or a guided walk
through the five fields. Input too thin to use ("an app for hikers") is sent
to `bmad-prd`. Input too large is the other failure: a few tens of thousands
of tokens is the practical ceiling, so condense a pile of material first.

`bmad-spec` is the only writer of the spec. Do not hand-edit it; run the
skill again with the change and it updates the spec in place, keeping
capability IDs stable. The PRD, UX, and architecture skills can run in any
order and feed the same spec. After every run it reports assumptions it made
and open questions it could not answer, for you to resolve.

`bmad-spec` does not split a spec into stories. When a spec it writes reads as
several slices, it offers once to hand off to `bmad-ticket`. To
split a spec at any time, run `bmad-ticket` with the spec folder.
It plans one epic with you in build order and records the stories in the
epic's `tickets.toml`. Each story cites the spec's capability IDs. A constraint or design decision that comes up while slicing
goes back into the spec as an update. See
[Choose a Planning Path](./choose-a-planning-path.md#1-start-epic-sized-work)
for how the epic then runs.

`bmad-spec` delegates story breakdown to `bmad-ticket`. To run
the planned stories unattended, explicitly dispatch each ticket to
[`bmad-build-auto`](../build/autonomous-development-loops.md) as `ticket <ref>`,
one run per ticket. No leaf file needs to be pulled first.

:::note[What each skill produces]
`bmad-product-brief`: `brief-<slug>.md` and `addendum.md`. `bmad-prfaq`: a
PRFAQ document with a short summary for the PRD or spec. `bmad-prd`:
`prd-<slug>.md` and `addendum.md`, or a validation report. `bmad-spec`:
`spec-<slug>.md` plus supporting files. Each lands in its own `<type>-<slug>/`
folder in the active initiative's folder, or in the output folder when no
initiative is active. Exact paths and options belong to each
skill; see
[Planning Skills and What They Produce](./choose-a-planning-path.md#planning-skills-and-what-they-produce).
:::

## What Comes Next

With a PRD in hand for multi-epic work, decide whether the work needs shared
design decisions: [Design UX and Architecture](./design-ux-and-architecture.md).
With a spec in hand for one epic, run `bmad-ticket` with the spec
folder and go to
[Break Work into Stories and Track It](./break-work-into-stories-and-track-it.md).
`````

---

## File: docs/plan/design-ux-and-architecture.md

`````markdown
---
title: 'Design UX and Architecture'
description: When UX and architecture work is necessary, and how documented decisions stop agents and epics from implementing a system in conflicting ways.
sidebar:
  order: 6
---

Use this page to decide whether a change needs UX or architecture work before
implementation. Most changes do not. Multi-epic and cross-system work usually
does, because without shared decisions each agent or session makes its own.

## Do You Need It?

| Work characteristics                              | Guidance                                                      |
| ------------------------------------------------- | ------------------------------------------------------------- |
| Clear, local change with established patterns     | Usually unnecessary                                           |
| Several related components with known constraints | Optional, based on coordination risk                          |
| Multiple epics or cross-system decisions          | Needed to align implementation                                |
| Regulated, high-risk, or enterprise initiative    | Follow required governance; architecture is normally required |

If several epics could be implemented by different agents or people, you need
architecture. If the product has a user interface whose look and behavior
matter to the outcome, you need UX.

Both change what `bmad-build` reads; neither changes how Build runs. Build
still runs one session at a time. What changes is that each session reads the
same decisions, so the sessions fit together.

## The Problem Without Shared Decisions

When several agents implement different parts of a system with no shared
guidance, each makes independent technical choices. The results conflict in
predictable places: one epic exposes REST while another writes GraphQL;
`snake_case` columns meet `camelCase` ones; Redux in one area and React
Context in the next; different directory layouts and test patterns per epic;
session cookies here and JWT there. Each choice is defensible alone. Together
they produce integration issues discovered mid-sprint, rework, and
inconsistent patterns.

## The Architecture Spine

Run `bmad-architecture` and you get a short architecture document (the
**spine**). It records only the decisions that would conflict if two people
made them independently: the design approach, the boundaries, how state is
changed, who owns shared data. The stack, the folder tree, and the full data
shape are starting points; the code owns them once they exist.

One test decides what belongs. If two units built this independently, could
they choose incompatibly? A decision goes in the spine only when the answer
is yes, the call is non-obvious, and it is a real trade-off. Everything else
is left to the code. Each decision gets a stable ID so specs and stories can
cite it.

The skill works from whatever you have: a spec, a raw idea, a long
architecture document to shorten, or an existing codebase, where it reads the
real code and records the conventions already there. Coaching is the default:
the important calls are shown with the alternatives weighed, then you choose.
A Fast path drafts the whole spine with `[ASSUMPTION]` tags instead. For a
new project it recommends a current, well-known starter, because a good one
already decides a lot of the architecture.

Point it at the whole system or at one epic; an epic spine inherits the
parent's decisions and records only what the parent left open. When it
finishes, it offers to attach itself to the spec, which is how Build and the
readiness gate find it. Seed
[project context](../existing-codebases/set-and-maintain-project-context.md) from it so every later skill
reads the same rules.

:::caution[Common mistakes]
Deciding the API style "as we go", documenting every minor choice, and a spine
written once and never updated are the three ways this step fails. Document
decisions that cross epic boundaries, keep the spine current as you learn, and
run `bmad-correct-course` for a significant mid-implementation change.
:::

## UX Design

Run `bmad-ux` when user experience matters to the outcome. It produces two
peer documents: `DESIGN.md` for how the product looks (colors, typography,
spacing, components) and `EXPERIENCE.md` for how it works (information
architecture, behavior and states, accessibility, key user flows). Both win
over any mock or wireframe on conflict.

The facilitator records your vision; it never volunteers colors, patterns, or
directions. Three working modes: a Fast path that drafts both documents with
`[ASSUMPTION]` tags, a Coaching path that walks the decisions with creative
tools (color themes, design directions, wireframes, key-screen mocks), or a
design handoff that builds a prompt for an external design tool and folds its
output back in. UX can lead the PRD, follow it, or stand alone.

Skip it for back-end work, internal tooling with no real interface, and
changes to an existing UI that already has established patterns.

## What Comes Next

The spine and the UX documents become input to the spec and to
[Break Work into Stories and Track It](./break-work-into-stories-and-track-it.md),
where the readiness gate checks that stories do not depend on decisions
nothing records.
`````

---

## File: docs/plan/explore-and-validate-an-idea.md

`````markdown
---
title: 'Explore and Validate an Idea'
description: Decide which early idea skill to use — generate options with brainstorming, pressure-test a held idea with Forge Idea, or skip idea work and go straight to requirements.
sidebar:
  order: 3
---

Use this page to decide what to do with an idea before you commit to
requirements or code. Idea work is optional. Skipping it is fine when you
already know what you want; skipping it on a vague idea means every later
document inherits the vagueness.

## Pick a Starting Point

Start from your situation, not from a preferred skill.

| Situation                                                         | Use                                                                                                           |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| "I have a topic and want far more ideas on it than I'd get alone" | `bmad-brainstorming`                                                                                          |
| "I hold an idea and want it clarified, tested, or made better"    | `bmad-forge-idea`                                                                                             |
| "I need to understand the market, domain, or technology first"    | `bmad-deep-recon` — see [Research a Decision](./research-a-decision.md)                                       |
| "I already have conviction and want it written down"              | A product brief — see [Define Requirements and a Specification](./define-requirements-and-a-specification.md) |
| "I have a product concept and want it proven customer-first"      | A PRFAQ — see [Define Requirements and a Specification](./define-requirements-and-a-specification.md)         |
| "I want my agents to discuss or decide together"                  | `bmad-party-mode` — see [Run Multi-Agent Discussions](../customize/run-multi-agent-discussions.md)            |

None of these are stages. Run whichever fit, in any order, and condense what
comes out before the next step.

:::tip[Not Sure?]
Run `bmad` and describe your situation. It recommends a starting point
based on what you have already produced.
:::

## Generate Options with Brainstorming

Run `bmad-brainstorming` when you have a topic and want to push past the
obvious ideas on it. You choose the stance for the session:

- **Facilitator**: the coach never supplies ideas. It runs techniques and asks
  sharper questions so every idea is yours.
- **Creative Partner**: it facilitates and plays along, trading ideas with you.
- **Ideate for me**: it runs the whole session itself and shows you the result.

Tell it what you are brainstorming and why; the goal shapes which techniques
it offers. You pick a batch of techniques, or let it choose, and it runs each
until it stops producing, aiming well past a hundred ideas before it lets you
wrap. Say when you want to narrow and it switches to prioritizing and
deciding. Sessions can be paused and resumed.

You get an HTML record of the session, and a short `brainstorm-<topic>.md`
holding only the chosen discoveries, shaped to feed `bmad-spec`,
`bmad-product-brief`, or `bmad-prd`.

## Pressure-Test an Idea with Forge Idea

Run `bmad-forge-idea` with a half-formed idea and it questions the idea, one
question at a time, until you can act on it with conviction or drop it. It
works on a software feature, a business model, or a decision you keep
circling. Better thinking is the goal; a written file is optional. A
conversation is the cheapest place to find a hole in the idea, because
changing your mind there costs nothing.

It first pins down the idea, your goal for the session (clarify it, test
whether it holds up, or make it better), and whether it is new or a change to
an existing project. Clarifying pins down terms and assumptions; testing goes
after the central claim first; improving drives each unresolved branch to a
concrete decision.

It then works one question at a time and includes its own best answer when
that helps you respond. A concrete proposal is easier to accept, reject, or
revise than an open prompt. Fuzzy terms do not pass: when `user`, `buyer`, and
`payer` collapse into one word, it asks you to pick. For an idea inside an
existing project, the project's files are the source of truth.

It does not agree or praise unless that helps you think. Say **"attack this"**,
**"defend this"**, or **"switch roles"** to change how the current branch is
argued.

Each turn brings two voices: one from your installed agents or personas, and
one generated for the topic (a competitor, a buyer, a finance reviewer). Name
a persona to change who participates, or ask to go one-on-one. See
[Run Multi-Agent Discussions](../customize/run-multi-agent-discussions.md) and
[Agents](../reference/skills-and-agents.md#agents) for the roster.

:::tip[Let it kill the idea]
Finding out cheaply that an idea doesn't hold is the win. Don't steer the
session toward a yes.
:::

## How a Forge Session Ends

Every run writes a `forge-report.html`. The session ends in one of three ways,
and each is a valid result:

| Outcome      | What it means                                                                                                                                |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Hardened** | The idea is specific enough to use. A short `forge-<slug>.md` records the decisions, the rejected options, and the reasons later skills need. |
| **Killed**   | The idea does not hold up. The report records why.                                                                                           |
| **Clearer**  | You understand the idea better, but there is nothing to hand off. The session log is the record.                                             |

`forge-<slug>.md` should be short enough to feed `bmad-spec`, `bmad-prd`, or
`bmad-prfaq`. If it reads like a document, it is too long.

:::note[Example]
**You:** "I want AI-generated weekly summaries in the app. Users get a digest email every Monday."

**Forge:** "Before the feature, who asked for this? My recommended answer is nobody did, and you're reasoning from 'AI summaries sound valuable.' Push back if there's a real signal."

**Skeptical CFO (generated):** "And a Monday digest is a recurring inference cost across your whole active base, forever. What revenue line pays for it?"

**You:** "Retention, I guess. People forget to come back."

**Forge:** "Then the load-bearing claim is 'a Monday email brings lapsing users back,' not 'AI summaries are valuable.' Those are different ideas. Which one are we forging?"
:::

The first idea was a feature. Two questions in, the real idea is a retention
bet you could test with a plain email and no model at all.

## Improve a Draft

Point `bmad-advanced-elicitation` at a recent piece of output (a section, a
plan, a draft, a decision) and it offers a short menu of critique methods,
such as pre-mortem, first principles, or red team, runs the ones you pick, and
shows proposed changes for you to apply or reject. The brief, PRD, UX, and
spec skills offer it at their own pauses. See
[`bmad-advanced-elicitation`](../reference/skills-and-agents.md#bmad-advanced-elicitation).

## What Comes Next

Idea work leaves a brainstorm intent, a forged idea, a session log, or a
clearer decision. When the next question is "what is true out there," go to
[Research a Decision](./research-a-decision.md). When it is "what exactly are
we building," go to
[Define Requirements and a Specification](./define-requirements-and-a-specification.md).
`````

---

## File: docs/plan/plan-inside-an-organization.md

`````markdown
---
title: 'Plan Inside an Organization'
description: How the full planning path works when several people must agree on the product, several engineers build it in parallel, and someone has to sign off — who owns which document, where approval happens, and how change flows.
sidebar:
  order: 2
---

Use this page when the work belongs to an organization rather than to you:
more than one person must agree on what the product is, more than one engineer
will build it, or someone must sign off before money is spent. In that
setting, planning documents are contracts between people first and input to
the skills second.

## The Scaled-Down Version First

A single builder, or a small team that already agrees, does not need most of
what follows. A forged idea, a PRFAQ summary, a brainstorm intent, or a
well-written issue goes straight to `bmad-spec`, and the rest is one spec per
epic. [Choose a Planning Path](./choose-a-planning-path.md) covers that route.
`bmad-spec` will tell you if the input is too thin; until it does, no PRD is
required.

Reach for the full path when one of these is true:

- People who did not do the thinking must approve what the product is.
- Several epics, teams, or agents will build against the same decisions and
  must not diverge.
- A regulator, a steering committee, or an enterprise process requires named
  documents as evidence.

## The PRD Is What the Organization Owns

The PRD is the document the organization owns. It is written by `bmad-prd`,
validated by it, and updated through it; nothing else in the chain claims to
say what the product is. Brainstorming, Forge Idea, Deep Recon, a product
brief, and a PRFAQ exist to get the PRD written well. Everything after the PRD
is derived from it:

- `bmad-ux` writes `DESIGN.md` and `EXPERIENCE.md` in its own `ux-<slug>/` folder, as input beside the PRD.
- `bmad-architecture` writes a short architecture document (the spine): the
  decisions that keep independently built epics compatible.
- `bmad-spec` writes one spec per epic from the PRD, pointing at the spine and
  the UX documents rather than copying them.
- `bmad-ticket` turns the specs into ordered entries and tracks their joined plans.

Nothing downstream reinterprets the PRD. If a spec needs an answer the PRD
does not give, the answer goes into the PRD first; the spec is re-run after.

## Bring the Documents You Have

Nothing here asks you to replace the planning system you already run. An
organization arrives with a PRD in Confluence or Notion, a backlog in Jira or
Linear, and a review cadence, and all of it stays.

- **Your PRD is the input.** `bmad-prd` opens with a brain dump and reads any
  files you point it at, so the first run is "here is our PRD". Ask it to
  **validate** and you get a findings report on the document as it stands,
  with nothing changed. Ask it to **create** from that input and you get the
  same requirements in the shape the later skills read, with `[ASSUMPTION]`
  tags on anything it had to fill in. After that, the copy your reviewers
  already edit is the source. When it changes, re-run `bmad-prd` in
  **Update** mode pointing at it and let the skill bring the PRD in line;
  never edit the PRD by hand to catch up.
- **The same holds for design and architecture.** A design system, an
  existing architecture document, or a live codebase is what `bmad-ux` and
  `bmad-architecture` start from. On an existing system the architecture
  skill reads the code and records the conventions already there rather than
  proposing new ones.
- **Your tracker stays your tracker.** Jira remains where the organization
  plans, reports, and reviews. `bmad-ticket` publishes leaf files when needed; build status lives in local joined plans. Tracker status is mirrored separately and never makes a build skip work.
- **Your reviews stay your reviews.** The five sign-off moments below are
  where the skills produce something reviewable. Put your existing approvals
  at those points and the documents the skills write become the material
  those meetings already needed.

The one thing that does change is where an edit goes: in the PRD the skills
read, not in a spec or a story, so that every derived document can be
regenerated from it.

## Who Owns What

| Role                     | Runs                                                                                              | Owns                                                                      |
| ------------------------ | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Product manager          | Brainstorming, Forge Idea, Deep Recon, then `bmad-product-brief` or `bmad-prfaq`, then `bmad-prd` | The PRD and its update cycle; the one-pager the steering committee reads  |
| Designer                 | `bmad-ux`                                                                                         | `DESIGN.md`, `EXPERIENCE.md`                                              |
| Tech lead or architect   | `bmad-architecture`                                                                               | The architecture spine                                                    |
| One engineer, per epic   | `bmad-spec`, `bmad-ticket`, Build per story, `bmad-retrospective`                      | That epic: its spec, its `tickets.toml`, its verdict                      |
| Whoever tracks the whole | `bmad-ticket`                                                                            | The ticket tree and plan statuses                                   |

The rows are roles, not headcount. One person can hold several; what matters
is that each document has exactly one owner, because each has exactly one
skill that writes it. An epic is a handful of Build sessions, usually a day's
work for one person. The organization's coordination lives in the PRD and the
spine; the epic itself never needs a committee.

## Where Sign-Off Happens

The skills give you five moments where a human decision is expected, and each
one blocks something specific. Put approvals there rather than inventing new
gates.

| Moment                    | What is judged                                                        | What it blocks               |
| ------------------------- | --------------------------------------------------------------------- | ---------------------------- |
| PRFAQ verdict             | Whether the concept is strong enough to resource                      | Writing the PRD              |
| PRD validate              | A findings report on the PRD without changing it                      | Design and architecture work |
| Architecture spine review | The decisions every epic will follow, with alternatives weighed       | Writing specs for the epics  |
| Ticket-breakdown approval | The entries, requirement coverage, and validated dependencies         | Starting the agreed work     |
| Retrospective verdict     | Did the epic meet its own acceptance criteria                         | Starting the next epic       |

Every one of these produces a written result, so the approval has something to
attach to. In regulated or enterprise settings those documents are the audit
trail. PRFAQ and Retrospective can run unattended (`-H`) when you want the
same check without a conversation.

:::note[Reviewers read copies; change the source]
People will review the PRD, the spine, and a spec, and they will ask for
changes in whichever one they happen to be reading. Apply the change where it
belongs: to the PRD if it is about what the product is, to the spine if it is
about how epics stay compatible, to the spec only if it is about that epic
alone. Then re-run the later skills. Editing a spec to work around a PRD that
no longer says the right thing is how the documents stop agreeing.
:::

## Several Epics at Once

One PRD, one spine, one spec per epic. Several engineers can each take an
epic at the same time when the boundaries are explicit; the spine is what
makes that safe, because it records the calls two people would otherwise make
differently: the API style, how state is changed, who owns shared data. Each
epic's spine inherits the parent's decisions and records only what the parent
left open. Run integration checks and a retrospective at every epic boundary,
not only at the end. [Design UX and Architecture](./design-ux-and-architecture.md)
shows what goes wrong without the spine.

## When Requirements Change Mid-Flight

They will. The path for a change is the same as the path for the original:

1. Run `bmad-prd` in **Update** mode with the change signal. It surfaces
   conflicts with earlier decisions before applying anything.
2. If the change touches a cross-epic decision, update the spine.
3. Re-run `bmad-spec` for each affected epic. It updates the spec in place
   and keeps capability IDs stable, so stories that are unaffected stay
   unaffected.
4. For the affected epics, re-slice the stories `bmad-spec` names as no longer
   matching, then revise the remaining breakdown with `bmad-ticket`. Keep historical plans and completed states.

For a change large enough to threaten the plan itself, run
`bmad-correct-course` before touching documents.

## What You Get

A set of documents that agree with each other because each has one writer and
one owner; five named points where the organization can say yes or no with
something in hand; and a change path that flows from the PRD outward instead
of from whichever document someone happened to edit. Engineering still
implements one Build session at a time, exactly as it would for a
single-builder change.
`````

---

## File: docs/plan/research-a-decision.md

`````markdown
---
title: 'Research a Decision'
description: Decision-grade research with Deep Recon, three ways — draft a prompt for your own deep-research tool, process a finished report, or run the research in place.
sidebar:
  order: 4
---

Use `bmad-deep-recon` when a planning decision should rest on evidence rather
than assumption. This page explains its three modes and how to pick between
them.

## What Deep Recon Is

Run `bmad-deep-recon` when you have a decision: enter a market or skip it,
pick a stack, choose a vendor, commit to a domain. The decision shapes which
questions get asked, which sources count, and what the final report
recommends. It is not only for software. Any decision that should rest on
evidence is in scope.

Completed research, whether Deep Recon ran it or processed a report from
elsewhere, ends as a cited `research-<topic>.md` that a PRD or product brief can read
without reprocessing the original. Draft mode produces only a research prompt
for you to run in another tool; the report comes when you bring the result
back through Process. See
[Define Requirements and a Specification](./define-requirements-and-a-specification.md)
for where it goes next.

## Research Types

A type is a set of questions, source rules, and freshness windows that makes
the research sharper than an unaided prompt. Deep Recon infers the type from
your ask, or you name it.

| Type           | Reach for it when                                                            |
| -------------- | ---------------------------------------------------------------------------- |
| `market`       | Sizing an opportunity, segments, pricing, go-to-market                       |
| `domain`       | Learning an industry or field: structure, players, rules, vocabulary         |
| `technical`    | Evaluating a technology area, integration approaches, implementation reality |
| `competitive`  | Tearing down named competitors: offers, pricing, trajectory, sentiment       |
| `user-voice`   | What users actually experience and want: reviews, communities                |
| `academic-lit` | Literature review, state of the art, grounding an approach in papers         |

**Explore** (the default) builds understanding. **Select** runs a structured
choose-between when you are picking among candidates. You can add your own
types through [bmad-customize](../customize/customize-bmad.md).

## The Three Modes

| Mode        | What happens                                                                                    | You provide                                         |
| ----------- | ----------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| **Draft**   | Deep Recon writes a research prompt for the type; you run it in your own tool                   | One paste into ChatGPT, Gemini, Grok, or Perplexity |
| **Process** | A finished report is filed, its claims checked against the type, and a standard summary written | The report, from any source                         |
| **Run**     | Deep Recon does the research here: search, verification, cited synthesis                        | Approval at one plan gate                           |

**Draft** exists because most people already pay for a deep-research tool, and
those products crawl widely. The prompt carries the type's questions, recency
rules, and a citation demand, tuned to the tool you name.

**Process** closes the loop. Point it at any finished report (the one your tool
just produced, an analyst PDF, a colleague's document). It leaves the original
untouched, pulls out every claim that bears on your decision, flags what the
material never covered, and writes the same summary a native run would. Draft
then Process is the usual pairing.

**Run** stays in your session. There is no round-trip, the framing is
project-aware, and you control how much effort it spends: `quick`, `standard`
(the default), or `deep` sets how many assistants, sources, and rounds it
uses, and `normal`, `high`, or `max` verification sets how many claims it
cross-checks. Anything you say in the request overrides the preset.

## Which Mode to Use

| Situation                                                                  | Use                                             |
| -------------------------------------------------------------------------- | ----------------------------------------------- |
| You subscribe to a deep-research tool and don't mind one manual round-trip | Draft, then Process                             |
| You already have a report, whatever produced it                            | Process                                         |
| You want results now, in one sitting, no app switching                     | Run                                             |
| The research needs internal sources or tools only your session can reach   | Run                                             |
| Broad public sweep first, targeted follow-up after                         | Draft + Process, then a focused Run on the gaps |

Draft uses a subscription you already pay for and usually covers more public
sources. Run costs time and tokens in this session, but it can use every tool
your project has. When you ask for research with no verb, Deep Recon states
this trade once and remembers your preference for the session.

A Run asks you to approve a plan (the decision, the questions, the effort, and
a time estimate) and then proceeds. Claims in the report come from sources
retrieved during this engagement, not from the model's memory. Your project
files shape what gets asked, not what gets found. Every claim carries its
publisher, publication date, and an inline citation, and a figure past its
freshness window is reported as history, not fact.

## Keep Research Current

Each run gets a `research-<topic>/` folder in the active initiative's folder, or in the output folder when no initiative is active:
the original imports, the extracted notes, and `research-<topic>.md`. The report names which claims age
fastest. **Refresh** re-checks only those claims and records what changed.
**Deepen** drills into one area without re-running the rest.

## Starting It

| Goal                         | Type this                                                                                          |
| ---------------------------- | -------------------------------------------------------------------------------------------------- |
| Research something           | `/bmad-deep-recon` then describe the decision, or just "research the self-hosted analytics market" |
| Force a type                 | "competitive research on Linear and Height"                                                        |
| Draft a prompt for your tool | "draft a deep research prompt about X for Gemini"                                                  |
| Process a report             | "there's a research report at ~/Downloads/report.pdf, process it"                                  |
| Choose between options       | "help me choose between Postgres and MySQL for this"                                               |
| Refresh an existing report   | "refresh the market research"                                                                      |
| Customize defaults           | `/bmad-customize bmad-deep-recon`                                                                  |

The v6 `bmad-market-research`, `bmad-domain-research`, and
`bmad-technical-research` skills merged into Deep Recon as the `market`,
`domain`, and `technical` types; the old names still forward here.
`````

---

## File: docs/plan/set-up-the-ticket-tree.md

`````markdown
---
title: 'Set Up the Ticket Tree'
description: Set up an initiative store, choose where tickets are tracked, and plan and track work with bmad-ticket.
sidebar:
  order: 8
---

`bmad-ticket` is how BMad plans and tracks work: it breaks work into epics and stories in the shared ticket tree. Build, Build Auto, code review, and retrospective consume that tree.

## Install the Skills

Install with the skills CLI. Run this in your project:

```bash
npx skills add bmad-code-org/BMAD-METHOD --skill bmad --skill bmod-core-tools --skill bmod-method --skill bmad-ticket
```

Add `--skill bmad-build` and any other skill you want in the same command. Then open your AI tool in the project, ask the `bmad` skill to run `bmad setup`, and check that the tool lists `bmad-ticket`. Update later by asking `bmad` to run `bmad setup` again.

:::note[Prerequisites]
You need Node.js with npm, Git, and [uv](https://docs.astral.sh/uv/). BMad setup and the ticketing scripts run through `uv`.
:::

## Create an Initiative Store

The initiative store is the folder where planning lives: one folder per initiative, plus a `backlog/` folder for standalone tickets. An initiative is one body of work, such as a product, a major feature, or a migration. Its planning documents and its tickets sit together in its folder.

### 1. Choose where the store lives

The store is your BMad output folder, `_bmad-output` by default. You can configure it to be any folder; the example below uses `_bmad-initiative-store` instead, and step 2 shows the setting. In a single repo, the default inside the project works fine.

When the work spans several repos, install BMad in the workspace folder that holds them and put the store there too. Start your AI tool from that workspace folder, so one session can reach the plan and every repo it touches. Give the store its own `git init`, which keeps planning history apart from each repo's code history.

```
shop-workspace/                          # start your AI tool here; not a repo itself
├── _bmad/                               # BMad install and configuration
├── _bmad-initiative-store/              # the store — its own git repo
│   ├── initiative-checkout/
│   │   ├── initiative-checkout.md
│   │   ├── tickets.toml               # the epics in build order
│   │   ├── prd-checkout/
│   │   │   └── prd-checkout.md
│   │   └── epic-cart-rules/
│   │       ├── epic-cart-rules.md
│   │       ├── tickets.toml           # every planned story, in build order
│   │       ├── story-cart-service-scaffold-plan.md  # the build's plan, with the story's status
│   │       └── story-cart-ui-shell.md               # a story's file, only when refined or published
│   ├── initiative-loyalty-program/
│   └── backlog/
│       └── bug-checkout-total-ignores-discount-codes.md
├── shop-api/                            # code repo
├── shop-web/                            # code repo
└── shop-mobile/                         # code repo
```

### 2. Point BMad at it

Skip this step when you keep the default. Otherwise set `output_folder` in `_bmad/custom/config.toml`, which is committed and applies to the whole team:

```toml
[core]
output_folder = "{project-root}/_bmad-initiative-store"
```

`{project-root}` is the folder that holds `_bmad/`. In the layout above, that is `shop-workspace/`.

### 3. Name the active initiative

Set the initiative you are working on in `_bmad/custom/config.user.toml`, which is personal and not committed:

```toml
[core]
active_initiative = "initiative-checkout"
```

The value is the initiative's folder name in the store. When it is unset, `bmad-ticket` offers to create the folder and record the setting for you. You can also ask the `bmad` skill to show, switch, create, or clear the active initiative at any time.

:::tip[One workspace, many projects]
If one workspace holds unrelated projects, tell your coding agent to follow the active initiative. Put a short rule in `AGENTS.md`, or whatever instruction file your tool reads, that names the setting and says which folders belong to which initiative:

```md
`active_initiative` in `_bmad/custom/config.user.toml` says what we are working on.

## If the active initiative contains `checkout`

Read `docs/shop.md` for how the shop repos fit together and how to run them.

## Otherwise

Ignore the `shop-*` repos and do not read `docs/shop.md` unless I ask.
```

The agent then stays out of repos that have nothing to do with the current work, and you switch its focus by changing one setting.
:::

## Bring Existing Planning Documents

The planning skills write into the active initiative's folder, each document as `<type>-<slug>/<type>-<slug>.md`. If you have a v6 project, ask the `bmad` skill to run `bmad migrate method`: it plans the move for the planning documents, `epics.md`, `sprint-status.yaml`, and the stories, shows you the plan, and applies it. To move only a few planning documents by hand, copy them in the same way:

```
_bmad-output/planning-artifacts/brief.md         → initiative-checkout/brief-checkout/brief-checkout.md
_bmad-output/planning-artifacts/prd.md           → initiative-checkout/prd-checkout/prd-checkout.md
_bmad-output/planning-artifacts/DESIGN.md        → initiative-checkout/ux-checkout/DESIGN.md
_bmad-output/planning-artifacts/EXPERIENCE.md    → initiative-checkout/ux-checkout/EXPERIENCE.md
_bmad-output/planning-artifacts/architecture.md  → initiative-checkout/architecture-checkout/architecture-checkout.md
```

UX is the exception to the naming: `bmad-ux` writes two peer documents, `DESIGN.md` and `EXPERIENCE.md`, which keep their names inside the `ux-<slug>` folder beside a short `ux-<slug>.md` that names them. Your source paths will differ.

## Configure Where Tickets Are Tracked

The first time you use `bmad-ticket`, it asks where tickets are tracked and writes your choice to `_bmad/custom/ticketing-store-config.toml`. That file is yours to edit, and edits survive skill updates.

| Choice        | What it means                                                                        |
| ------------- | ------------------------------------------------------------------------------------ |
| Repo          | The default. Tickets are markdown files in the store. No account needed.             |
| GitHub Issues | Tickets publish as issues, with sub-issues and blocked-by relations.                 |
| Jira          | Tickets publish as Jira issues.                                                      |
| Linear        | Tickets publish as Linear issues.                                                    |
| Notion        | Tickets publish as rows in a Notion database.                                        |
| Trello        | Tickets publish as cards.                                                            |

With a tracker, the markdown files remain the working copy and the tracker is where the team sees them. Setup offers to connect the tool, create the labels or fields it needs, and prove the connection with a test ticket. Say "reconfigure the ticket store" to change it later.

:::note[Repo is the default, and the most tested]
Repo is the default and the choice that has been tested most. The tracker options still need a lot of testing. If you use one of those trackers, trying it and reporting what happens is some of the most useful help you can give, and all feedback is welcome.

Hooks are not integrated yet, so nothing syncs on its own: a tracker and the ticket files are brought in line only when you run the skill. Hooks may be added later.
:::

## Use `bmad-ticket`

The skill turns intent into tickets a coding agent can build from, at three levels. An initiative holds epics. An epic holds stories, spikes, and bugs. An initiative or an epic is itself the specification at its level: it holds the requirements, and its children are cut from them. When the requirements outgrow the ticket, `bmad-spec` writes a spec folder inside that initiative or epic folder.

It takes almost any input. The best input is a `bmad-spec` output together with the documents that produced it. Give it the spec folder and it plans one epic whose stories cite the spec's `CAP-N` ids. A PRD alone, meeting notes, or a one-paragraph idea also work.

| Say                                      | What happens                                                                                |
| ---------------------------------------- | ------------------------------------------------------------------------------------------- |
| "Split this initiative into epics"       | Proposes epic boundaries from your source and records the agreed order in `tickets.toml`.   |
| "Incept the first epic"                  | Plans the whole epic with you into an ordered breakdown of stories.                         |
| "What's next?"                           | Lists what is ready to refine or start, in progress, and blocked.                           |
| "Review the stories"                     | Writes each story's file from its entry if needed, then reviews and improves it with you: description, check, references, order, and prerequisites. |
| "File a bug: checkout ignores discounts" | Writes one ticket straight into `backlog/`, with no epic needed.                            |

Each initiative and epic keeps its breakdown in a `tickets.toml` file beside its ticket file. The initiative's file lists the epics in build order. An epic's file lists every planned story and bug as an entry, in build order, each with an `id` that names it under the epic: what it delivers, how it will be verified, what it waits on (`after`), and what is still uncertain. When something must be settled before implementation, the skill asks you to answer it or records it as the entry's `unknown`. It adds a spike when you ask for one. By default the last entry is a "Refactor sweep" story for cleanup found during the epic.

An entry needs no file to be built. `bmad-build` plans the story's acceptance criteria when it builds, from the epic and the entry, so detail is not written months before it is used. It writes that plan beside `tickets.toml`, as `story-<slug>-plan.md`, and the plan is never sent to a tracker. A story gets its own file only when you refine it or publish it to a tracker. From then on the file is truth: refining edits the file, and the entry keeps only the story's `id`, `type`, `title`, prerequisites (`after`, edited in both places), and `hitl`.

A story's `after` can name a story in another epic, or a whole epic. Ask "what's next?" about the initiative to see every epic at once.

### How epics are cut

An epic is one capability that one owner delivers to production. A module, service, or bounded context can be an epic when it is also the ownership or deployment boundary. A unit the work only consumes or configures gets no epic. It is a touch point, named in the initiative's Boundaries with the epic that owns the work there. If your team cuts epics by its own rule, tell the skill and it offers to save the rule to `_bmad/custom/bmad-ticket.toml`.

After the epics are agreed, the skill lists the decisions that more than one epic must adopt, such as a contract, a data format, or a shared value list. It offers `bmad-architecture` to settle them in the architecture spine. If you decline, each becomes a story in the opening epic that the other epics wait on. Work in one repo or one unit needs no architecture pass. It is needed from the second unit that must adopt a decision.

When your source contradicts the code, the skill records a `Source conflict:` line in the Notes of the initiative or epic and tells you. When the source is a BMad spec, it offers to pass the correction to `bmad-spec`.

## Hand a Story to Build

Name the story to `bmad-build`, for example "build story 1.2" for the second story of the first epic. There is no file to write first. Build reads the story's entry and its epic, plus the story file when you refined one. It plans the story's acceptance criteria from the epic's Requirements and Done when, the entry's description, and its `Verify:` check.

:::note[Refining is optional]
A story needs no refining before `bmad-build`. Build refines it as part of the build: it questions you and writes the acceptance criteria itself. If you will build unattended, with `bmad-build-auto`, a loop, or a factory, nobody answers questions during the build, so review the sequence and each story with `bmad-ticket` first. `bmad-ticket` writes full acceptance criteria only for a bug, a ticket with no epic, or when you ask.
:::

The story's `status` lives in the build's plan. Build moves it as it works and stops at `built`; only you, or an orchestrator, mark a story done. When you have checked the work, say "mark story 1.2 done" to `bmad-ticket`. On the repo store that is an edit to the plan that you commit with your work. With a tracker, say "start story 1.2" before you build, so the ticket publishes if it has not and its card moves to in progress. The tracker's status is read into the story's file as `tracker_status`, so moving a card on the board never makes build skip planning.

## Tell Us What You Find

Feedback helps improve `bmad-ticket` and its tracker integrations. The most useful reports say what you gave the skill, what you asked for, what it produced, and what you expected instead. Open a [GitHub issue](https://github.com/bmad-code-org/BMAD-METHOD/issues) with "bmad-ticket" in the title, or post in [Discord](https://discord.gg/gk8jAdXWmj).
`````

---

## File: skills/bmad-architecture/assets/spine-template.md

`````markdown
---
name: '{name}'
type: architecture-spine
purpose: build-substrate    # build-substrate (default) · discussion · report · deck
altitude: feature           # initiative (keeps features) · feature (keeps epics) · epic (keeps stories)
paradigm: '{named design pattern, e.g. hexagonal, layered, pipes-and-filters, actor}'
scope: '{what this spine governs}'
status: draft               # draft · final
created: '{date}'
updated: '{date}'
binds: []                   # capability / unit IDs governed (from the driving spec; at epic altitude, also the inherited parent AD ids)
sources: []
companions: []
---

# Architecture Spine — {name}

<!-- TEMPLATE GUIDE — act on these comments, then delete them; never emit a comment in the finished spine. This is a shape, not a script: keep only the sections this spine needs and cut the rest (no empty headers). A small intent may be just paradigm + a few ADs + conventions; a platform earns more. An inherited epic spine is usually mostly Inherited Invariants + a thin Deferred. Decisions, not rationale (rationale lives in the memlog). Carry shape in diagrams; prose only where it must. -->

## Design Paradigm

<!-- Name the pattern (a known one loads a whole model for free) and map its layers to namespaces/directories. The smallest, most durable thing here. -->

## Inherited Invariants

<!-- Only when this spine inherits a higher-altitude parent. The parent's ADs/conventions/paradigm that bind here, by their ORIGINAL ids — read-only, never renumbered, not re-derived. A local decision that contradicts one is a conflict to surface, not an override. Cut this section otherwise. -->

| Inherited | From parent | Binds here |
| --- | --- | --- |
| {AD-id / convention} | {parent spine} | {what it constrains in this scope} |

## Invariants & Rules

<!-- The durable heart: calls a future builder can't read off compliant code. One block per decision: stable ascending id (never reused/renumbered), Binds, Prevents (the divergence), Rule (enforceable). Tag [ADOPTED] when the user or existing reality settled it. Include a dependency-direction diagram (who may depend on whom) — it IS a rule; author it as valid mermaid, never an empty graph. -->

### AD-1 — {decision}

- **Binds:** {capability / unit ids / fr/nfr's, areas, or `all`}
- **Prevents:** {the divergence this stops}
- **Rule:** {the constraint downstream must follow}

## Consistency Conventions

<!-- Defaults that bind where independent builders would drift. Cut rows that don't apply; add rows the project needs. -->

| Concern | Convention |
| --- | --- |
| Naming (entities, files, interfaces, events) | |
| Data & formats (ids, dates, error shapes, envelopes) | |
| State & cross-cutting (mutation, errors, logging, config, auth) | |

## Stack

<!-- SEED — verified current at authoring; the code owns this once it exists. Name + version only; the why lives in the memlog. One row per language, framework, key dependency, platform, or chain that's pinned. -->

| Name | Version |
| --- | --- |
| {language / framework / key dep / platform / chain} | {pinned version} |

## Structural Seed

<!-- The shapes worth fixing at cold-start — not a fixed list. Include only what's non-obvious at this altitude, and use as many diagrams as convey it, each as VALID mermaid (never a placeholder or empty graph). Candidates: system/container/context view; DEPLOYMENT & ENVIRONMENTS and external provider/infra topology (cover the operational envelope here when this altitude owns it — don't let it fall through); core-entity ERD (names + relationships only; an attribute that's itself an invariant is an AD, not a diagram); a minimal source tree. The code owns the detail — this is scaffold, not a mirror to maintain. -->

```text
{root}/
  {dir}/   # {what lives here}
```

## Capability → Architecture Map

<!-- Present when a spec drove this run. Bridges the spec's capabilities to where they live + what governs them; the consistency auditor's checklist. Cut otherwise. -->

| Capability / Area | Lives in | Governed by |
| --- | --- | --- |
| {CAP-id / area} | {component / module} | {AD-id, convention, paradigm} |

## Deferred

<!-- Decisions intentionally pushed down, each with the reason it can wait — including whole dimensions this altitude doesn't own yet. The half of the contract that keeps the spine lean. -->
`````

---

## File: skills/bmad-architecture/references/headless.md

`````markdown
# Headless

No interactive user: infer everything, ask nothing, but never invent — record inferences as `assumptions[]` and gaps that need a human as `open_questions[]`. Detect headless from a `headless: true` flag, a non-interactive / no-TTY invocation, an activation hook that declares it, or a first message that pre-supplies all inputs and asks for an artifact path back; when ambiguous, default to interactive.

Drive the run from the payload in the first message — `intent`, `altitude`, `purpose`, the driving input (spec package / PRD / raw intent / brownfield path), a parent spine path at lower altitude, and `doc_workspace` if a specific folder is required. Infer anything absent from the inputs or workspace; don't invent stack, constraints, or scope to fill a gap. You still verify named tech on the web (you can't ask, but you can check) and still drive every write through the shared `{project-root}/_bmad/scripts/memlog.py`. Run the full Reviewer Gate (`references/reviewer-gate.md`) non-interactively: `scripts/lint_spine.py` plus **every `{workflow.finalize_reviewers}` lens as a parallel subagent** (and any ad-hoc lens the spine's criticality warrants). Headless skips only the human picking from the menu — never the reviewers themselves; apply the clear fixes and record anything unresolved in `open_questions[]`. For a true authority collision, list it in `conflicts_with_prior_decisions[]`. For the Validate intent, always write the report to `{doc_workspace}` and add `"offer_to_update": true`. If intent stays ambiguous after inference, halt blocked.

End with JSON only, omitting keys for artifacts not produced — the shape below is the fully-produced (`complete`) case; a `blocked` run produces no spine, so it omits `spine`, `memlog`, and `companions` entirely (see the note under the block):

```json
{
  "status": "complete | partial | blocked",
  "intent": "create | update | validate",
  "altitude": "initiative | feature | epic",
  "purpose": "build-substrate | discussion",
  "doc_workspace": "<resolved run folder>",
  "spine": "{doc_workspace}/<folder name>.md",
  "memlog": "{doc_workspace}/.memlog.md",
  "companions": [],
  "assumptions": [],
  "open_questions": [],
  "conflicts_with_prior_decisions": [],
  "reason": "<one line, only when blocked>"
}
```

`complete` stands alone · `partial` (spine produced, but `open_questions[]` non-empty or critical inputs inferred) means review before downstream use · `blocked` means no spine produced — return only `status`, `intent`, `reason`, and `doc_workspace` (if bound), omitting `spine`, `memlog`, `companions`, and the artifact arrays that don't exist.
`````

---

## File: skills/bmad-architecture/references/reviewer-gate.md

`````markdown
# Reviewer Gate

The spine's pre-handoff review. Runs at Finalize (after distill + reconcile) and *is* the Validate intent. The difference is the ending: at Finalize you apply the clear fixes yourself; under Validate you report and don't change the spine.

Cheap deterministic pass first: `uv run {skill-root}/scripts/lint_spine.py --workspace {doc_workspace}` settles the mechanical misses (placeholders, duplicate `AD` IDs, missing Binds/Prevents/Rule, unpinned Stack versions), so reviewers spend judgment on the semantic half.

Assemble the menu: a **rubric walker** that judges the spine against the good-spine checklist below, **+ every entry in `{workflow.finalize_reviewers}`**, + ad-hoc lenses you invent or offer as the spine's rigor, altitude, and criticality warrant — a security/compliance lens for regulated stakes, a seam reviewer cross-team, a data-integrity lens for a heavy data model. Scale *whether and how heavily the gate runs* to the stakes: a throwaway prototype may run it quietly or skip the gate entirely; a high-criticality or platform-altitude spine earns more lenses and the explicit all / subset / skip menu. But once the gate runs, the `{workflow.finalize_reviewers}` always run — they are the configured floor, never cherry-picked out; only the ad-hoc lenses are optional. (Headless never skips the gate.)

Dispatch every entry as a **parallel subagent against the spine** (prefix convention: `skill:` / `file:` / plain text). Each writes its full review to `{doc_workspace}/reviews/review-{lens}.md` — a subfolder, so the gate's scratch stays out of the deliverable folder — and returns ONLY a compact summary (verdict, top 2–5 findings, file path) — the parent never holds full review text. An inline self-check does not count: the independent context is the point, because a fresh reviewer finds the divergences the author talks past. If subagents are unavailable, run sequentially — write the file first, then flush it from context.

**Good-spine checklist** (what the rubric walker judges): it fixes the real divergence points for the level below and misses none; every `AD`'s Rule is enforceable and actually prevents its stated divergence; nothing under Deferred could let two units diverge; named tech is verified-current; it ratifies rather than contradicts a brownfield codebase; if a spec drove it, it covers that spec's capabilities; if a parent spine is inherited, no new `AD` weakens or contradicts an inherited one; and every dimension the altitude owns is decided, deferred, or an open question — a whole dimension left silent is a finding, especially the operational/environmental envelope (deployment & environments, infra/provider strategy, operations) a domain-focused draft skips.

Surface findings tiered, never dumped: a one-sentence gate verdict, then critical + high; medium/low roll into a tail ("plus N more in {file}"). Per finding: autofix, discuss, defer to Deferred / open items, or ignore. **At Finalize this is your own gate — apply the clear fixes rather than handing over a list; surface only what genuinely needs the user.** Under the **Validate intent**, fold every reviewer's output into one bespoke HTML + markdown report and open the HTML.
`````

---

## File: skills/bmad-architecture/scripts/lint_spine.py

`````python
#!/usr/bin/env python3
# /// script
# requires-python = ">=3.11"
# ///
"""lint-spine — the mechanical half of spine decision-integrity, done deterministically.

LLMs miscount IDs and miss literal placeholders; a grep does not. This linter owns the
checks a script does better than a prompt, and leaves the semantic half (is each Rule
actually enforceable? does the boundary make sense?) to the rubric walker.

It reads the spine, `<folder name>.md`, from a workspace and reports, as compact JSON on stdout:

  - placeholder    literal TBD / TODO / "similar to AD-n" / unfilled {template-token}
  - ad_id          duplicate or non-monotonic AD-n identifiers
  - ad_fields      an AD-n block missing Binds / Prevents / Rule
  - version_pin    a ## Stack table row with no version

Fenced code blocks are blanked (replaced with equal-count blank lines) before scanning, so
mermaid and source trees don't trip false positives AND reported line numbers still line up
with the real file. Reported lines are absolute file lines (frontmatter offset added). Exit
code is always 0 — findings travel in the JSON; the caller (Reviewer Gate / rubric walker)
decides what to do with them.
"""

from __future__ import annotations

import argparse
import json
import re
import sys
from pathlib import Path

AD_HEADING = re.compile(r"^#{2,4}\s*AD-(\d+)\b(.*)$", re.MULTILINE)
HEADING = re.compile(r"^#{1,6}\s", re.MULTILINE)
FENCE = re.compile(r"```.*?```", re.DOTALL)
PLACEHOLDER_WORD = re.compile(r"\b(TBD|TODO|FIXME|XXX)\b")
SIMILAR_TO = re.compile(r"similar to AD-\d+", re.IGNORECASE)
TEMPLATE_TOKEN = re.compile(r"\{[a-z_][a-z0-9_ /.-]*\}")


def split_frontmatter(text: str) -> tuple[str, str, int]:
    """Return (frontmatter, body, body_line_offset).

    Frontmatter is the content between the first two lines that are *exactly* `---`
    (line-exact, like memlog.split — a `---` inside a value or a body thematic break never
    truncates it). body_line_offset is the number of file lines before the body begins, so a
    body-relative line number plus the offset gives the absolute file line. Absent frontmatter
    → ('', text, 0)."""
    lines = text.split("\n")
    if lines and lines[0] == "---":
        for i in range(1, len(lines)):
            if lines[i] == "---":
                fm = "\n".join(lines[1:i])
                body = "\n".join(lines[i + 1 :])
                return fm, body, i + 1
    return "", text, 0


def blank_fences(text: str) -> str:
    """Replace each fenced block with the same number of newlines, so scanning skips fenced
    content while every line number outside the fence stays put."""
    return FENCE.sub(lambda m: "\n" * m.group(0).count("\n"), text)


def line_of(text: str, idx: int) -> int:
    return text.count("\n", 0, idx) + 1


def find_placeholders(body: str, offset: int, name: str) -> list[dict]:
    findings: list[dict] = []
    scan = blank_fences(body)
    # (regex, label, severity) — TBD/TODO and dangling cross-refs are unambiguous; a bare
    # {template-token} can be legitimate brace prose, so it is flagged low ("possible") to keep
    # the mechanical pass near-zero false-positive rather than train reviewers to ignore it.
    for rx, label, severity in (
        (PLACEHOLDER_WORD, "placeholder marker", "high"),
        (SIMILAR_TO, "unresolved cross-reference", "high"),
        (TEMPLATE_TOKEN, "possible unfilled template token (verify)", "low"),
    ):
        for m in rx.finditer(scan):
            findings.append(
                {
                    "category": "placeholder",
                    "severity": severity,
                    "detail": f"{label}: {m.group(0)!r}",
                    "location": f"{name} (line {offset + line_of(scan, m.start())})",
                }
            )
    return findings


def find_frontmatter_placeholders(frontmatter: str, name: str) -> list[dict]:
    """Catch unfilled tokens left in frontmatter (e.g. paradigm/scope/date) — part of the
    spine contract, but outside the body that find_placeholders scans."""
    findings: list[dict] = []
    for rx, label, severity in (
        (PLACEHOLDER_WORD, "placeholder marker", "high"),
        (TEMPLATE_TOKEN, "possible unfilled template token (verify)", "low"),
    ):
        for m in rx.finditer(frontmatter):
            findings.append(
                {
                    "category": "placeholder",
                    "severity": severity,
                    "detail": f"frontmatter {label}: {m.group(0)!r}",
                    "location": f"{name} frontmatter (line {1 + line_of(frontmatter, m.start())})",
                }
            )
    return findings


def find_ad_issues(body: str, offset: int, name: str) -> list[dict]:
    findings: list[dict] = []
    scan = blank_fences(body)  # AD headings shown inside a code fence are not live ADs
    matches = list(AD_HEADING.finditer(scan))
    seen: dict[int, int] = {}
    prev: int | None = None
    for m in matches:
        num = int(m.group(1))
        file_line = offset + line_of(scan, m.start())
        loc = f"{name} AD-{num} (line {file_line})"
        if num in seen:
            findings.append(
                {
                    "category": "ad_id",
                    "severity": "high",
                    "detail": f"AD-{num} id reused (also at line {seen[num]})",
                    "location": loc,
                }
            )
        else:
            seen[num] = file_line
        if prev is not None and num <= prev:
            findings.append(
                {
                    "category": "ad_id",
                    "severity": "high",
                    "detail": f"AD-{num} is non-monotonic (follows AD-{prev}); ids must ascend and never renumber",
                    "location": loc,
                }
            )
        prev = num if prev is None else max(prev, num)

        # block text = from this heading to the next heading of any level
        start = m.end()
        nxt = HEADING.search(scan, start)
        block = scan[start : nxt.start()] if nxt else scan[start:]
        low = block.lower()
        missing = [f for f in ("binds", "prevents", "rule") if f not in low]
        if missing:
            findings.append(
                {
                    "category": "ad_fields",
                    "severity": "high",
                    "detail": f"AD-{num} missing required field(s): {', '.join(missing)}",
                    "location": loc,
                }
            )
    return findings


def find_unpinned_stack(body: str, offset: int, name: str) -> list[dict]:
    """Flag a `## Stack` table row that names something but leaves its version blank or a
    placeholder. Pinning lives in the body table now, not frontmatter. A row whose name is
    still a `{token}` skeleton is left to the placeholder pass, not double-reported here.

    Fences are blanked first (like find_placeholders / find_ad_issues), so a pipe-row or
    heading inside a code block is never read as live Stack content. The heading match is
    `## Stack` with a word boundary, so a renamed heading (`## Stack & Versions`) still
    counts. Name and Version columns are located from the header row, so a reordered table
    pairs name to version correctly; both default to the canonical positions (0, 1)."""
    findings: list[dict] = []
    in_stack = False
    header_seen = False
    name_idx, ver_idx = 0, 1
    scan = blank_fences(body)
    for i, raw in enumerate(scan.splitlines()):
        if HEADING.match(raw):
            in_stack = re.match(r"^##\s+Stack\b", raw) is not None
            header_seen = False
            name_idx, ver_idx = 0, 1
            continue
        if not in_stack or not raw.lstrip().startswith("|"):
            continue
        if set(raw.strip()) <= set("|-: "):
            continue  # separator row
        cells = _table_cells(raw)
        if not header_seen:
            header_seen = True
            for j, c in enumerate(cells):
                if c.lower() == "name":
                    name_idx = j
                elif c.lower() == "version":
                    ver_idx = j
            continue
        dep = cells[name_idx] if len(cells) > name_idx else ""
        version = cells[ver_idx] if len(cells) > ver_idx else ""
        if not dep or TEMPLATE_TOKEN.search(dep):
            continue
        if not version or TEMPLATE_TOKEN.search(version):
            findings.append(
                {
                    "category": "version_pin",
                    "severity": "medium",
                    "detail": f"Stack entry {dep!r} has no version",
                    "location": f"{name} (line {offset + i + 1})",
                }
            )
    return findings


def _table_cells(row: str) -> list[str]:
    """Split a markdown table row into trimmed cells, dropping the leading/trailing pipe."""
    s = row.strip()
    if s.startswith("|"):
        s = s[1:]
    if s.endswith("|"):
        s = s[:-1]
    return [c.strip() for c in s.split("|")]


def lint(text: str, name: str = "spine") -> dict:
    frontmatter, body, offset = split_frontmatter(text)
    findings: list[dict] = []
    findings += find_frontmatter_placeholders(frontmatter, name)
    findings += find_placeholders(body, offset, name)
    findings += find_ad_issues(body, offset, name)
    findings += find_unpinned_stack(body, offset, name)
    counts: dict[str, int] = {}
    for f in findings:
        counts[f["severity"]] = counts.get(f["severity"], 0) + 1
    return {
        "ok": len(findings) == 0,
        "spine": name,
        "total_findings": len(findings),
        "by_severity": counts,
        "findings": findings,
    }


def main(argv: list[str] | None = None) -> int:
    ap = argparse.ArgumentParser(description="Lint an architecture spine for mechanical integrity.")
    ap.add_argument("--workspace", required=True, help="run folder containing the spine, named after the folder")
    ap.add_argument("-o", "--output", help="write JSON here instead of stdout")
    args = ap.parse_args(argv)

    workspace = Path(args.workspace).resolve()
    spine_path = workspace / f"{workspace.name}.md"
    if not spine_path.exists():
        result = {"ok": False, "error": f"{spine_path} not found", "findings": [], "total_findings": 0}
    else:
        try:
            text = spine_path.read_text(encoding="utf-8")
        except (OSError, UnicodeDecodeError) as e:
            # honor the "exit code is always 0" contract: a read/decode failure travels in JSON
            result = {"ok": False, "error": f"could not read {spine_path}: {e}", "findings": [], "total_findings": 0}
        else:
            result = lint(text, spine_path.name)

    out = json.dumps(result, indent=2)
    if args.output:
        Path(args.output).write_text(out + "\n", encoding="utf-8")
    else:
        print(out)
    return 0


if __name__ == "__main__":
    if sys.platform == "win32":
        # Piped output on Windows defaults to a legacy code page, not UTF-8.
        sys.stdout.reconfigure(encoding="utf-8")
        sys.stderr.reconfigure(encoding="utf-8")
    sys.exit(main())
`````

---

## File: skills/bmad-architecture/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
`````

---

## File: skills/bmad-architecture/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-architecture.
#
# Override files (not edited here):
#   {project-root}/_bmad/custom/bmad-architecture.toml         (team)
#   {project-root}/_bmad/custom/bmad-architecture.user.toml    (personal)

[workflow]

# --- Configurable below. Overrides merge per BMad structural rules: ---
#   scalars: override wins • arrays: append

# Steps to run before the standard activation (config load, greet).
# Use for pre-flight loads, approved-stack policy checks, etc.
activation_steps_prepend = []

# Steps to run after greet but before the workflow begins.
# Use for context-heavy setup that should happen once the user has been acknowledged.
activation_steps_append = []

# Persistent facts the workflow keeps in mind for the whole run
# (approved stacks, banned dependencies, platform constraints, compliance guardrails).
# Each entry is either a literal sentence, a skill prefixed with `skill:`, or a `file:`-prefixed
# path/glob whose contents are loaded as facts.
#
# Empty by default. Repo-wide context belongs in AGENTS.md (see bmad-project-context), which every
# skill already sees; use this for context only this skill needs, loaded on demand rather than
# carried as constant memory. Common opt-ins (set in team/user override TOML):
#   "Our org is AWS-only -- do not propose GCP or Azure."
#   "file:{project-root}/docs/engineering-standards.md"
#   "file:{project-root}/**/project-context.md"           # if you keep a project-context.md
persistent_facts = []

# Executed when the workflow completes (after the spine is final and the user has been told).
# String scalar (single instruction) or array of instructions executed in order. Empty for none.
on_complete = ""

# The architecture spine template. Treated as expert prior knowledge, not a checklist — the LLM
# adapts it to the project, altitude, and domain, and drops sections a project genuinely doesn't
# need. Override the path in team/user TOML to enforce a different spine shape.
spine_template = "assets/spine-template.md"

# Run folder location. The spine (`{run_folder_pattern}.md`), its .memlog.md, and any fuller rendering the run
# produces all land inside `{spine_output_path}/{run_folder_pattern}/`. Resume-check scans
# `{spine_output_path}` for prior unfinished runs.
#
# The default pattern fits the common case (one spine per initiative, at the altitude above epics).
# At EPIC altitude, override run_folder_pattern to carry the epic identity so per-epic runs don't
# collide — e.g. set it (team/user TOML) to "architecture-epic-{epic_id}", binding
# {epic_id} from the driving spec / the activating payload. Headless callers may instead pass an
# explicit doc_workspace and bypass the pattern entirely.
spine_output_path = "{output_folder}/{active_initiative}"
run_folder_pattern = "architecture-{slug}"

# Prose-editorial standards applied at finalize ONLY to a fuller prose document the run produces
# (a discussion report, full architecture doc, or design addendum) — never to the spine or other
# short, structured outputs, which are terse and carry decisions in AD-n blocks and diagrams by
# design. Each entry is a `skill:`, `file:`, or plain-text directive applied before the user sees
# the polished draft. Suggested order: structural passes first, prose mechanics last. Append-only.
# The default entry runs bmad-review's two editorial lenses in order:
# structure, then prose on top of the structure findings. The `lenses=` suffix
# names them; drop it to let bmad-review pick what fits the content.
doc_standards = [
  "skill:bmad-review lenses=structure,prose",
]

# External-source registry. Natural-language directives describing knowledge bases, MCP tools, or
# internal systems the LLM may consult ON DEMAND during the run (not preemptively) — approved-stack
# catalogs, internal platform docs, version registries. Each entry names the tool, the trigger
# condition, and any fields it needs. If a named tool is unavailable at runtime, the LLM falls back
# to standard behavior (e.g. web research) and notes the gap. Empty by default.
#
# Examples (set in team/user override TOML):
#   "When choosing a datastore, consult corp:platform_catalog before recommending one."
#   "For current library versions, query corp:artifact_registry before web search."
external_sources = []

# External-handoff routing applied at Finalize to push outputs beyond local files (Confluence,
# Notion, ticket systems). Each entry names the MCP tool, the destination, and required fields.
# Runs after polish; returned URLs/IDs are surfaced. Unavailable tools are skipped and flagged;
# local files always exist. Empty by default.
external_handoffs = []

# --- Finalize reviewers ---
# Extra review lenses spawned as parallel subagents at the validation gate (Finalize and the
# Validate intent), on top of the skill's built-in good-spine checklist and the lint_spine.py
# mechanical floor. The GATE is stakes-gated — a throwaway spine may run it quietly or skip it —
# but whenever the gate runs, every entry here runs with it (the configured floor, never cherry-
# picked); only ad-hoc lenses are optional, and headless never skips the gate.
#
# Entries follow the standard prefix convention:
#   "skill:NAME"   invoke the named review skill as a subagent against the spine
#   "file:PATH"    load the file as a review prompt; spawn an adversarial subagent applying it
#   plain text     use the text directly as the subagent's review prompt
#
# Resolved on-demand (not at activation). Override TOML may append.
finalize_reviewers = [
  "Verify every committed decision was web-researched or reality-checked rather than asserted from training data: current library/framework versions, that each named technology still exists and fits, and — greenfield — the live defaults of any starter it leans on. Flag anything that could be out of date and wasn't confirmed against the web, the existing project, or the current starter.",
  "Attack the spine as an adversary: construct two units one level down that each obey every AD to the letter yet still build incompatibly — clashing shared-data shapes, two owners of one entity, conflicting state-mutation paths. Every pair you find is a hole to close with a new or tightened AD.",
]
`````

---

## File: skills/bmad-architecture/SKILL.md

`````markdown
---
name: bmad-architecture
description: 'Work out and record the architecture decisions that keep separately built parts of a system consistent, in a short architecture document. Creates, updates, or validates one; works from a spec, a raw idea, or an existing codebase. Use when the user says "create the architecture", "create technical architecture", "architecture spine", or "create a solution design"'
---
# BMad Architecture

## Overview

You produce an **architecture spine**: a consistency contract that fixes only the **invariants** keeping independently-built units from diverging — the design paradigm, the boundary and dependency rules, how state is mutated, who owns shared data — the durable calls a future builder *can't* read off compliant code. Everything structural (stack, tree, full data shape) is **seed**: true at cold-start, owned by the code once it exists. Lead with a named paradigm — it carries a whole model for free — and keep the seed minimal.

One test decides what belongs:

> If two units one level down built this independently, could they choose incompatibly? Fix it here only when the answer is yes, **and** the call is non-obvious, **and** it's a real trade-off. Otherwise name it under Deferred and move on.

Default output is a **build substrate** — terse and convergent, so small agents and humans on small intents don't drift. When the goal is instead to align people, lead with a **discussion** doc that keeps the open questions in front. Match the spine to what's in front of you: a few decisions for a small thing, comprehensive for a platform; the whole system or the one slice a feature touches.

Record decisions, not rationale (rationale lives in the memlog). Carry shape in diagrams, not prose. Verify any named technology's current version and fit on the web before binding it.

## How you work

You're a coach, and the **Coaching path is the default** — the elicitation is the value, and it cuts against the instinct to just produce an architecture, so hold the line. Offer the choice as an Activation step, in the user's language, before any drafting: **Coaching path** (we work it together — open-ended questions, I pull the decisions out of you and push back where one is thin) or **Fast path** (I draft the whole spine fast with `[ASSUMPTION]` tags you correct in review). Unless the user clearly wants speed, **coach; don't silently draft.** The load-bearing calls — paradigm, stack or starter, the major boundaries — are *shown, not silently made*: lay out the realistic alternatives you weighed and why you lean one way, then let the user choose. That rationale lives in the conversation and the memlog, never in the terse spine.

Elicit, don't quiz: open-ended "how are you thinking about X?" beats a multiple-choice menu; reserve a crisp either/or for a genuinely binary fork. On the Fast path, inferring and tagging *is* the job.

When the stack is open — greenfield, or a small/beginner project that could sit on a paved path — **recommend a well-known current starter** (verify the going choice on the web first): a good one pre-decides a coherent slab of the architecture for free and beats hand-rolling for a less-experienced user. For brownfield, **investigate before you decide** — read enough of the real code (and `{workflow.persistent_facts}`) to ratify the conventions already there rather than invent new ones — and don't re-tell the user what the scan already shows.

## Read the input to know the job

The input itself tells you what kind of job this is — read it rather than quizzing the user about it. A spec package (`spec-<slug>.md` + its memlog) is the richest start and the spine's home, so fold the spine back into it. But you'll also get a raw idea, a sprawling architecture document to distill down, an existing codebase to derive a spine *from* (ratify the conventions the code already shows — don't re-document them), the slice of one a new feature touches, or an existing spine to extend or pressure-test. Prefer a `.memlog.md` over re-reading the source it came from. Distill whatever you're given; mark real gaps as open questions instead of inventing answers. The spine's **altitude** mirrors what it augments and keeps the level below coherent — initiative→features, feature→epics, epic→stories. Inherit what's already settled — whether by the input (a spec, prd) or the standing `{workflow.persistent_facts}` — silently; don't re-decide or re-ask it. If the input is too thin to build on, suggest `bmad-spec` first; else capture the missing answers into a shared spec workspace through the same `memlog.py`, so `bmad-spec` can later derive the spec without drift.

**Inheriting a parent spine** (e.g. pointed at one epic of a spec whose feature/initiative spine already exists): load the parent spine first and treat its `AD`s, conventions, and paradigm as **binding, read-only** constraints — log each as a `constraint` entry, list them under the spine's *Inherited Invariants* (parent `AD` IDs, never renumbered), and don't re-derive them. Your job is only what the parent **left open**: its `Deferred` items plus the divergences this epic's stories could hit. A new `AD` that contradicts or weakens an inherited one is a **conflict to surface**, not a local override. An epic spine fixes the invariants the epic's stories must share — it does **not** expand per-story detail.

## How a run works

The **memlog** (`.memlog.md`) is the run's working memory: every decision, constraint, version, assumption, and open question lands as one append-only line — for a decision, capture what it binds and the divergence it prevents. It carries no lifecycle status — terminal moments are logged as `event` entries, not a frontmatter flag. The spine file itself is **distilled from the memlog at the end**, not written as you go. Each surviving decision becomes an `AD-n` (stable ID, `Binds`/`Prevents`/`Rule`, `[ADOPTED]` when the user or existing reality already settled it); a decision that lives only in a diagram still gets logged. Resume a prior run by reloading its memlog.

Writes go through the shared script (don't read the file back except on resume):

- `uv run {project-root}/_bmad/scripts/memlog.py init --workspace {doc_workspace} --field scope="…" --field purpose="…" --field altitude="…"`
- `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type <decision|constraint|version|assumption|question|direction|event> --text "…"`

## Resolution rules

- Bare paths and `{skill-root}` (e.g. `references/headless.md`) resolve from this skill's installed directory.
- `{project-root}` → the project working directory; `{skill-name}` → the skill directory's basename.
- `{workflow.<name>}` → a merged `customize.toml` field; `{doc_workspace}` → the bound run folder.
- Forward slashes only. Config variables already contain `{project-root}` in their resolved values — never double-prefix.

## On Activation

**Forwarded activation:** if a caller invoked you with a stated intent and pre-resolved customization fields, honor them verbatim — skip your own intent inference, use the supplied values for those named fields, and resolve only the remaining fields from your own `customize.toml`.

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} --key workflow`.
   - Script not found: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - Any other failure: read `{skill-root}/customize.toml` and use defaults.

   Run `{workflow.activation_steps_prepend}`, then `{workflow.activation_steps_append}`. Hold `{workflow.persistent_facts}` as standing context — empty unless the user opted in — and consult `{workflow.external_sources}` on demand.
2. Resolve config: `uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root} --key core.output_folder --key core.active_initiative`. `{date}` is the current system datetime. `{slug}` is what the architecture is about, in kebab-case: the run lands in `architecture-{slug}/architecture-{slug}.md`.
   - Script not found, or no `output_folder`: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - No `active_initiative`: hand off to the `bmad` skill to set or create one, then run the command again and continue. Headless: write loose.
3. Headless (no interactive user) → follow `references/headless.md` for the whole run. Otherwise greet the user. Detect the intent from the conversation and input — **create** (the default), **update** an existing spine, or **validate** one (see those sections). If the real ask is requirements / UX / a capability contract / epic breakdown / an agent, invoke the `bmad-prd`, `bmad-ux`, `bmad-spec`, `bmad-ticket`, or `bmad-workflow-builder` (if the BMad Builder module is installed) skill instead.
4. If a run folder for this target already exists under `{workflow.spine_output_path}`, offer to resume from its memlog rather than restart.
5. Interactive create: offer the working mode — **Coaching path** (default) or **Fast path** (see *How you work*) — before any drafting; default to Coaching unless the user asks for speed.
6. **Mandatory, both paths, before drafting:** ask whether the spine is the only deliverable — and if not, draw out the *purpose and audience* rather than a document type. "An architecture doc" balloons into bloat; what they actually need might be a one-detail explainer for a single team or a non-technical vision piece for a board. Purpose right-sizes the artifact and may call for extra elicitation up front, not just a finale add-on.

For a new spine, bind `{doc_workspace}` to `{workflow.spine_output_path}/{workflow.run_folder_pattern}/`, seed the spine, `{workflow.run_folder_pattern}.md`, from `{workflow.spine_template}`, run `memlog.py init`, and tell the user the path. **At epic altitude, scope the folder to the epic** (set `run_folder_pattern` per `customize.toml`) so per-epic runs don't collide.

## Reviewer Gate

The spine's pre-handoff review — full mechanics in `references/reviewer-gate.md`. Load it when finalizing or validating: a deterministic `lint_spine.py` pass, then a rubric walker (good-spine checklist) + every `{workflow.finalize_reviewers}` lens dispatched as parallel subagents against the spine, scaled to stakes. At Finalize you apply the clear fixes; under the Validate intent you deliver a bespoke HTML report and then get user input.

## Finalize

Walk the sequence; reviewer fixes land before polish.

1. **Distill.** Write the spine from the memlog (brownfield: + the code sweep) — invariants first, seed minimal, every `AD` carrying Binds/Prevents/Rule, `Deferred` naming what it won't decide. No placeholders; never invent to fill a gap. The template's `<!-- -->` notes are guidance — act on them, then strip them; the finished spine carries no template comment, and only the diagrams that convey the structure (as many as the altitude needs, valid mermaid). Sweep the breadth the altitude owns — every structural dimension is decided, deferred, or an open question; a whole dimension left silent (e.g. the operational/environmental envelope: deployment & environments, infra/provider strategy, operations) is the failure, not a clean spine. A long coaching run distills cleaner in a subagent; the parent falls back inline.
2. **Reconcile inputs.** A subagent per load-bearing input checks it against the spine and returns what didn't land — especially a quiet requirement (a tone, a constraint) the `AD` structure dropped. Before the gate.
3. **Reviewer pass.** Run the Reviewer Gate (`references/reviewer-gate.md`). Resolve before polish.
4. **Triage.** Open questions and `[ASSUMPTION]` tags: blockers (unsafe for what's next) resolved one at a time; the rest deferred with a revisit condition in the memlog.
5. **Renderings & polish.** The spine is the build deliverable; with it and the memlog now in place, produce any *additional* human-facing artifact the user needs, scoped to the purpose and audience drawn out up front. The up-front question already flagged whether one's needed; if it wasn't, still offer one here, seeding concrete options: an interactive HTML+SVG deck to walk a team through the architecture and drive discussion, a fuller HTML/md solution design, a C4 set, or a view of how the work splits across teams/epics. Build only what they pick, right-sized to that purpose; apply `{workflow.doc_standards}` polish to that prose only, never to the spine.
6. **External handoffs.** Run `{workflow.external_handoffs}`; surface returned URLs/IDs. Offer to invoke the `bmad-spec` skill to adopt the spine as a companion, keeping `AD` IDs stable so downstream can cite them.
7. **Close.** Set the spine's own frontmatter `status: final`, `updated: {date}`; log a `memlog.py append --type event --text "spine finalized"` (the memlog has no status field). Share paths. Next, **lead with `bmad-spec`** — recommend adopting/refreshing the spine as a spec companion (always the top recommendation when a spec was an input, and a useful next step even when it wasn't), then `bmad-ticket` or — epic altitude — `bmad-build`; or invoke the `bmad` skill to route.
8. Run `{workflow.on_complete}`.

## Update

Amend an existing spine or provided artifact. Resume from its `.memlog.md` (the authority on what was decided), not the rendered spine. Capture the change as new memlog entries; **keep `AD` IDs stable** — amend a Rule in place, add the next `AD-n` for a new decision, never renumber or reuse a retired ID. Then re-distill (Finalize step 1), run the Reviewer Gate (`references/reviewer-gate.md`), and close as in Finalize. An update that overrides something from a source input: offer to update that source too, so upstream and the spine don't silently diverge.

## Validate

The standalone intent — critique an existing spine without changing it. Run the Reviewer Gate (`references/reviewer-gate.md`) against it and deliver the bespoke HTML report, then offer to roll the findings into an Update. (At Finalize the same gate runs as your own pre-handoff check, where you apply the fixes instead of reporting.)
`````

---

## File: skills/bmad-prd/assets/headless-schemas.md

`````markdown
# Headless Mode Output Examples

Every headless run ends with one of these payloads. Omit keys for artifacts not produced.

## Common fields

- `status` — `"complete"`, `"blocked"`, or `"partial"`
- `intent` — `"create"`, `"update"`, or `"validate"` (matches the detected intent)
- `reason` — required when `status` is `"blocked"`; one-sentence explanation
- `assumptions` — array of inferred values that were not directly confirmed by inputs
- `open_questions` — array of items that need a human decision before the artifact can be considered final

## Create

```json
{
  "status": "complete",
  "intent": "create",
  "prd": "{doc_workspace}/<folder name>.md",
  "addendum": "{doc_workspace}/addendum.md",
  "memlog": "{doc_workspace}/.memlog.md",
  "open_questions": [],
  "assumptions": [],
  "external_handoffs": [
    {"directive": "Confluence upload", "tool": "corp:confluence_upload", "url": "https://confluence.corp/PROD/123", "status": "ok"}
  ]
}
```

## Update

```json
{
  "status": "complete",
  "intent": "update",
  "prd": "{doc_workspace}/<folder name>.md",
  "memlog": "{doc_workspace}/.memlog.md",
  "changes_summary": "1-3 sentences describing what changed and why",
  "conflicts_with_prior_decisions": [],
  "open_questions": [],
  "external_handoffs": [
    {"directive": "Confluence upload", "tool": "corp:confluence_upload", "url": "https://confluence.corp/PROD/123", "status": "ok"}
  ]
}
```

## Validate

```json
{
  "status": "complete",
  "intent": "validate",
  "validation_report": "{doc_workspace}/validation-report.md",
  "findings_summary": {
    "critical": 0,
    "high": 0,
    "medium": 0,
    "low": 0
  },
  "offer_to_update": true
}
```

`validation_report` is always written for Validate intent — the path here is required, not optional.

## Blocked

```json
{
  "status": "blocked",
  "intent": "update",
  "reason": "Change signal ambiguous — could be a scope expansion or a clarification; no inferred direction"
}
```

Always include the intent (best-guess if not certain) and a one-sentence `reason`.
`````

---

## File: skills/bmad-prd/assets/prd-template.md

`````markdown
# PRD Template

## Essential Spine *(almost always present)*

```markdown
---
title: {Product Name}
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
---

# PRD: {Product Name}
*Working title — confirm.*

## 0. Document Purpose
[1 paragraph: who this PRD is for (PM, stakeholders, downstream workflow owners), how it's structured (Glossary-anchored vocabulary, features grouped with FRs nested, assumptions tagged inline and indexed). If UX work or other inputs already exist, name them here and reference where they live — this PRD builds on them, it does not duplicate.]

## 1. Vision
[2-3 paragraphs: what this is, what it does for the user, why it matters. Compelling enough to stand alone.]

## 2. Target User

### 2.1 Jobs To Be Done
[Bulleted. Emotional, social, functional, contextual — whichever apply. Even "this is for me as the builder" is a valid framing for a hobby project.]

### 2.2 Non-Users (v1) *(add when the audience boundary is non-obvious)*
[Who this is explicitly not for in v1.]

### 2.3 Key User Journeys
*Named-persona narratives the product enables. Numbered globally as UJ-1 through UJ-N. FRs reference journeys by ID inline ("realizes UJ-3"); SMs may also cross-reference. If a UX doc already exists, mirror its UJ IDs here and point to the source.*

**Default shape:** a named scene with entry state, path, climax, and resolution. Each beat forces specificity the team would otherwise leave implicit — auth assumptions, screen order, what tells the user value landed. Read together as a short narrative; the example below shows the form.

- **UJ-1. {One-line title — persona doing the thing.}**
  - **Persona + context:** one line, grounded enough to explain the *why*.
  - **Entry state:** authenticated? which surface? coming from where?
  - **Path:** 3-5 concrete beats — taps, screens, decisions.
  - **Climax:** the moment value is delivered and how the user knows.
  - **Resolution:** state they're left in, what's next.
  - **Edge case** *(optional)*: one real failure mode and what the user does next.

  *Written out, that becomes:*
  > **UJ-3. Priya checks the trip damage before she's even home.**
  > Priya, budgeting on a single income with a new baby, finishes a grocery run and gets in the car. Already authenticated via biometric on a previous session. She opens the app, taps the FAB camera, and scans the receipt. The app OCRs the total and shows a single-screen overlay: this trip $84.20, weekly cap $250, $172.10 remaining, three days left in the week. She closes the app and drives home. **Edge case:** if she scanned a receipt earlier today, the app asks whether this replaces or adds to that trip before counting it against the cap.

- **UJ-2. ...**

**Scope dial:**
- **Lighter** — hobby/solo, library/CLI, or when the UJ is essentially a JTBD restated: a single sentence works (`{Persona}, {context}, {what they do and why}.`).
- **Heavier** — auth, multi-device handoff, complex navigation, or anything feeding downstream UX/architecture: add a numbered Flow, an Edge cases list, and a capability → FR mapping (`The system must {capability}. → FR-N`).

## 3. Glossary
*Downstream workflows and readers must use these terms exactly. FRs, UJs, and SMs use Glossary terms verbatim; introducing a synonym anywhere in the PRD is a discipline violation. If §4 introduces a new domain noun, add it to the Glossary in the same pass.*

- **Term** — Definition. Relationships to other Glossary terms. Cardinality where relevant.
- **Term** — ...

[Every domain noun the rest of the document uses. Defined once. No synonyms anywhere else in the PRD.]

## 4. Features
*Each subsection is a coherent feature: behavioral description first, FRs nested under it, optional feature-specific NFRs and notes. FRs are numbered globally (FR-1 through FR-N) so downstream artifacts have stable references even if features get reorganized. Reference user journeys by ID inline ("realizes UJ-2") where the chain matters.*

### 4.1 {Feature Name}
**Description:** [Behavioral narrative — how this feature works, who uses it, the user experience, edge cases. Realizes UJ-X, UJ-Y. Use Glossary terms exactly. Embed inline `[ASSUMPTION: ...]` tags where you inferred without confirmation.]

**Functional Requirements:**

#### FR-1: {Short capability name}

[Actor] can [capability] [under conditions]. Realizes UJ-X.

**Consequences (testable):**
- {Specific testable condition, e.g. "System returns HTTP 429 when request rate exceeds 100/sec per merchant."}
- {Another testable condition.}

**Out of Scope:** *(optional — what this FR explicitly does NOT cover)*
- {bound}

#### FR-2: ...

**Feature-specific NFRs:** *(only if any apply uniquely to this feature)*
- Performance / security / accessibility / etc. specific to this feature.

**Notes:** *(optional — open questions specific to this feature, `[NOTE FOR PM]` callouts)*

### 4.2 {Feature Name}
...

## 5. Non-Goals (Explicit)
[Bulleted. What this product is *not* and what it will *not* do in v1. Does outsized work for downstream readers and workflows — prevents the "let me also add this nearby thing" failure mode at every level (epic, ticket, code). Inline `[NON-GOAL for MVP]` callouts within §4 Features cover deferred items within features; this section captures the broader "we are not building X / we are not becoming Y" statements.]

## 6. MVP Scope

### 6.1 In Scope
[Bulleted, crisp.]

### 6.2 Out of Scope for MVP
[Bulleted. Each item with a one-line reason if the reason matters. Mark items deferred to v2/v3 explicitly. Add `[NOTE FOR PM]` callouts where a deferred item is emotionally load-bearing — flags it for revisit if timeline permits.]

## 7. Success Metrics

*Each SM cross-references the FR(s) it validates. Counter-metrics counterbalance specific primary or secondary metrics.*

**Primary**
- **SM-1**: Metric — definition, target. Validates FR-X, FR-Y.

**Secondary**
- **SM-2**: Metric — definition, target. Validates FR-Z.

**Counter-metrics (do not optimize)**
- **SM-C1**: Metric — why this should *not* be optimized. Counterbalances SM-1.

[Length scales with stakes. Hobby/utility PRD: a single sentence may be enough ("Success: I use this weekly and don't abandon it after a month"). Public launch / enterprise: full quantitative breakdown with measurement methods. Counter-metrics are as load-bearing as primary metrics — they prevent the architect from optimizing the wrong thing and the dev from gaming the wrong target.]

## 8. Open Questions
[Numbered. Things still unknown — they become future tickets or follow-up research, not silent gaps.]

## 9. Assumptions Index
*Every `[ASSUMPTION]` from the document, surfaced for explicit confirmation:*
- Inline assumption from §X.Y — short description.
- ...
```

---

## Adapt-In Menu *(add the clusters the product calls for)*

### Cross-cutting quality and shape *(most non-trivial PRDs)*
- **Cross-Cutting NFRs** — system-wide non-functional requirements not tied to a single feature (performance, security, reliability, observability). Add when system-wide quality attributes are meaningful.
- **Constraints and Guardrails** — Safety, Privacy, Cost. Subsection per cluster. Add when any of these are real concerns.
- **Why Now** — add when timing is load-bearing (a market shift, a technology enabler, a regulatory deadline). Drop when timing is incidental.

### Consumer / branded products
- **Aesthetic and Tone** — visual references, anti-references, voice/tone for any product-generated text.
- **Information Architecture** — top-level surfaces, navigation, screens.
- **Monetization** — free vs. paid, pricing assumptions, ads policy.
- **Platform** — web, mobile, PWA, native, v1 vs. v2+.

### Enterprise initiatives
- **Stakeholders and Approvals** — who must sign off, at what stage.
- **Risk and Mitigations** — operational, security, business, reputational risk register.
- **ROI / Business Case** — quantified benefit, cost, payback period.
- **Operational Requirements** — SLAs, RTO/RPO, support tier, on-call expectations.
- **Integration and Dependencies** — SSO, existing enterprise systems, data sources, downstream consumers.
- **Rollout and Change Management** — phased rollout plan, training, internal communication.
- **Data Governance** — residency, sovereignty, classification, retention.
- **Audit Trail / Decision Provenance** — formal documentation requirements for regulated environments.

### Regulated domains
- **Compliance and Regulatory** — HIPAA, PCI-DSS, GDPR, SOX, SOC 2, Section 508 / WCAG 2.1 AA, FedRAMP, etc. — whichever apply. If any item needs depth, add a `[NOTE FOR PM]` callout to revisit or move to an addendum.

### Developer products (libraries, APIs, CLIs, SDKs)
- **API Contracts / Public Surface** — endpoint shapes, breaking change policy.
- **Versioning and Deprecation Policy**.
- **Performance Budgets** — latency, throughput, resource use.
- **Language / Runtime Targets and Dependency Policy**.

### Embedded / hardware
- **Hardware Constraints** — memory, power, form factor.
- **Deployment and Update Mechanism** — OTA, manual, image-based.
- **Environmental and Reliability Requirements**.

### Small-scope all-inclusive *(use when scope is 1-2 stories' worth and the user wants a single captured artifact — chosen during the Right-skill check in Discovery)*
- **Stories** — story-level specs listed inline at the end of the doc. Each story: *"As a [persona], I can [action] [under conditions]. Acceptance: [testable criteria]."* Numbered Story-1, Story-2, ... for reference. Pair with very lean §1 Vision, §2 Target User (often just JTBD + one UJ), §3 Glossary (handful of terms), §4 Features (often a single feature), §6 MVP Scope (in/out very tight). The whole doc fits on a page or two and captures intent + implementable stories in one place. If the user doesn't want the captured artifact at all, `bmad-build` is the better path — this cluster is only for "I want a doc *and* the stories."
`````

---

## File: skills/bmad-prd/assets/prd-validation-checklist.md

`````markdown
# PRD Quality Rubric

A judgment rubric for the validator subagent. Walk the PRD with these dimensions in mind and write substantive findings — not box-ticking. The goal is a review that tells the user whether this PRD is *good*, not whether it has the right section headers.

Most PRDs do not need every dimension scrutinized equally. Calibrate to the agreed stakes, the PRD's shape (consumer product, internal tool, regulatory update, technical capability spec), and what the PRD itself is trying to do. Be specific — cite locations, quote phrases, name what's missing. Abstract criticism is failure of nerve.

## How to use this rubric

1. Read the full PRD (and addendum.md if present) before writing anything.
2. For each of the seven dimensions below, form a judgment — *strong / adequate / thin / broken* — backed by specifics from the PRD.
3. Write findings only where they add information. A `strong` dimension may need no findings; a `broken` one needs concrete, fixable ones.
4. Severity ranks impact on the PRD's usefulness, not how easy the fix is. A vague Vision statement is *critical* even though it's a one-paragraph fix; a glossary drift might be *low* even though it appears in many places.
5. The overall verdict is your synthesis — 2–3 sentences that name what holds up and what's at risk. Earn it with the dimension judgments.

## Output format

Write findings to `{doc_workspace}/review-rubric.md`:

```markdown
# PRD Quality Review — {prd_name}

## Overall verdict
[2–3 sentences. What holds up, what's at risk. Earned by the dimension judgments below.]

## Decision-readiness — [strong | adequate | thin | broken]
[1–3 paragraphs of judgment with specific PRD locations.]

### Findings
- **[critical|high|medium|low]** [Title] (§ location) — [Note]. *Fix:* [suggested fix].

## Substance over theater — [verdict]
...

(repeat for each dimension)

## Mechanical notes
[Glossary drift, ID continuity, broken cross-refs, Assumptions Index roundtrip. Lighter weight — these matter for downstream but don't drive the overall verdict.]
```

## The seven dimensions

### 1. Decision-readiness

Can a decision-maker act on this PRD? Are the trade-offs surfaced honestly, or has the PRD smoothed everything to neutral? Would someone pushing back find their objection acknowledged or dodged?

Look for:
- Decisions that are stated as decisions, not buried as "considerations."
- Trade-offs named with what was given up, not just what was chosen.
- Open Questions that are actually open — not rhetorical questions with an answer in the next sentence.
- `[NOTE FOR PM]` callouts at real tensions, not at safe checkpoints.

Red flag: a PRD where every choice "balances" everything, every NFR is "important," every persona "values" the product.

### 2. Substance over theater

Is the content earned, or is it furniture? Distinguish:

- **Persona theater** — Personas that don't drive a single decision in the PRD. More than four personas. Personas whose only function is to make the PRD look thorough.
- **Innovation theater** — claimed novelty that isn't novel. Differentiation sections written because the template had one, not because Discovery surfaced something.
- **NFR theater** — copied boilerplate ("system must be scalable / secure / reliable") without product-specific thresholds.
- **Vision theater** — a Vision statement that could swap into any PRD in this category without change.

Flag what reads like furniture, even if it's well-written furniture.

### 3. Strategic coherence

Does the PRD have a thesis? Do the features serve a unified arc, or is it a list of capabilities someone wanted?

Look for:
- A stated thesis the PRD bets on (problem framing, user insight, market move).
- Feature prioritization that follows from the thesis — not from "what's easy first."
- Success Metrics that validate the thesis, not metrics that just measure activity (DAU/MAU when the thesis is about engagement quality is a tell).
- Counter-metrics named when SMs exist.
- Coherent MVP scope kind — problem-solving, experience, platform, or revenue — with scope logic that matches.

Red flag: a PRD that reads as a backlog with section headings.

### 4. Done-ness clarity

Would an engineer reading this PRD know what "done" looks like for each FR?

Look for:
- FRs with at least one testable consequence per FR — verifiable condition, measurable outcome.
- "System handles X gracefully," "reasonable performance," "user-friendly" — flag every one.
- Acceptance criteria implied or explicit. Sometimes the FR's consequences carry this; sometimes the PRD genuinely needs an Acceptance section.
- For non-functional sections (UX, performance, security): bounds, not adjectives.

This is the dimension downstream story creation will lean on hardest. Be unforgiving here.

### 5. Scope honesty

Are omissions explicit, or is the reader meant to infer them?

Look for:
- A Non-Goals section where it would do real work — and `[NON-GOAL for MVP]` callouts where omissions could be silently assumed.
- `[ASSUMPTION: …]` tags on inferences the user didn't directly confirm, indexed at the end.
- `[NOTE FOR PM]` callouts at deferred decisions and unresolved tensions.
- De-scoping proposed honestly, not done silently.

Open-items density: count Open Questions + `[ASSUMPTION]` + `[NOTE FOR PM]` callouts relative to stakes. High counts on a low-stakes PRD is fine; high counts on a green-light-to-build PRD is a blocker.

### 6. Downstream usability

If this PRD feeds UX, architecture, or story creation, can those workflows source-extract from it cleanly?

Look for:
- Glossary present; every domain noun used identically across FRs, UJs, SM definitions.
- FR / UJ / SM IDs contiguous, unique, and cross-references that resolve.
- Each section makes sense pulled out alone — cross-references via Glossary terms, not "see above."
- UJs each have a named protagonist; no floating UJs.

For standalone PRDs (no downstream), this dimension matters less — say so.

### 7. Shape fit

Has the PRD been forced into a shape that doesn't match the product?

- Consumer product / multi-stakeholder B2B / meaningful UX → UJs with named protagonists are load-bearing.
- Internal tool, single-operator role → capability spec shape; UJs may be overhead; SMs may be operational rather than user-facing.
- Regulatory or compliance update → constraint traceability is non-negotiable; UJs may be irrelevant.
- Hobby / solo → rigor light, substance bar still applies.
- Brownfield → existing-code references must be accurate; new UJs and existing UJs must be distinguished.
- Chain-top (feeds UX → architecture → stories) → downstream usability matters more; standalone PRDs can be lighter on traceability.

Flag PRDs that are over-formalized (UJ density for a single-operator tool) or under-formalized (consumer product with no UJs).

## Mechanical notes

Cover these as a tail section, not a primary dimension. They matter for downstream but don't drive the verdict on whether the PRD is good.

- Glossary drift (case, plural, synonyms across the PRD).
- ID continuity (gaps, duplicates, unresolved cross-references).
- Assumptions Index roundtrip (every inline `[ASSUMPTION]` indexed; index entries all appear inline).
- UJ protagonist naming (each UJ has a named protagonist carrying context inline).
- Required sections present for the agreed stakes and product type.
`````

---

## File: skills/bmad-prd/references/headless.md

`````markdown
# Headless Mode

Load this file when bmad-prd is invoked headless (no interactive user). Follow it for the whole run.

## Detection

Headless mode is in effect when any of the following is true:

- the invoking caller sets a `headless: true` flag (or equivalent argument the harness exposes),
- the invocation is from another skill or a non-interactive runner (no TTY, no user message stream),
- `{workflow.activation_steps_prepend}` includes an entry that explicitly declares headless,
- the first message comes from an automation context that pre-supplies all inputs and asks for an artifact path back.

When ambiguous, default to interactive.

## Inputs the caller is expected to provide

The caller passes inputs in their first message (free-form structured payload; no fixed schema, but every field below should be present when applicable):

- `intent` — `"create"`, `"update"`, or `"validate"`. If absent, infer from the artifact set.
- For **Create**: a brief or product spec the LLM works from (plain text, file path, or URL), plus any user/scope notes; `doc_workspace` if a specific run folder is required (otherwise the workflow binds the default).
- For **Update**: the existing PRD path (`prd-<slug>.md`, or a workspace path that contains one), and a change signal (the request: what to change and why).
- For **Validate**: the existing PRD path (or workspace path), and optionally a checklist override path. Workspace defaults to the PRD's containing directory.

Anything the caller does not provide is either inferred from inputs/workspace or recorded as `assumptions[]` / `open_questions[]` in the JSON status. Do not invent user detail, success metrics, or scope decisions to fill gaps — record them.

## General

Do not ask. Complete the intent using what is provided, what exists in `{doc_workspace}`, or what you can discover yourself. If intent remains ambiguous after inference, halt with `status: "blocked"` and a `reason` field — do not prompt. Do not greet.

Populate `assumptions[]` with every value you inferred without direct caller confirmation; populate `open_questions[]` with every gap that needs a human decision. Use `status: "partial"` when the artifact was produced but `open_questions[]` is non-empty or critical inputs were inferred (Create with no brief; Update with a vague signal acted on best-effort; Validate that could not load the checklist). `complete` = stands on its own; `partial` = caller should review before downstream use; `blocked` = no artifact produced.

End with the JSON response (an example of each payload is in `assets/headless-schemas.md`). The `intent` field must match the detected intent. Omit keys for artifacts not produced.

## Mode-specific overrides

**Update.** Apply the change, log it via `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type change --text "<change + rationale>"`, and surface any conflict-with-prior-decision in `conflicts_with_prior_decisions[]` in the JSON status. Halt `blocked` if intent is ambiguous.

**Validate.** Always write both `validation-report.html` and `validation-report.md` to `{doc_workspace}` regardless of finding count. Always include `"offer_to_update": true` in the JSON status. Skip the browser-open step in `references/validate.md` — write the artifacts and return.
`````

---

## File: skills/bmad-prd/references/validate.md

`````markdown
# Validate

The Validate intent playbook. Standalone — this intent critiques an existing PRD without changing it and ends after the user has seen the report; it does not run Finalize. The synthesis pipeline below is also reused for mid-session report requests during Create/Update.

## Orient

Source-extract against `.memlog.md`, any original inputs, and the PRD/addendum themselves. Delegate to subagents per PRD Discipline → "Extract, don't ingest" (in SKILL.md); the parent assembles from extracts.

## Run the Reviewer Gate

Run the Reviewer Gate (see SKILL.md) against the PRD, the main file `<folder name>.md` (and `addendum.md` if present). The rubric walker is the default entry in the gate menu; under Validate intent it additionally runs the synthesis pipeline below. The Finalize discipline pass during Create/Update does NOT render a report — findings stay in-conversation.

## Rubric-walker pipeline

The rubric walker is the primary review entry. Spawn it as a subagent with this prompt:

> You are validating a PRD against the quality rubric at `{workflow.validation_checklist_template}`. Read the full rubric first, then read the PRD, the main file `{doc_workspace}/<folder name>.md` (and `addendum.md` if present). Form a judgment per dimension — *strong / adequate / thin / broken* — and write findings only where they add information. Cite specific PRD locations and quote phrases. Severity ranks impact on the PRD's usefulness, not how easy the fix is. Write your review to `{doc_workspace}/review-rubric.md` in the format the rubric specifies. Return ONLY a compact summary (overall verdict, dimension verdicts, finding counts by severity, file path).

The Reviewer Gate may also dispatch additional reviewers from `{workflow.finalize_reviewers}` (adversarial-general by default) and any ad-hoc reviewers the parent judges warranted. Each writes its review to `{doc_workspace}/review-{lens}.md` and returns a compact summary. Run in parallel.

## Synthesis pipeline

Once every selected reviewer has returned, the parent synthesizes one consolidated report. **Do not skip this step under Validate intent** — it produces the persistent artifact the user opens.

### Inputs

- `{doc_workspace}/review-rubric.md` — primary, structured by the seven dimensions
- Zero or more `{doc_workspace}/review-{lens}.md` files — extra reviewers (adversarial, etc.)
- `{workflow.validation_report_template}` — the HTML skeleton

### What the synthesis pass does

1. Read every reviewer file in `{doc_workspace}/review-*.md`.
2. Fill the HTML skeleton:
   - **Header.** PRD name, path. Grade derived from the rubric verdicts and severity counts: *Excellent* = all dimensions strong/adequate, no high/critical findings · *Good* = ≤1 thin dimension, no critical findings · *Fair* = multiple thin dimensions or any high finding · *Poor* = any broken dimension or any critical finding. Set the matching `grade-excellent | grade-good | grade-fair | grade-poor` class.
   - **Synthesis block.** Lift the rubric's *Overall verdict* paragraph as the lead; if adversarial or ad-hoc reviewers materially shift the picture, add a second paragraph that names what they surfaced.
   - **Dimension summary cards.** One per dimension that was assessed. Colored verdict text. Skip dimensions the rubric marked n/a for this PRD (e.g. downstream usability for a standalone PRD).
   - **Dimension sections.** One `<section class="dimension">` per assessed dimension, in rubric order. `<details open>` for *thin* and *broken*; closed for *strong* and *adequate*. Each contains the dimension judgment (the prose from review-rubric.md) and the findings list.
   - **Reviewer sections.** One `<section class="reviewer-section">` per extra reviewer that ran. The source file path goes in the `<span class="reviewer-source">`. Closed by default. Adversarial findings keep their adversarial voice — do not soften.
   - **Mechanical notes.** Bullet list from the rubric's "Mechanical notes" section. Skip the block if empty.
   - **Footer.** Rubric path, ISO timestamp.
3. Write the filled HTML to `{doc_workspace}/validation-report.html`.
4. Write the markdown twin to `{doc_workspace}/validation-report.md` (same content, grouped by severity rather than by dimension — see format below; this is the canonical form for downstream re-reading).
5. Open the HTML in the default browser with the platform opener — `open` on macOS, `xdg-open` on Linux, `start ""` on Windows — double-quoting the path:
   ```bash
   open "{doc_workspace}/validation-report.html"
   ```
   If the command fails, don't retry with another opener: tell the user the file path and move on. Skip the open step in headless mode (see `references/headless.md`).

### Markdown twin format

```markdown
# Validation Report — {prd_name}

- **PRD:** `{prd_path}`
- **Rubric:** `{rubric_path}`
- **Run at:** {ISO timestamp}
- **Grade:** {Excellent | Good | Fair | Poor}

## Overall verdict
{synthesis paragraphs}

## Dimension verdicts
- Decision-readiness — {verdict}
- Substance over theater — {verdict}
- (etc. for each assessed dimension)

## Findings by severity

### Critical (n)
**[Dimension or Reviewer]** — Title (§ location)
{Note}
Fix: {suggested fix}

### High (n)
...

### Medium (n)
...

### Low (n)
...

## Mechanical notes
- {bullet}

## Reviewer files
- `review-rubric.md`
- `review-adversarial-general.md` (if present)
- (etc.)
```

Re-running validation overwrites the consolidated report in place. The individual `review-*.md` files are preserved so the user can drill in.

## Close

Surface artifact paths; the rendered HTML/markdown is the persistent artifact. Always offer to roll findings into an Update.
`````

---

## File: skills/bmad-prd/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
`````

---

## File: skills/bmad-prd/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-prd.
#
# Override files (not edited here):
#   {project-root}/_bmad/custom/bmad-prd.toml         (team)
#   {project-root}/_bmad/custom/bmad-prd.user.toml    (personal)

[workflow]

# --- Configurable below. Overrides merge per BMad structural rules: ---
#   scalars: override wins • arrays: append

# Steps to run before the standard activation (config load, greet).
# Use for pre-flight loads, compliance checks, etc.
activation_steps_prepend = []

# Steps to run after greet but before the workflow begins.
# Use for context-heavy setup that should happen once the user has been acknowledged.
activation_steps_append = []

# Persistent facts the workflow keeps in mind for the whole run
# (standards, compliance constraints, stylistic guardrails).
# Each entry is either a literal sentence, a skill prefixed with `skill:`, or a `file:`-prefixed path/glob
# whose contents are loaded as facts.
#
# Empty by default. Repo-wide context belongs in AGENTS.md (see bmad-project-context), which every
# skill already sees; use this for context only this skill needs, loaded on demand rather than
# carried as constant memory. Common opt-ins (set in team/user override TOML):
#   "file:{project-root}/**/project-context.md"                               # if you keep a project-context.md
#   "skill:acme-co:terms-and-conditions"                                      # a skill that contains some relevant info
#   "Investor PRDs must include a market sizing section."                    # generic agent instruction
persistent_facts = []

# Executed when the workflow completes (after the user has been told the
# PRD is ready). Accepts either a string scalar (single instruction)
# or an array of instructions executed in order. Empty for none.
on_complete = ""

# Default PRD structure. Treated as a starting point — the LLM adapts it
# to the product, project type, and domain. Override the path in team/user TOML
# to enforce a different structure (e.g. regulated-industry, internal-tool, investor-input).
prd_template = "assets/prd-template.md"

# PRD quality rubric used at the Validate intent and at Finalize step 3.
# A subagent walks the rubric against the PRD and writes a substantive review
# organized by quality dimensions (decision-readiness, substance, strategic
# coherence, etc.). Override the path in team/user TOML to enforce an
# org-specific rubric (regulated-industry compliance, investor-pitch standards,
# etc.). The filename "checklist" is retained for back-compat with override
# files; the content is a judgment rubric, not a boolean checklist.
validation_checklist_template = "assets/prd-validation-checklist.md"

# HTML skeleton the synthesis pass fills directly when consolidating reviewer
# outputs into a validation report. No substitution engine — the parent LLM
# reads every {doc_workspace}/review-*.md, fills the skeleton's TEMPLATE_*
# placeholders, and writes the result. Fully overridable to match org branding.
# Uses inline CSS, no external dependencies, and native HTML <details> for
# collapse — no JS.
validation_report_template = "assets/validation-report-template.html"

# Run folder location. The PRD (`{run_folder_pattern}.md`), optional addendum, memlog, and optional
# validation report all land inside `{prd_output_path}/{run_folder_pattern}/`.
# Resume-check scans `{prd_output_path}` for prior unfinished runs.
prd_output_path = "{output_folder}/{active_initiative}"
run_folder_pattern = "prd-{slug}"

# Document standards applied to human-consumed docs at finalize. Each entry is
# a `skill:`, `file:`, or plain-text directive; the parent LLM applies the
# findings before the user sees the draft. Encodes standards, not options.
#
# Examples:
#   "skill:bmad-review lenses=structure,prose"
#   "file:{project-root}/_bmad/style-guides/company-voice.md"
#   "Convert all dates to ISO 8601 format."
#
# Suggested order (broader passes first, narrower last):
#   1. Structural (cuts, reorganization, section sizing)
#   2. Content/voice/conventions (org standards, tone, terminology, compliance)
#   3. Prose mechanics (grammar, clarity, typos)
#
# Override the array in team/user TOML to add additional standards. Append-only:
# base entries cannot be removed or replaced (resolver has no removal mechanism).
# The default entry runs bmad-review's two editorial lenses in order:
# structure, then prose on top of the structure findings. The `lenses=` suffix
# names them; drop it to let bmad-review pick what fits the content.
doc_standards = [
  "skill:bmad-review lenses=structure,prose",
]

# External-source registry. Natural-language directives describing knowledge
# bases, MCP tools, or internal systems the LLM may consult during the workflow
# when a relevant need surfaces. The LLM does NOT query these preemptively —
# it consults them on demand (during Discovery, validation, drafting, etc.).
# Each entry names the tool, the conditions for using it, and any fields the
# tool needs. If a named MCP tool is unavailable at runtime, the LLM falls
# back to standard behavior and notes the gap. Empty by default.
#
# Lifecycle note: distinct from persistent_facts. persistent_facts are loaded
# once at activation and kept in mind for the whole run; external_sources are
# a registry consulted on demand and only when the conversation surfaces a
# matching need.
#
# Examples (set in team/user override TOML):
#   "When researching internal product context, consult corp:kb_search (database='product-docs') before web search."
#   "For competitive landscape during Discovery, query corp:competitive_db with category={project_name}."
#   "When validating domain-compliance claims, cross-check against corp:hipaa_reference for healthcare or corp:pci_reference for fintech."
external_sources = []

# External-handoff routing. Natural-language directives the LLM applies at
# Finalize to route outputs beyond local files (Confluence, Notion, Google
# Drive, ticket systems, etc.). Each entry names the MCP tool, the destination,
# and the fields the tool needs. Handoffs run after the artifact is polished
# and before the final user-facing message. URLs or IDs returned by the
# destination are captured and surfaced to the user. If a named tool is
# unavailable at runtime, the handoff is skipped and flagged in the JSON
# status; local files always exist regardless. Fires automatically — users
# can opt out in their prompt for a specific run. Empty by default.
#
# Lifecycle note: distinct from persistent_facts and external_sources.
# Fired once at Finalize step 6, never during Discovery or drafting.
#
# Examples (set in team/user override TOML):
#   "After finalize, upload the PRD and addendum.md to Confluence via corp:confluence_upload (space_key='PROD', parent_page='PRDs', label='prd')."
#   "Mirror the PRD to Notion via notion:create_page (database_id='abc123', title='PRD: '+{project_name})."
#   "When the PRD references a parent initiative, link via corp:jira_link on the epic key in frontmatter."
external_handoffs = []

# --- Finalize reviewers ---
# Reviewers spawned at Finalize step 3 (and at the Validate intent) alongside
# the structural checklist validator. The authoring skill assembles the gate
# menu (validator + these reviewers + any ad-hoc reviewers it judges warranted
# by the artifact content) and lets the user pick all, a subset, or skip. Gate
# UX is stakes-calibrated: hobby/solo scope may run defaults quietly or skip;
# higher stakes get the explicit menu.
#
# Entries follow the standard prefix convention (same as persistent_facts and
# doc_standards):
#   "skill:NAME"   invoke the named review skill as a subagent against the PRD
#   "file:PATH"    load the file as a review prompt; spawn an adversarial
#                  subagent applying that prompt to the PRD
#   plain text     use the text directly as the subagent's review prompt
#
# Override TOML may append additional reviewers. Arrays append per BMad rules.
#
# Resolved on-demand by the authoring skill (not pulled at activation): only
# when entering the Validate intent or assembling the gate at Finalize step 3.
finalize_reviewers = []
`````

---

## File: skills/bmad-prd/SKILL.md

`````markdown
---
name: bmad-prd
description: Create, update, or validate a PRD. Use when the user wants help producing, editing, or validating a PRD
---
# BMad PRD

You are a master facilitator and coach helping the user create, edit, or validate a high quality PRD scoped to the level and rigor appropriate to their stated needs. Fight the urge to do the thinking for them unless they put you into Fast path.

## Conventions

- Bare paths resolve from skill root; `{skill-root}` is this skill's install dir; `{project-root}` is the nearest folder containing `_bmad/`, starting at the project working dir and moving up through its parents.
- `{workflow.<name>}` resolves to fields in `customize.toml`'s `[workflow]` table (overrides win per BMad merge rules).
- `{doc_workspace}` is the bound run folder.
- **File roles.** `.memlog.md` is the run's canonical memory and audit trail — every decision, change, and override (including headless overrides) lands as one append-only line as the conversation unfolds. All writes go through the shared script, never by hand: `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type <decision|change|override|assumption|event> --text "<one-line gist, reason included>"` (atomic; read it back only to resume or audit). The PRD is distilled toward it; whatever isn't logged is lost on resume. `addendum.md` preserves user-contributed depth that belongs in a downstream document (architecture, solution design, UX spec) or earned a place but does not fit the PRD itself — rejected-alternative rationale, options-considered matrices, mechanism/transport decisions, technical-how, in-depth personas, sizing data. Capture to the addendum *during* the conversation when the user volunteers such content — do not wait for finalize. Audit and override information never goes in the addendum.

## On Activation

**Forwarded activation:** if a caller invoked you with a stated intent and pre-resolved customization fields (e.g. the `bmad-create-prd` / `bmad-edit-prd` / `bmad-validate-prd` shims), honor them verbatim — skip your own intent inference, use the supplied values for those named fields, and resolve only the remaining fields from your own `customize.toml`.

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} --key workflow`.
   - Script not found: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - Any other failure: read `{skill-root}/customize.toml` directly and use defaults.
2. Run `{workflow.activation_steps_prepend}`. Treat `{workflow.persistent_facts}` as foundational context (entries prefixed `file:` are loaded). `{workflow.external_sources}` is an org-configured registry of internal tools (knowledge bases, MCP tools); consult them alongside generic web research on the same triggers, org tools preferred when their directive matches. Research itself fires during Discovery — see **Research subagents**.
3. Resolve config: `uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root} --key core.output_folder --key core.active_initiative`. `{date}` is the current system datetime. `{slug}` is what the PRD is about, in kebab-case: the run lands in `prd-{slug}/prd-{slug}.md`.
   - Script not found, or no `output_folder`: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - No `active_initiative`: hand off to the `bmad` skill to set or create one, then run the command again and continue. Headless: write loose.
4. If headless, follow `references/headless.md` for the whole run. Otherwise greet the user. In the greeting, let the user know that at any point they can invoke `bmad-party-mode` for multi-agent perspectives or `bmad-advanced-elicitation` for deeper exploration on a specific section. Then scan for misroute on the first message: if the signal points elsewhere (game → BMad GDS; express build → `bmad-build`; one-pager → `bmad-product-brief`; vet product idea → `bmad-prfaq`; agent skill or custom agent → `bmad-workflow-builder`, if the BMad Builder module is installed), suggest they might want the other options before continuing.
5. Detect intent: **Create** (no PRD), **Update** (existing PRD), **Validate** (critique only). If ambiguous, ask. For Create intent, before binding a fresh workspace, scan `{workflow.prd_output_path}` for prior in-progress runs (folders matching `{workflow.run_folder_pattern}` whose main file's frontmatter `status` is not `final`); if any exist, offer to resume rather than starting over.

Run `{workflow.activation_steps_append}`.

Activation is complete. If `activation_steps_prepend` or `activation_steps_append` were non-empty, confirm every entry was executed in order before proceeding. Do not begin the main workflow until all activation steps have been completed.

## Intent Modes

**Create.** Bind `{doc_workspace}` to `{workflow.prd_output_path}/{workflow.run_folder_pattern}/`. Write the PRD as `{workflow.run_folder_pattern}.md` with YAML frontmatter (title, status, created, updated — initial `status: draft`), and seed the memlog with `uv run {project-root}/_bmad/scripts/memlog.py init --workspace {doc_workspace} --field topic="<PRD/product name>"` so subsequent decisions land in a known file. Tell the user the path. Run `## Discovery`, then `## Finalize`.

**Update.** Reconcile the PRD with a change signal. Source-extract against PRD, addendum, `.memlog.md`, and original inputs (extract, don't ingest). If `.memlog.md` is missing, init it with `uv run {project-root}/_bmad/scripts/memlog.py init --workspace {doc_workspace}`, then spawn a one-time bootstrap subagent to reverse-engineer a thin log from the PRD (one `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type decision --text "<recovered decision>"` per recovered decision) before continuing. Surface conflicts with prior decisions before applying. Then `## Finalize`.

**Validate** (or *analyze*). Critique without changing. Load `references/validate.md`.

## Discovery

Order: **Brain dump → Stakes calibration → Working mode → mode-scoped work.** Get to working mode fast — two or three turns, not ten. Users in a hurry must not be held hostage by upstream probing.

**Brain dump.** Always the first move, even when the user opens with paragraphs of context (that is intake, not the dump). Ask for verbal context *and* any existing inputs they want you to read — product brief, research, customer transcripts, competitive analysis, prior PRD draft, design docs. Paths or paste; big docs are fine, you will subagent-extract. A simple "anything else?" surfaces what they almost forgot.

**Research subagents (default).** During Discovery, spawn web-research subagents to ground the picture: what exists in the space, how comparables position themselves, current landscape. Subagent does the search; parent receives a digest.

**Elicitation, not direction.** Discovery pulls the user's vision out; it does not insert yours. Open-ended "tell me about X" beats multiple choice. When you find yourself naming wedges, picking MVP cuts, or proposing phases, stop — you have crossed from elicitation into authoring. Hand the pen back. Infer-and-confirm ("I'm assuming X works like Y — right?") is fine; quizzing the user through a tree of LLM-shaped choices is not.

**Stakes calibration.** One short probe before working mode: hobby / internal / launch — enough to calibrate rigor and section depth. Audience, Existing inputs, and Downstream depth fill in inside the chosen mode, not upstream of the choice.

**Working mode.** Offer the choice in the user's language:

- **Fast path** — I batch remaining gaps into one or two consolidated questions, then draft the full PRD with `[ASSUMPTION]` tags where I inferred. You review and we iterate. The initial quality depends on how much you gave me upfront.
- **Coaching path** — we walk PM-thinking sections together. Once chosen, I ask which entry point fits: **Vision + Features** (capability-first — for enterprise, dev products, internal tools, anyone who thinks in features), **Journey-led** (user-first — for consumer, UX-heavy, multi-stakeholder products; journeys with named protagonists carry persona context inline, no standalone persona section), or *let me suggest* based on what I heard. The chosen entry sets the section order.

The workspace persists; stop and resume freely.

**Concern scan.** As you read what the user gave you, name the concerns this product actually carries — compliance, integration density, operational SLAs, hardware constraints, public-API contracts, monetization, data governance, whatever applies. The list is open; recognize what's there, do not classify into a fixed shape. These concerns drive which template sections to pull in from the Adapt-In Menu and which to invent when no cluster names them.

**Form-factor.** If not stated in sources, probe — mobile / web / desktop / multi-surface / hardware / API.

**User Journeys are captured, not authored.** When UJs are warranted (consumer / multi-stakeholder B2B / meaningful UX — drop or downscale for internal tooling with a single operator role, regulatory-only updates, hobby/solo, pure technical PRDs), prompt the user to narrate a real session with a named protagonist (Mary, mom of three — not "the user") — what the person does, in what order, where it lands — then structure the answer into UJ-N form and confirm. Persona context lives inline at the moments that matter; no standalone persona section.

## PRD Discipline

**Shape.** Features grouped; FRs nested with globally numbered stable IDs. Cross-cutting NFRs in their own section; skip traceability matrices. Capabilities, not implementation — tech choices live in `addendum.md`. Treat `{workflow.prd_template}` as expert prior knowledge, not a checklist. The **Essential Spine** is the expected default — present it unless the product genuinely doesn't need a section, and when you drop one, do so for a reason a reviewer would agree with. The **Adapt-In Menu** is conditional: pull in the clusters the product's concerns need to best define the requirements. When the product carries a concern the menu doesn't name, invent the section — name it well, decide what belongs in it, place it where it serves the reader or the PRD. Reorder and combine for readability. Never include a section because it appears; never skip a concern because no template section covered it. Counter-metrics named when Success Metrics exist.

**Extract, don't ingest.** Source documents go to subagents for extraction; the parent assembles from extracts. Only load source documents into the parent context wholesale when no subagents are available.

**Length scales with stakes.** Hobby / solo PRDs aim for about two pages. Internal tools land around five to eight. Launch and chain-top PRDs run as long as their FRs and concerns require. Whatever the length, detail that doesn't earn its place in the PRD's main narrative belongs in `addendum.md` — moving overflow there is correct; padding the PRD to look thorough is not.

## Reviewer Gate

Used by the Validate intent and at Finalize step 3.

Assemble the menu: rubric walker against `{workflow.validation_checklist_template}` (the PRD quality rubric) + each entry in `{workflow.finalize_reviewers}` + any ad-hoc reviewers the artifact warrants. Stakes-calibrated — hobby/solo may run quietly or skip; higher stakes get the explicit all/subset/skip menu.

Dispatch entries as parallel subagents against the PRD (and `addendum.md` if present) using the standard prefix convention (`skill:` / `file:` / plain text). Each writes its full review to `{doc_workspace}/review-{lens}.md` and returns ONLY a compact summary (verdict, top 2-5 findings, file path) — the parent never holds full review text. The rubric walker uses the prompt and output format in `references/validate.md`. If subagents are unavailable, run sequentially: write the file *before* anything else, then flush the review from working context.

Surface findings tiered, never dumped. Lead with a one-sentence gate verdict, then walk critical + high findings; medium/low roll into a single tail ("plus N more in {file}"). Read the full `review-{lens}.md` only when the user drills into a specific finding. Per finding: autofix, discuss, defer to open items, or ignore.

Under Validate intent, the parent additionally runs the synthesis pipeline in `references/validate.md` — folding every selected reviewer's output into a single HTML + markdown report and opening the HTML.

## Finalize

Tell the user the sequence in one sentence, then walk it. Polish goes last so it does not redo work after reviewer fixes.

1. **Memlog audit.** Walk `.memlog.md` with the user; each entry captured in PRD, in addendum, or set aside.
2. **Input reconciliation.** Subagent per user-supplied input against the PRD + `addendum.md`. Each writes its extract to `{doc_workspace}/reconcile-{input}.md` and returns ONLY a compact summary (input name, gaps 2-5, file path). Surface gaps — especially qualitative ideas (tone, voice, feel) the FR structure silently drops. Must happen before polish.
3. **Reviewer pass.** Run `## Reviewer Gate`. Resolve before polish.
4. **Triage open items.** All Open Questions, `[ASSUMPTION]` tags, `[NOTE FOR PM]` callouts. Phase-blockers (would make the PRD unsafe for UX/architecture/epics) surfaced one at a time and resolved; non-blockers deferred with owner + revisit condition logged via `memlog.py append`. If phase-blocker count is high, flag it.
5. **Polish.** Apply `{workflow.doc_standards}` to the PRD and `addendum.md` in declared order (structural passes before prose — prose should not polish soon-to-be-cut text). Parallelize across documents, sequential within.
6. **External handoffs.** Execute `{workflow.external_handoffs}`; surface returned URLs/IDs. Skip and flag unavailable tools.
7. **Close.** Set the PRD's frontmatter `status: final` and `updated` to `{date}` so future invocations distinguish this PRD from in-progress drafts. Record finalization via `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type event --text "PRD finalized"`. Share artifact paths. Common next: `bmad-ux`, `bmad-architecture`, `bmad-ticket`; invoke the `bmad` skill for authoritative routing.
8. Run `{workflow.on_complete}` if non-empty.
`````

---

## File: skills/bmad-spec/assets/headless-schemas.md

`````markdown
# Headless JSON Response

The default invocation is headless: input goes in, JSON comes out. The contract is intentionally tiny — return the outcome and the files touched. Anything else a caller needs is inside those files (spec-{slug}.md, companions, `.memlog.md`).

## Success

```json
{
  "status": "complete",
  "files": [
    "_bmad-output/initiative-quarter-drop/spec-quarter-drop/spec-quarter-drop.md",
    "_bmad-output/initiative-quarter-drop/spec-quarter-drop/glossary.md",
    "_bmad-output/initiative-quarter-drop/spec-quarter-drop/.memlog.md"
  ]
}
```

`files` lists every file written or modified in this run, in any order. The spec folder, kernel filename, memlog location, capabilities, companions, and verdict are all readable from those files; no need to re-encode them in the response.

## Blocked

```json
{
  "status": "blocked",
  "error_code": "insufficient_intent",
  "reason": "Input was a one-line idea with no surrounding context; too thin to distill. Suggest bmad-prd to draw the vision out first."
}
```

Defined `error_code` values:

- `insufficient_intent` — input too thin to distill into a kernel.
- `missing_slug` — input is sparse or multi-source and no slug was provided by the caller or derivable from a source path.
`````

---

## File: skills/bmad-spec/assets/spec-template.md

`````markdown
---
id: SPEC-{slug}
companions: []     # files downstream MUST read alongside spec-{slug}.md. Paths may point inside the spec folder (spec-authored) or outside it (adopted from an upstream skill).
sources: []        # files fully absorbed into the SPEC (audit only; downstream does NOT read these). Never the memlog.
---

> **Canonical contract.** This SPEC and the files in `companions:` are the complete, preservation-validated contract for what to build, test, and validate. Source documents listed in frontmatter are for traceability — consult them only if you need narrative rationale or prose color this contract intentionally omits.

# {Spec Title}

## Why

{One paragraph naming the force behind this work. A spec can exist for any of:
  - **a pain to solve** — a user or operator is stuck on a specific gap;
  - **an opportunity to capture** — something newly possible we want to claim;
  - **a vision to realize** — a thing we want to make exist because we want it to exist;
  - **a mandate to meet** — a regulation, deprecation, deadline, or contractual obligation.

Name which (or which combination) applies, who is affected, and the backdrop that makes it matter now. This is the anchor every downstream trade-off resolves against.}

## Capabilities

- **CAP-1**
  - **intent:** {One sentence. "User or system can do X to achieve Y." WHAT, not HOW.}
  - **success:** {Testable or demonstrable criterion. Something a test or a real demonstration can decide.}

## Constraints

- {A non-negotiable that bends design. If it doesn't rule anything out, it doesn't belong.}

## Non-goals

- {Explicit out-of-scope item. At least one. Stops downstream from filling the vacuum.}

## Success signal

- {One or two sentences. World-change moment, not dashboard. Concrete enough to write a test or run a demonstration against.}

## Assumptions

<!-- Optional. Omit this section entirely if empty. Inferred calls made without direct confirmation from the input. -->

- {Statement of fact the Spec proceeded under, e.g. "Assumed mobile-first since input mentioned GPS but no platform."}

## Open Questions

<!-- Optional. Omit this section entirely if empty. Gaps the input did not resolve that need a human decision before downstream skills consume the Spec. -->

- {Question phrased so a human can answer it, e.g. "Is offline playback in scope for CAP-2?"}
`````

---

## File: skills/bmad-spec/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
`````

---

## File: skills/bmad-spec/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-spec.
#
# Override files (not edited here):
#   {project-root}/_bmad/custom/bmad-spec.toml        (team)
#   {project-root}/_bmad/custom/bmad-spec.user.toml   (personal)

[workflow]

# --- Configurable below. Overrides merge per BMad structural rules: ---
#   scalars: override wins • arrays: append

# Steps to run before the standard activation (config load, greet).
activation_steps_prepend = []

# Steps to run after greet but before the operation begins.
activation_steps_append = []

# Persistent facts the workflow keeps in mind for the whole run.
# Each entry is either a literal sentence, a skill prefixed with `skill:`,
# or a `file:`-prefixed path/glob whose contents are loaded as facts.
# Empty by default. Repo-wide context belongs in AGENTS.md (see bmad-project-context),
# which every skill already sees; use this for context only this skill needs, loaded on
# demand rather than carried as constant memory. Add a `file:` entry in team/user TOML to
# opt in (e.g. `file:{project-root}/**/project-context.md` if you keep one).
persistent_facts = []

# Executed when the workflow completes. Scalar or array of instructions.
on_complete = ""

# Spec template. The five-field kernel skeleton. Override the path in
# team/user TOML to enforce a different shape (e.g. a hypothesis field
# for research initiatives, or a mechanics field for games).
spec_template = "assets/spec-template.md"

# Output path for spec folders: the active initiative's folder under {output_folder}.
spec_output_path = "{output_folder}/{active_initiative}"

# Run-folder pattern inside spec_output_path. Resolved against the
# input-derived slug at activation. Same slug = same folder, so a
# second invocation updates the existing spec in place (capability
# IDs preserved). Override to add {date} or other components if a
# fresh dated history is preferred.
run_folder_pattern = "spec-{slug}"
`````

---

## File: skills/bmad-spec/SKILL.md

`````markdown
---
name: bmad-spec
description: 'Condense any input — an idea, brief, PRD, transcript, or mixed notes — into a short spec: one kernel file plus supporting files that downstream skills build from. Also updates and validates existing specs. Use when the user says "create a spec", "distill this into a spec", "validate this spec", or "update the spec"'
---

# BMad Spec
## Overview

Canonical transformer for the BMad spec-kernel ecosystem. Takes any intent input — vague idea, brain dump, PRD, GDD, RFC, brief, Slack thread, customer email, meeting transcript, mockups, mixed multi-source — and produces **spec-{slug}.md** carrying the five-field kernel (Why, Capabilities, Constraints, Non-goals, Success signal) plus companion files for load-bearing content that does not fit or would bloat the kernel with expansive line-item detail. Together they are the machine contract every downstream BMad skill consumes.

Multiple skills may call to update the same spec over time.

## Conventions

- Bare paths (e.g. `assets/spec-template.md`) resolve from the skill root.
- `{skill-root}` is this skill's install dir; `{project-root}` is the nearest folder containing `_bmad/`, starting at the working dir and moving up through its parents.
- `{workflow.<name>}` resolves to fields in `customize.toml`.

## On Activation

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} --key workflow`.
   - Script not found: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - Any other failure: read `{skill-root}/customize.toml` directly.
2. Run `{workflow.activation_steps_prepend}`. Treat `{workflow.persistent_facts}` as foundational context (`file:` entries are loaded).
3. Resolve config: `uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root} --key core.output_folder --key core.active_initiative`. `{date}` is the current system datetime.
   - Script not found, or no `output_folder`: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - No `active_initiative`: hand off to the `bmad` skill to set or create one, then run the command again and continue. Headless: write loose.
4. Detect mode. **Headless** when any of: no TTY, programmatic caller (another skill or non-interactive runner), or the first message pre-supplies all inputs and asks for an artifact path back. **Interactive** otherwise. In interactive mode, greet the user and mention that `bmad-party-mode` and `bmad-advanced-elicitation` are available for deeper exploration on any field.

Run `{workflow.activation_steps_append}`.

Activation is complete. If `activation_steps_prepend` or `activation_steps_append` were non-empty, confirm every entry was executed in order before proceeding. Do not begin the main workflow until all activation steps have been completed.

## Workspace

The spec is **always a folder** named `{workflow.spec_output_path}/{workflow.run_folder_pattern}`, resolving by default to `{output_folder}/{active_initiative}/spec-{slug}/`. A spec goes inside what it specifies: when the caller names an epic or initiative folder the spec is for, as `bmad-ticket` does, the spec folder is `<that folder>/{workflow.run_folder_pattern}` instead.

`{slug}` describes the thing being specced, not the input shape:

- Source artifact already carries a slug (e.g., `prd-foo-bar/`): inherit (`foo-bar`).
- Sparse, in-chat, or multi-source input: interactive asks; headless caller provides it as part of the input. If absent and underivable, headless blocks with `error_code: "missing_slug"`.
- Same slug = same folder. A second invocation with the same `{slug}` lands at the existing spec folder and updates in place, preserving capability IDs.

**No input.** Interactive: ask the user to share a file path, paste content, explain the idea in detail, or point to a source. Headless: respond with JSON containing `error_code: "insufficient_intent"`.

Inside the spec folder:

```
<spec-folder>/
  spec-<slug>.md           ← the kernel, named after the folder — DERIVED from .memlog.md, never hand-edited
  <companion-1>.md         ← optional, content-typed (e.g. glossary.md); spec-authored ones are derived too
  <companion-2>.md
  .memlog.md               ← canonical, append-only memory; what spec-{slug}.md is distilled from
```

## Memory and derivation

`.memlog.md` is canonical — an append-only, chronological record of every decision, constraint, capability (with its stable `CAP-N`), assumption, open question, and bit of user direction, one line each in the order it happened, never edited or reordered. `spec-{slug}.md` and every spec-authored companion are **derived on each run** from the memlog (the decision-of-record) plus the sources it cites for raw content — never hand-patched.

Deriving the contract from a living log instead of editing the contract in place is what lets the steps around the spec (PRD, UX, architecture, epics) run in any order and feed the same spec without merge drift: the log only accumulates, the artifact is re-rendered. So the spec is updated *only* by re-deriving it here — bmad-spec is its single writer; a hand-edit to `spec-{slug}.md` from outside is unsupported and is overwritten on the next derive.

Writes go through the shared script — `{project-root}/_bmad/scripts/memlog.py`, the same location as `resolve_customization.py` (atomic; never read it back except to resume):

- `uv run {project-root}/_bmad/scripts/memlog.py init --workspace {spec-folder} --field topic="<what is being specced>"` — once, at create.
- `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {spec-folder} --type <decision|constraint|capability|assumption|question|direction|note|event> --text "<one-line gist, reason included>"` — as each lands.
- Terminal moments (a validation verdict, "spec finalized") are `--type event` entries; the memlog carries no status field.

## The Operation

Read the input and its ancillary linked materials. If there is no input, follow the no-input branch in **Workspace** (ask or block). If a prior `.memlog.md` exists at the target folder, read it — the operation becomes an update, and the memlog (not the rendered `spec-{slug}.md`) is the authority on what was decided and on capability IDs. Preserve those IDs; new capabilities get the next unused `CAP-N`; never reuse retired IDs. Otherwise this is a create, and the first move is `memlog.py init`.

When the input is structured and pre-sorted (a PRD with an addendum, a GDD, a brief produced by an upstream BMad skill), trust the authored separation: lift kernel-fitting content into spec-{slug}.md, lift overflow into appropriately-named companions. When the input is mixed (a brain dump, a transcript, an RFC, a customer email), do the sorting yourself: walk each claim, apply the three-lens load-bearing test (Spec Law rule 7), and route to the kernel field or a companion.

Distill the input into the five-field kernel using `{workflow.spec_template}` as the skeleton. When input is rich, extract directly — no elicitation. When input is sparse, choose: **express** (best-effort distill, every gap becomes an `open_questions[]` entry) or **guided** (walk the five fields with the user one at a time). Headless defaults to express and logs the choice. Interactive asks.

A recognized domain implication the input leaves unaddressed *is* such a gap — name it as an `open_questions[]` entry (healthcare input silent on PHI/HIPAA, payments silent on PCI, control systems silent on fail-safe) and move on. Flag it; never invent the answer or coach toward it. If these dominate, the input is too thin — suggest `bmad-prd`.

Write lean from the first pass: every sentence must earn its place. Decoration costs tokens and dilutes downstream readers.

Log each decision, capability, constraint, and accepted change to `.memlog.md` as it is made — that running record is what the render reads. Because the log is append-only, a later entry supersedes an earlier one on the same point while the history stays intact. When two currently-live sources or companions disagree on the same field, or an either/or never got resolved, surface it to the user rather than silently choosing — the resolution is itself a new memlog entry.

If the input is genuinely too thin to distill (e.g. "an app for hikers" with no surrounding context), stop and suggest `bmad-prd` (or sibling ceremony skill). This skill distills; it does not coach.

## Load-bearing

A claim is **load-bearing** if any consumer (downstream skill, implementing agent, verification pass) would change a decision without it.

## Companions

When load-bearing content does not fit the five-field kernel, it lives in a companion. The kernel cites it; the companion holds it. Companions are part of the contract; every consumer reads `companions:` in spec-{slug}.md frontmatter to discover them. Companions follow the same lean discipline as spec-{slug}.md (Spec Law rule 8).

**Spawn a companion when the content needs more than one kernel-shape line:** multi-item catalogs (per-entity matrices like archetypes, drinks, modes, routes), tables, diagrams (always), editorial voice rules, long-form reference material the kernel cites by name (glossary, brownfield notes, project conventions). Single-line decision-benders stay in Constraints; intent+success pairs stay in Capabilities. If a kernel field is starting to bullet into sub-bullets, the content has outgrown the kernel and wants a companion.

Companions are either:

- **Spec-authored** companions are written by bmad-spec and live as **siblings of spec-{slug}.md** (e.g., `glossary.md`, `patron-archetypes.md`). bmad-spec owns them and may edit them on update operations.
- **Adopted** companions are load-bearing artifacts written by an upstream skill that downstream still needs to read. bmad-spec references them into `companions:` by relative path but does NOT edit them (e.g., a `DESIGN.md` or `EXPERIENCE.md` from a UX run, an integration partner's API spec). The originating skill owns them.

Two rules govern companions:

1. **Name spec-authored companions for the content type they hold.** `glossary.md`, `<entity-class>.md` (e.g. `patron-archetypes.md`, `medication-routes.md`, `flight-modes.md`), `stack.md`, `conventions.md`, `brownfield.md`, `architecture-diagrams.md`, `state-machines.md`, `failure-modes.md`, `compliance-references.md`. The principle: "a reader should know what is inside before opening it." Adopted companions keep whatever name their originating skill gave them.
2. **Diagrams always land in a companion**, regardless of size. spec-{slug}.md kernel holds prose only. Mermaid blocks, ASCII diagrams, and image references all live in a companion (e.g. `architecture-diagrams.md`), with sibling image files referenced from there.

Pre-existing project-wide docs (e.g. `project-context.md`) that downstream needs are listed as **adopted companions**, never duplicated into spec-{slug}.md or a spec-authored companion.

## Spec Law

Every spec must satisfy these eight rules. The operation aims for them; the self-validate sweep enforces them.

1. **Each capability has both `intent` and `success`.** Missing either = not a capability.
2. **Intents describe WHAT, not HOW.** Implementation prescription belongs in a companion (stack, conventions).
3. **Constraints actually bend design decisions.** A "constraint" that rules nothing out is decoration.
4. **Non-goals are explicit.** At least one. Absence means downstream skills fill the vacuum.
5. **Success signal is concrete enough to test or demonstrate against.** "Users love it" doesn't qualify.
6. **Capability IDs are stable and unique.** Never reused, never renumbered.
7. **Preservation.** Every load-bearing source claim lands in spec-{slug}.md or a companion. Wrapper ceremony does not.
8. **Lean prose.** Every sentence carries load-bearing content. Cut decoration, hedges, backstory, throat-clearing. Applies to spec-{slug}.md, companions, and `.memlog.md`.

## Self-Validate

After every create or update, sweep the resulting artifact in **two passes** before presenting.

**Pass 1 — Coherence.** Judge the spec against Spec Law rules 1–6 and 8. For anything that fails or feels weak, attempt to fix it without inventing content the input did not support. Calls made without direct confirmation become `assumptions[]`; gaps that could not be filled become `open_questions[]`.

**Pass 2 — Preservation.** Walk the source claim by claim. Confirm each load-bearing claim landed in spec-{slug}.md or a companion. Wrapper-ceremony drops are logged under "Wrapper-only content" so the drop is on the record, not silent.

Record the verdict for each pass to `.memlog.md` (`append --type event`). In interactive mode, review it with the user. In headless mode, `.memlog.md` is one of the files returned, so the caller (or its downstream LLM) reads the verdict there.

## Spec with no change signal

When the user points the skill at an existing spec folder (or its spec-{slug}.md) with no change signal, offer to review assumptions or open questions, or determine what they want to do.

## Handing off to `bmad-ticket` (optional, interactive-only)

Requires `spec-{slug}.md` on disk — run the normal Operation first if it doesn't exist yet. Headless runs never do this, even when the invocation text asks for it: if mode detection (On Activation, step 4) resolved headless, skip this section entirely and proceed with the normal headless response. In interactive mode, offer the handoff at most once per run when the input reads as multiple independently shippable slices; a decline ends the offer for this run, not forever.

Hand the spec folder to `bmad-ticket` as the requirement source: it plans the work with the user as an epic whose `tickets.toml` entries cite this spec's `CAP-N` ids, and it runs the board from there. Load-bearing detail the slicing conversation surfaces (a constraint, a design decision) comes back here as a spec update, never into a ticket alone.

When a spec update runs, search the ticket root for this spec folder's path in References; where tickets cite it, name the entries and tickets whose description no longer matches and offer to re-slice them with `bmad-ticket`. The update itself never edits a ticket.

## Output

**Interactive** — share the spec folder path conversationally. Name the capability count, the companions produced, and the verdict in one or two sentences. If `assumptions[]` or `open_questions[]` are non-empty, list them (short — one line each) and invite the user to walk through them. Make clear that addressing them can update the source input (if it was a file), the spec, or both — whichever combination the user prefers. Do not dump JSON or present a wall of output.

**Headless** — return JSON per `assets/headless-schemas.md`.

Run `{workflow.on_complete}` if set.

## After Spec is Output

Any update to the spec — resolved assumptions, answered open questions, other changes — is appended to `.memlog.md` as it happens. When a change overrides something that came from a source input, offer to update that source too, so upstream and the spec don't silently diverge.

## Frontmatter conventions

- `companions:` array of `.md` files downstream MUST read alongside spec-{slug}.md to have the full contract. Paths may point inside the spec folder (spec-authored companions like `glossary.md`) or outside it (adopted companions like `../ux-foo-bar/DESIGN.md`). The split between spec-authored and adopted is implicit by path; downstream treats both the same.
- `sources:` array of paths to files that were **fully absorbed** into the SPEC, with no remaining downstream value (e.g., a PRD whose every load-bearing claim is now in the kernel). Listed for audit and for bmad-spec to re-read on update. Downstream does NOT read these. Files that downstream still needs to read belong in `companions:`, not here.
- **Do not list** the memlog, README files, organizational artifacts, or any operational record of how upstream skills produced their artifacts. Those are not source content; they are process metadata that downstream consumers don't need.
`````

---

## File: skills/bmad-ticket/assets/bug-template.md

`````markdown
---
id: [the entry's id in tickets.toml; the next unused one for a ticket with no entry]   # tracker_id and remote are written at publish on a tracker
type: bug
title: "[What is wrong, from the user's view]"
parent: [folder name of the epic, or of the initiative when there are no epics; none for a standalone bug in backlog/]
covers: []
after: []   # prerequisites only: a sibling's id; "<epic id>.<entry id>" in another epic; epic-<slug> for that whole epic
assignee: ""   # a tracker's assignee, mirrored by query; otherwise assignee, blocked_at, and blocked_reason live in the plan
# status lives in the plan beside this file, not here; tracker_status by a tracker sync
refined: false   # true once the user approves its criteria
hitl: false
risk: [low|medium|high]
severity: [P0|P1|P2|P3]
estimate: ""   # points, when estimation is on
---

<!-- The refined shape. A done or dropped ticket stays as it was written. -->

# [Title]

## Description

[What the user sees go wrong and what should happen instead, from their side. 2–4 sentences.]

## Reproduction

[Exact steps, environment, and what happens versus what should happen. Logs or errors verbatim.]

## Cause Hypothesis

[Where the defect likely lives and why — a hypothesis, never a prescribed fix.]

## Acceptance Criteria

1. **[The expected behavior holds]**
   **Given** [the state from the reproduction]
   **When** [the reproduction steps are followed]
   **Then** [what should happen, as it should now happen]
2. **Tests cover the condition found and fixed**
   **Given** the test suite
   **When** it runs
   **Then** a test that fails on the defect and passes on the fix covers the reproduction, and one covers each related case the fix touched
3. **Or: no change is needed, with proof**
   **Given** the reproduction
   **When** it is run on the current code
   **Then** the expected behavior already holds, or the report was mistaken, with the evidence recorded in Notes — this supersedes 1 and 2

## References

- parent — [path or url, or none]
- [source — path, section]
- [logs, screenshots, or sample data — location]

## Notes

[Only what is not in the repo or the source, and what is not settled. Cut if empty.]

- Decision: [a choice the user made, dated]
- Assumption: [a choice made while drafting that the user has not confirmed]
- Open question: [what is not settled; answering it is part of the ticket's work]

<!-- Example, not part of the ticket: match its level of detail. What is good here: a reproduction someone else can follow, with the actual and expected values; a cause hypothesis that is not a fix; one criterion for the behavior, one for the tests that cover the condition found and fixed, and one that supersedes both when the reproduction shows no change is needed. -->

```markdown
---
id: 7
type: bug
title: "Checkout total ignores an applied discount code after the shopper changes quantity"
parent: none
covers: []
refined: true
hitl: false
risk: medium
severity: P2
---

# Checkout total ignores an applied discount code after the shopper changes quantity

## Description

A shopper applies a valid code, then changes an item's quantity, and the total goes back to full price while the discount line still shows the code. They are charged full price at payment.

## Reproduction

1. Production, Chrome 130, logged out. Add "Canvas tote" (29.00) ×1 to the cart.
2. Apply SAVE10. Total shows 26.10, discount line shows SAVE10 −2.90.
3. Change quantity to 2.
4. Actual: total 58.00, discount line still SAVE10 −2.90. Expected: 52.20, discount line SAVE10 −5.80.
5. Continue to payment: amount charged is 58.00.
Log line at step 3: `pricing.recompute cart=… discount=null`.

## Cause Hypothesis

The quantity-change path recomputes the total without passing the applied code, so the discount is dropped while the UI keeps the stale line. Likely in the recompute call, not in the discount engine, since step 2 is right.

## Acceptance Criteria

1. **The expected behavior holds**
   **Given** a cart with a valid code applied
   **When** the shopper changes an item's quantity
   **Then** the total recomputes with the code still applied and the discount line shows the new amount
2. **Tests cover the condition found and fixed**
   **Given** the pricing test suite
   **When** it runs
   **Then** a test that fails on the defect and passes on the fix covers a quantity change with a code applied, and one covers removing an item with a code applied
3. **Or: no change is needed, with proof**
   **Given** the reproduction
   **When** it is run on the current code
   **Then** the total already recomputes with the code applied, or the report was mistaken, with the evidence recorded in Notes — this supersedes 1 and 2

## References

- parent — none
- logs — support ticket #4471, attachment pricing.log
```
`````

---

## File: skills/bmad-ticket/assets/epic-template.md

`````markdown
---
tracker_id: ""   # with remote (the url), written at publish on a tracker; cut on the repo store
key: ""   # tracker project or team for everything under this, when it differs from the store's
type: epic
title: "[The outcome this container exists to reach]"
parent: [folder name of the initiative]
covers: [parent capability ids this epic owns; keep these when adding an epic-local spec; a constraint is cited in References, not covered]
after: []   # epics this whole container waits on; the order of epics is in the initiative's tickets.toml
assignee: ""   # status is added when work starts (in-progress | done | dropped)
risk: [low|medium|high — the highest expected among its children]
estimate: ""   # t-shirt, when estimation is on
estimate_basis: ""   # envelope | spec | entries | stories
---

<!-- At initiative slicing, the envelope: frontmatter, Description, Outcome, Done when, Boundaries, References, and known Notes. Requirements are completed at this epic's inception, and its children are planned in `tickets.toml` beside this file. -->

# [Title]

## Description

[What is true when this is done and why it matters. With a spec, one paragraph pointing at it; without one, the intent in full.]

## Outcome

[One sentence: for whom, what changes, and the signal that shows it worked — the spec's, named not restated, when there is one.]

## Requirements

[The requirement source at this altitude: the source's lines as stable ids for children to cite, each mapping to a parent id in covers. Reuse the parent's ids when the lines are the parent's; when the epic splits or adds one, mint `E<n> (R<m>)` so the map is on the line. An epic with no parent ids, such as the platform baseline, leaves covers empty and cites the source section on each line instead. A referenced numbered source replaces it; a separate spec only when the source outgrows this section.]

## Done when

[Three to six checks a person can run without opening a child — the measures, limits, and behaviors from the source. Each fails today. Closing every child is not one.]

## Boundaries

[Which boundary this container follows — team, service, UI, capability — and what it is not. Point at the spec's non-goals when there is one.]

## References

[`type — location, section`. The spec at this level when one exists (its references are followed from there); otherwise what this was split from.]

- parent — [path or url, section containing the upstream requirement ids]
- spec — [local spec when present; its capability ids map back to parent ids]
- constraint — [the source section that binds this container: privacy, platform, licensing, performance]
- [an input the spec does not carry — location, section]

## Notes

[What is not settled at this level. Cut if empty.]

- Assumption: [a choice made while slicing that the user has not confirmed]
- Open question: [what the source does not settle and which children wait on it]
- Unknown: [what is not yet known and which entries wait on it, recorded now so it is not found late]
- Parked: [a requirement id not placed on any child, and why, with the user's knowledge]
- Decision: [a choice the user made, dated, so it is not asked again — a declined suggestion belongs here too]
- Source conflict: [id or section — what the source says vs what the code or another source shows]
- Waits on [epic] because: [the one-line reason for each entry in after]

<!-- Example, not part of the ticket: an incepted epic with no spec of its own. The parent assigned R1–R4 to this epic from an unnumbered PRD; Requirements records those lines using the same ids. Done when holds deliverable checks. Notes holds decisions and unknowns. -->

```markdown
---
type: epic
title: "Shoppers manage their cart"
parent: initiative-checkout
covers: [R1, R2, R3, R4]
assignee: ""
risk: medium
---

# Shoppers manage their cart

## Description

A shopper adds items, changes quantities, applies discount codes, and recovers from every refusal with a reason — and the total they see is always the one the payment step receives.

## Outcome

Shoppers who reach the cart continue to payment more often because the total never surprises them; the PRD's cart-to-payment completion measure is the signal.

## Requirements

- R1: Add, remove, and change quantity of any item; the total updates at once. (PRD, Capabilities, Cart)
- R2: Apply one discount code; refused codes say why. (PRD, Capabilities, Cart)
- R3: The total shown is the total charged, before tax. (PRD, Capabilities, Cart)
- R4: Cart actions respond within 300 ms on the slowest supported device. (PRD, Constraints)

## Done when

1. R1–R3 work end to end on the live site, refusals included, with the reason shown.
2. The end-to-end suite proves the shown total equals the amount sent to payment on every cart path.
3. R4 holds on the slowest supported device under the load test.
4. A shopper with no code applied sees no change from today's cart.

## Boundaries

The cart UI. Not the pricing service (epic Pricing rules), not tax (epic Tax and payment).

## References

- prd — _bmad-output/initiative-checkout/prd-checkout/prd-checkout.md, sections Capabilities and Constraints
- design — https://figma.com/design/ab12cd/checkout, frame Cart

## Notes

- Decision: codes are case-insensitive (user's decision, 2026-08-12).
- Unknown: whether the discount engine can validate a code within R4; entry 04 waits on it.
```
`````

---

## File: skills/bmad-ticket/assets/initiative-template.md

`````markdown
---
tracker_id: ""   # with remote (the url), written at publish on a tracker; cut on the repo store
key: ""   # tracker project or team for everything under this, when it differs from the store's
type: initiative
title: "[The outcome this container exists to reach]"
parent: none
covers: [capability ids from the spec at this level, or from Requirements below when the source has none; a constraint is cited in References, not covered]
after: []   # epics this whole container waits on; the order of epics is in the initiative's tickets.toml
assignee: ""   # status is added when work starts (in-progress | done | dropped)
risk: [low|medium|high — the highest expected among its children]
estimate: ""   # t-shirt, when estimation is on
estimate_basis: ""   # envelope | spec | entries | stories
---

# [Title]

## Description

[What is true when this is done and why it matters. With a spec, one paragraph pointing at it; without one, the intent in full.]

## Outcome

[One sentence: for whom, what changes, and the signal that shows it worked — the spec's, named not restated, when there is one.]

## Requirements

[The requirement source at this altitude: the source's lines as stable ids for children to cite, each mapping to a source id in covers. A referenced numbered source replaces it; a separate spec only when the source outgrows this section.]

## Done when

[Three to six checks a person can run without opening a child — the measures, limits, and behaviors from the source. Each fails today. Closing every child is not one.]

## Boundaries

[Which boundary this container follows — team, service, UI, capability — and what it is not. Point at the spec's non-goals when there is one. Then one line per unit the work touches that gets no epic, and the tracer path across epics in one sentence.]

- Touch point: [unit] — [what is consumed or configured there]; owner: epic-[slug]

## References

[`type — location, section`. The spec at this level when one exists (its references are followed from there); otherwise what this was split from.]

- spec — [path from {project-root}, or url for a remote source; section Capabilities]
- constraint — [the source section that binds this container: privacy, platform, licensing, performance]
- [an input the spec does not carry — location, section]

## Notes

[What is not settled at this level. Cut if empty.]

- Assumption: [a choice made while slicing that the user has not confirmed]
- Open question: [what the source does not settle and which children wait on it]
- Unknown: [what is not yet known and which entries wait on it, recorded now so it is not found late]
- Parked: [a requirement id not placed on any child, and why, with the user's knowledge]
- Decision: [a choice the user made, dated, so it is not asked again — a declined suggestion belongs here too]
- Source conflict: [id or section — what the source says vs what the code or another source shows]
- Waits on [epic] because: [the one-line reason for each entry in after]

<!-- An initiative is the business outcome its epics serve, tied to a company goal, usually spanning quarters and more than one boundary. For a solo developer or small team with no larger goal above it, a whole product is a fine initiative.
Example, not part of the ticket: match its level of detail. This one keeps a separate spec because its source outgrew the section: Description points at it, covers cites its ids, Requirements is cut, Outcome names its signal. Done when reads as business outcomes a product owner checks at the end, not deliverables. The epic order, with what each needs from the one before, is in `tickets.toml` beside this file. -->

```markdown
---
type: initiative
title: "Checkout that shoppers finish"
parent: none
covers: [C1, C2, C3, C4, C5, C6, P1, P2, T1]
assignee: ""
risk: high
---

# Checkout that shoppers finish

## Description

Shoppers go from cart to paid order in one pass, with the price they saw, on any supported device or as a guest. The spec owns the capabilities, constraints, and non-goals; this initiative delivers them for the fall release.

## Outcome

The Q4 revenue target depends on lifting cart-to-payment completion from 55% to 70%; this initiative owns that number, the spec's success signal, measured over a full quarter.

## Done when

1. Cart-to-payment completion holds at the spec's target for one full quarter after release.
2. Every capability in C1–C6, P1–P2, and T1 is live for all shoppers, not behind a flag.
3. Refund and chargeback rates are no worse than the quarter before release.
4. No P0 or P1 checkout bug open for more than a day during the first month.

## Boundaries

The web store's checkout flow, the payment-provider integration, and guest checkout across web and mobile. Not the order-management backend beyond the order it creates, not loyalty, not the catalog; see the spec's non-goals. Tracer path: one catalog item priced, carted, taxed, and paid by a signed-in shopper.

- Touch point: notification service — a new order-confirmation template, no code change; owner: epic-tax-and-payment

## References

- spec — _bmad-output/initiative-checkout/spec-checkout/spec-checkout.md
- constraint — the same spec, section Constraints, PCI scope and response time
- prd — _bmad-output/initiative-checkout/prd-checkout/prd-checkout.md, for history only

## Notes

- Parked: gift cards (C7); not in the fall release, user's call 2026-08-12.
- Decision: one payment provider for v1, replacing the current one (user's decision, 2026-08-12).
- Unknown: whether the tax service can meet the response-time constraint at peak; epic Tax and payment owns the answer.
```
`````

---

## File: skills/bmad-ticket/assets/spike-template.md

`````markdown
---
id: [the entry's id in tickets.toml; the next unused one for a ticket with no entry]   # tracker_id and remote are written at publish on a tracker
type: spike
title: "[The question this answers]"
parent: [folder name of the epic, or of the initiative when there are no epics; none for a standalone ticket in backlog/]
covers: []
after: []   # prerequisites only: a sibling's id; "<epic id>.<entry id>" in another epic; epic-<slug> for that whole epic
assignee: ""   # a tracker's assignee, mirrored by query; otherwise assignee, blocked_at, and blocked_reason live in the plan
# status lives in the plan beside this file, not here; tracker_status by a tracker sync
refined: false   # true once the user approves its full criteria; a pulled spike has this line only when its entry says `refine = true`
hitl: true
risk: [low|medium|high]
estimate: ""   # points, when estimation is on
---

<!-- The refined shape. A done or dropped ticket stays as it was written. -->

# [Title]

## Description

[One sentence, the question; reviewed with the user at refine. Approach below says how it is answered and which tickets wait on it.]

## Approach

[The steps, as many as the question needs: what to research, what to design, what to prototype, who signs off. The time box. What a good-enough answer looks like.]

## Acceptance Criteria

1. **[The candidates are compared on the criteria that decide]**
   Verify: [where the comparison is recorded]
2. **[The prototype shows the chosen answer works for the cases that matter]**
   Verify: [where the prototype and its results live]
3. **[The answer is recorded, with its consequences, where the waiting tickets can find it]**
   Verify: [where]
4. **[The people who must accept it have]**
   Verify: [who, recorded where]

## References

- parent — [path or url]
- [source — path, section]

## Notes

[Assumptions and open questions about the spike itself, each marked. Cut if empty.]

<!-- Example, not part of the ticket: match its level of detail. What is good here: the unknown was a placeholder in the architecture; the spike names who waits, runs research, design, prototype, and sign-off in a time box, says what good enough is, and records the answer where the stories will find it. hitl because people accept the finding. -->

```markdown
---
id: 2
type: spike
title: "How do field devices merge conflicting observations after days offline?"
parent: epic-field-sync
covers: []
refined: true
hitl: true
risk: low
---

# How do field devices merge conflicting observations after days offline?

## Description

The architecture left sync as a placeholder: "devices reconcile on reconnect." Researchers in the field edit the same observation records on several tablets for days without a link, then sync over a slow satellite connection. Three planned stories in this epic — edit observations offline, sync on reconnect, show merge results to the team lead — cannot be refined for implementation until we know how conflicting edits merge, what that does to the data model, and whether the link can carry it.

## Approach

Time box: four days. At the end, the best answer so far is recorded, not a perfect one.

1. Research: compare three ways to merge — a CRDT library, last-writer-wins per field with a conflict log, and a custom merge on the observation schema — on the criteria that decide: correctness on the six conflict cases the field team listed, payload size over a 2 kbps link, library maturity, and how much of the data model each forces us to change.
2. Design: a one-page note on the chosen approach, its data-model consequences, and the cases it does not resolve automatically.
3. Prototype: two tablets and a simulated link; run the six conflict cases and measure sync payload and time.
4. Sign-off: the lead engineer accepts the design; the field science lead accepts how unresolved conflicts are shown to people.

Good enough: all six cases merge as the field team expects, or the exceptions are listed with how a person resolves them, and a full day's edits sync in under ten minutes on the simulated link.

## Acceptance Criteria

1. **The three approaches are compared on the deciding criteria**
   Verify: the comparison table is in epic-field-sync/spike-04-offline-merge-comparison.md
2. **The prototype merges the six conflict cases on a simulated link**
   Verify: the prototype and its run log are under epic-field-sync/spike-04-offline-merge-prototype/, with payload and time per case
3. **The answer and its data-model consequences are recorded where the stories will find them**
   Verify: the design note is epic-field-sync/spike-04-offline-merge-design.md and the choice is in epic-field-sync.md Notes as a decision
4. **The lead engineer and the field science lead have accepted it**
   Verify: both named in that decision line with the date

## References

- parent — epic-field-sync/epic-field-sync.md
- architecture — docs/architecture.md, section Sync (the placeholder)
- field team conflict cases — docs/research/conflict-cases.md

## Notes

- Assumption: the satellite link is never better than 2 kbps up; confirm with the operations lead.
- Open question: is the conflict log kept forever, or pruned after the lead resolves it? The field science lead answers.
```
`````

---

## File: skills/bmad-ticket/assets/story-template.md

`````markdown
---
id: [the entry's id in tickets.toml; the next unused one for a ticket with no entry]   # tracker_id and remote are written at publish on a tracker
type: story
title: "[What exists or works when this is done]"
parent: [folder name of the epic, or of the initiative when there are no epics; none for a standalone ticket in backlog/]
covers: [ids from the epic's spec, referenced numbered source, or Requirements; the ones this ticket delivers toward]
after: []   # prerequisites only: a sibling's id; "<epic id>.<entry id>" in another epic; epic-<slug> for that whole epic
assignee: ""   # a tracker's assignee, mirrored by query; otherwise assignee, blocked_at, and blocked_reason live in the plan
# status lives in the plan beside this file, not here; tracker_status by a tracker sync
refined: false   # true once the user approves its full criteria; a pulled story has this line only when its entry says `refine = true`
hitl: false
risk: [low|medium|high]
estimate: ""   # points, when estimation is on
---

<!-- Pulled: a one-sentence Description, Acceptance Criteria as one Verify: line, References, local Notes. Refined: the same, reviewed with the user. Numbered criteria and Boundaries only for a ticket with no epic, an entry with `refine = true`, or on request. A done or dropped ticket stays as it was written. -->

# [Title]

## Description

[One sentence, what this delivers; reviewed with the user at refine. With numbered criteria: what exists or works when this is done and how it advances the epic, 2–4 sentences; the criteria carry the proof, do not restate them.]

## Acceptance Criteria

[One line, `Verify: how it will be checked`, nothing else. No epic, `refine = true`, or on request: the numbered criteria below instead.]

1. **[Short name of the behavior]**
   **Given** [the state before: data, user, config]
   **When** [the one action]
   **Then** [what is observed: a response, a record, a screen — something a person or a test can check]
   **And** [a further observation from the same action, when there is one]
2. **[Short name of the behavior]**
   **Given** [...]
   **When** [...]
   **Then** [...]

## Boundaries

- Must not change: [adjacent behavior a valid implementation could damage — name behavior, not files]

## References

- parent — [path or url]
- [source document — path, section]
- [design or prototype — path, for UI work]

## Notes

[Only what is not in the repo or the source, and what is not settled. Cut if empty.]

- [A frozen interface, a declined option]
- Decision: [a choice the user made, dated]
- Assumption: [a choice made while drafting that the user has not confirmed; confirmed, it becomes a Decision line]
- Open question: [what only this ticket waits on. Touches siblings: the parent's Notes. Gates work: a spike.]

<!-- Example, not part of the ticket: match its level of detail. What is good here: the Description is what the shopper can do, end to end; every criterion states a rule, not an instance, with its failure path, fails today and passes only through this work; Boundaries names behavior, not files; References points at the nearest document; Notes holds only what is not in the repo or the source, plus one assumption for the user to confirm. Numbered criteria because its entry says `refine = true`. -->

```markdown
---
id: 4
type: story
title: "A shopper applies a discount code and sees the new total"
parent: epic-cart-rules
covers: [R2, R3]
after: [3]
refined: true
hitl: false
risk: medium
---

# A shopper applies a discount code and sees the new total

## Description

A shopper with items in the cart enters a discount code, and the cart total updates to show the discount before tax. An invalid or expired code tells them why it was refused and leaves the total as it was.

## Acceptance Criteria

1. **Valid code reduces the total**
   **Given** a cart and a valid discount code
   **When** the shopper applies it
   **Then** the total drops by the code's value and a discount line shows the code and the amount
2. **Expired code is refused with the reason**
   **Given** a cart and an expired code
   **When** the shopper applies it
   **Then** the total is unchanged and the message says the code expired and when
3. **Unknown code is refused without revealing valid codes**
   **Given** a cart and a code that does not exist
   **When** the shopper applies it
   **Then** the total is unchanged and the message says the code is not recognized, nothing more
4. **An applied code survives a quantity change**
   **Given** an applied code
   **When** the shopper changes an item's quantity
   **Then** the total recomputes with the code still applied
5. **Payment receives the discounted total**
   **Given** an applied code
   **When** the shopper continues to payment
   **Then** the amount sent to payment equals the total shown

## Boundaries

- Must not change: catalog prices, tax computation, the cart for shoppers who apply no code.

## References

- parent — _bmad-output/initiative-checkout/epic-cart-rules/epic-cart-rules.md, Requirements R2 and R3
- design — https://figma.com/design/ab12cd/checkout, frame Cart

## Notes

- Decision: the discount engine's `validate(code, cart) -> {amount, reason}` interface is frozen (2026-08-12).
- Assumption: the refusal messages above are final copy; no design text exists for them.
```
`````

---

## File: skills/bmad-ticket/assets/tickets-template.toml

`````toml
# tickets.toml — a container's agreed children. tickets.py reads it.
# The order of the tables is the build order: move a table to reorder. `id` names a child for good and is never reused; it is not an order.
# An epic's file holds [[entry]] tables. An initiative's holds [[epic]] tables; with no epics it holds [[entry]] tables.
# Keys tickets.py does not know are ignored, so a team may add its own.
# Status never lives here: an entry with no plan and no leaf file is planned; its plan beside it carries `status`, and on a tracker `query` writes `tracker_status` in the leaf file.

# ---- In an epic folder: one table per planned story, spike, or bug ----

[[entry]]
id = 1                      # the leaf file carries it as `id:`; refer to it as 1 here, as <epic id>.1 from another epic
type = "story"              # story | spike | bug
title = "Cart service scaffold"
description = "Stands up the cart service with one add-item path through UI, API, and store."
verify = "A shopper adds one item on the deployed site and sees it in the cart."
covers = ["R1"]             # ids from the epic's requirement source
after = []                  # real prerequisites only, never the sequence; empty means it can run beside anything above it. A bare number is a sibling's id; everything else is quoted
hitl = true                 # a person runs the starter
risk = "low"                # low | medium | high
# estimate = 2              # points, when estimation is on
# unknown = "..."           # known uncertainty, one sentence
# references = ["architecture-checkout/architecture-checkout.md#ad-8"]   # what this entry needs beyond the epic's own References; pull writes them to the ticket
# notes = ["..."]           # the user's words only, never filled by the agent; pull writes them to the ticket's Notes
# refine = true            # refine to full acceptance criteria before it starts
# plan_checkpoint = true    # a person reviews the ticket, or the builder's plan, before work starts
# done_checkpoint = true    # work pauses after this ticket until a person says continue

[[entry]]
id = 2
type = "story"
title = "Apply and refuse discount codes"
description = "Adds code validation to the pricing call and shows the refusal reason."
verify = "A valid code lowers the total; an expired one is refused with its reason."
covers = ["R2", "R3"]
after = [1, "1.3"]          # a sibling's id; "<epic id>.<entry id>" for an entry in another epic; "epic-<slug>" for that whole epic
hitl = false
risk = "medium"

# ---- In an initiative folder: one table per epic; table order is the build order ----

[[epic]]
id = 1                                      # entries in other epics refer to this epic's entries as 1.<entry id>
slug = "epic-pricing-rules"                 # the epic's folder name
title = "Pricing rules"
covers = ["P1", "P2"]

[[epic]]
id = 2
slug = "epic-cart-rules"
title = "Shoppers manage their cart"
covers = ["C1", "C2", "C3"]
after = [{ epic = 1, needs = "the one-function pricing contract" }]   # what this epic needs from which; the entries that wait on it carry the `after`
`````

---

## File: skills/bmad-ticket/config/gh-ticketing.toml

`````toml
# Ticketing config: GitHub Issues. Read by every skill that creates, reads, or moves tickets.
# A starter: at first use the skill copies the chosen one to _bmad/custom/ticketing-store-config.toml
# (references/store-setup.md), and that copy is the one edited. An org can instead host its own starter
# anywhere — a repo path or a url — and setup copies from it. Skills read
# [tickets] and [verbs] through a script, so comments are never loaded.
# Every ticketing config has the same fields; only the prose and the maps differ. `description` sits
# outside [tickets] so it is never loaded; it is for choosing a config.
# One tool: the gh CLI, 2.94 or later (sub-issues and dependencies). Labels carry type and status,
# sub-issues carry hierarchy, blocked-by relations carry blocking, milestones put initiatives on the roadmap.
# Using the defaults below: the repo needs the type, status, and hitl labels; `setup` creates them.
#
# The active initiative is `core.active_initiative` in _bmad/custom/config.user.toml, read at activation. The ticket tree under `output_folder`
# has the same layout for every store (layout in references/board.md, Layout); write pushes it to this tracker.
# An empty field below that an operation needs, and that cannot be inferred from what the user gave:
# use what they tell you for this run and offer to record it in this file (references/store-setup.md).

description = "GitHub Issues through the gh CLI: labels, sub-issues, blocked-by, milestones. For teams whose code and planning already live on GitHub."

[tickets]
store = "github"
key = ""    # ids are the repo's issue numbers; leave empty
repo = ""   # OWNER/REPO; empty = the repo of the current checkout (origin)
access = """
Use the gh CLI against `repo`, passing `-R <repo>` on every call when it is set. If a call fails
on auth or a missing label, tell the user what is blocking, confirm the setup with them, and ask them to authenticate when that is the cause.
"""

fields = """
risk is a label risk:<value>; severity is a label P0-P3. `setup` creates them with the other
labels; publish and update set them like any label. estimate is a label est:<value> (`setup` creates none; add on first use). Empty field: no label.
"""

reference = """
A reference is a line in the ticket's References section: `type — location, section`, location a
path from {project-root} or a url for a remote source, opened before it is cited. GitHub renders urls as links; nothing more to do.
"""

# Verb prose lives in its own table so a skill can load one verb at a time.
[verbs]
setup = """
Connect: `gh auth login`.
Labels must exist before they are used. List the repo's labels (`gh label list --limit 1000 --json name`)
and run `gh label create <name>` for each of these it lacks, comparing names ignoring case: the
[tickets.types] values except initiative (a milestone), the [tickets.status] values except done and
dropped (closed states), `hitl`, risk:low, risk:medium, risk:high, and P0-P3. A create that fails with
"already exists" found the label after all: leave it. Never `--force`: it recolours a label the repo
already has. Offer this at first publish, or whenever a label is missing.
"""

write = """
One issue per new ticket, created in tree and prerequisite order so every relation has a number to point at.
Create with content only: `gh issue create --title <t> --body-file <b> --label <type>,<[tickets.status].<state>>[,hitl,risk:<r>,P<n>]
[--milestone <initiative>]`, and write tracker_id and remote from the url it prints before anything else. Then add the
relations with `gh issue edit <n> --parent <n> --add-blocked-by <n,n>`, leaving out any that `gh issue view <n> --json
parent,blockedBy` already shows, since adding one twice fails. Keep --parent and --blocked-by off the create: gh adds them
after the issue exists, and when that step fails it exits non-zero without printing the url, so a rerun files the ticket
twice. A create that fails with no url may still have made the issue: before creating again, list the newest issues
(`gh issue list --state all --limit 100 --json number,title,body,createdAt`) twice, a few seconds apart since the list can
trail a create, and take the one whose title and body are what was sent. None matches and the list does not reach back to
the failed create: ask before creating again. Changes go through `gh issue edit <n>` (--body-file, --add-assignee,
--add-blocked-by, label swaps; --add-sub-issue on the parent). No parent: omit --parent; no initiative: omit --milestone.
Needs gh 2.94+; check `--help` when a flag is doubted.
Status is [tickets.status].<state>, the ticket's state (`state` in `tickets.py` rows), as a label swap on an open issue. done: remove the status label and `gh issue close <n>`;
dropped: the same with `--reason "not planned"`. An initiative is a
milestone (`gh api repos/{owner}/{repo}/milestones -f title=<name>`) holding its tree's issues.
The body sent is the whole local file, without the frontmatter lines tracker_id,
remote, tracker_status, status, assignee.
After a write, mirror tracker_id (the number), remote (the url), tracker_status, assignee, and after in the
local file — never status, which is the build's; commit local files on the current branch when `output_folder` is in a git repo, never push.
An `after` naming a sibling's id or `<epic id>.<entry id>` is that entry's item, published first when it is still local; `epic-<slug>` is the epic's item.
"""

query = """
One issue: `gh issue view <n> --json
number,title,labels,assignees,state,stateReason,milestone,parent,blockedBy,subIssuesSummary,body`.
Type, hitl, risk, severity, and status are labels — except done and dropped, the closed states per
[tickets.status]; the body's covers holds the source requirement ids. Children: `gh issue view <n>
--json subIssues`. Words: `gh issue list --search "<words>" --state all`, narrowed by
--label. The local file: `tickets.py find <folder> <n>`.
A child the tracker holds with no file in the tree gets an `[[entry]]` with the next unused `id` in its parent's `tickets.toml`, then its file.
Mirror the issue's status label or closed state into the local file as tracker_status, the BMad word per [tickets.status], beside tracker_id and remote — never into status, which is the build's.
Candidates to start (one that `tickets.py next` lists under `ready_to_refine` gets its criteria first): open (in the initiative's milestone, or with no milestone for a standalone ticket), tracker_status backlog (the backlog label and no other status label), every
blockedBy closed as completed, no assignee, no blocked_at. Offer all ready tickets; when one must be chosen, the
one that unblocks the most work behind it.
"""

# BMad type -> this store's type.
[tickets.types]
initiative = "milestone"   # an initiative is a milestone, not an issue; its epics carry it
epic = "epic"
story = "story"
spike = "spike"
bug = "bug"

# BMad state -> this store's status; `query` reads it back the other way into tracker_status. The file's `status` is the build's and is never sent.
[tickets.status]
backlog = "backlog"
in-progress = "in-progress"
review = "review"
done = "closed"   # state, not a label
dropped = "closed: not planned"   # state, not a label
`````

---

## File: skills/bmad-ticket/config/jira-ticketing.toml

`````toml
# Ticketing config: Jira Cloud. Read by every skill that creates, reads, or moves tickets.
# A starter: at first use the skill copies the chosen one to _bmad/custom/ticketing-store-config.toml
# (references/store-setup.md), and that copy is the one edited. An org can instead host its own starter
# anywhere — a repo path or a url — and setup copies from it. Skills read
# [tickets] and [verbs] through a script, so comments are never loaded.
# Every ticketing config has the same fields; only the prose and the maps differ. `description` sits
# outside [tickets] so it is never loaded; it is for choosing a config.
# One tool: acli, Atlassian's official CLI (`acli jira workitem ...`): create with --parent, transition,
# links, comments, assign, JQL. The Atlassian Rovo MCP server (https://mcp.atlassian.com/v1/mcp/authv2)
# is the alternative for an org that prefers MCP; it has no link-creation tool.
#
# The active initiative is `core.active_initiative` in _bmad/custom/config.user.toml, read at activation. The ticket tree under `output_folder`
# has the same layout for every store (layout in references/board.md, Layout); write pushes it to this tracker.
# An empty field below that an operation needs, and that cannot be inferred from what the user gave:
# use what they tell you for this run and offer to record it in this file (references/store-setup.md).

description = "Jira Cloud through acli: work types, transitions, links, JQL. For teams already on Jira, usually with an admin who can adjust the project workflow, or customize this starter."

[tickets]
store = "jira"
key = ""    # the default Jira project key, e.g. "SHOP"; Jira assigns the number. A container's own `key` overrides it for everything under it
site = ""   # https://yourorg.atlassian.net
access = """
Use acli against `site`. Custom fields go through `--from-json`; check a subcommand's spelling
with `acli jira workitem --help` before first use. If a call fails on auth, a work type, or a
status, tell the user what is blocking, confirm the setup with them, and ask them to authenticate when that is the cause.
"""

fields = """
severity is Jira's priority: P0 = Highest, P1 = High, P2 = Medium, P3 = Low; set it at create
(--priority, or --from-json when the flag is absent) and change it the same way. risk is a label
risk-low, risk-medium, or risk-high. estimate is the Story Points field (points on a story; the t-shirt as a label est:<size> on an epic), through --from-json. Empty field: default priority, no label.
"""

reference = """
A reference is a line in the ticket's References section: `type — location, section`, location a
path from {project-root} or a url for a remote source, opened before it is cited. acli creates no web links; the description line is the
reference.
"""

# Verb prose lives in its own table so a skill can load one verb at a time.
[verbs]
setup = """
Connect: `brew tap atlassian/homebrew-acli && brew install acli`, then
`acli jira auth login --web --site <site>`.
acli cannot create work types or statuses; those live in the project's settings. At first publish,
read an existing work item (`acli jira workitem view <key> --json --fields issuetype,status`) and
the project's work types, compare with [tickets.types] and [tickets.status], and tell the user
exactly what to add or what to change in their copy of this file. Jira's defaults (To Do, In
Progress, Done; resolution Won't Do) need no setup; the maps below also expect an In Review status.
Do not ask for a level above Epic: by default the initiative stays local and everything below it
publishes. A team-managed project's admin adds a status as a board column under
Project settings, Board; a company-managed project needs a Jira admin to add it to the workflow.
"""

write = """
The project is the ticket's own `key`, else that of the nearest container above it, else `key` here.
Create, parents before children, prerequisites first:
`acli jira workitem create --project <key> --type <per [tickets.types]> --summary <t>
--description-file <b> --parent <key> --label <type>[,hitl,risk-<risk>]`, priority per `fields` on
a bug. Changes: `edit` (body, labels), `assign`, `transition --status <[tickets.status].<state>>` (the ticket's state, `state` in `tickets.py` rows) —
status never moves by edit — `link create --out <blocker> --in <blocked> --type "Blocks"`,
`comment create`. No parent: omit --parent — the item sits loose in the project. Custom fields go through --from-json; when dropped shares a status with done,
set the resolution the same way so the two stay distinct.
Initiative: with [tickets.types].initiative empty, the default, it is never sent — everything below
it publishes, the initiative stays local, and epics sit loose in the project. Set that type only when the
project has a level above Epic; then epics take it as --parent.
The body sent is the whole local file, without the frontmatter lines tracker_id,
remote, tracker_status, status, assignee.
After a write, mirror tracker_id (the key), remote (the url), tracker_status, assignee, and after in the
local file — never status, which is the build's; commit local files on the current branch when `output_folder` is in a git repo, never push.
An `after` naming a sibling's id or `<epic id>.<entry id>` is that entry's item, published first when it is still local; `epic-<slug>` is the epic's item.
"""

query = """
One work item: `acli jira workitem view <key> --json --fields
key,issuetype,summary,status,assignee,parent,labels,issuelinks,resolution,description`. after is the
"is blocked by" links; Done with resolution Won't Do is dropped; the description's covers holds the source requirement ids. Children:
`search --jql "parent = <key>"`. Words: `search --jql 'project = <key> AND text ~ "<words>"'`,
narrowed by issuetype or status per the maps. The local file: `tickets.py find <folder> <key>`.
A child the tracker holds with no file in the tree gets an `[[entry]]` with the next unused `id` in its parent's `tickets.toml`, then its file.
Mirror the item's status into the local file as tracker_status, the BMad word per [tickets.status], beside tracker_id and remote — never into status, which is the build's.
Candidates to start (one that `tickets.py next` lists under `ready_to_refine` gets its criteria first): tracker_status backlog ([tickets.status].backlog on the tracker), every "is blocked by" link done, unassigned, no blocked_at. Offer all
ready tickets; when one must be chosen, the one that unblocks the most work behind it.
"""

# BMad type -> this store's type.
[tickets.types]
initiative = ""   # empty: the initiative stays local. Jira's default hierarchy is Epic > Story > Subtask; a level above Epic needs Plans (Premium or Enterprise). Set to "Initiative" only when the project has that level.
epic = "Epic"
story = "Story"
spike = "Task"   # plus label spike
bug = "Bug"

# BMad state -> this store's status; `query` reads it back the other way into tracker_status. The file's `status` is the build's and is never sent.
[tickets.status]
backlog = "To Do"
in-progress = "In Progress"
review = "In Review"   # not a default Jira status; `setup` asks for it. Leave empty for no review
done = "Done"
dropped = "Done"   # Jira default has no separate status; resolution Won't Do distinguishes it
`````

---

## File: skills/bmad-ticket/config/linear-ticketing.toml

`````toml
# Ticketing config: Linear. Read by every skill that creates, reads, or moves tickets.
# A starter: at first use the skill copies the chosen one to _bmad/custom/ticketing-store-config.toml
# (references/store-setup.md), and that copy is the one edited. An org can instead host its own starter
# anywhere — a repo path or a url — and setup copies from it. Skills read
# [tickets] and [verbs] through a script, so comments are never loaded.
# Every ticketing config has the same fields; only the prose and the maps differ. `description` sits
# outside [tickets] so it is never loaded; it is for choosing a config.
# One tool: Linear's official MCP server. Initiatives hold projects, parent issues are epics, labels carry
# type, relations carry blocking. Linear ships no CLI; community CLIs are not used here.
#
# The active initiative is `core.active_initiative` in _bmad/custom/config.user.toml, read at activation. The ticket tree under `output_folder`
# has the same layout for every store (layout in references/board.md, Layout); write pushes it to this tracker.
# An empty field below that an operation needs, and that cannot be inferred from what the user gave:
# use what they tell you for this run and offer to record it in this file (references/store-setup.md).

description = "Linear through its MCP server: projects, parent issues, relations, labels. For product teams on Linear."

[tickets]
store = "linear"
key = ""    # Linear team key, e.g. "SHOP"; Linear assigns the number
team = ""   # the default team, key or name. A container's own `key` overrides it for everything under it
access = """
Use the Linear MCP (create_issue, update_issue, create_project, update_project, get_issue,
list_issues, list_issue_statuses, create_issue_label, create_comment) against `team`. Confirm from the tool schema whether parent
and blockedBy take identifiers (SHOP-12) or ids. If a call fails on auth, a label, or a state,
tell the user what is blocking, confirm the setup with them, and ask them to authenticate when that is the cause.
"""

fields = """
severity is Linear's priority, set on the issue: P0 = Urgent, P1 = High, P2 = Medium, P3 = Low.
risk is a label risk:<value>; `setup` creates the three. estimate is Linear's estimate field on a story; a label est:<size> on an epic. Empty field: no priority, no label.
"""

reference = """
A reference is a line in the ticket's References section: `type — location, section`, location a
path from {project-root} or a url for a remote source, opened before it is cited. Also pass the url in the issue's links so it shows in
the sidebar.
"""

# Verb prose lives in its own table so a skill can load one verb at a time.
[verbs]
setup = """
Connect: `claude mcp add --transport http linear-server https://mcp.linear.app/mcp`, then sign in
over OAuth. Do not use the deprecated /sse endpoint.
`create_issue_label` for every non-empty value in [tickets.types], `hitl`, and risk:low, risk:medium,
risk:high that the team lacks. States
cannot be created from the MCP: `list_issue_statuses`, compare with [tickets.status], and tell the
user what to add under Settings, Teams, Issue statuses, or what to change in their copy of this
file. Linear's defaults (Backlog, Todo, In Progress, Done, Canceled) need no setup; the maps below
also expect an In Review state in the Started group. Offer this at first publish.
"""

write = """
The team is the ticket's own `key`, else that of the nearest container above it, else `team` here.
`create_issue` creates, `update_issue` changes — title, description, state, assignee, parent,
blockedBy, labels, priority, project; confirm the exact tool names against the server's list at
first use. The issue's state is [tickets.status].<state>, the ticket's state (`state` in `tickets.py` rows). Labels carry type, hitl, and
risk:<value>; priority carries severity per `fields`; the tool schema says whether parent and
blockedBy take identifiers or ids. A standalone ticket: omit parent and project. With [tickets.types].initiative empty, the default,
the initiative is never sent and everything below it publishes, epics as parent issues; set it to
map onto a Linear Initiative only when the server exposes initiative tools, otherwise it stays
local. Create parents before children, prerequisites first.
The body sent is the whole local file, without the frontmatter lines tracker_id,
remote, tracker_status, status, assignee.
After a write, mirror tracker_id (the identifier), remote (the url), tracker_status, assignee, and after in
the local file — never status, which is the build's; commit local files on the current branch when `output_folder` is in a git repo, never push.
An `after` naming a sibling's id or `<epic id>.<entry id>` is that entry's item, published first when it is still local; `epic-<slug>` is the epic's item.
"""

query = """
One issue: `get_issue` with includeRelations — labels, priority, state, assignee, parent,
project, relations, description; after is the "blocked by" relations, and the description's
covers holds the source requirement ids. Many: `list_issues` by project, parent, label, state, or
query words. The local file: `tickets.py find <folder> <identifier>`.
A child the tracker holds with no file in the tree gets an `[[entry]]` with the next unused `id` in its parent's `tickets.toml`, then its file.
Mirror the issue's state into the local file as tracker_status, the BMad word per [tickets.status], beside tracker_id and remote — never into status, which is the build's.
Candidates to start (one that `tickets.py next` lists under `ready_to_refine` gets its criteria first): tracker_status backlog ([tickets.status].backlog on the tracker), every "blocked by" relation done, unassigned, no blocked_at. Offer all
ready tickets; when one must be chosen, the one that unblocks the most work behind it.
"""

# BMad type -> this store's type.
[tickets.types]
initiative = ""   # empty: the initiative stays local. Linear has no Epic object and Projects cannot nest; set to "initiative" only to map onto Linear Initiatives, which nest five deep and allow several parents, so the tree cannot round-trip.
epic = "epic"   # label on a parent issue
story = "story"
spike = "spike"
bug = "bug"

# BMad state -> this store's status; `query` reads it back the other way into tracker_status. The file's `status` is the build's and is never sent.
[tickets.status]
backlog = "Backlog"   # or "Todo" — the team's choice
in-progress = "In Progress"
review = "In Review"   # not a default Linear state; `setup` asks for it. Leave empty for no review
done = "Done"
dropped = "Canceled"
`````

---

## File: skills/bmad-ticket/config/notion-ticketing.toml

`````toml
# Ticketing config: Notion. Read by every skill that creates, reads, or moves tickets.
# A starter: at first use the skill copies the chosen one to _bmad/custom/ticketing-store-config.toml
# (references/store-setup.md), and that copy is the one edited. An org can instead host its own starter
# anywhere — a repo path or a url — and setup copies from it. Skills read
# [tickets] and [verbs] through a script, so comments are never loaded.
# Every ticketing config has the same fields; only the prose and the maps differ. `description` sits
# outside [tickets] so it is never loaded; it is for choosing a config.
# One tool: Notion's official MCP server. One database holds every ticket; properties carry type,
# status, assignee, and hitl; two self-relations carry hierarchy (Parent) and blocking (Blocked by);
# a board view grouped by Status is the kanban. `setup` creates all of it.
#
# The active initiative is `core.active_initiative` in _bmad/custom/config.user.toml, read at activation. The ticket tree under `output_folder`
# has the same layout for every store (layout in references/board.md, Layout); write pushes it to this tracker.
# An empty field below that an operation needs, and that cannot be inferred from what the user gave:
# use what they tell you for this run and offer to record it in this file (references/store-setup.md).

description = "Notion through its MCP server: one tickets database with a board view, relations for hierarchy and blocking. For teams that plan in Notion and want tickets beside their docs."

[tickets]
store = "notion"
key = ""        # prefix of the database's ID property, e.g. "SHOP" gives SHOP-12; empty = page ids only
database = ""   # url or id of the tickets database; empty = `setup` creates it
access = """
Use the Notion MCP (notion-fetch, notion-create-pages, notion-update-page,
notion-query-data-sources, notion-create-comment, notion-get-users) against `database`. Confirm
from the tool schema how relation and person properties are passed. If a call fails on auth or
`database` is empty, tell the user what is blocking, confirm the setup with them, and ask them to authenticate when that is the cause.
"""

fields = """
risk and severity are select properties: Risk (low, medium, high) and Severity (P0-P3); `setup`
creates them, publish and update set them. estimate is a text property Estimate, created with the rest. Empty field: property left empty.
"""

reference = """
A reference is a line in the ticket's References section: `type — location, section`, location a
path from {project-root} or a url for a remote source, opened before it is cited. Notion renders urls as links; a Notion page url becomes
a mention.
"""

# Verb prose lives in its own table so a skill can load one verb at a time.
[verbs]
setup = """
Connect: `claude mcp add --transport http notion https://mcp.notion.com/mcp`, then sign in over
OAuth and share the database, or the page it will be created under, with the connection.
When `database` is empty: ask which page to create under, then `notion-create-database` named
Tickets with properties Title (title), Type (select, options per [tickets.types]), Status (status,
options per [tickets.status]), Parent (relation to this database), Blocked by (relation to this
database), Assignee (person), Covers (text), Risk (select: low, medium, high), Severity (select:
P0, P1, P2, P3), Estimate (text), hitl (checkbox), and, when `key` is set, ID (unique id
with that prefix). Then `notion-create-view`: a board grouped by Status. Give the user the url and
put it in `database` in this file.
When `database` is set: `notion-fetch` it and compare its properties and options with the maps;
add a missing option with `notion-update-data-source`, and tell the user about anything the
schema needs a person to change. Offer this at first publish.
"""

write = """
`notion-create-pages` into `database` creates; `notion-update-page` changes any property or the
content — Status, Assignee, Parent, Blocked by, Covers, Risk, Severity, hitl. The tool schema says
how relation and person properties are passed; `notion-get-users` resolves a name. Status is [tickets.status].<state>, the ticket's state (`state` in `tickets.py` rows). No parent: leave the Parent relation empty. An initiative
is a page of Type initiative with no Parent. Create parents before
children, prerequisites first.
The body is the page content: the whole local file, without the frontmatter lines
tracker_id, remote, tracker_status, status, assignee.
After a write, mirror tracker_id (the ID property when `key` is set, else the page id), remote (the page
url), tracker_status, assignee, and after in the local file — never status, which is the build's; commit local files on the current branch
when `output_folder` is in a git repo, never push.
An `after` naming a sibling's id or `<epic id>.<entry id>` is that entry's item, published first when it is still local; `epic-<slug>` is the epic's item.
"""

query = """
One page: `notion-fetch` — Title, Type, Status, Assignee, Parent, Blocked by, Covers, Risk,
Severity, hitl, and the content. Many: `notion-query-data-sources` on `database` filtered by any
property; `notion-search` scoped to `database` for a looser match. The local file: `tickets.py find <folder> <id>`.
A child the tracker holds with no file in the tree gets an `[[entry]]` with the next unused `id` in its parent's `tickets.toml`, then its file.
Mirror the page's Status into the local file as tracker_status, the BMad word per [tickets.status], beside tracker_id and remote — never into status, which is the build's.
Candidates to start (one that `tickets.py next` lists under `ready_to_refine` gets its criteria first): tracker_status backlog (Status [tickets.status].backlog on the tracker), every page in Blocked by done, Assignee empty, no blocked_at. Offer all
ready tickets; when one must be chosen, the one that unblocks the most work behind it.
"""

# BMad type -> this store's type (Type select options; `setup` creates them).
[tickets.types]
initiative = "initiative"
epic = "epic"
story = "story"
spike = "spike"
bug = "bug"

# BMad state -> this store's status (Status property options; `setup` creates them); `query` reads it back the other way into tracker_status.
# The file's `status` is the build's and is never sent.
[tickets.status]
backlog = "Backlog"
in-progress = "In Progress"
review = "Review"
done = "Done"
dropped = "Dropped"
`````

---

## File: skills/bmad-ticket/config/repo-ticketing.toml

`````toml
# Ticketing config: tickets as files in the repo (default). Read by every skill that creates, reads, or moves tickets.
# A starter: at first use the skill copies the chosen one to _bmad/custom/ticketing-store-config.toml
# (references/store-setup.md), and that copy is the one edited. An org can instead host its own starter
# anywhere — a repo path or a url — and setup copies from it. Skills read
# [tickets] and [verbs] through a script, so comments are never loaded.
# Every ticketing config has the same fields; only the prose and the maps differ. `description` sits
# outside [tickets] so it is never loaded; it is for choosing a config.
#
# Tickets and artifacts live together as markdown files under `output_folder` (layout in references/board.md,
# Layout), committed in the monorepo or in a repo of their own. Status and assignee change by commit, and the skill never pushes,
# so others see them only after a push and pull.
#
# The active initiative is `core.active_initiative` in _bmad/custom/config.user.toml, read at activation.
# An empty field below that an operation needs, and that cannot be inferred from what the user gave:
# use what they tell you for this run and offer to record it in this file (references/store-setup.md).

description = "Quick setup, and the default when nothing else is chosen. Tickets are markdown files under version control: in the monorepo, or in their own repo (recommended for mono and poly repos alike). No tracker or account needed. The simplest store — right for a solo developer or a very small team with no other tracking."

[tickets]
store = "repo"

access = """
Read and write the files under `output_folder`. If it is missing, tell the user what is blocking and confirm the setup with them.
"""

fields = """
risk and severity live in the frontmatter (risk: low|medium|high on any ticket; severity: P0-P3
on bugs); estimate and estimate_basis the same way. Nothing else carries them.
"""

reference = """
A reference is a line in the ticket's References section: `type — location, section`, location a
path from {project-root} or a url for a remote source, opened before it is cited.
"""

# Verb prose lives in its own table so a skill can load one verb at a time.
[verbs]
setup = """
Tickets live in `output_folder` beside the documents. To keep them outside the project, set `output_folder`
under `[core]` in `_bmad/custom/config.toml` to a folder beside it — the session then launches one level
up — or in the workspace that already holds the repos. `git init` a new folder so tickets have
history; skip that when the user picks a folder already under git, the monorepo included. Then
create `backlog/` and the active initiative's folder there when they do not exist.
"""

write = """
No entry yet: add an `[[entry]]` with the next unused `id` to the parent's `tickets.toml`; the file carries that `id`. Every change — body, parent (move the file and its plan, or the container folder, into the new
parent's folder, and set the plan's `ticket` to the leaf's id there), after — is an edit to the file. A leaf's `status`, `assignee`, `blocked_at`, and `blocked_reason` live in its plan beside it, and a pulled file has none: the build writes draft, ready-for-dev, in-progress, in-review, and built as it works, and build-auto writes blocked; done is the user's or an orchestrator's, and dropped is `bmad-ticket`'s when the user says so. No skill moves a leaf past built. `bmad-ticket` sets a leaf's status or assignee only with `tickets.py mark [<folder>] <ref> <status> [--assignee <who>] [--blocked <reason>]`, which writes the plan and creates it when there is none. A container's `status` is `bmad-ticket`'s: absent until work under it starts, then in-progress, done, or dropped. Build records: the plan, `<type>-<slug>-plan.md`
beside the leaf, whether or not the leaf has a file.
Commit only the files you wrote on the current branch. Root not in a git repo: skip the commit and say so.
"""

query = """
A ticket is named by its path and its `id` under its epic. `tickets.py find <folder> <ref>` resolves an id, `"2.3"`, a file name, or title words to the one ticket: its row, its entry's text fields, and the paths `epic_file`, `story_file`, and `plan`; by a frontmatter field, `grep -rl "^type: bug" root` — same for parent; status and assignee come from `tickets.py status`, which reads the plans. A container's children are the ticket files and container folders in its folder; planned children with no file are the entries in its `tickets.toml`. Frontmatter holds id,
type, title, parent, covers (the source requirement ids), after, refined, hitl, risk, estimate, and severity on bugs; a
pulled file always carries after and hitl and the rest only when its entry sets them. An absent after or hitl reads as empty; an absent covers or estimate reads the entry's. The plan's frontmatter holds status (absent until a build starts),
assignee, and blocked_at and blocked_reason when blocked; `tickets.py` reads a leaf file's own status only when there is no plan.
Candidates to start (one that `tickets.py next` lists under `ready_to_refine` gets its criteria first): no status, or draft or ready-for-dev; blocked_at empty; everything in after done; unassigned. Several ready tickets are parallel work;
offer them all. When one must be chosen, take the one that unblocks the most work behind it.
"""

# BMad type -> this store's type.
[tickets.types]
initiative = "initiative"
epic = "epic"
story = "story"
spike = "spike"
bug = "bug"

# BMad state -> this store's status. The files are the store: the words in a plan's `status` read as these states (none, draft, ready-for-dev -> backlog; blocked -> in-progress; in-review, built -> review).
[tickets.status]
backlog = "backlog"
in-progress = "in-progress"
review = "review"
done = "done"
dropped = "dropped"
`````

---

## File: skills/bmad-ticket/config/trello-ticketing.toml

`````toml
# Ticketing config: Trello. Read by every skill that creates, reads, or moves tickets.
# A starter: at first use the skill copies the chosen one to _bmad/custom/ticketing-store-config.toml
# (references/store-setup.md), and that copy is the one edited. An org can instead host its own starter
# anywhere — a repo path or a url — and setup copies from it. Skills read
# [tickets] and [verbs] through a script, so comments are never loaded.
# Every ticketing config has the same fields; only the prose and the maps differ. `description` sits
# outside [tickets] so it is never loaded; it is for choosing a config.
# One tool: the official Trello MCP server. Trello is flat: lists carry status, labels carry type, card
# urls in descriptions carry hierarchy and blocking. Using the defaults below: the board needs the lists
# Backlog, In Progress, Review, Done, Initiatives, Epics and a label per type plus hitl; `setup` says how. Members, comments, attachments, and label creation
# are not in the MCP yet. Weakest fit of the shipped configs; expect to tune it.
#
# The active initiative is `core.active_initiative` in _bmad/custom/config.user.toml, read at activation. The ticket tree under `output_folder`
# has the same layout for every store (layout in references/board.md, Layout); write pushes it to this tracker.
# An empty field below that an operation needs, and that cannot be inferred from what the user gave:
# use what they tell you for this run and offer to record it in this file (references/store-setup.md).

description = "Trello through its MCP server: lists as status, labels as type, card links as hierarchy. For small teams that run a Trello board and want to keep it."

[tickets]
store = "trello"
key = ""     # ids are card short links (from the card url); leave empty
board = ""   # the board url
access = """
Use the Trello MCP (trelloReadBoard, trelloReadCard, trelloWriteCard, trelloWriteChecklist,
trelloSearch) against `board`. Write tools take the server's ids, not urls: resolve a url with
`trelloReadCard get` first. If a call fails on auth, a list, or a label, tell the user what is blocking, confirm the setup with them, and ask them to authenticate when that is the cause.
"""

fields = """
risk is a label risk:<value>; severity a label P0-P3. The MCP cannot create labels: `setup` asks
the user to add them with the type labels. estimate is an `estimate:` line in the description. Empty field: no label.
"""

reference = """
A reference is a line in the ticket's References section: `type — location, section`, location a
path from {project-root} or a url for a remote source, opened before it is cited. Attachments are not in the MCP yet; the description line
is the reference.
"""

# Verb prose lives in its own table so a skill can load one verb at a time.
[verbs]
setup = """
Connect: `claude mcp add --transport http trello https://mcp.trello.com/v1`, then sign in over
OAuth with write permission.
The MCP can create a board but not lists or labels. When `board` is empty, offer to create one;
then ask the user to add the lists in [tickets.status] (Backlog, In Progress, Review, Done) plus
Initiatives and Epics, and labels for every value in [tickets.types] plus hitl, risk:low, risk:medium, risk:high, and
P0-P3. Check with
`trelloReadBoard` before first publish and name what is still missing.
"""

write = """
`trelloWriteCard create` makes a card, `update` changes description or labels, `move` changes
list, `archive` is dropped. Write tools take server ids: resolve a url with `trelloReadCard get`
first. Status is the list [tickets.status].<state>, the ticket's state (`state` in `tickets.py` rows); initiative cards live in the Initiatives list,
epics in Epics, both carrying a `status:` line at the top of the description. Labels carry type,
hitl, risk, severity. Trello has no fields for the rest — conventions in the description:
`parent: <card url>` as its first line, an `assignee:` line (no member tools yet), after as
card urls; the parent card keeps a checklist with each child's url. A standalone ticket: no parent line. Create parents before
children, prerequisites first.
The body is the description: the whole local file, without the frontmatter lines
tracker_id, remote, tracker_status, status (assignee stays, as the `assignee:` line above).
After a write, mirror tracker_id (the card short link), remote (the card url), tracker_status, assignee, and
after in the local file — never status, which is the build's; commit local files on the current branch when `output_folder` is in a git repo,
never push.
An `after` naming a sibling's id or `<epic id>.<entry id>` is that entry's item, published first when it is still local; `epic-<slug>` is the epic's item.
"""

query = """
One card: `trelloReadCard get` by url — name, list, labels, description (assignee, parent,
covers, after live there), checklists; a container's children are its checklist items. Many:
`trelloSearch` scoped to `board`, or `trelloReadBoard` filtered by the maps. The local file: `tickets.py find <folder> <short link>`.
A child the tracker holds with no file in the tree gets an `[[entry]]` with the next unused `id` in its parent's `tickets.toml`, then its file.
Mirror the card's list into the local file as tracker_status, the BMad word per [tickets.status], beside tracker_id and remote — never into status, which is the build's.
Candidates to start (one that `tickets.py next` lists under `ready_to_refine` gets its criteria first): tracker_status backlog (the [tickets.status].backlog list), every after card in the Done list, assignee line
empty, no blocked_at. Offer all ready tickets; when one must be chosen, the one that unblocks the most work
behind it.
"""

# BMad type -> this store's type.
[tickets.types]
initiative = "initiative"
epic = "epic"
story = "story"
spike = "spike"
bug = "bug"

# BMad state -> this store's status; `query` reads it back the other way into tracker_status. The file's `status` is the build's and is never sent.
[tickets.status]
backlog = "Backlog"
in-progress = "In Progress"
review = "Review"
done = "Done"
dropped = "archived"
`````

---

## File: skills/bmad-ticket/references/board.md

`````markdown
# Board

Operations on existing tickets. Read an existing ticket before changing it; if it has been published to a tracker, query its current remote state too.

## Publish

Approving a breakdown records it in the epic's `tickets.toml`; it does not start work. On the repo store, committing the approved `tickets.toml` is the publish. On a tracker, resolve `{workflow.publication}`: `on_start` publishes each ticket when it starts; `at_inception` offers to publish the agreed set after validation; `auto` is `at_inception`. An explicit user request takes precedence.

Publish through `write`. Publishing is not a status: with the repo store it is the commit of the approved files; with a tracker it is the remote item existing, `tracker_id` and `remote` set in the file. Publishing to a tracker writes the leaf's file first, with `tickets.py pull` when it has none, because that file is the body the tracker receives. A type mapped to `""` is never sent: it stays local and its children attach to the nearest published ancestor, which is how an initiative behaves on a tracker whose hierarchy has no level above the epic. Say so once when it first matters rather than at every publish. At first publish, or when maps name missing fields or statuses, offer `setup`. If any prerequisite is still local, include it in the proposed publication scope.

An unrefined ticket may be published for planning visibility. One that needs refining keeps `refined: false`; the `backlog` state means available to start. With a tracker, that line stays in the published body. Refinement updates that same file and remote item, preserving identity.

Before starting a candidate, read it and its source. One in `next`'s `ready_to_refine` group is refined first per `slice.md`. Confirm it is in `ready_to_start` and not assigned to someone else. Then, with the user's approval, publish it if it is not yet published, and start it: on a tracker, `write` transitions the item to `[tickets.status].in-progress` and the file gets `tracker_status: in-progress`; on the repo store, hand the ticket to the build, which writes `status` in the ticket's plan as it works. In an unattended run, an entry's `plan_checkpoint` waits for a person to approve the ticket, or the builder's plan when it is not refined, and `done_checkpoint` waits after the ticket closes.

## Progress and closure

- A ticket the user names — "refine 1.2", "start ABCD-13", "build the cart scaffold" — resolves through `uv run {skill-root}/scripts/tickets.py --project-root {project-root} find <folder> <ref>` before you pull, refine, start, or mark it: one ticket with its row, its entry's `description`, `verify`, `references`, `notes`, and `unknown`, its `folder`, and the absolute paths `epic_file`, `story_file` (null until pulled), and `plan` (where its plan is or goes). Words that match more than one ticket: ask.
- Progress lives on tickets, not in a separate sprint/status file. `tickets.py next <folder>` (same `--project-root`) proposes candidates grouped by state; `status <folder>` reports every ticket with its `status`, `tracker_status`, and state, what it blocks, counts by state, and the remaining chain; a row's `gated_by` is its epic file's `after`. On an initiative, each `epics` row carries the epics it declares in `after` with their `needs`, and its epic file's gate as `gated_by`. `<folder>` is an epic, `backlog/`, or the initiative for all its epics at once. With a tracker, query before either view and pass `--synced` to `next`. `next`'s `ready_to_start` group is the tickets, with or without a file, that are not started, blocked, or waiting to be refined and whose prerequisites are done or in review, in build order; a row's `gated_by` epics must be done. Offer the first and start it as above, passing its row's `ref` to `find`.
- `unpinned_after` lists an epic that has tickets but none waiting on the epic its `after` names: add the prerequisite with the user. `undeclared_after` lists an entry that waits on an epic its own epic does not declare, and `order_conflict` an epic that waits on one later in build order: settle each with the user per `slice.md`. `drift: true` on a `status` row: show the file's and the entry's `after` and `hitl` to the user and make them equal. `problems` on `next` or `status` lists plans the tree could not use: one whose `ticket` names nothing, or whose `status` is unknown (its ticket reads as blocked). Show each to the user and fix the plan; `mark` sets a valid status.
- A done standalone ticket stays in `backlog/` beside its plan, which joins it by file stem; `status` or `next` on `backlog/` shows what is still open.
- Offer all unblocked, unassigned candidates when work can run in parallel.
- A leaf's `status`, `assignee`, `blocked_at`, and `blocked_reason` live in its plan, `<type>-<slug>-plan.md` beside where its file is or would be (`tree-rules.md`); a leaf file's own `status` is read only when there is no plan. Who writes each status:

  | Status | Written by |
  |---|---|
  | `draft`, `ready-for-dev`, `in-progress`, `in-review`, `built` | `bmad-build`, `bmad-build-auto` as they work |
  | `blocked` | `bmad-build-auto` when it halts, with the reason in the plan |
  | `done` | the user, or an orchestrator, through `tickets.py mark` |
  | `dropped` | `bmad-ticket`, when the user says so |

  No skill moves a ticket past `built`, and `bmad-ticket` does not move a leaf the build is working. When the user says a ticket is done, run `mark` for them. Assignee changes, and a `status` the user asks for — `done`, `dropped`, or a person working the ticket by hand — go through `write`; on the repo store, for a leaf, that is `tickets.py --project-root {project-root} mark [<folder>] <ref> <status> [--assignee <who>] [--blocked <reason>]` followed by the commit its verb describes. `mark` takes its folder and ref as `find` does, writes the plan and never the leaf file, and creates a plan holding only frontmatter when there is none. `mark` writes what it is told; the checks above are yours. On a tracker, `write` transitions the item to `[tickets.status].<state>` and never sends `status` as a word; the tracker's status comes back as `tracker_status` on `query`. A container's `status` is `bmad-ticket`'s, an edit to its file: absent until work under it starts, then `in-progress`, `done`, or `dropped`; containers never take review. On done with estimation on, ask for the actual (`estimate.md`).
- A ticket waiting on a person or an answer, not on a prerequisite: `mark` with `--blocked <reason>` sets `blocked_at` (today) and `blocked_reason`; a `mark` without it clears both when the ticket moves. `next` lists it under `blocked`.
- Closing every child does not close the parent. Run the closure check in `validate.md` against its requirements and Done when; the user confirms the parent is complete.
- Drop only after a `Dropped:` line in Notes says why. A dropped ticket still blocks its dependents, in any epic: `status` lists them under `blocks`; remove or repoint it in each one's `after` with the user. An entry with no file and no plan is dropped by deleting it from `tickets.toml`. Cancelling a container cancels its descendants after the user confirms.
- Whatever `query` returns lands in the tree: `tracker_id`, `remote`, `tracker_status`, `assignee`, and `after` into frontmatter, never `status`; a ticket with no file gets one per the layout. A body that differs from the file: show and ask.

## Layout

```
{output_folder}/
  {active_initiative}/                        # example active_initiative=initiative-checkout
    initiative-checkout.md
    tickets.toml                             # the epics in build order
    spec-checkout/
    epic-cart-rules/
      epic-cart-rules.md
      tickets.toml                           # every planned entry, refined or not
      spec-cart-rules/
      story-cart-service-scaffold.md           # refined; its id is in its frontmatter
      story-cart-service-scaffold-plan.md      # the build's plan, with the ticket's status
      story-apply-discount-codes-plan.md       # a plan for an entry with no file
      spike-discount-engine-latency.md
  backlog/
    bug-checkout-total-ignores-discount-codes.md
```
`````

---

## File: skills/bmad-ticket/references/estimate.md

`````markdown
# Estimating

Only when `{workflow.estimation}` has `enabled = true`; otherwise never raise it. Points and t-shirts share one unit: a t-shirt is a range of summed story points, so an epic sized before inception and its stories pointed later reconcile.

## Where an estimate lives

`estimate` in every ticket's frontmatter; `estimate_basis` in a container's. The basis says what the number rests on: `envelope` (intent only), `spec`, `entries` (breakdown entries pointed), `stories` (stories pointed from their reviewed files). Every re-estimate appends an `Estimate:` line in Notes — old value, new value, basis, reason. The store's `fields` global says how a tracker carries it.

## When to offer one

- Epic definition complete, before the story breakdown: an imagined split. Name the probable stories, point each per the rubric, sum, map to the t-shirt. The reasoning goes in the epic's Notes, marked as imagined; inception replaces it. Basis `spec`, or `envelope` when there is none.
- Whole epic breakdown approved: point every breakdown entry, pulled or not, and re-estimate the epic from the sum. Basis `entries`; refinement can change the estimate.
- Story refined: point it from its criteria and re-sum the whole ticket set if it moved. Basis `stories` once every ticket has a reviewed file; publication does not change the basis.
- Story closed: ask whether the actual matched. A miss is a Notes line on the story; a 1-2 that needed a person is the miss that matters most.

## What the size says

XL, or a sum above the map's top range, is the signal to offer splitting the epic before it is sliced: say which imagined stories cluster into what, per `{workflow.slice_to_epics}`. S or below at the envelope is the signal for the Small epic path (SKILL.md, Intake). The user overrides either way; an override is a `Decision:` line.

## Calibration

On request, or offered once a store holds ten or more closed tickets with both an estimate and an actual: `query` them, hand a subagent the pairs and the current rubric and map, and take back a proposed rubric and map that fit the team's distribution, with the reason for each change. Show it; write it to the team override of `customize.toml` only with approval.
`````

---

## File: skills/bmad-ticket/references/slice.md

`````markdown
# Slicing

An initiative is sliced into epics: containers the product owner and developer own and complete.

Inception plans the whole selected epic so AI agents can build it. `{workflow.slice_to_epics}` and `{workflow.slice_to_tickets}` carry the recommended split; the user's preference comes first. `{workflow.ordering}` says what opens and closes a parent.

When an initiative has epics, every story is under an epic. Work that fits one epic is one epic. An initiative with no epics happens only at the user's request and is planned like an epic with stories directly underneath.

## Every container, in order

Apply this when authoring the initiative or incepting the selected epic.

1. **Envelope.** A document in the folder is an input, not the container. When the container file is missing, offer to create it from its template: title, a paragraph of intent, Outcome, Done when, boundaries, known decisions, and references. An epic also records its parent and the parent requirement ids it owns in `covers`. Done when is three to six checks at the altitude of the source's ids; it is the definition of the container and is never deferred.
2. **The requirement source at this altitude.** The container's own Requirements section holds it: the source's lines as stable ids, each mapping to a parent id in `covers`. A referenced numbered source replaces it; a numbered spec anywhere in the container's folder is that source. Offer `bmad-spec` only when the source outgrows the section or the user asks, and pass it the container's folder: its `spec-<slug>/` goes inside that folder, owns the ids, and `covers` still maps them upward. The architecture spine, UX design, and research inform everything below either way.
3. **Complete the container** per `{workflow.container_definition}` and `ticket.md`: the requirement source, References, and a re-read of Outcome and Done when against it. Propose the fields together with reasons, discuss what is unsettled, and confirm before slicing. With estimation on, offer an imagined-split size per `estimate.md`; XL is the cue to offer splitting the epic first.

## Learn the codebase and team first

Start from the parent chain: the parent ticket, its spec, and what they reference, including the design when the work is user-facing. Then the codebase: greenfield or brownfield; mono or poly repo; team, service, and UI boundaries; vocabulary and recorded decisions — from the architecture document, else the repo layout. Then the areas the slices will touch, enough to draw lanes that do not collide. Tell the user what you read and concluded; ask what is wrong or missing and whether there are other references or tools you do not already know of.

Where the source contradicts the code or another source, add `Source conflict: <id or section> — <what the source says> vs <what was found>` to the Notes of the container whose source it is, and tell the user. When the source is a BMad spec, offer to pass the correction to `bmad-spec`.

## Ask the questions that decide the split

Use what is already known. Ask the remaining questions that change the split: what is first worth demoing; what is least certain; what will the first piece teach about the rest; whether the user has a split in mind; how the team defines epics. Group related questions and say which answer you would pick and why.

When the user states how their team cuts epics, offer to save it as `slice_to_epics` under `[workflow]` in `{project-root}/_bmad/custom/bmad-ticket.toml`. It replaces the default, so keep the default lines the team still wants.

## Initiative into epics

Existing epics are the working set: read them first and refine in place, or drop one per `board.md` when it no longer fits; add only after the user confirms the set is insufficient. Draft the set, run the tree check in `validate.md` on the draft, then present it in recommended build order, and say in one sentence which rule cut it: for each epic, title, one to three sentences of what is true when it is done, the parent ids it owns, its boundary, and what it needs from the epics before it. Name the tracer path across epics when the first demo cuts through several. List every unit the source touches that gets no epic as a touch point with the epic that owns it. With it, list the decisions more than one epic must adopt: a contract, a message or data format, a shared value list. One repo or one unit has none. Work with the user on order, boundaries, merges, splits, and anything unplaced.

On confirmation, write the initiative's `tickets.toml` from `{skill-root}/assets/tickets-template.toml`: one `[[epic]]` per epic in build order, each with an `id`, `after` naming what it needs and from which epic; `after` on an epic file only for a whole-epic gate. Each new epic gets its folder and envelope with Outcome and Done when.

For the decisions more than one epic must adopt, offer `bmad-architecture` to settle them in the spine, and cite the spine section in each adopting epic's References. Declined: add each as a `story` entry (`hitl = true`) in the opening epic's `tickets.toml`, and give each adopting epic `after = [{ epic = <opening>, needs = <the decision> }]`; its entries name that entry in `after` at their inception.

Stop at this level unless the user wants to incept an epic now; complete and plan only the selected epic. The others retain their scope, references, and place in the order without a breakdown.

## Epic into stories

The epic is ready to be worked and its spec exists or the user chose to go without. Read its breakdown, every child ticket, and their results before proposing changes. Plan the entire epic per `{workflow.slice_to_tickets}` and `{workflow.ordering}`, not only its next story. Put each unknown that must be settled before implementation to the user: answer it now, record it as the `unknown` of the entries it affects, or add a spike when they ask for one. Never guess the design of work that depends on it.

Draft one breakdown in build order, each entry with an `id` it keeps for good, run the set check in `validate.md` on the draft, then present it. Each entry names its type and title, `hitl` when needed, requirement ids and what it delivers toward them, its prerequisites, what exists when it is done, how that result will be verified, known uncertainty, and `refine` per `{workflow.refinement}`; when the epic will run unattended, also ask for `plan_checkpoint` and `done_checkpoint` per entry. Each touch point this epic owns is an entry or part of one. Name the tracer bullet, what can run in parallel, and any deferred scope. Give an entry `references` for the spine section, design screen, or document it needs beyond the epic's own References. With the approval question, ask once whether the user wants to change a description or add a note or reference to any entry; `notes` holds only what the user said, in their words. Adjust size, order (the order of the tables), and prerequisites with the user until they approve the set; past the size in `{workflow.slice_to_tickets}`, offer a split first. A split at inception is a second epic folder and envelope, a new `[[epic]]` in the initiative's breakdown with its `after`, the covers ids moved, and the agreed entries placed under the right epic; no file is renamed.

For each `after` on this epic in the initiative's breakdown, put the provider in `after` of every entry that needs it: `<epic id>.<entry id>` when the providing entry exists, `epic-<slug>` until it does.

On approval, write the whole set into the epic's `tickets.toml` from `{skill-root}/assets/tickets-template.toml`, one sentence each for `description` and `verify`. No leaf file is written until its entry is pulled. With estimation on, each entry carries its points.

Then run `uv run {skill-root}/scripts/tickets.py --project-root {project-root} status <initiative folder>`. In `epics`, this epic's `blocks` lists the tickets in other epics whose `after` names the whole epic; replace each with the entry that delivers what it waits for. Add the prerequisite for every `unpinned_after` it reports. For every `undeclared_after`, an entry here waits on an epic the initiative does not declare: with the user, add that epic to this epic's `after` in the initiative's breakdown with its `needs`, or drop the prerequisite. For every `order_conflict`, an epic waits on one later in build order: reorder or merge the epics, or record why in a `Decision:` line.

Record the breakdown's decisions in the epic's Notes as dated `Decision:` lines — tracer bullet, sequencing, deferred scope. Publication follows `board.md`.

Then tell the user the entries are ready to build, and what that means for how they build. With `bmad-build`, each ticket is refined during the build: the builder questions the user and writes the criteria itself, so refining here repeats that work. With `bmad-build-auto`, a loop, or a factory, nobody answers questions during the build and the entry is all the builder gets: recommend one more review of the sequence and of each entry now, and offer the checkpoints where none are set.

## Refining a ticket

Resolve what the user names with `uv run {skill-root}/scripts/tickets.py --project-root {project-root} find <folder> <ref>` first: `<epic id>.<entry id>`, an entry id inside the epic folder, a tracker id, a file name, or words from the title; it returns one ticket with its `folder` and the paths `epic_file`, `story_file` (null until pulled), and `plan`. Pull with `tickets.py pull <epic folder> <id>` (same `--project-root`): it writes `<type>-<slug>.md` from the entry in a fixed layout, no `status` line, `after` and `hitl` always, and other fields only when the entry sets them (`refined: false` when it asks for refinement). An absent `after` or `hitl` reads as empty; an absent `covers` or `estimate` reads the entry's. The templates apply only to tickets you write. From then on the file is truth. The entry keeps `id`, `type`, `title`, `after`, and `hitl` current; its content fields are not maintained after pull. Refinement edits go to the file, and through `write` once published; a changed `after` or `hitl` goes to the file and the entry both, since the file's wins and `tickets.py status` flags the difference as `drift`. Reordering is still moving tables in `tickets.toml`.

An entry goes to the builder as it is, with its epic and its file when it has one: the builder plans story criteria from the epic's Requirements and Done when, the entry's description, and its `Verify:` check, which it may extend and never weaken.

Refining follows `{workflow.refinement}`. For an epic's stories — the next one by default, several or all on request — pull the file when the entry has none, then review the file with the user: read it, its source, the finished siblings and their build records; improve description, `Verify:`, references, and notes in the file; adjust order (move tables) and prerequisites, and run the set check on the revised draft when they change. A reviewed file has no `status`; it is intent that is ready for the build. `refined` is only for a ticket with full criteria, below; the review does not set it. No Given/When/Then unless the ticket is a bug, has no epic, its entry says `refine = true`, or the user asks.

For a ticket with no epic, a bug, and an entry where `pull` returns `refine: true`, expand the ticket before it starts. Read the ticket, its source, the finished siblings and the build records beside them, and the code it will touch at the revision the work starts from. A `Source conflict:` found here is recorded as above, and a ticket covering the conflicting id stays `refined: false` until a `Decision:` line settles it. Confirm its prerequisites are done or in review and expand it per `ticket.md`: criteria, boundaries, references, and decisions. Resolve questions that prevent implementation; set `refined: true` when the user approves. A reply approves what was presented; publication and starting work are approved separately unless the user asks for them together. Refining alone does not change status or assignee.

When completed work changes the picture, revisit the whole remaining breakdown with the user. Update unstarted entries and tickets in place, preserving each `id`; published changes go through the store. Do not rewrite completed or active work as a new plan. Run the set check on the revised draft before the user approves it, and record the reason in the epic's Notes.
`````

---

## File: skills/bmad-ticket/references/store-setup.md

`````markdown
# Setting up the store

Runs at first use — the store config `{project-root}/_bmad/custom/ticketing-store-config.toml` is missing or unreadable — and whenever the user wants the store configured, reconfigured, or switched. The outcome is a working config at that path and a store proven against its own verbs.

1. **Choose.** Offer the starters in `{skill-root}/config/` by the `description` each opens with; the user may also bring their own file or url, or build a custom one from the nearest starter. Quick setup is the repo starter: files, no account, working in minutes. Every hosted starter has a free tier; choosing one costs an account setup, or only the config when the user already has a personal or enterprise account. Copy the choice to the store config path.
2. **Review.** Open the copy with the user: suggest they read it, and answer their questions from it. List every empty field an operation needs — key, site, team, database — and fill them from their answers. The copy is theirs; edits survive skill updates.
3. **Connect.** Offer to run the store's `setup` verb: tool install, auth, and the labels, lists, statuses, or properties the maps name.
4. **Prove it.** Offer a test. A store that already has items: `query` a few back — that proves auth, the maps, and the path back. An empty store: create one ticket in `backlog/` titled "BMad setup test — safe to delete" and prove `write` and `query`: create it, query it back, move it to `in-progress`, assign it, then drop it. Give the user the `remote` url so they can watch the item and its history; a repo store has no url — the proof is the file in the layout and the commit.
5. **Customize.** Offer a `bmad-customize` pass on this skill: review the defaults in its `customize.toml` — slicing, publication timing, ordering, criteria, scoring, checks, the templates — so the user knows what is theirs to change. `bmad-customize` decides what changes and whether they land in the team or personal override file. When it finishes, return here for the restart guidance.
6. **Restart.** Suggest clearing the session, or starting a new one, and rerunning what they came to do: the next activation reads the finished config clean.
`````

---

## File: skills/bmad-ticket/references/ticket.md

`````markdown
# Writing a ticket

A ticket starts from what is known: what exists when it is done, how it will be verified, what must not change, and what is already decided. Ask only what remains unsettled, offering a default for each, then draft into the ticket tree. Under an epic with a breakdown, add the ticket's entry first, with the next unused `id`, placed where it goes in the build order. Open only the template for the type: `{workflow.initiative_template}`, `{workflow.epic_template}`, `{workflow.story_template}`, `{workflow.spike_template}`, or `{workflow.bug_template}`. Its placeholders say what each section holds; its example is the level of detail to match, not copied.

## Each fact lives in one place

The input (PRD, brief, notes) owns the product argument. The requirement source at a level is the container's own Requirements section, an existing numbered source, or a separate spec when the source outgrows the section. Reference that source rather than duplicating it. The container owns Description, Outcome, Done when, Boundaries, References, Notes. An epic's `covers` records the parent requirement ids it owns, and every id it assigns locally — in Requirements or its own spec — maps to one of them. An initiative's `covers` records its source ids. Adding a local spec does not replace upstream coverage; update affected references and child mappings with it.

A story under an epic is one slice of its build order: `covers` cites ids from the epic's requirement source, and its description says what it delivers toward them. Several stories can cover one requirement. Where full criteria are written (`{workflow.refinement}` says when), add criteria for what changes and for the failure paths, boundaries, and binding decisions; point at the source for the rest.

## Rules the template cannot carry

- Acceptance criteria, where `{workflow.refinement}` calls for them, follow `{workflow.acceptance_criteria}`; every sentence follows `{workflow.prose}`. When the user's text misses either, offer the rewrite with the reason; show what is missing, not only what is written.
- References name the nearest document, not the documents behind it. Attach per the store's `reference` global.
- No source-code paths or snippets; the builder reads the repo. A snippet stays only when it is the decision itself, not an illustration of it. A path the user wants recorded goes in Notes.
- A UI ticket links its design in References; criteria stay functional, layout lives in the design. No design and user-facing: offer `bmad-ux` first; declined, say the builder will guess the layout unless they add details in Notes.
- `hitl: true` only when a person must do part of the work; say which step in the Description, and spell known steps out in the criteria or Notes.
- Risk on every ticket, severity on a bug, proposed per `{workflow.scoring}` with a one-line reason; the user's value wins.
- A bug carries a reproduction and a cause hypothesis, never a fix. Missing steps: ask; unclear: tighten until someone else could follow them. Run them when cheap; if the behavior already holds, say so with evidence and create nothing. Criteria include tests for the condition found and fixed, and name the other valid outcome: proof no change is needed.
- A spike names the question, who waits on the answer, and where it is recorded. A spike is `hitl`. When tickets in more than one epic wait on the answer, it is a decision several epics adopt: handle it per `slice.md`, not as a spike inside one of them.
- Notes holds what is not in the repo or the source and what is unsettled, each line marked, and only what is local to this ticket. Anything touching more than one ticket lives in the parent's Notes and is referenced; an unknown that gates work goes to the user, and becomes a spike its dependents list in `after` only when they ask for one. `Assumption:` — offer each; confirmed, it becomes a dated decision; corrected, the ticket changes. `Open question:` — answering it is part of the ticket's work when it starts. `Unknown:` — it gates the start; settle it with the user before the build begins, as a dated decision. Never resolve any of them by guessing.

## Refining an existing ticket

A story under an epic is refined per `slice.md`: pulled when it has no file, then reviewed in its file. Once a leaf file exists it is truth; the entry keeps only `id`, `type`, `title`, `after`, and `hitl` current, `after` edited in both places. For a ticket that carries full criteria: read the local ticket and open what it references; query its remote state if already published to a tracker. With the user: confirm they still agree with it; check its description against the current requirement source, criteria and references; find what is missing, unclear, or wrong; settle questions that prevent implementation. Save unpublished changes locally; published changes go through `write`. Preserve identity and any existing `status`, `tracker_status`, and assignee. A ticket keeps `refined: false` until it passes self-review in full and the user approves it. A container may end in a re-slice per `slice.md`.

## Self-review before the user sees it

Read it back at its current level of detail: an entry needs its description and verification approach; a refined ticket needs runnable acceptance criteria and settled prerequisites. Check size per `{workflow.slice_to_tickets}`, wording, references, and source consistency. Fix what you find; mention changes to the user's intent.
`````

---

## File: skills/bmad-ticket/references/tree-rules.md

`````markdown
# Rules for skills that use the ticket tree

Every skill that takes work from the tree, builds it, reviews it, or looks back on it follows these rules: `bmad-ticket`, `bmad-build`, `bmad-build-auto`, `bmad-code-review`, `bmad-retrospective`, and `bmad migrate`.

## Finding the tree

- The tree is `{output_folder}/{active_initiative}`: `output_folder` and `active_initiative` from `[core]` in the merged BMad config.
- `tickets.py` is installed at `{project-root}/_bmad/method/scripts/tickets.py`. `bmad-ticket` declares it in its `bmod.toml`. Other skills run it from there and never open `bmad-ticket`'s folder.
- Called with no folder, `tickets.py next`, `status`, and `find` resolve the active initiative themselves. When no initiative is set, they exit with an error that names the missing key, and the calling skill works without the tree.
- A ticket is read through `tickets.py find`: its entry's fields, its epic file, its story file when one was refined, and its plan path, whether or not the plan exists yet. The build's input is the entry and its epic, plus the story file when there is one. No skill writes a ticket file to start work.

## The plan file

- One plan per leaf, written by `bmad-build` or `bmad-build-auto`. It sits in the leaf's folder (the epic folder, or `backlog/`) and is named `<type>-<slug>-plan.md`, where `<type>-<slug>` is the name the leaf's file would have. `find` returns this path.
- Its frontmatter carries `ticket: <entry id>`. That field joins the plan to its entry, so a title change that renames the plan never breaks the link. A backlog leaf has a file and no entry, so its plan carries `ticket: <file stem>` instead. Its `type` is the build's (`feature`, `bugfix`, `refactor`, `chore`), never a ticket type.
- It stays local on every store. A tracker never receives it.
- On a tracker store, publishing writes the leaf's file, because that file is the body the tracker receives. `tracker_id`, `remote`, and `tracker_status` stay in that file. The plan is still a separate file beside it.

## Status

- On the repo store, a leaf's status lives in its plan's `status`. An older leaf file can still carry `status`; `tickets.py` reads it only when there is no plan. An entry with no file and no plan is `planned`, and it is ready to start once its prerequisites are done or in review; an epic file's `after` still waits for that epic to be done. No pull is needed.
- The board state comes from `status`: none, `draft`, `ready-for-dev` → `backlog`; `in-progress`, `blocked` → `in-progress`; `in-review`, `built` → `review`; `done` and `dropped` are themselves.
- `assignee`, `blocked_at`, and `blocked_reason` also sit in the plan's frontmatter. `tickets.py mark` writes them. Given a leaf with no plan, it creates the plan with only its frontmatter.

| Status | Written by |
|---|---|
| `draft`, `ready-for-dev`, `in-progress`, `in-review`, `built` | `bmad-build`, `bmad-build-auto` as they work |
| `blocked` | `bmad-build-auto` when it halts, with the reason in the plan |
| `done` | the user, or an orchestrator, through `tickets.py mark` |
| `dropped` | `bmad-ticket`, when the user says so |

- An open ticket follows its type's template. A done or dropped ticket and its plan are the record: no skill reshapes them to a newer template.
- No skill moves a ticket past `built`: the build's last status, meaning the build finished and nobody has called it done. When the user tells a skill that a ticket is done, the skill runs `mark` for them. `bmad-code-review` never changes `status`.

## Baseline

- Before any code change, the build records `baseline_revision` in the plan: the commit the work starts from. It keeps an existing value when it resumes. The field is called `baseline_revision` in both build skills.
- Code review diffs from `baseline_revision`. The retrospective reads it to find each ticket's changes.

## Where review and retrospective write

- `bmad-code-review` appends a `## Code Review` section to the reviewed ticket's plan. Each run adds a dated block with that run's findings. Deferred findings go into the same block.
- `bmad-retrospective` writes `epic-<slug>-retrospective.md` in the epic folder, with `verdict` in its frontmatter. It does not edit the epic file. Closing the epic is still `bmad-ticket`'s closure check, confirmed by the user.
`````

---

## File: skills/bmad-ticket/references/validate.md

`````markdown
# Validating tickets

Run `{workflow.checks}` through agents that were not in this conversation; each gets only its scope's arrays. When `checks.dependencies` resolves empty, stop and tell the user: an override file sets `checks` as one string, which replaces every shipped check; it must be rewritten as `checks.<scope>` arrays. `tickets.py` reads only the prerequisites that were written; one that is missing is for these agents to find.

- **A proposed set of epics, a proposed epic breakdown, or a re-slice:** on the draft, before the user is asked to approve it. This always runs; no path, mode, or setting skips it. First walk the draft through each of `checks.dependencies` yourself and fix what that finds, then give the agent the corrected draft as text. Present the draft with what the check changed and what it confirmed. Run it again when the user's changes add, remove, merge, or reorder items. At initiative slicing, check the tree's scope ownership without requiring stories or full detail in future epics.
- **A refined ticket:** before execution.
- **A container:** before closure, its implemented coverage and Done when.

How:

- One subagent per epic: its container, the draft breakdown or `tickets.toml`, every child ticket including completed work, requirement source and companions, and `checks.ticket`, `checks.set`, `checks.dependencies`. One subagent for the tree: the initiative, the draft or written epic envelopes, source, and `checks.tree`, `checks.dependencies`. A single ticket: one subagent with its source and `checks.ticket`. Closure: `checks.closure`.
- Give each agent the other `{workflow}` keys its checks rest on and say which tickets carry full criteria (`refined: true`); once `tickets.toml` exists, give the full `tickets.py status` command for the folder.
- Merge findings into fix (mechanical), suggest (a guideline, with its reason), or ask (needs the user). Resolve coverage gaps, missing prerequisites, and contradictions before proceeding; the user decides suggestions and scope changes.
- A declined suggestion recorded as a `Decision:` line is not raised again unless new evidence changes its basis.

The user may request any scope independently.
`````

---

## File: skills/bmad-ticket/scripts/read_toml.py

`````python
#!/usr/bin/env python3
# /// script
# requires-python = ">=3.11"
# ///
"""Read named keys from a TOML file without loading the rest into context."""

import argparse
import json
import sys
import tomllib
from pathlib import Path

sys.dont_write_bytecode = True

_MISSING = object()


def load(source: str) -> dict:
    return tomllib.loads(Path(source).expanduser().read_text(encoding="utf-8"))


def extract(data, dotted: str):
    current = data
    for part in dotted.split("."):
        if isinstance(current, dict) and part in current:
            current = current[part]
        else:
            return _MISSING
    return current


def main() -> int:
    parser = argparse.ArgumentParser(description="Print selected keys from a TOML file.")
    parser.add_argument("--file", "-f", required=True, help="Path of the TOML file")
    parser.add_argument(
        "--key", "-k", action="append", default=[], help="Dotted key (repeatable). Omit for the whole file as JSON."
    )
    args = parser.parse_args()

    try:
        data = load(args.file)
    except Exception as error:  # noqa: BLE001 — any read or parse failure is reported the same way
        sys.stderr.write(f"error: cannot read {args.file}: {error}\n")
        return 1

    if not args.key:
        print(json.dumps(data, indent=2, ensure_ascii=False))
        return 0

    found = {}
    missing = []
    for key in args.key:
        value = extract(data, key)
        (missing.append(key) if value is _MISSING else found.__setitem__(key, value))
    for key in missing:
        sys.stderr.write(f"missing: {key}\n")

    if len(args.key) == 1:
        if missing:
            return 2
        value = found[args.key[0]]
        print(value.rstrip("\n") if isinstance(value, str) else json.dumps(value, indent=2, ensure_ascii=False))
    else:
        print(json.dumps(found, indent=2, ensure_ascii=False))
    return 2 if missing else 0


if __name__ == "__main__":
    if sys.platform == "win32":
        # Piped output on Windows defaults to a legacy code page, not UTF-8.
        sys.stdout.reconfigure(encoding="utf-8")
        sys.stderr.reconfigure(encoding="utf-8")
    raise SystemExit(main())
`````

---

## File: skills/bmad-ticket/scripts/tickets.py

`````python
#!/usr/bin/env python3
# /// script
# requires-python = ">=3.11"
# ///
"""tickets — read a ticket tree and answer what is next.

A container folder holds its ticket file, `tickets.toml`, flat leaf files named `<type>-<slug>.md`,
and the builds' plan files. An epic's `tickets.toml` lists its planned leaves as `[[entry]]` tables
(`id`, `type`, `title`, `after`, and whatever else the plan records); an initiative's lists its
epics as `[[epic]]` tables (`id`, `slug`, `after = [{epic, needs}]`). Tables are in build order.
`id` names an entry for good and is never reused; a leaf file carries it in frontmatter, which is
how the file joins its entry. An entry needs no leaf file to start. A ticket needs refining before
it starts when it is a bug, its entry says `refine = true`, or it has no entry. A leaf file's frontmatter
adds `tracker_status` and `refined`; its `after` and `hitl` replace the entry's, absent reading as empty,
and a difference from the entry is `drift`.

A plan is any other `.md` whose frontmatter has `ticket` and whose `type` is not a leaf type. An
integer `ticket` joins the entry with that id in the plan's folder; a string joins the leaf file
with that stem (a backlog leaf). A plan is never a row of its own. It holds the ticket's `status`,
`assignee`, `blocked_at`, and `blocked_reason`; a leaf file's own fields are read only when the
ticket has no plan. A plan whose `ticket` names nothing is skipped, and one whose `status` is unknown
blocks its ticket; next and status list both under `problems`.

`status` is draft, ready-for-dev, in-progress, in-review, or built from the builds (built is their
last: the build finished and nobody has called it done), blocked from build-auto, done from the
user or an orchestrator through mark, or dropped; absent means no build has started.
On a tracker store `tracker_status` mirrors the tracker's word (backlog, in-progress, review, done,
dropped). A ticket's `state` is `planned` with no file and no plan, else `tracker_status`, else
derived from `status`: absent, draft, ready-for-dev -> backlog; in-progress, blocked -> in-progress;
in-review, built -> review; done; dropped.

`after` lists real prerequisites: a sibling's id as a bare integer, or a quoted string that is
`<epic id>.<entry id>` for an entry in another epic of the same initiative, `epic-<slug>` for that
whole epic, a sibling's file name, or a tracker id. An epic file's own `after` names epics and holds every
ticket under it; rows show it as `gated_by`. A dropped prerequisite still blocks.

On an initiative, or an epic it lists, next and status report `unpinned_after` (a declared epic
`after` no entry of the waiting epic pins), `undeclared_after` (an entry's `after` into an epic its
own epic does not declare), and `order_conflict` (an epic that waits on one later in build order).
status's `epics` rows carry the declared `after` with its `needs`, and the epic file's own gate as
`gated_by`.

  next   [<dir>]                 tickets whose prerequisites are done or in review, grouped by state, in
                                 build order; an epic's own `after` waits for that epic to be done
  status [<dir>]                 every ticket in build order, what it blocks, counts by state, longest chain
  find   [<dir>] <ref>           the one ticket a reference names, with its entry's text fields and the
                                 absolute paths `epic_file`, `story_file` (null until pulled), and `plan`
                                 (where its plan is or goes)
  pull   <dir> <id>              write entry id's leaf file: `after` and `hitl` always, other fields only
                                 when the entry sets them; no status
  mark   [<dir>] <ref> <status> [--assignee <who>] [--blocked <reason>]
                                 set a ticket's status in its plan, creating a frontmatter-only plan when
                                 there is none; --blocked sets blocked_at and blocked_reason, else both are
                                 cleared (repo store only)

`<dir>` is an epic folder, a backlog folder, or an initiative folder (all its epics). `<ref>` is
`<epic id>.<entry id>`, an entry id inside an epic folder, a tracker id, a file name, or words
from the title that match one ticket. Each row of next, status, and find carries `ref`, a reference
find resolves in the folder the command ran on.
`--project-root` names the project holding `_bmad/` when the tickets live outside it. A relative
`<dir>` that is not a folder under the working directory is looked up under `{output_folder}`, then
the project root.

With no `<dir>`, next, status, find, and mark run on the active initiative, `{output_folder}/{active_initiative}`.
The project root is `--project-root`, else the first folder at or above the working directory that
holds `_bmad/`. `active_initiative` and `output_folder` (`[core]`) come from the
BMad config, merged by the project's `_bmad/scripts/config_utils.py`. `{project-root}` is
substituted, and a relative path is taken from the project root.

Output is one JSON object on stdout. Exit 0 on success, 1 on a malformed tree, 2 when
the store forbids the operation.
"""

import argparse
import codecs
import importlib.util
import json
import os
import re
import sys
import tomllib
import unicodedata
from datetime import date
from pathlib import Path

sys.dont_write_bytecode = True

STATUSES = ("draft", "ready-for-dev", "in-progress", "in-review", "built", "done", "blocked", "dropped")
STATES = ("backlog", "in-progress", "review", "done", "dropped")
CONTAINER_STATUSES = ("in-progress", "done", "dropped")
STATE_OF = {
    "": "backlog",
    "draft": "backlog",
    "ready-for-dev": "backlog",
    "in-progress": "in-progress",
    "blocked": "in-progress",
    "in-review": "review",
    "built": "review",
    "done": "done",
    "dropped": "dropped",
}
LEAF_TYPES = ("story", "spike", "bug")
CONTAINER_TYPES = ("initiative", "epic")
NAME_RE = re.compile(r"^(story|spike|bug)-(.+)\.md$")
CROSS_RE = re.compile(r"^(\d+)\.(\d+)$")
EPIC_RE = re.compile(r"^epic-[^/]+$")
BREAKDOWN = "tickets.toml"
QUOTED_COMMENT_RE = re.compile(r"""^("(?:[^"\\]|\\.)*"|'(?:[^']|'')*')\s+#.*$""")
FRONTMATTER_RE = re.compile(r"\A---\n(.*?)\n---(?:\n|\Z)", re.S)
PLAN_FIELDS = ("status", "assignee", "blocked_at", "blocked_reason")


class TicketError(Exception):
    pass


class StoreRefusal(Exception):
    pass


# ---------------------------------------------------------------- frontmatter


def parse_frontmatter(text: str, lenient: bool = False) -> dict:
    """Minimal YAML subset: `key: value`, lists as `[a, b]`, quoted or bare scalars. Lenient
    skips block lists instead of refusing them, for plans written from the build's template."""
    m = FRONTMATTER_RE.match(text)
    if not m:
        return {}
    data = {}
    for line in m.group(1).splitlines():
        if lenient and (line[:1].isspace() or line.startswith("- ")):
            continue
        if line.lstrip().startswith("- "):
            raise TicketError("frontmatter lists must be inline: `key: [a, b]`")
        if not line.strip() or line.lstrip().startswith("#") or ":" not in line:
            continue
        key, _, value = line.partition(":")
        value = value.split("   #")[0].strip()
        quoted = QUOTED_COMMENT_RE.match(value)
        data[key.strip()] = _scalar(quoted.group(1) if quoted else value)
    return data


def _scalar(value: str):
    if value.startswith("[") and value.endswith("]"):
        inner = value[1:-1].strip()
        return [] if not inner else [_scalar(v.strip()) for v in inner.split(",")]
    if len(value) >= 2 and value[0] == value[-1] and value[0] in "\"'":
        if value[0] == '"':
            try:
                return str(json.loads(value))
            except ValueError:
                pass
        return value[1:-1] if value[0] == '"' else value[1:-1].replace("''", "'")
    if value in ("true", "false"):
        return value == "true"
    if re.fullmatch(r"-?\d+", value):
        return int(value)
    return value


def set_frontmatter_value(text: str, key: str, value: str) -> str:
    m = FRONTMATTER_RE.match(text)
    if not m:
        raise TicketError("ticket has no frontmatter")
    block = m.group(1)
    pattern = re.compile(rf"^{re.escape(key)}:.*$\n?", re.M)
    if value == "":
        block = pattern.sub("", block).rstrip("\n")
    elif pattern.search(block):
        block = pattern.sub(lambda _: f"{key}: {value}\n", block, count=1).rstrip("\n")
    else:
        block = f"{block}\n{key}: {value}"
    return text[: m.start(1)] + block + text[m.end(1) :]


def _list(value, where: str) -> list:
    if value in (None, ""):
        return []
    if not isinstance(value, list):
        raise TicketError(f"{where}: after must be a list")
    return value


def _flag(value) -> bool:
    return str(value).lower() == "true"


def _one_of(value, allowed: tuple, where: str, field: str):
    """`value` when it is absent (`""`) or one of `allowed`; else the error naming them."""
    if value not in ("", *allowed):
        raise TicketError(f"{where}: {field} {value!r} is not one of {', '.join(allowed)}")
    return value


# ---------------------------------------------------------------- loading


def read_text(path: Path) -> str:
    # utf-8-sig: Windows editors can save a byte-order mark, which would hide the frontmatter.
    return path.read_text(encoding="utf-8-sig")


def load_breakdown(folder: Path) -> dict:
    path = folder / BREAKDOWN
    if not path.is_file():
        return {}
    where = f"{folder.name}/{BREAKDOWN}"
    try:
        data = tomllib.loads(read_text(path))
    except tomllib.TOMLDecodeError as e:
        raise TicketError(f"{where}: {e}") from e
    for table in ("entry", "epic"):
        rows = data.get(table, [])
        if not isinstance(rows, list) or not all(isinstance(r, dict) for r in rows):
            raise TicketError(f"{where}: write `[[{table}]]` tables, one per {table}")
        for r in rows:
            for key in ("covers", "after", "references", "notes"):
                if not isinstance(r.get(key, []), list):
                    raise TicketError(f"{where}: `{key}` must be a list")
            if _id(r.get("id")) is None:
                raise TicketError(f"{where}: every {table} needs an integer `id`")
            if table == "epic":
                if not isinstance(r.get("slug"), str) or not r["slug"]:
                    raise TicketError(f"{where}: epic {r['id']} needs a `slug`")
                for a in r.get("after", []):
                    if not isinstance(a, dict) or (_id(a.get("epic")) is None and not isinstance(a.get("epic"), str)):
                        raise TicketError(
                            f'{where}: an epic\'s `after` takes tables: [{{ epic = <id or slug>, needs = "..." }}]'
                        )
    return data


def _id(value):
    return value if isinstance(value, int) and not isinstance(value, bool) else None


def load_container(folder: Path) -> dict:
    path = folder / f"{folder.name}.md"
    if not path.is_file():
        raise TicketError(f"{folder.name}: no {path.name}")
    fm = parse_frontmatter(read_text(path))
    if fm.get("type") not in CONTAINER_TYPES:
        raise TicketError(
            f"{folder.name}/{path.name}: type {fm.get('type')!r} is not one of {', '.join(CONTAINER_TYPES)}"
        )
    status = _one_of(fm.get("status", ""), CONTAINER_STATUSES, f"{folder.name}/{path.name}", "status")
    return {
        "slug": folder.name,
        "tracker_id": str(fm.get("tracker_id", "") or ""),
        "status": status,
        "raw_after": _list(fm.get("after"), f"{folder.name}.md"),
    }


def load_folder(folder: Path, problems: list[str]) -> list[dict]:
    """One row per ticket in a folder, in build order: every breakdown entry, joined to its
    leaf file when one exists, then leaf files the breakdown does not list. Plans then set
    the status fields of the rows they join."""
    where = folder.name
    rows = {}
    for e in load_breakdown(folder).get("entry", []):
        n, kind = _id(e.get("id")), e.get("type")
        if n is None:
            raise TicketError(f"{where}/{BREAKDOWN}: every entry needs an integer `id`")
        if kind not in LEAF_TYPES:
            raise TicketError(f"{where}/{BREAKDOWN}: entry {n} type {kind!r} is not one of {', '.join(LEAF_TYPES)}")
        if n in rows:
            raise TicketError(f"{where}/{BREAKDOWN}: two entries with id {n}")
        rows[n] = {
            "epic": where,
            "id": n,
            "file": None,
            "type": kind,
            "tracker_id": "",
            "title": str(e.get("title", "")),
            "status": "",
            "tracker_status": "",
            "state": "planned",
            "assignee": "",
            "refined": False,
            "refine": kind == "bug" or _flag(e.get("refine", False)),
            "description": str(e.get("description", "")),
            "verify": str(e.get("verify", "")),
            "unknown": str(e.get("unknown", "")),
            "references": [str(v) for v in e.get("references", [])],
            "notes": [str(v) for v in e.get("notes", [])],
            "risk": str(e.get("risk", "")),
            "hitl": _flag(e.get("hitl", False)),
            "covers": [str(c) for c in e.get("covers", [])],
            "estimate": e.get("estimate", ""),
            "blocked_at": "",
            "blocked_reason": "",
            "raw_after": _list(e.get("after"), f"{where}/{BREAKDOWN} entry {n}"),
            "entry_after": None,
        }
    unlisted, stray, plans = {}, [], []
    seen = {}
    for path in sorted(folder.glob("*.md")):
        text = read_text(path)
        fm = parse_frontmatter(text, lenient=True)
        if not fm and text.startswith("---") and NAME_RE.match(path.name):
            raise TicketError(f"{where}/{path.name}: frontmatter does not close")
        if fm.get("type") not in LEAF_TYPES:
            if "ticket" in fm:
                plans.append((path.name, fm))
            continue
        try:
            fm = parse_frontmatter(text)
        except TicketError as e:
            raise TicketError(f"{where}/{path.name}: {e}") from e
        status = _one_of(fm.get("status", ""), STATUSES, f"{where}/{path.name}", "status")
        tracker_status = _one_of(fm.get("tracker_status", ""), STATES, f"{where}/{path.name}", "tracker_status")
        n = _id(fm.get("id"))
        if n is not None:
            if n in seen:
                raise TicketError(f"{seen[n]} and {path.name} share the id {n}")
            seen[n] = path.name
        row = rows.get(n) if n is not None else None
        if row is None:
            row = {"epic": where, "id": n, "raw_after": [], "entry_after": None, "covers": [], "title": ""}
            row["refine"] = True
            if n is None:
                stray.append(row)
            else:
                unlisted[n] = row
        else:
            row["entry_after"] = row["raw_after"]
            row["entry_hitl"] = row["hitl"]
        row.update(
            {
                "file": path.name,
                "type": fm.get("type"),
                "tracker_id": str(fm.get("tracker_id", "") or ""),
                "title": str(fm.get("title", "") or row["title"]),
                "status": status,
                "tracker_status": tracker_status,
                "state": tracker_status or STATE_OF[status],
                "assignee": str(fm.get("assignee", "") or ""),
                "refined": _flag(fm.get("refined", False)),
                "refine": row["refine"] or fm.get("type") == "bug",
                "hitl": _flag(fm.get("hitl", False)),
                "covers": [str(c) for c in fm["covers"]] if isinstance(fm.get("covers"), list) else row["covers"],
                "estimate": fm.get("estimate", row.get("estimate", "")),
                "blocked_at": fm.get("blocked_at", ""),
                "blocked_reason": str(fm.get("blocked_reason", "") or ""),
            }
        )
        row["raw_after"] = _list(fm.get("after", []), f"{where}/{path.name}")
    out = list(rows.values()) + [unlisted[n] for n in sorted(unlisted)] + stray
    join_plans(out, plans, where, problems)
    return out


def join_plans(rows: list[dict], plans: list[tuple[str, dict]], where: str, problems: list[str]) -> None:
    """Set each plan's status fields on the one row its `ticket` names; a bad plan is a problem, not an error."""
    for name, fm in plans:
        ticket = fm["ticket"]
        if isinstance(ticket, str) and ticket.isascii() and ticket.isdigit():
            ticket = int(ticket)
        if _id(ticket) is not None:
            row = next((r for r in rows if r["id"] == ticket), None)
        elif isinstance(ticket, str) and ticket:
            row = next((r for r in rows if r["file"] == f"{ticket}.md"), None)
        else:
            row = None
        if row is None:
            problems.append(f"{where}/{name}: ticket {ticket!r} names no entry or leaf file in {where}; skipped")
            continue
        if "plan" in row:
            raise TicketError(f"{where}/{row['plan']} and {name} are both plans for ticket {ticket!r}")
        fields = {k: str(fm.get(k, "") or "") for k in PLAN_FIELDS}
        try:
            _one_of(fields["status"], STATUSES, f"{where}/{name}", "status")
        except TicketError as e:
            problems.append(f"{e}; the ticket reads as blocked until the plan is fixed")
            fields.update(status="blocked", blocked_reason=f"{name} has an unknown status {fields['status']!r}")
        row.update({"plan": name, "state": row["tracker_status"] or STATE_OF[fields["status"]], **fields})


def epic_folders(initiative: Path) -> list[Path]:
    return sorted(
        d
        for d in initiative.glob("epic-*")
        if d.is_dir() and ((d / f"{d.name}.md").is_file() or (d / BREAKDOWN).is_file())
    )


def load_tree(folder: Path) -> dict:
    """The folder asked about plus every epic its tickets can name."""
    epics = epic_folders(folder)
    if epics or "epic" in load_breakdown(folder):
        scope, initiative, folders = None, folder, None
    elif folder in epic_folders(folder.parent):
        scope, initiative, folders = folder.name, folder.parent, epic_folders(folder.parent)
        epics = folders
    else:
        scope, initiative, folders = folder.name, None, [folder]
    listed = load_breakdown(initiative).get("epic", []) if initiative else []
    order = [e.get("slug") for e in listed]
    epics.sort(key=lambda d: (order.index(d.name) if d.name in order else len(order), d.name))
    if folders is None:
        folders = [*epics, folder]
    epic_ids = {}
    for e in listed:
        if e["id"] in epic_ids.values():
            raise TicketError(f"{initiative.name}/{BREAKDOWN}: two epics with id {e['id']}")
        if e["slug"] in epic_ids:
            raise TicketError(f"{initiative.name}/{BREAKDOWN}: two epics with slug {e['slug']}")
        epic_ids[e["slug"]] = e["id"]
    problems = []
    tickets = [t for f in folders for t in load_folder(f, problems)]
    for t in tickets:
        t["key"] = f"{t['epic']}/{t['id']}" if t["id"] is not None else f"{t['epic']}/{t['file']}"
    tree = {
        "scope": scope,
        "initiative": initiative,
        "folders": {f.name: f for f in folders},
        "epic_ids": epic_ids,
        "containers": {f.name: load_container(f) for f in epics},
        "tickets": tickets,
        "problems": problems,
    }
    _resolve(tree)
    _check_cycles(tickets, tree["containers"])
    return tree


def _resolve(tree: dict) -> None:
    tickets, containers = tree["tickets"], tree["containers"]
    by_key = {t["key"]: t for t in tickets}
    slugs = {i: slug for slug, i in tree["epic_ids"].items()}
    ids = {c["tracker_id"]: slug for slug, c in containers.items() if c["tracker_id"]}
    ids.update({t["tracker_id"]: t["key"] for t in tickets if t["tracker_id"]})

    def sibling(t, ref, where):
        """An integer is always a sibling's id; a string is never one."""
        mates = [o for o in tickets if o["epic"] == t["epic"]]
        if _id(ref) is not None:
            hit = next((o for o in mates if o["id"] == ref), None)
            if hit is None:
                raise TicketError(f"{where}: after {ref!r} names no entry in {t['epic']}")
            return hit["key"]
        for o in mates:
            if o["file"] and ref in (o["file"], o["file"][:-3]):
                return o["key"]
        return None

    def resolve(t, refs, where):
        keys = []
        for ref in refs:
            text = str(ref)
            key = sibling(t, ref, where) if "id" in t else None
            m = CROSS_RE.match(text)
            if key is None and m:
                slug = slugs.get(int(m.group(1)))
                if slug is None:
                    raise TicketError(f"{where}: after {ref!r} names no epic id in this initiative's {BREAKDOWN}")
                key = f"{slug}/{int(m.group(2))}"
                if key not in by_key:
                    raise TicketError(f"{where}: after {ref!r} names no entry in {slug}")
            if key is None and EPIC_RE.match(text):
                if text not in containers:
                    raise TicketError(f"{where}: after {ref!r} names no epic in this initiative")
                key = text
            if key is None:
                key = ids.get(text)
            if key is None and NAME_RE.match(text if text.endswith(".md") else f"{text}.md"):
                raise TicketError(
                    f"{where}: after {ref!r} matches no ticket in {t['epic']}; a file name names a pulled ticket in the "
                    "same folder only: use the entry's id, or move a backlog ticket into the epic as an entry"
                )
            if key is None:
                raise TicketError(f"{where}: after {ref!r} matches no ticket")
            if key not in keys:
                keys.append(key)
        return keys

    for t in tickets:
        where = f"{t['epic']}/{t['file']}" if t["file"] else f"{t['epic']}/{BREAKDOWN} entry {t['id']}"
        t["after"] = resolve(t, t.pop("raw_after"), where)
        planned = t.pop("entry_after")
        t["gated_by"] = []
        entry_hitl = t.pop("entry_hitl", None)
        t["drift"] = planned is not None and (
            sorted(resolve(t, planned, where)) != sorted(t["after"]) or entry_hitl != t["hitl"]
        )
    for slug, c in containers.items():
        gates = resolve({"epic": slug}, c.pop("raw_after"), f"{slug}.md")
        c["after"] = gates
        for t in tickets:
            if t["epic"] == slug:
                t["gated_by"] = gates


def _check_cycles(tickets: list[dict], containers: dict) -> None:
    sys.setrecursionlimit(max(1000, 3 * len(tickets) + 100))
    graph = {t["key"]: t["after"] + t["gated_by"] for t in tickets}
    members = {}
    for t in tickets:
        members.setdefault(t["epic"], []).append(t["key"])
    for slug, c in containers.items():
        members.setdefault(slug, []).extend(c["after"])
    state = {}

    def visit(node, path):
        if state.get(node) == "done":
            return
        if state.get(node) == "active":
            raise TicketError("cycle through " + " -> ".join(path + [node]))
        state[node] = "active"
        for b in graph.get(node, members.get(node, [])):
            visit(b, path + [node])
        state[node] = "done"

    for node in graph:
        visit(node, [])


# ---------------------------------------------------------------- views


def done_keys(tree: dict) -> set:
    done = {t["key"] for t in tree["tickets"] if t["state"] == "done"}
    return done | {slug for slug, c in tree["containers"].items() if c["status"] == "done"}


def in_scope(tree: dict) -> list[dict]:
    return [t for t in tree["tickets"] if tree["scope"] in (None, t["epic"])]


def classify(tree: dict) -> dict:
    done = done_keys(tree)
    # A ticket in review meets an `after`; an epic gate still waits for the epic to be done.
    met = done | {t["key"] for t in tree["tickets"] if t["state"] == "review"}
    groups = {"ready_to_refine": [], "ready_to_start": [], "in_progress": [], "blocked": []}
    for t in in_scope(tree):
        s = t["state"]
        if s in ("done", "dropped"):
            continue
        if t["status"] == "blocked" or t["blocked_at"]:
            groups["blocked"].append(t)
        elif s in ("in-progress", "review"):
            groups["in_progress"].append(t)
        elif not (all(b in met for b in t["after"]) and all(b in done for b in t["gated_by"])):
            groups["blocked"].append(t)
        elif t["refine"] and not t["refined"]:
            groups["ready_to_refine"].append(t)
        else:
            groups["ready_to_start"].append(t)
    return groups


def longest_remaining_chain(tree: dict) -> list[str]:
    remaining = {t["key"]: t for t in tree["tickets"] if t["state"] not in ("done", "dropped")}
    memo = {}

    def chain(k):
        if k in memo:
            return memo[k]
        best = []
        for b in remaining[k]["after"] + remaining[k]["gated_by"]:
            if b in remaining:
                c = chain(b)
                if len(c) > len(best):
                    best = c
        memo[k] = best + [k]
        return memo[k]

    longest = []
    for t in in_scope(tree):
        if t["key"] in remaining:
            c = chain(t["key"])
            if len(c) > len(longest):
                longest = c
    return [ref(k, None, tree) for k in longest]


def ref(key: str, epic: str | None, tree: dict) -> str | int:
    """A key as the plan writes it: a sibling's id, `<epic id>.<id>` elsewhere, an epic's slug."""
    slug, _, n = key.partition("/")
    if not n:
        return slug
    if slug == epic and n.isdigit():
        return int(n)
    if n.isdigit() and slug in tree["epic_ids"]:
        return f"{tree['epic_ids'][slug]}.{n}"
    return key


def declared_after(tree: dict) -> dict:
    """Each epic the initiative's `tickets.toml` lists, in build order, with its `after` as `[{epic, needs}]`."""
    if not tree["initiative"]:
        return {}
    listed = load_breakdown(tree["initiative"]).get("epic", [])
    slugs = [e["slug"] for e in listed]
    by_id = {i: slug for slug, i in tree["epic_ids"].items()}
    out = {}
    for e in listed:
        out[e["slug"]] = []
        for a in e.get("after", []):
            needed = by_id.get(a.get("epic"), a.get("epic"))
            if needed not in slugs:
                raise TicketError(
                    f"{tree['initiative'].name}/{BREAKDOWN}: {e['slug']} is after {a.get('epic')!r}, which is no epic listed"
                )
            out[e["slug"]].append({"epic": needed, "needs": a.get("needs", "")})
    return out


def unpinned_after(tree: dict, declared: dict) -> list[dict]:
    """Declared `after` lines whose waiting epic has tickets but none waiting on the named epic."""
    out = []
    for slug, edges in declared.items():
        mine = [t for t in tree["tickets"] if t["epic"] == slug]
        for a in edges:
            needed = a["epic"]
            pinned = any(b == needed or b.startswith(f"{needed}/") for t in mine for b in t["after"] + t["gated_by"])
            if mine and not pinned and tree["scope"] in (None, slug):
                out.append({"epic": slug, "after": needed, "needs": a["needs"]})
    return out


def cross_epic_after(tree: dict, declared: dict) -> dict:
    """`undeclared_after`: an entry's `after` into an epic its epic does not declare. `order_conflict`: an epic
    that waits, declared or through its tickets, on an epic later in the initiative's build order."""
    order = list(declared)
    undeclared, conflicts = [], []

    def conflict(epic, needed):
        pair = {"epic": epic, "after": needed}
        if order.index(needed) > order.index(epic) and pair not in conflicts and tree["scope"] in (None, epic):
            conflicts.append(pair)

    for slug, edges in declared.items():
        for a in edges:
            conflict(slug, a["epic"])
    for slug, c in tree["containers"].items():
        for b in c["after"]:
            if slug in declared and b.partition("/")[0] in declared:
                conflict(slug, b.partition("/")[0])
    for t in tree["tickets"]:
        if t["epic"] not in declared:
            continue
        allowed = {a["epic"] for a in declared[t["epic"]]}
        for b in t["after"]:
            needed = b.partition("/")[0]
            if needed == t["epic"] or needed not in declared:
                continue
            conflict(t["epic"], needed)
            if needed not in allowed and tree["scope"] in (None, t["epic"]):
                undeclared.append(
                    {"epic": t["epic"], "after": needed, "ref": row_ref(t, tree), "names": ref(b, t["epic"], tree)}
                )
    return {"undeclared_after": undeclared, "order_conflict": conflicts}


def row_ref(t: dict, tree: dict) -> str | None:
    """What `find` resolves to this ticket in the folder the command ran on; never the title,
    which can repeat across epics."""
    if t["id"] is not None and t["epic"] in tree["epic_ids"]:
        return f"{tree['epic_ids'][t['epic']]}.{t['id']}"
    if t["id"] is not None and t["epic"] == tree["scope"]:
        return str(t["id"])
    return t["file"]


def public(t: dict, tree: dict, blocks: dict | None = None) -> dict:
    row = {
        k: t[k]
        for k in (
            "epic",
            "id",
            "file",
            "type",
            "tracker_id",
            "title",
            "status",
            "tracker_status",
            "state",
            "assignee",
            "hitl",
            "covers",
            "estimate",
            "refine",
            "refined",
            "blocked_at",
            "blocked_reason",
        )
    }
    row["ref"] = row_ref(t, tree)
    row["after"] = [ref(b, t["epic"], tree) for b in t["after"]]
    if t["gated_by"]:
        row["gated_by"] = t["gated_by"]
    if blocks is not None:
        row["blocks"] = [ref(b, t["epic"], tree) for b in blocks.get(t["key"], [])]
        if t["drift"]:
            row["drift"] = True
    return row


# ---------------------------------------------------------------- store


def find_project_root(start: Path) -> Path | None:
    for p in [start, *start.parents]:
        if (p / "_bmad").is_dir():
            return p
    return None


def project_root_for(args, start: Path) -> Path | None:
    return Path(args.project_root).resolve() if args.project_root else find_project_root(start)


def store_config(project_root: Path | None) -> dict:
    """The `[tickets]` table of the project's store config, empty when there is none."""
    if not project_root:
        return {}
    cfg = project_root / "_bmad" / "custom" / "ticketing-store-config.toml"
    if not cfg.is_file():
        return {}
    tickets = tomllib.loads(read_text(cfg)).get("tickets", {})
    return tickets if isinstance(tickets, dict) else {}


def store_name(project_root: Path | None) -> str:
    return store_config(project_root).get("store", "repo")


def central_config(project_root: Path) -> dict:
    """The BMad config with its layers merged by the project's own `config_utils.py`."""
    path = project_root / "_bmad" / "scripts" / "config_utils.py"
    if not path.is_file():
        raise TicketError(f"cannot read the BMad config: {path} is missing")
    spec = importlib.util.spec_from_file_location("bmad_config_utils", path)
    module = importlib.util.module_from_spec(spec)
    spec.loader.exec_module(module)
    try:
        return module.load_central_config(project_root)
    except module.ConfigError as e:
        raise TicketError(str(e)) from e


def tickets_root(project_root: Path, config: dict | None = None) -> Path:
    """`{output_folder}` for the project: the ticket tree lives beside the documents."""
    config = central_config(project_root) if config is None else config
    core = config.get("core", {})
    output = str(core.get("output_folder", "") if isinstance(core, dict) else "")
    output = output.replace("{project-root}", str(project_root))
    return project_root / output


def active_initiative(project_root: Path) -> Path:
    """`{output_folder}/{active_initiative}` for the project."""
    config = central_config(project_root)
    core = config.get("core", {})
    name = core.get("active_initiative") if isinstance(core, dict) else None
    if not isinstance(name, str) or not name.strip():
        raise TicketError(
            "no active initiative: set core.active_initiative in _bmad/custom/config.user.toml, or pass a folder"
        )
    folder = (tickets_root(project_root, config) / name.strip()).resolve()
    if not folder.is_dir():
        raise TicketError(f"active initiative folder not found: {folder}")
    return folder


# ---------------------------------------------------------------- commands


def _folder(args) -> Path:
    if args.dir is None:
        root = project_root_for(args, Path.cwd())
        if root is None:
            raise TicketError("no project root found: no _bmad/ at or above the working directory; pass --project-root")
        # The store is then read from this project even when output_folder lies outside it.
        args.project_root = str(root)
        return active_initiative(root)
    folder = Path(args.dir).resolve()
    root = None if folder.is_dir() or Path(args.dir).is_absolute() else project_root_for(args, Path.cwd())
    if root is not None:
        try:
            bases = [tickets_root(root), root]
        except TicketError:  # no BMad config to name the store: the project root alone
            bases = [root]
        for base in bases:
            if (base / args.dir).is_dir():
                return (base / args.dir).resolve()
    if not folder.is_dir():
        raise TicketError(f"not a folder: {folder}")
    return folder


def cmd_next(args) -> dict:
    folder = _folder(args)
    store = store_name(project_root_for(args, folder))
    if store != "repo" and not args.synced:
        raise StoreRefusal(f"store is {store}: sync ticket status from the tracker first, then rerun with --synced")
    tree = load_tree(folder)
    declared = declared_after(tree)
    return {
        "folder": folder.name,
        "store": store,
        **{k: [public(t, tree) for t in v] for k, v in classify(tree).items()},
        "unpinned_after": unpinned_after(tree, declared),
        **cross_epic_after(tree, declared),
        **({"problems": tree["problems"]} if tree["problems"] else {}),
    }


def cmd_status(args) -> dict:
    folder = _folder(args)
    tree = load_tree(folder)
    declared = declared_after(tree)
    tickets = in_scope(tree)
    counts = {}
    for t in tickets:
        counts[t["state"]] = counts.get(t["state"], 0) + 1
    blocks = {}
    for t in tree["tickets"]:
        for b in t["after"]:
            blocks.setdefault(b, []).append(t["key"])
    out = {
        "folder": folder.name,
        "store": store_name(project_root_for(args, folder)),
        "tickets": [public(t, tree, blocks) for t in tickets],
        "counts": {"total": len(tickets), **counts},
        "longest_remaining_chain": longest_remaining_chain(tree),
        "unpinned_after": unpinned_after(tree, declared),
        **cross_epic_after(tree, declared),
        **({"problems": tree["problems"]} if tree["problems"] else {}),
    }
    if tree["scope"] is None:
        out["epics"] = [
            {
                "slug": slug,
                "id": tree["epic_ids"].get(slug),
                "status": c["status"],
                "after": declared.get(slug, []),
                "gated_by": c["after"],
                "blocks": [ref(b, None, tree) for b in blocks.get(slug, [])],
            }
            for slug, c in tree["containers"].items()
        ]
    return out


PULLED = """---
{frontmatter}
---

# {heading}

## Description

{description}

## Acceptance Criteria

Verify: {verify}

## References

- parent — {parent}
{references}{notes}"""


def title_slug(title: str) -> str:
    title = unicodedata.normalize("NFKD", title).encode("ascii", "ignore").decode("ascii")
    return re.sub(r"[^a-z0-9]+", "-", title.lower()).strip("-")[:60].rstrip("-") or "untitled"


def resolve_ticket(tree: dict, text: str) -> dict:
    """The one row a reference names; see `<ref>` above."""
    tickets = tree["tickets"]
    ref = text.strip()
    if not ref:
        raise TicketError("the ticket reference is empty")
    low = ref.lower()
    hits = []
    m = CROSS_RE.match(ref)
    if m:
        slug = {i: s for s, i in tree["epic_ids"].items()}.get(int(m.group(1)))
        hits = [t for t in tickets if slug and t["epic"] == slug and t["id"] == int(m.group(2))]
    elif ref.isdigit() and tree["scope"]:
        hits = [t for t in tickets if t["epic"] == tree["scope"] and t["id"] == int(ref)]
    for pool in (in_scope(tree), tickets):
        if not hits:
            hits = [t for t in pool if t["file"] and low in (t["file"].lower(), t["file"][:-3].lower())]
    if not hits:
        hits = [t for t in tickets if t["tracker_id"] and low == t["tracker_id"].lower()]
    if not hits and not ref.isdigit():
        hits = [t for t in in_scope(tree) if low in t["title"].lower()]
    if not hits:
        raise TicketError(f"no ticket matches {ref!r}")
    if len(hits) > 1:
        names = ", ".join(ref_name(t, tree) for t in hits)
        raise TicketError(f"{ref!r} matches more than one ticket: {names}")
    return hits[0]


def leaf_stem(t: dict, tree: dict) -> str:
    """The leaf file's stem, or the one `pull` gives it: `<type>-<slug of the title>`, with `-<id>` added when a
    file or an earlier entry in the folder already has that name."""
    if t["file"]:
        return t["file"][:-3]
    stem = f"{t['type']}-{title_slug(t['title'])}"
    taken = set()
    for o in tree["tickets"]:
        if o is t:
            break
        if o["epic"] == t["epic"] and not o["file"]:
            taken.add(f"{o['type']}-{title_slug(o['title'])}")
    taken |= {o["file"][:-3] for o in tree["tickets"] if o["epic"] == t["epic"] and o["file"]}
    return f"{stem}-{t['id']}" if stem in taken else stem


def plan_path(t: dict, tree: dict) -> Path:
    """The joined plan, else `<leaf stem>-plan.md`."""
    folder = tree["folders"][t["epic"]]
    if t.get("plan"):
        return folder / t["plan"]
    return folder / f"{leaf_stem(t, tree)}-plan.md"


def cmd_find(args) -> dict:
    folder = _folder(args)
    tree = load_tree(folder)
    t = resolve_ticket(tree, args.ref)
    home = tree["folders"][t["epic"]]
    return {
        **public(t, tree),
        "folder": home.name,
        # Unlisted and stray leaves have no entry, so no entry text.
        "description": t.get("description", ""),
        "verify": t.get("verify", ""),
        "references": t.get("references", []),
        "notes": t.get("notes", []),
        "unknown": t.get("unknown", ""),
        "epic_file": str(home / f"{t['epic']}.md") if t["epic"] in tree["containers"] else None,
        "story_file": str(home / t["file"]) if t["file"] else None,
        "plan": str(plan_path(t, tree)),
    }


def ref_name(t: dict, tree: dict) -> str:
    return f"{t['file'] or t['id']} in {t['epic']}"


def cmd_pull(args) -> dict:
    folder = _folder(args)
    tree = load_tree(folder)
    t = next((t for t in in_scope(tree) if t["epic"] == folder.name and t["id"] == args.id), None)
    if t is None:
        raise TicketError(f"{folder.name}/{BREAKDOWN} has no entry {args.id}")
    if t["file"]:
        raise TicketError(f"entry {args.id} is already pulled: {t['file']}")
    path = folder / f"{leaf_stem(t, tree)}.md"
    if path.exists():
        raise TicketError(f"{path.name} exists already; change entry {args.id}'s title")
    after = [str(ref(b, t["epic"], tree)) for b in t["after"]]
    root = project_root_for(args, folder)
    epic_file = folder / f"{folder.name}.md"
    try:
        parent = Path(os.path.relpath(epic_file, root)).as_posix() if root else epic_file.as_posix()
    except ValueError:  # another drive on Windows
        parent = epic_file.as_posix()
    notes = ([f"Unknown: {t['unknown']}"] if t["unknown"] else []) + t["notes"]
    # Other empty fields are left out; `after` and `hitl` stay because `status` compares them with the entry.
    # No status: the build writes it when it starts.
    fields = [
        ("id", str(t["id"])),
        ("type", t["type"]),
        ("title", json.dumps(t["title"], ensure_ascii=False)),
        ("parent", t["epic"]),
        ("covers", f"[{', '.join(t['covers'])}]" if t["covers"] else ""),
        ("after", f"[{', '.join(after)}]"),
        ("refined", "false" if t["refine"] else ""),
        ("hitl", "true" if t["hitl"] else "false"),
        ("risk", t["risk"]),
        ("estimate", json.dumps(str(t["estimate"])) if t["estimate"] != "" else ""),
    ]
    path.write_text(
        PULLED.format(
            frontmatter="\n".join(f"{k}: {v}" for k, v in fields if v != ""),
            heading=t["title"],
            parent=parent,
            description=t["description"],
            verify=t["verify"],
            references="".join(f"- {r}\n" for r in t["references"]),
            notes="\n## Notes\n\n" + "".join(f"- {n}\n" for n in notes) if notes else "",
        ),
        encoding="utf-8",
    )
    return {"file": path.name, "refine": t["refine"]}


def quoted(value: str) -> str:
    """A double-quoted scalar that `parse_frontmatter` reads back exactly."""
    # parse_frontmatter cuts a value at "   #" and splits lines on these characters, so they go in as escapes.
    text = json.dumps(value, ensure_ascii=False).replace("   #", "   \\u0023")
    return text.translate({c: f"\\u{c:04x}" for c in (0x85, 0x2028, 0x2029)})


def cmd_mark(args) -> dict:
    """Write status, assignee, and the blocked fields to the ticket's plan; never to its leaf file."""
    folder = _folder(args)
    store = store_name(project_root_for(args, folder))
    if store != "repo":
        raise StoreRefusal(f"store is {store}: change status through the store's write verb, not this script")
    tree = load_tree(folder)
    t = resolve_ticket(tree, args.ref)
    path = plan_path(t, tree)
    blocked = {"blocked_at": "", "blocked_reason": ""}
    if args.blocked is not None:
        blocked = {"blocked_at": quoted(date.today().isoformat()), "blocked_reason": quoted(args.blocked)}
    created = not t.get("plan")
    if created:
        assignee = args.assignee if args.assignee is not None else t["assignee"]
        fields = [
            ("title", quoted(t["title"])),
            ("ticket", str(t["id"]) if t["id"] is not None else quoted(t["file"][:-3])),
            ("status", args.status),
            ("assignee", quoted(assignee) if assignee else ""),
            *blocked.items(),
        ]
        text = "---\n" + "".join(f"{k}: {v}\n" for k, v in fields if v != "") + "---\n"
    else:
        raw = path.read_bytes()
        text = set_frontmatter_value(raw.decode("utf-8-sig").replace("\r\n", "\n"), "status", args.status)
        for key, value in blocked.items():
            text = set_frontmatter_value(text, key, value)
        if args.assignee is not None:
            text = set_frontmatter_value(text, "assignee", quoted(args.assignee))
    data = text.encode("utf-8")  # before the file is opened, so a failure leaves the plan as it was
    if not created:
        # Write the plan back with its own line endings and byte-order mark.
        if b"\r\n" in raw:
            data = data.replace(b"\n", b"\r\n")
        if raw.startswith(codecs.BOM_UTF8):
            data = codecs.BOM_UTF8 + data
    try:
        with path.open("xb" if created else "wb") as f:
            f.write(data)
    except FileExistsError:
        raise TicketError(f"{path.name} exists already and is not the plan for {ref_name(t, tree)}") from None
    fm = parse_frontmatter(text, lenient=True)
    return {
        "plan": str(path),
        "created": created,
        **{k: fm.get(k, "") for k in PLAN_FIELDS},
    }


def main() -> int:
    parser = argparse.ArgumentParser(description="Read a ticket tree and answer what is next.")
    parser.add_argument(
        "--project-root",
        help="project holding _bmad/; default: walk up from the ticket folder, or the working directory with no folder",
    )
    sub = parser.add_subparsers(dest="command", required=True)
    p = sub.add_parser("next", help="tickets whose prerequisites are done or in review, by state")
    p.add_argument("dir", nargs="?", help="default: the active initiative")
    p.add_argument("--synced", action="store_true", help="tracker status was mirrored just now")
    p.set_defaults(func=cmd_next)
    p = sub.add_parser("status", help="every ticket resolved")
    p.add_argument("dir", nargs="?", help="default: the active initiative")
    p.set_defaults(func=cmd_status)
    p = sub.add_parser("find", help="the one ticket a reference names")
    p.add_argument("dir", nargs="?", help="default: the active initiative")
    p.add_argument("ref")
    p.set_defaults(func=cmd_find)
    p = sub.add_parser("pull", help="write an entry's leaf file")
    p.add_argument("dir")
    p.add_argument("id", type=int)
    p.set_defaults(func=cmd_pull)
    p = sub.add_parser("mark", help="set a ticket's status in its plan (repo store only)")
    p.add_argument("dir", nargs="?", help="default: the active initiative")
    p.add_argument("ref")
    p.add_argument("status", choices=STATUSES)
    p.add_argument("--assignee")
    p.add_argument(
        "--blocked", metavar="REASON", help="set blocked_at to today and blocked_reason; else both are cleared"
    )
    p.set_defaults(func=cmd_mark)
    args = parser.parse_args()
    try:
        print(json.dumps(args.func(args), ensure_ascii=False, default=str))
        return 0
    except StoreRefusal as e:
        print(json.dumps({"error": str(e)}), file=sys.stderr)
        return 2
    except (TicketError, OSError, ValueError) as e:
        print(json.dumps({"error": str(e)}), file=sys.stderr)
        return 1


if __name__ == "__main__":
    if sys.platform == "win32":
        # Piped output on Windows defaults to a legacy code page, not UTF-8.
        sys.stdout.reconfigure(encoding="utf-8")
        sys.stderr.reconfigure(encoding="utf-8")
    raise SystemExit(main())
`````

---

## File: skills/bmad-ticket/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
scripts = ["scripts/tickets.py"]
`````

---

## File: skills/bmad-ticket/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-ticket.
# Team overrides:     {project-root}/_bmad/custom/bmad-ticket.toml
# Personal overrides: {project-root}/_bmad/custom/bmad-ticket.user.toml

[workflow]

# --- Universal defaults. Merge: scalars override, arrays append. ---
activation_steps_prepend = []
activation_steps_append = []
persistent_facts = []   # `file:` entries are paths or globs under {project-root}; anything else is a fact verbatim
on_complete = ""

# What refining means. An epic's stories are not expanded to full acceptance criteria here: the builder
# questions the user and writes the criteria into its plan.
refinement = """
- Refining an epic's stories pulls the file when the entry has none, then reviews the file with the user: description, `Verify:`, references, notes, order, and prerequisites. Edits go to the file; order to `tickets.toml`; `after` to both, since the file's wins once it exists. It writes no Given/When/Then.
- Full acceptance criteria are written here only for a ticket with no epic, for a bug, and for an entry the user asks it for. That entry carries `refine = true`; a bug entry always does. Never propose it for a story.
"""

# When tickets publish to a tracker; on the repo store, committing the approved tickets.toml is the publish. auto: the whole
# agreed breakdown at inception, so the team sees the plan there; on_start: each ticket when it starts. The user can override.
publication = "auto" # auto | on_start | at_inception

# How to propose epic boundaries, offered while slicing an initiative. A team replaces this with its
# own rule, or points at where its boundaries are recorded — a url, AGENTS.md, a team map, anything
# the agent can read.
slice_to_epics = """
An epic is one capability from the source, or a tightly coupled pair, delivered to production by one owner: a dev or pair with agent lanes.
- Work that fits one epic is proposed as one epic; say so. Never offer an initiative without epics. When the user asks for tickets with no epic, do it and say once that the initiative folder fills with ticket files.
- Propose epics along the source's capabilities. Merge two when one owner and one module deliver both. Two epics need at most a contract between them; say what each needs from the ones before it.
- A unit (module, service, or bounded context) is an epic boundary when it is also the ownership or deployment boundary and its Done when still reads as something a product owner can check. A module that is only a code folder is not.
- A unit the work only consumes or configures gets no epic: it is a touch point, named in the initiative's Boundaries with the epic that owns the work there.
- No boundary applies: one epic. Split only for a distinct outcome, owner, or a part the user wants usable early, never for a ticket count. An epic whose boundary names more than one outcome or owner is two epics.
- The platform baseline (scaffold, environments, CI, deployment, operations) is the opening epic, or the first stories of the first epic. Every epic delivers to production; its Done when includes the integrated verification for what it delivers.
"""

# What a container (initiative or epic) must say at its altitude, and who decides it. Offered while
# authoring the initiative and completing the selected epic at inception. A team replaces the
# field guidance, the counts, or the split between the product owner and inception.
container_definition = """
The product owner and developer agree on the container's scope from the source. Complete the initiative before splitting it, and the selected epic before inception: constraints as references, known unknowns in Notes. Discuss unsettled fields rather than repeating decisions already made.
Inception defines the whole epic's ticket set, descriptions, verification approaches, dependencies, and integration points. A decision that binds several tickets in one epic is made at inception, or recorded as the `unknown` its entries wait on. A decision a second epic must adopt is an architecture decision: it lives in the architecture spine or the opening epic, never in a spike inside one of the epics that need it.
"""

# How to slice an epic into stories, at inception.
slice_to_tickets = """
A slice is one implementation step toward the epic's Done when, small enough that one agent session, starting from the ticket, its epic, and the source, plans and finishes it. It need not be user-visible on its own; the epic is the unit of value.
- The first ticket is the tracer bullet: the thinnest path through every layer the epic touches, proving they connect. Whatever setup that needs, including a starter the user runs, is a hitl step on that ticket. For it, and for anything the user wants demoable, the entry says what someone can see running when it is done.
- Contracts and stubs early: a boundary two lanes share gets its interface and a stub as its own slice, so both lanes open at once.
- Lanes: slices in one lane touch shared code in order; slices in different lanes never touch the same code. Two slices that would: one slice, or one blocks the other.
- Done is one runnable check. A slice whose check needs another slice's work belongs after it.
- Too small when setup outweighs the work: merge. An epic that is itself one session's work gets one slice.
- Eight to twelve slices is typical, not a limit. Fifteen can be right when they are one lane with one owner; six can be too many when two owners are inside. Past the typical range, say so and offer a split; the user decides.
- Split, never shrink: "for now", "placeholder", "simplified", "wired later" is a second slice.
- After the tracer bullet, the slice the user is least sure of.
"""

# Ordering guidance offered while slicing and writing. A team tightens or replaces any of these in
# its override file.
ordering = """
- Put setup and each hitl step on the first ticket it blocks; with epics, that ticket is under the relevant epic, never loose under the initiative. Initiative-wide setup belongs to the opening epic.
- Offer an opening refactor when poor code quality or missing standards would make the epic's tickets hard.
- An epic of more than three entries gets a closing story "Refactor sweep", blocked by every other entry except a closing end-to-end suite. Its scope is set when it starts, from the build records and the review findings deferred during the epic. It takes cleanup only; scope pushed out of another story is a new story. Propose it by default; when the user declines, record a `Decision:` line in the epic's Notes.
- Tests are part of every ticket, never a ticket of their own — except one closing end-to-end suite across the epic, after the sweep, offered when the source has a test plan or the user wants one.
"""

acceptance_criteria = """
- Each criterion is one behavior someone can observe and check without having written the code.
- Each is false before this ticket and true after it, through this ticket's work alone.
- Given/When/Then at the level of behavior, not mechanics: "Given a signed-in user with an empty cart", never "click login, then click cart". What must be true, never how to build it; a criterion that names a function, file, or library is an implementation step.
- The rule, not an example: "rejects any quantity over stock on hand", not "rejects quantity 999". A literal only when the value is the requirement — a limit, a rounding rule, exact text.
- Cover the happy path, the boundaries, and the failure cases that matter. One criterion per rule, not per test case.
- Enough that building the wrong thing cannot pass, no more: usually three to eight. More means split, or the criteria became a test plan.
"""

# The risk and severity scales. A team replaces the scale or the floors here; how each store carries
# the fields is the `fields` global in the ticketing store config.
scoring = """
- Every ticket gets a risk. low: a mistake shows at once and a revert is clean. medium: a mistake can slip past review or is costly to unwind — shared code, caching, background jobs, per-environment config. high: schema migrations, data deletion or transformation, auth, payments, anything a revert cannot undo. Empty: not scored.
- High risk names one check outside the ticket's own criteria — a person who confirms, a suite beyond the ticket's tests, a monitor; named in Notes.
- A bug gets a severity. P0: outage, data loss, or security exposure — drop everything. P1: core function broken, no workaround. P2: impaired, a workaround exists. P3: cosmetic.
"""


prose = """
- Short declarative sentences, common words, the project's own vocabulary; the reader has only the ticket.
- Say each thing once; no invented terms, no filler, no metaphor.
- Offer a rewrite with the reason when a sentence breaks these; the user decides.
"""

# Validation checks, one array per scope, beyond the standards above (those are re-checked from their own keys).
# Arrays append: a team's override file adds checks to any scope and cannot remove these. An override that sets
# `checks` as one string replaces the whole table; write `checks.<scope> = [...]` instead. How they run is
# references/validate.md.
checks.ticket = [
  "Every file or link referenced exists and opens; no unfilled placeholder; every assumption is marked.",
  "Nothing contradicts the requirement source, its companions, the architecture, or a recorded decision.",
  "An entry has a description, known uncertainty, and a `verify` check someone other than the builder can run. Full criteria are required before execution only for a bug, an entry with `refine = true`, and a ticket with no epic.",
  "A ticket that needs full criteria and whose `covers` includes an id named in a `Source conflict:` line with no later `Decision:` settling it has `refined: false`.",
]

# The set: every entry in the epic's `tickets.toml` or draft breakdown, pulled or not.
checks.set = [
  "Together the tickets account for every requirement in the epic's spec, referenced numbered source, or Requirements, except scope deferred with the user. Each covers id exists there; several tickets may cover one id, each description saying which part it delivers.",
  "UX, architecture, constraints, and integration work are accounted for even when they lack ids. The combined results must satisfy the epic's Done when; citing an id alone is not coverage.",
  "Every `after` is a real prerequisite, those in other epics included, and none restates the order. Once `tickets.toml` exists, `tickets.py status` on the epic runs clean: no cycle, no `drift`, no `unpinned_after`, `undeclared_after`, or `order_conflict`; the tracer bullet and sequencing decisions are in the epic's Notes.",
  "Given the epic, an entry, and its references, a builder could write that story's acceptance criteria if it had to. If it could not, fix the epic's requirements or the references; the entry stays one sentence each for `description` and `verify`.",
  "An epic of more than three entries has the refactor sweep after all other work, or a `Decision:` line says why not.",
]

# The tree: the initiative and its epic envelopes, drafted or written.
checks.tree = [
  "Every initiative requirement has an accountable epic or an agreed deferral. Shared requirements say which part each epic delivers; no unexplained overlap.",
  "Each epic's covers cites parent ids. Every id it assigns in Requirements or a local spec maps to one of them, and a child citing a local id resolves through that map to a parent id; an epic with empty covers cites a source section on every line instead. Verify the map resolves; a non-empty covers is not coverage.",
  "Every unit the source touches is an epic, a touch point with an owning epic, or named out of scope.",
  "Every decision two or more epics must adopt has one home: a spine section, or an entry in the opening epic that the others are blocked by.",
  "Every epic envelope has Outcome, Done when, boundaries, upstream coverage, and references; a future epic needs neither a breakdown nor a completed spec. The initiative's `tickets.toml` lists every epic in build order, and each `after` names what is needed and from which epic. It opens with a platform-baseline epic, or a `Decision:` line says why not. Each epic's Done when includes production delivery.",
  "With epics, no leaf sits directly under the initiative.",
]

# Missing prerequisites, run with the set and with the tree. An item is an entry in the set and an epic in the tree;
# a prerequisite is an `after` on either.
checks.dependencies = [
  "Needs: list what must exist before each item can start and before its check can run: code, schema, setup, test tooling, fixtures, an entry point, a decision, an investigation's answer. The item builds it itself, or something in its `after` delivers it.",
  "Collisions: two items with no dependency path between them can run at the same time, so they share no code, config, schema, or setup. Where they would, one goes in the other's `after` or they merge.",
  "Shared setup: whatever more than one item needs (test harness, scaffold, schema, a shared component or contract, an integration) has one owner, the earliest item that needs it, and the others list it in `after`.",
  "Handoffs: where one item relies on another's output (a default, an interface, a link target), both descriptions say so, so neither builder invents its own.",
]

checks.closure = [
  "Verify the container's Done when against implementation evidence and its requirements, including constraints and companions. All known children done is evidence, not proof the parent is complete; explain any dropped work and confirm remaining scope with the user.",
]

# Ticket templates, one per type. They apply to tickets the agent writes; `tickets.py pull` writes a fixed layout.
# Swap a template to change the criteria format (the shipped one is
# numbered Given/When/Then; a table, a checklist) or add your own sections.
# Keep a home, under any name, for each section references/ticket.md refers to: Description,
# Acceptance Criteria, Boundaries (stories), References, and Notes; for containers also Outcome,
# Requirements, and Done when; Reproduction and Cause Hypothesis for bugs, Approach for spikes. `Decision:`, `Dropped:`, and `Estimate:` lines live in Notes.
# Spikes use "Verify:" lines instead of Given/When/Then, and a pulled story carries one Verify: line unless its entry has `refine = true`; both deliberate.
initiative_template = "{skill-root}/assets/initiative-template.md"
epic_template = "{skill-root}/assets/epic-template.md"
story_template = "{skill-root}/assets/story-template.md"
spike_template = "{skill-root}/assets/spike-template.md"
bug_template = "{skill-root}/assets/bug-template.md"

# Estimation, off by default. Points and t-shirts share one unit: a t-shirt is a range of summed
# story points. How it is used is references/estimate.md; how a store carries the field is its
# `fields` global. Calibration proposes changes to the rubric and map from closed tickets.
# Must stay last: a TOML table header ends the [workflow] key list above.
[workflow.estimation]
enabled = false
leaf_scale = [1, 2, 3, 5]
rubric = "1-2: an agent can do it and it is well understood. 3-5: heavy hitl guidance, or work a person must do."
tshirt = { XS = "1-3", S = "4-8", M = "9-20", L = "21-40", XL = "41+" }
`````

---

## File: skills/bmad-ticket/SKILL.md

`````markdown
---
name: bmad-ticket
description: Create and manage tickets at every level — slice an initiative into epics, break an epic into stories, write or refine a ticket, and run the board (publish, ready, move, assign, status, cancel). Use when the user says "Create a new initiative", "slice this", "split this up", "break this into stories", "incept this epic", "make a ticket", "refine this ticket", "what's ready", "status of a story", "publish ticket changes".
---

# BMad Ticket

## What you are here to do

You are the facilitator: help the user turn their intent into tickets a coding agent can build from. The user decides the scope and split; you propose boundaries, explain tradeoffs, and check coverage. Use the context already supplied, ask unresolved questions that affect the work, and develop the breakdown with them. When they delegate the thinking, investigate and self-review before presenting the result; keep assumptions and open questions visible.

At every altitude above the leaf the ideal shape is: intent (an idea, brief, PRD, intent.md) gets a container ticket, and that container is the spec at its altitude — its Requirements hold the source's lines as stable ids, informed by what else exists (an architecture spine, UX design, research), and its children are cut from them. So at any container: create its envelope if it is missing, then complete it from the source.

## Terms

- Container: an initiative or an epic — holds other tickets
- Leaf: a story, spike, or bug handed to an agent to implement. Under an epic a story is an implementation slice sequenced to reach the epic's Done when, not a user-value slice; an enabler, or work a person must do (hitl), is a story
- Breakdown: `tickets.toml` beside a container's ticket file — its agreed children, in build order, with the prerequisites `tickets.py` reads
- Entry: one planned leaf in a breakdown, with a stable `id`: description, requirement references, prerequisites (`after`), verification approach, and known uncertainty. It needs no file to be built: the build reads the entry and its epic. With no file and no plan its state is `planned`
- Pull: write an entry's leaf file with `tickets.py pull`, the first step of refining it or of publishing it to a tracker; from then on the file is truth. Starting a ticket needs no pull
- Refine: for an epic's stories, pull the file when the entry has none, then review it with the user: description, `Verify:`, references, notes, order, prerequisites. Given/When/Then is written here only for a bug, a ticket with no epic, an entry with `refine = true`, or on request; otherwise the builder plans story criteria from the epic's requirements, the entry's description, and its `Verify:` check
- Opening epic: the first `[[epic]]` in the initiative's breakdown
- Inception: plan the whole selected epic with the user and record it in the epic's breakdown
- hitl: boolean frontmatter field on a leaf; at least part needs a person
- store: the ticketing system of record — git-backed, a tracker, or both
- State: `planned`, `backlog`, `in-progress`, `review`, `done`, or `dropped` — what the board groups by and what a tracker sees; derived from a ticket's status fields as described under The ticket tree

## On activation

1. Resolve config: `uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root} --key core.output_folder --key core.active_initiative`.
   - Script not found: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.

   Tickets are drafted under `{output_folder}/{active_initiative}/` — an initiative folder, or a backlog folder scoped however the user wants. Unset: offer to create the initiative folder, or a backlog folder, and record it as `active_initiative` under `[core]` in `_bmad/custom/config.user.toml`.
2. Read the store config: `uv run {skill-root}/scripts/read_toml.py --file {project-root}/_bmad/custom/ticketing-store-config.toml -k tickets` — store guidance, access, and the type and status maps. Substitute `{output_folder}` in every value. Missing or unreadable: follow `{skill-root}/references/store-setup.md` instead of continuing.
3. Resolve `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} -k workflow.activation_steps_prepend -k workflow.activation_steps_append -k workflow.persistent_facts -k workflow.on_complete`.
4. Run `{workflow.activation_steps_prepend}`; treat `{workflow.persistent_facts}` (set with `bmad-customize`) as foundational context for the session — entries prefixed `file:` are paths or globs under `{project-root}` to load, the rest are facts verbatim — together with whatever is already in your context — registered MCP servers and CLIs, and anything injected from AGENTS.md, CLAUDE.md, or the like. Use what is known; do not ask for it again.
5. Run `{workflow.activation_steps_append}`. When the requested operation ends, run `{workflow.on_complete}`.

## Intake

Before routing, size the ask from what the user said and what is in context, and say which path you are taking and why; the user overrides, and an override is a `Decision:` line. Standalone: one bug or story into `backlog/`, no container, no spec question, single-ticket checks. Small epic: an epic envelope under the initiative, the spec question asked once and easy to decline, two to six entries, no learn-the-codebase subagents; the check on the draft in `validate.md` still runs. Full inception: the epic path in `slice.md`. Initiative: authored and split into epics per `slice.md`. A spec folder handed over by `bmad-spec` is the epic's requirement source: `covers` cites its `CAP-N` ids and the spec question is already answered.

## Routing

| The user wants | Read |
|---|---|
| an initiative started or authored, split into epics; an epic incepted into stories, re-sliced; an entry pulled to refine it | `{skill-root}/references/slice.md` |
| one ticket written or refined | `{skill-root}/references/ticket.md` |
| tickets published, started, moved, assigned, blocked, closed, dropped; what is ready or next; status of a ticket or tree; a tree cancelled | `{skill-root}/references/board.md` |
| a ticket, a set, or a tree validated | `{skill-root}/references/validate.md` |
| an epic or story sized, re-estimated, actuals recorded, the scale calibrated | `{skill-root}/references/estimate.md` |
| the store set up, reconfigured, or switched | `{skill-root}/references/store-setup.md` |
| the rules every skill that uses the tree follows: finding it, the plan file, who writes each status, the baseline, where review and retrospective write | `{skill-root}/references/tree-rules.md` |

Recommend refining only a ticket in `tickets.py next`'s `ready_to_refine`; one in `ready_to_start` goes to the build as it is.

Save agreed work into the ticket tree. Future epics stay as envelopes until selected for inception.

### Autonomous mode

When the user asks you to do the thinking without the conversation, the same guidance, self-review, and subagents apply. Gaps become open questions in Notes and choices become marked assumptions, never silent guesses. The check on a draft in `validate.md` always runs. Before publish, ask once which other validations to run unless already said, and still get a yes to publish unless they said to publish too.

## Loaded on demand

Load each of the following when a step names it; resolve keys by script rather than opening `customize.toml`. Templates are opened directly.

**Customization** — `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} -k workflow.<key>` (repeat `-k`):

| Key | Holds |
|---|---|
| `slice_to_epics` | how to propose epic boundaries |
| `container_definition` | what a container says at its altitude, and what waits for inception |
| `slice_to_tickets` | how to slice an epic into session-sized implementation steps |
| `ordering` | which tickets open and close a parent |
| `acceptance_criteria` | how acceptance criteria are written |
| `scoring` | the risk and severity scales |
| `estimation` | on/off, the point scale, rubric, and t-shirt map |
| `prose` | how ticket prose reads |
| `checks` | the validation checks, one array per scope: `checks.ticket`, `.set`, `.tree`, `.dependencies` (missing prerequisites), `.closure` |
| `refinement` | what refining means, and where full acceptance criteria are written |
| `publication` | when tickets publish to a tracker: as each starts, or the whole breakdown at inception (the default) |
| `initiative_template`, `epic_template`, `story_template`, `spike_template`, `bug_template` | the template file per type |

**Store operations** — `uv run {skill-root}/scripts/read_toml.py --file {project-root}/_bmad/custom/ticketing-store-config.toml -k verbs.<name>` (repeat `-k`), then follow the verb as written:

| Verb | For |
|---|---|
| `setup` | connect the tool; create what the maps name |
| `write` | create or change a ticket — body, state, assignee, parent, blocking, fields |
| `query` | one ticket, a container's children, a search, what is ready |

How a ticket cites a document is the `reference` global, read at activation. A field a verb needs that is empty and cannot be inferred: use what the user tells you for this run and offer to record it per `{skill-root}/references/store-setup.md`.

## The ticket tree

Tickets live under `{output_folder}`, beside the documents. A container is a markdown file; a leaf with a parent is an entry in the parent's `tickets.toml`, with a file once refined; a backlog leaf is a file. A leaf is built from its entry and its epic, plus its file when there is one. By default the tree is the store (git-backed, the repo starter). A tracker, when configured, is a remote: `write` pushes a ticket to it, `query` reads it back, and a ticket the tracker knows but the tree does not gets its file at first `query`.

A container is a folder `<type>-<slug>/` holding its same-named ticket file, its `tickets.toml`, its spec, and its children. A leaf's file is `<type>-<slug>.md` in its parent's folder, written when its entry is pulled to refine it or published to a tracker, or in `backlog/` with no parent. Its frontmatter `id` is its entry's `id`, an integer assigned once and never reused, and the only name it has on the repo store: `3` inside its epic, `2.3` from another epic (the epic's `id` is in the initiative's `tickets.toml`). No file or folder name carries an id, except the `-<id>` that `pull` adds when two titles in a folder give the same name; digits in a title stay in the name, and a title change renames the file. A tracker adds `tracker_id` and `remote` at publish. The order of tables in `tickets.toml` is the build order; `id` is not. When an initiative has epics, every leaf is under one. A new ticket starts from its type's template. The build's plan is a separate file beside the leaf's, described below; `bmad-ticket` writes it only through `tickets.py mark` on the repo store, and it is never sent to a tracker. Other skills' artifacts sit beside the ticket, named after it. Once a leaf file exists it is truth: the entry keeps `id`, `type`, `title`, `after`, and `hitl` current (a changed `after` is written to both), and its other fields are not maintained after pull. A ticket the user names — `1.2`, a tracker id, a file name, words from a title — resolves to one ticket through `tickets.py find <dir> <ref>`, which returns its row, its entry's text fields, and the paths `epic_file`, `story_file`, and `plan`.

A leaf's `status`, `assignee`, `blocked_at`, and `blocked_reason` live in its plan, `<type>-<slug>-plan.md` beside where its file is or would be, joined by the plan's `ticket` field; a leaf file's own `status` is read only when there is no plan, and a leaf with neither has had no build started. bmad-build and bmad-build-auto write `draft`, `ready-for-dev`, `in-progress`, `in-review`, and `built` as they work, and bmad-build-auto writes `blocked` when it halts. `done` is the user's, or an orchestrator's, through `tickets.py mark`; `bmad-ticket` writes `dropped` when the user says so, and runs `mark` for the user when they say a ticket is done or that they are working it by hand. No skill moves a ticket past `built`, which means the build finished and nobody has called it done. `tree-rules.md` holds the full table. `tracker_status` exists only on a tracker store: the BMad word for the tracker's current status (`backlog`, `in-progress`, `review`, `done`, `dropped`), written by `query` next to `tracker_id` and `remote`, never sent. A ticket's state, which `next` and `status` group by and which `write` sends to a tracker as `[tickets.status].<state>`, is `planned` for an entry with no file and no plan; else `tracker_status` when present; else from `status`: none, `draft`, or `ready-for-dev` is `backlog`; `in-progress` or `blocked` is `in-progress`; `in-review` or `built` is `review`; `done` and `dropped` are themselves. A tracker's status must never drive build routing (a card moved to In Progress on the board still gets planned), and the build's own steps never need to reach the tracker. A container's `status` is `bmad-ticket`'s: absent until work under it starts, then `in-progress`, `done`, or `dropped`.

Work that reads a lot and returns a little runs in a subagent: learning the codebase, opening references, reading a tree from the store, a validation check, web searches. If the harness blocks subagents, say so and continue inline.
`````

---

## File: skills/bmad-ux/assets/color-themes.md

`````markdown
# Color Themes Renderer

Subagent prompt. Produce one self-contained HTML page at the supplied `.working/color-themes-{n}.html` path showing 4-6 distinct theme variations side by side so the user can pick.

Each variation: header (name + one-line emotional register), token chips for every semantic role decided so far, and one realistic UI snippet using the palette (content drawn from the conversation, not lorem). Include light and dark side-by-side when both modes are in scope. Avoid near-identical pastels — variations must differ in register, not just hue.

Inline CSS only, system font stack, no JS, no network. Document concrete hex values in `<style>` comments per variation so the user can lift them if they pick that theme. The spine itself stays semantic.

Return to the parent: file path, one-line per variation, mode coverage. Do not dump HTML into the parent context. If interactive, open the file with the platform opener — `open "PATH"` on macOS, `xdg-open` on Linux, `start ""` on Windows, path always double-quoted. On failure, give the user the path instead.
`````

---

## File: skills/bmad-ux/assets/design-directions.md

`````markdown
# Design Directions Renderer

Subagent prompt. Produce 3-6 distinct visual directions for the product's hero screen, each a separate self-contained HTML file at `.working/direction-{name}.html` (or one combined `directions-{n}.html` if the parent's intent says side-by-side).

Each direction is a *complete visual personality* applied to the same key screen — not a palette swap. Differ on density, type weight, motion implication, brand register. Each file: 2-3 sentence rationale, near-1:1 hero screen mockup in a phone or browser frame, ideally a secondary screen, at least one state variant visible (aging row, empty state, etc).

Use real product content from the conversation. Voice/tone from `.memlog.md` applied to every visible string — no lorem. Inline CSS, system fonts, no JS or network. Document hex values in `<style>` comments per direction.

Return to the parent: file paths, one-line personality summary per direction, what hero screen was depicted. Do not dump HTML into parent context. If interactive, open each file in the browser.
`````

---

## File: skills/bmad-ux/assets/design-example-editorial.md

`````markdown
---
name: Linen & Logic
colors:
  surface: '#fbf9f4'
  surface-dim: '#dbdad5'
  surface-bright: '#fbf9f4'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f5f3ee'
  surface-container: '#f0eee9'
  surface-container-high: '#eae8e3'
  surface-container-highest: '#e4e2dd'
  on-surface: '#1b1c19'
  on-surface-variant: '#4e453d'
  inverse-surface: '#30312e'
  inverse-on-surface: '#f2f1ec'
  outline: '#80756b'
  outline-variant: '#d1c4b9'
  surface-tint: '#715a3f'
  primary: '#59452b'
  on-primary: '#ffffff'
  primary-container: '#735c41'
  on-primary-container: '#f5d6b4'
  inverse-primary: '#e0c1a1'
  secondary: '#a43b2c'
  on-secondary: '#ffffff'
  secondary-container: '#fd7d69'
  on-secondary-container: '#71160b'
  tertiary: '#374a5f'
  on-tertiary: '#ffffff'
  tertiary-container: '#4f6278'
  on-tertiary-container: '#caddf8'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#fdddbb'
  primary-fixed-dim: '#e0c1a1'
  on-primary-fixed: '#281804'
  on-primary-fixed-variant: '#58432a'
  secondary-fixed: '#ffdad4'
  secondary-fixed-dim: '#ffb4a7'
  on-secondary-fixed: '#400200'
  on-secondary-fixed-variant: '#842417'
  tertiary-fixed: '#d0e4ff'
  tertiary-fixed-dim: '#b4c8e2'
  on-tertiary-fixed: '#071d30'
  on-tertiary-fixed-variant: '#35485d'
  background: '#fbf9f4'
  on-background: '#1b1c19'
  surface-variant: '#e4e2dd'
typography:
  display-lg:
    fontFamily: Libre Caslon Text
    fontSize: 48px
    fontWeight: '400'
    lineHeight: '1.1'
    letterSpacing: -0.02em
  display-lg-mobile:
    fontFamily: Libre Caslon Text
    fontSize: 36px
    fontWeight: '400'
    lineHeight: '1.1'
  headline-md:
    fontFamily: Libre Caslon Text
    fontSize: 32px
    fontWeight: '400'
    lineHeight: '1.2'
  headline-sm:
    fontFamily: Libre Caslon Text
    fontSize: 24px
    fontWeight: '400'
    lineHeight: '1.3'
  body-lg:
    fontFamily: DM Sans
    fontSize: 18px
    fontWeight: '400'
    lineHeight: '1.6'
    letterSpacing: 0.01em
  body-md:
    fontFamily: DM Sans
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.6'
  label-caps:
    fontFamily: DM Sans
    fontSize: 12px
    fontWeight: '500'
    lineHeight: '1.4'
    letterSpacing: 0.1em
  caption:
    fontFamily: DM Sans
    fontSize: 13px
    fontWeight: '400'
    lineHeight: '1.4'
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  unit: 8px
  gutter: 24px
  margin-mobile: 20px
  margin-desktop: 64px
  editorial-gap: 80px
---

## Brand & Style

The design system is rooted in the philosophy of "Slow Design"—an intentional departure from the frantic pace of fast fashion. It evokes a tactile, "linen-weight" sensation through high-end editorial layouts and a restrained aesthetic. The target audience values provenance over presence, seeking a reflective and sophisticated discovery experience that feels as much like a boutique magazine as a digital marketplace.

The style is **Editorial Minimalism** with **Tactile** accents. It prioritizes breathable white space, asymmetrical layouts that mimic printed lookbooks, and a soft, sun-faded palette. Every interaction is designed to be deliberate and "anti-hype," eschewing aggressive animations for subtle transitions and quiet confidence.

## Colors

The palette is inspired by natural fibers and weathered landscapes. 
- **Warm White (#F9F7F2)** serves as the primary canvas, providing a soft, non-clinical background that reduces eye strain.
- **Bone (#E3DED1)** and **Dust (#C2B9A7)** are used for structural depth, subtle dividers, and secondary surfaces.
- **Tobacco (#735C41)** is the primary ink color, used for high-contrast typography and essential UI elements.
- **Sun-faded Red (#B84A39)** and **Wool Blanket Blue (#4A5D73)** are used sparingly as "organic accents"—highlighting editorial picks or signifying subtle state changes without disrupting the tranquil atmosphere.

## Typography

Typography is the primary vehicle for the brand’s sophisticated voice. 
- **Libre Caslon Text** is the voice of the curator. Its classic proportions and elegant serifs provide the editorial weight required for discovery and storytelling.
- **DM Sans** provides a quiet, functional counterpoint. It is used for body copy and navigational elements, ensuring clarity without competing with the headlines. 

Large display titles should often use "optical sizing" logic—tighter leading and slightly negative letter spacing to create a cohesive visual block. Labels are always tracked out (0.1em) to maintain a sense of airy premiumness.

## Layout & Spacing

This design system employs a **Fluid Editorial Grid**. While it follows a 12-column structure on desktop, it encourages "asymmetrical breathing room"—intentionally leaving columns empty to direct focus toward high-quality imagery.

Spacing is generous. The `editorial-gap` (80px+) should be used between major content sections to allow the user to pause and reflect. Mobile layouts should maintain a minimum of 20px side margins to ensure the content feels framed like a page, rather than bleeding to the edges of the device. Elements should lean toward vertical stacks to mimic the scroll of a digital journal.

## Elevation & Depth

Depth is communicated through **Tonal Layering** and **Ambient Shadows** rather than sharp borders.
- **Surfaces:** Use the "Bone" color to define containers against the "Warm White" base. 
- **Shadows:** Shadows are highly diffused and tinted with the "Tobacco" hue (`rgba(115, 92, 65, 0.08)`). They should feel like a soft glow of light hitting fabric, with large blur radii (20px+) and very low opacity.
- **Borders:** When borders are necessary, they are 1px thick and rendered in "Dust," creating a "ghost" outline that barely separates elements from the background.

## Shapes

The shape language is **Soft (0.25rem)**. While a sharp edge feels too aggressive and a pill-shape feels too digital/tech-heavy, a subtle rounding of corners mimics the natural softening of woven textiles over time. 

Larger containers (Cards, Modals) may use `rounded-lg` (0.5rem) to emphasize their tactile, object-like quality. Imagery should always follow these corner radii to maintain a cohesive, "framed" appearance.

## Components

- **Buttons:** Primary buttons use a solid "Tobacco" fill with "Warm White" text. Secondary buttons are "Bone" with "Tobacco" text or simply "Tobacco" text with a 1px "Dust" border. Padding is generous horizontally to create an elegant, elongated silhouette.
- **Cards:** Editorial cards feature large imagery, a "headline-sm" title, and a "caption" subline. Shadows are only applied on hover to simulate a gentle lift.
- **Inputs:** Minimalist underlines in "Dust" that transition to "Tobacco" on focus. Label text remains in "label-caps" above the field.
- **Chips/Tags:** Used for material types (e.g., "100% Linen"). These are rendered in "Bone" backgrounds with "Tobacco" text, using the "Soft" corner radius.
- **Icons:** Must be "Hand-drawn" or "Fine-line" style. Lines should have slight imperfections and vary in weight to reinforce the tactile, artisanal nature of the fashion being discovered.
- **Navigation:** A simple, centered bottom bar or a top-weighted "Ghost" header that disappears on scroll to maximize the editorial viewport.
`````

---

## File: skills/bmad-ux/assets/design-example-mobile.md

`````markdown
---
name: Quill
description: Daily writing companion. Calm, intentional, dark-mode-by-default. No streaks, no gamification.
colors:
  surface-base: '#FAF9F7'
  surface-raised: '#FFFFFF'
  ink-primary: '#1A1B1F'
  ink-secondary: '#6B655A'
  ink-disabled: '#B5AFA5'
  accent: '#A87434'
  border-hairline: '#E8E4DD'
  surface-base-dark: '#1A1B1F'
  surface-raised-dark: '#23252B'
  ink-primary-dark: '#F0EDE8'
  ink-secondary-dark: '#A39E94'
  ink-disabled-dark: '#5E5A53'
  accent-dark: '#D4A574'
  border-hairline-dark: '#2E3036'
typography:
  title:
    note: 'Platform native — iOS Title 1 · Android Headline Small'
  body:
    note: 'Platform native — iOS Body · Android Body Large'
  meta:
    note: 'Platform native — iOS Footnote · Android Body Small'
rounded:
  sm: 6px
  md: 12px
spacing:
  '1': 4px
  '2': 8px
  '3': 12px
  '4': 16px
  '5': 24px
  '6': 32px
---

## Brand & Style

Quill is designed against the grain of contemporary habit apps. Where most products weaponize the user's calendar with streak counters and re-engagement nudges, Quill insists on something quieter — a daily prompt, a place to write, and the unspoken assurance that today's entry is enough. Showing up is the point, not the streak.

The visual language follows. Calm surfaces in warm off-white (light) or deep ink (dark, the default). Generous breathing room. No chromatic color competing for attention except a single warm tobacco that signals save-and-send. Text-first. Hand-on-paper, not buzz-on-screen.

## Colors

The palette is restrained on purpose — a writing surface should not compete with the writing.

- **Warm White (`#FAF9F7`)** is the primary canvas in light mode. Slightly warm to reduce eye strain and keep the surface from feeling clinical.
- **Deep Ink (`#1A1B1F`)** is the dark-mode canvas and the primary body text color in light mode. Quill defaults to dark because most writing happens at night.
- **Tobacco (`#A87434` light / `#D4A574` dark)** is the only chromatic color. Used exclusively for the save indicator and primary action — never for decoration, never for state badges.
- **Hairline (`#E8E4DD` light / `#2E3036` dark)** separates list items at the lowest possible contrast. Anything heavier feels like UI rather than paper.

Avoid: red error fills (Quill is a journal, not a form), gradients (the surface is paper), and saturated accent variants — one accent, used sparingly.

## Typography

Platform conventions are the spec. iOS uses Title 1 / Body / Footnote; Android uses Headline Small / Body Large / Body Small. Dynamic type honored at every level — the largest accessibility setting must still render legibly without truncation.

Headlines are rare. The Today prompt is set in `title`; everything else is `body` or `meta`. No display sizes, no all-caps labels.

## Layout & Spacing

Scale: 4 / 8 / 12 / 16 / 24 / 32 px. The largest gaps land between major surfaces; the smallest sit between tightly related elements. Vertical rhythm follows a hard rule: composer breathes, list items don't.

Mobile margins follow platform conventions (iOS 16pt, Android 16dp). Single-column always; modal stacks one level deep, never two.

## Elevation & Depth

Quill avoids elevation as a visual device. Cards and composer surfaces sit on `surface-raised`, distinguished from `surface-base` only by tone. Shadows are reserved for the rare moment of literal physical metaphor — never for hierarchy. Hierarchy comes from layout and typography, not shadow.

## Shapes

`rounded/sm` (6px) for inputs, list rows, and small surfaces. `rounded/md` (12px) for cards and the composer. Nothing fully rounded; no pills, no perfect circles for surfaces. The aesthetic is paper-with-soft-corners, not iOS-button-pill.

Imagery follows container corners exactly.

## Components

- **Prompt card** — `surface-raised`. One per day. Today's prompt in `title`. Tap to open composer. No icon, no decoration; the prompt itself is the affordance.
- **Composer** — Full-screen text view. Clean text field, generous vertical padding, single-line save indicator in the header.
- **Save indicator** — Text only. Uses `ink-secondary`, never a checkmark icon, never a colored badge.
- **Entry row** (Library) — Date in `meta`, first line of body in `body` (truncated to one line). Hairline divider only, no fill.
- **Settings row** — Label left, value or chevron right. Tobacco accent only on destructive confirmations.

## Do's and Don'ts

| Do | Don't |
|---|---|
| Single accent color, used sparingly on save & primary action | Color-code by sentiment, mood, or category |
| Text-only state indicators (`Saved.`) | Iconography for state (✓, ⚠, ●) |
| Hairline dividers at lowest legible contrast | Card shadows, gradient fills, accent fills behind text |
| Generous vertical rhythm in composer | Compress to fit more on screen |
| Honor platform conventions for navigation | Override platform nav with custom drawer or hamburger |
`````

---

## File: skills/bmad-ux/assets/design-example-shadcn.md

`````markdown
---
name: Drift
description: Focused task tracker for solo founders and small async teams. shadcn/ui on Next.js + Tailwind; this DESIGN.md specifies the brand-layer delta only.
colors:
  # Brand overrides on top of shadcn defaults. All unlisted tokens inherit
  # from shadcn (background, foreground, muted, muted-foreground, popover,
  # popover-foreground, card, card-foreground, border, input, ring, destructive).
  primary: '#0F4C81'
  primary-foreground: '#FFFFFF'
  accent: '#F59E0B'
  accent-foreground: '#1A1208'
  primary-dark: '#5C8AC2'
  primary-foreground-dark: '#0A1A2A'
  accent-dark: '#FBC470'
  accent-foreground-dark: '#1A1208'
typography:
  # Body, label, and muted inherit from shadcn (Geist Sans). Only display is overridden.
  display:
    fontFamily: 'Instrument Serif'
    fontSize: 36px
    fontWeight: '400'
    lineHeight: '1.15'
    letterSpacing: -0.01em
  display-sm:
    fontFamily: 'Instrument Serif'
    fontSize: 24px
    fontWeight: '400'
    lineHeight: '1.2'
rounded:
  # Tighter than shadcn defaults — Drift reads sharper.
  sm: 4px
  md: 6px
  lg: 8px
spacing:
  # shadcn / Tailwind defaults inherited; no overrides.
components:
  button-primary:
    background: '{colors.primary}'
    foreground: '{colors.primary-foreground}'
    radius: '{rounded.md}'
  focus-card:
    background: '{colors.accent}'
    foreground: '{colors.accent-foreground}'
    radius: '{rounded.md}'
    border: 'none'
  command-palette-result-active:
    background: '{colors.accent}'
    foreground: '{colors.accent-foreground}'
---

## Brand & Style

Drift is a focused task tracker for solo founders and small async teams. The product premise is that *work is a moving thing* — momentum matters more than perfectly groomed backlogs, and the right tool surfaces what you're working on *now* without making you administer a system to find it. The brand expression follows: a serif display moment in an otherwise sober sans-serif surface, a single warm accent that means *this is what's live*, and visual restraint everywhere else.

Drift inherits shadcn/ui defaults wholesale. This DESIGN.md specifies only the brand-layer deltas — primary color, accent color, display typography, slightly tighter corners, and a handful of brand-specific components. The 80% of components that ship from shadcn (Button, Card, Dialog, Sheet, Command, Popover, Toast) inherit shadcn's visual specs as-is. Customizing those is *explicitly* against the brand discipline — shadcn's defaults are the contract.

## Colors

The Drift palette is two colors of brand-layer plus shadcn defaults for everything else.

- **Primary Navy (`#0F4C81` light / `#5C8AC2` dark)** is the brand color. Used on primary buttons, active nav items, link underlines, and the "current week" indicator. Replaces shadcn's default `primary`.
- **Focus Amber (`#F59E0B` light / `#FBC470` dark)** is the accent. Used exclusively to indicate the task or project currently in focus — the one you're working on *right now*. Never used for chrome, never used decoratively, never used for state badges. Amber means "live."
- **All other tokens** (`background`, `foreground`, `muted`, `muted-foreground`, `border`, `input`, `ring`, `card`, `popover`, `destructive`) inherit from shadcn defaults. If the brand can't justify overriding a token, it doesn't override it.

Avoid: chromatic flourishes, gradient surfaces, custom destructive colors (use shadcn's), more than two brand colors. The discipline is two-colors-and-stop.

## Typography

Body / label / caption inherit shadcn's Geist Sans ramp. Only the `display` role is brand-overridden, set in **Instrument Serif** at 36px (24px small variant). The serif moment appears in:

- Empty-state hero text on Today and project surfaces
- Project titles in the project detail header
- The "Welcome back, {name}" greeting at first session of the day

Everything else stays in Geist Sans. The serif is a punctuation mark, not a default voice.

## Layout & Spacing

shadcn / Tailwind spacing scale inherited as-is (the 4-based scale: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64). Maximum content width: `max-w-3xl` (768px) — Drift is not a wide-table product, and forcing one-column reading keeps the surface focused.

Single-column layout. Sidebar nav on `lg` (1024px+); on smaller viewports, the sidebar becomes a sheet triggered from the top bar.

## Elevation & Depth

Inherited from shadcn — subtle shadow on hover/active states, no elevation as a visual hierarchy device. Drift adds nothing on top of this; brand discipline is "shadcn's shadows are correct."

## Shapes

Tighter than shadcn defaults: `rounded/sm` (4px) for inputs, `rounded/md` (6px) for cards and buttons, `rounded/lg` (8px) for dialogs and the command palette. The crispness reads "tool" rather than "consumer app." Pill shapes (`rounded/full`) appear only on status badges.

## Components

Drift uses the following shadcn components as-is, unchanged: `Button`, `Card`, `Dialog`, `Sheet`, `Popover`, `DropdownMenu`, `Toast`, `Tabs`, `Avatar`, `Separator`. The contract: don't customize these.

Brand-layer-overridden components:

- **Button (primary variant)** — `{colors.primary}` fill, `{colors.primary-foreground}` text, `{rounded.md}` corner. Other variants (secondary, outline, ghost, destructive) inherit shadcn defaults.
- **Focus card** — Custom Drift component. The "this is what you're working on now" card on Today and project detail. `{colors.accent}` fill, no border, slightly elevated. Appears at most once per surface.
- **Command palette result (active)** — Override on shadcn's `Command` component: the highlighted/keyboard-selected result row uses `{colors.accent}` instead of shadcn's default `accent` token. Reinforces "this is what will fire if you hit Enter."

## Do's and Don'ts

| Do | Don't |
|---|---|
| Inherit shadcn defaults for everything not in the brand layer | Override shadcn's color tokens beyond `primary` and `accent` |
| Use `{colors.accent}` only for "live / now / in-focus" | Use accent for state, chrome, or hover affordances |
| `display` typography sparingly — empty states, hero greetings | Set body text in `display` to "make it pretty" |
| Tighter corners than shadcn (4 / 6 / 8) | Use shadcn's default 6/8/12 (Drift reads sharper) |
| Single-column layouts inside `max-w-3xl` | Wide multi-column tables (Drift is not a spreadsheet) |
`````

---

## File: skills/bmad-ux/assets/excalidraw-wireframe.md

`````markdown
# Excalidraw Wireframe Renderer

Subagent prompt. Produce one `.excalidraw` file at `.working/ia-{date}.excalidraw` (IA diagram) or `.working/flow-{name}-{date}.excalidraw` (flow wireframe).

## CRITICAL: two-character `index` fields only

Every element's `index` field must be **exactly two characters** (`a0`, `aZ`, `b3`, ...). Three-character indices cause a silent *"Error: invalid file"* with no diagnostic output. Assign sequentially across all elements; advance the leading letter when the trailing alphanumeric exhausts (`a0..a9, aA..aZ`, then `b0..`). Verify before writing.

## Shape

Valid Excalidraw file: `{type: "excalidraw", version: 2, source: "https://excalidraw.com", elements: [...], appState: {gridSize: null, viewBackgroundColor: "#ffffff"}, files: {}}`. Each element needs the standard Excalidraw element fields (`id, type, x, y, width, height, angle, strokeColor, backgroundColor, fillStyle, strokeWidth, strokeStyle, roughness, opacity, groupIds, frameId, roundness, seed, version, versionNonce, isDeleted, boundElements, updated, link, locked, index`). Text elements add `text, fontSize, fontFamily, textAlign, verticalAlign, baseline, containerId, originalText, lineHeight`.

## Content

**IA diagram:** boxes-and-arrows of auth stack, main app surfaces, modal routes, settings stack, cross-cutting affordances. Color sparingly to distinguish category. Layout for human legibility, not graph correctness.

**Flow wireframe:** screen-by-screen rectangles left-to-right, simple shapes inside (nav bar, CTA, content blocks) at low fidelity. Arrows labeled with the user action that causes transition. Annotations alongside for climax and edge-case beats.

Return to the parent: file path, kind, one-line subject, element count, confirmation that all indices are two-character. Do not dump JSON into parent context. Tell the user to open in Excalidraw desktop or excalidraw.com.
`````

---

## File: skills/bmad-ux/assets/experience-example-mobile.md

`````markdown
---
name: Quill
status: final
sources:
  - ../prd-quill/prd-quill.md
updated: 2025-09-02
---

# Quill — Experience Spine

> Illustrative example. Single-surface mobile (iOS + Android parity). Consumer posture, calm by default. Paired with `design-example-mobile.md` (Quill DESIGN.md). Demonstrates: microcopy as gating discipline, Inspiration & Anti-patterns earning its place, Responsive & Platform omitted (single-surface).

## Foundation

Single-surface mobile, iOS + Android with parity. No UI system named — inherits platform conventions for navigation, system gestures, dynamic type. `DESIGN.md` is the visual identity reference; this spine is the experience. Dark mode is the default surface; light is a setting.

## Information Architecture

| Surface | Reached from | Purpose |
|---|---|---|
| Today | App open (cold) | Today's prompt + entry composer |
| Library | Tab bar | Past entries, searchable |
| Entry detail | Library row tap | Read / edit one entry |
| Settings | Today header gear | Account, export, theme |

Bottom tab bar (Today / Library / Settings). No drawer. Modal stacks one level deep, never two.

→ Composition reference: `mockups/today-cold.html`, `mockups/composer.html`. Spine wins on conflict.

## Voice and Tone

Microcopy. Brand voice and aesthetic posture live in `DESIGN.md`.

| Do | Don't |
|---|---|
| "Today's prompt." | "Time to write!" |
| "Saved." | "✓ Auto-saved successfully" |
| "We couldn't reach the cloud — your work is on this device." | "Network error" |
| Short, complete sentences. | Streak counters, encouragement, exclamation marks. |

## Component Patterns

Behavioral. Visual specs live in `DESIGN.md.Components`.

| Component | Use | Behavioral rules |
|---|---|---|
| Prompt card | Today | One per day. Tap opens composer. |
| Composer | Today + entry detail | No formatting toolbar in v1. Autosave on pause ≥ 600ms. |
| Entry row | Library list | Tap → entry detail. Long-press reserved for system text selection. |
| Save indicator | Composer header | Cycles `Editing…` → `Saved.` (≥ 800ms visible). |
| Settings row | Settings list | Tap → detail or toggle. |

## State Patterns

| State | Surface | Treatment |
|---|---|---|
| Cold open | Today | Show today's prompt (cached). If no cache, `Today's prompt is loading.` with skeleton. |
| Empty library | Library | `No entries yet — Today's prompt is your first.` Link to Today. |
| Search empty | Library search | `No matches.` No suggestions. |
| Offline write | Composer | Save locally. No banner. Sync on next foreground. |
| Sync error | Settings → Account | Surfaced here only. Never block writing. |
| Focus | Composer | Native cursor + keyboard. No custom focus chrome. |

## Interaction Primitives

- Tap to act. Long-press reserved for system text selection.
- Swipe-to-delete on entry rows (native pattern, confirm sheet).
- Pull-to-refresh on Library only.
- **Banned:** carousels, hero animations on open, badge counts, streaks, push-notification re-engagement.

## Accessibility Floor

Behavioral. Visual contrast lives in `DESIGN.md`.

- VoiceOver / TalkBack: every interactive element labeled with role + state. Save indicator announces `Saved` on transition.
- Dynamic type honored through `DESIGN.md` typography tokens. UI must remain legible at largest setting — no truncated controls.
- Reduce Motion: skip the save-indicator fade; show `Saved.` immediately.
- Tap targets ≥ 44pt (iOS) / 48dp (Android).
- Focus traversal follows reading order on every surface.

## Inspiration & Anti-patterns

- **Lifted from Day One:** the single daily entry framing — one prompt, one composer, no inbox.
- **Lifted from iA Writer:** the no-toolbar composer; formatting is a settings-level decision, not a per-entry one.
- **Rejected — Streaks (Duolingo, most habit apps):** streaks weaponize the user's calendar. Quill's value is showing up *today*, not punishing missed days.
- **Rejected — AI prompt suggestions inside the composer:** the composer is for writing, not negotiating with a model. AI lives only in the daily prompt generation.

## Key Flows

### Flow 1 — Daily write (Mira, late evening, after work)

1. Mira opens app.
2. Today surface shows today's prompt (cached if offline).
3. She taps the composer entry point.
4. Composer opens, keyboard active.
5. She writes; autosave fires on pause.
6. She taps Back.
7. **Climax:** Today surface shows `Saved.` and the entry's first line below the prompt — proof the day is captured.

Failure: cold prompt fetch fails → composer still opens with cached generic prompt; banner on Today only after Mira returns.

### Flow 2 — Recall past entry (Mira, three weeks later, looking for what she wrote about her mother)

1. Mira taps Library.
2. Scrolls or searches.
3. Taps entry row.
4. Entry detail opens in read mode.
5. She taps anywhere to enter edit mode (cursor at tap point).
6. Edits autosave.
7. **Climax:** `Saved.` visible in entry header — the older self and the present self are in continuous conversation.

Empty state: no entries → message routes back to Today.
`````

---

## File: skills/bmad-ux/assets/experience-example-shadcn.md

`````markdown
---
name: Drift
status: final
sources:
  - ../prd-drift/prd-drift.md
updated: 2026-04-02
---

# Drift — Experience Spine

> Illustrative example. Single-surface responsive web. shadcn/ui on Next.js + Tailwind. Paired with `design-example-shadcn.md` (Drift DESIGN.md). Demonstrates: component-library inheritance, keyboard-first interaction primitives, the "shadcn + brand-layer" pattern that covers most modern web SaaS.

## Foundation

Single-surface responsive web. shadcn/ui on Next.js 15+ with Tailwind CSS. The component library does most of the work; brand discipline is "respect the defaults except where the brand layer overrides them." `DESIGN.md` is the visual identity reference and names the override surface; this spine is the experience. Single-tenant per project; users can belong to multiple projects but each project is a self-contained workspace.

## Information Architecture

| Surface | Reached from | Purpose |
|---|---|---|
| Today | App open / `g t` | Current focus, in-progress tasks pulled from all projects |
| Projects | Sidebar / `g p` | List of active and archived projects |
| Project detail | Projects row / `g 1`–`g 9` | Tasks in this project, organized by lane |
| Search | `⌘K` / `Ctrl+K` | Command palette — surface, navigate, act |
| Settings | Avatar menu | Account, theme, keyboard shortcuts, billing |

Sidebar collapses to icons on `md`; becomes a `Sheet` on `sm`. Modal stacks one level deep (e.g., open `Dialog` on top of a surface, never on top of another dialog).

→ Composition reference: `mockups/today.html`, `mockups/project-detail.html`, `mockups/command-palette.html`. Spine wins on conflict.

## Voice and Tone

Microcopy. Brand voice and aesthetic posture live in `DESIGN.md`.

| Do | Don't |
|---|---|
| "What are you working on?" | "Let's get productive! 🚀" |
| "3 tasks in motion" | "You have 3 active items." |
| "Closed. Nice work." | "Task completed successfully ✓" |
| "Nothing in motion. Pick something." | "No active tasks. Click below to get started!" |
| Manager-facing: counts and verbs. Employee-facing: same. | Different tone per audience — Drift talks to everyone the same way. |

## Component Patterns

Behavioral. Visual specs live in `DESIGN.md.Components` (or in shadcn defaults, when inherited).

| Component | Use | Behavioral rules |
|---|---|---|
| Task row | Projects, Today | Click anywhere on row opens edit dialog. Checkbox toggles done state with optimistic update. Hover reveals quick-actions (`focus`, `defer`, `archive`). |
| Focus card | Today, Project detail | At most one focus card per surface — the task or project marked with `focus` state. `f` keyboard shortcut sets focus on the active row. |
| Command palette | Global (⌘K) | Fuzzy search across all projects, tasks, and commands. `Enter` fires the highlighted result. `→` previews a result. Escape closes. |
| Project header | Project detail | Inline-editable title (click to edit, blur to save). Status pill: active / archived / done. |
| Empty state | Anywhere | shadcn's empty pattern + one Drift-specific sentence. `display-sm` for the headline, body text below, single primary action. |

## State Patterns

| State | Surface | Treatment |
|---|---|---|
| Cold app load | Today | shadcn `Skeleton` rows (4-6) match expected layout. Resolves on data. |
| No focus | Today | `display-sm`: "Nothing in motion. Pick something." Below: list of in-progress tasks from all projects. |
| Empty project | Project detail | `display-sm`: "{Project title} is empty." Body: "Add a first task to get going." Single primary button. |
| Command palette no matches | ⌘K | "No matches. Start typing a task or project name, or pick an action below." Followed by 4-5 common commands. |
| Offline | Global (status bar) | shadcn `Toast` once: "You're offline. Changes will sync when you reconnect." Local writes continue. |
| Permission denied | Projects (others' private) | Surface hidden from sidebar. No "blocked" screen. |
| Stale data | Project detail | If background refresh detects changes, shadcn `Toast`: "Updated by {collaborator_name}. Refresh." Manual refresh, no auto. |

## Interaction Primitives

**Keyboard-first.** Drift's primary audience is developers and power users; the keyboard surface is the product, the mouse is fallback.

- `⌘K` / `Ctrl+K` — Command palette (universal)
- `g t` / `g p` — Go to Today / Projects (vim-style)
- `g 1`–`g 9` — Go to project by sidebar position
- `f` — Set focus on highlighted task/project
- `c` — Create new task (context-aware: in the active project)
- `Esc` — Close dialogs, exit edit mode, clear command palette
- `/` — Focus search in current surface

**Mouse:** click to act, drag deferred to v2. Hover reveals row actions on `md+` (touch users tap to reveal).

**Banned everywhere:** infinite scroll (pagination only), drag-to-reorder in v1, hover-only affordances on `sm` viewports, modal stacks > 1 level deep.

## Accessibility Floor

Behavioral. Visual contrast lives in `DESIGN.md` (inherits shadcn's WCAG AA-compliant defaults; brand overrides verified to maintain ratios).

- WCAG 2.2 AA across the responsive web surface.
- Screen reader announces page surface on navigation: "Today, focus surface" / "Project: {name}, task list, {N} tasks."
- Keyboard shortcuts available without modifier on most surfaces (vim-style `g t` etc.) — users with motor-control limitations get the same surface as power users.
- `Tab` order matches reading order on every surface. `Esc` always closes the topmost modal/popover.
- Command palette is fully keyboard-operable; results announce as they update via `aria-live`.
- Focus rings inherit shadcn's `ring` token — visible at AA contrast against `background`.

## Responsive & Platform

| Breakpoint | Behavior |
|---|---|
| `≥ lg` (1024px+) | Sidebar visible. Today is a 2-column layout: focus + in-motion list. |
| `md` (768–1023px) | Sidebar collapses to icons. Today stacks to single column. |
| `< md` (`sm`) | Sidebar becomes a `Sheet` triggered from top bar. Command palette opens fullscreen. |

Drift is responsive web, not a native mobile app. The product works on phones for read + simple-edit, but the primary surface is desktop / laptop.

## Inspiration & Anti-patterns

- **Lifted from Linear:** the keyboard-first discipline. `⌘K` is the command center; vim-style nav (`g t`); no drag for primary navigation; status pill vocabulary.
- **Lifted from Notion:** inline-editable titles. Click-to-edit on project header, blur to save. No edit/view mode toggle.
- **Lifted from shadcn:** the entire surface vocabulary. Drift's brand is *what we add to shadcn*, not a from-scratch design system. This is a deliberate posture, not a shortcut.
- **Rejected — Streaks, badges, achievement notifications:** Drift is a tool, not a habit app. Task closure is its own reward; no celebratory animation, no "🎉 5-day streak!" toast.
- **Rejected — AI-suggested next tasks:** Drift surfaces what's in motion, doesn't tell the user what to work on. The user picks focus; the tool surfaces consequences.
- **Rejected — Multi-column kanban as default project view:** lists are linear; kanban hides progress behind columns. Optional v2; not the default.

## Key Flows

### Flow 1 — Morning focus (Sarah, solo founder, 8:45am Tuesday)

1. Sarah opens Drift in a browser tab.
2. App loads Today. `display-sm`: "Welcome back, Sarah." Focus card shows yesterday's marked task — "Finish landing page hero copy" — still in motion.
3. She hits `⌘K`, types "ship hero", sees the matching task and presses Enter to open it.
4. Inline edit: she updates the task description with two new bullets. Tab + Tab triggers save.
5. **Climax:** Sarah closes the dialog. Today re-renders: focus card still shows the hero task, but now with the updated body text visible at a glance. She doesn't have to navigate anywhere — the surface that greeted her now reflects the work she just did. She picks up her coffee and starts writing.

Failure: data save fails → shadcn `Toast` (destructive variant): "Couldn't save. Trying again." Inline edit retained; another `Enter` retries.

### Flow 2 — Async handoff (Devon and Mara, small remote team, mid-afternoon)

1. Devon finishes wiring up the auth flow and marks the task `done`.
2. Mara, time-zoned three hours ahead and online during overlap, opens Drift.
3. Today loads; her focus is on her own front-end work, but the in-motion list shows Devon's auth task now marked done and one task below it newly assigned to her — "Wire up post-auth redirect" — that Devon set during checkout.
4. She hits `f` on the row to mark it as her focus, then `Enter` to open it.
5. **Climax:** The focus card swaps. Her surface now shows the post-auth redirect task as the live thing she's working on; the projects sidebar shows the Auth project highlighted; the command palette `⌘K` defaults its first result to "Go to Auth project." The state of the team's progress is *embedded in her surface* — no Slack thread to scroll, no status doc to read.

Failure: Devon hadn't actually assigned the follow-up — Mara mis-assigned to herself. She hits `Esc`, `f` again to unfocus, and reassigns to Devon. No "are you sure" dialog; Drift trusts the user.
`````

---

## File: skills/bmad-ux/assets/headless-schemas.md

`````markdown
# Headless Mode Output Examples

Every headless run ends with one of these payloads. Omit keys for artifacts not produced.

## Common fields

- `status` — `"complete"`, `"blocked"`, or `"partial"`
- `intent` — `"create"`, `"update"`, or `"validate"` (matches the detected intent)
- `reason` — required when `status` is `"blocked"`; one-sentence explanation
- `assumptions` — array of inferred values that were not directly confirmed by inputs
- `open_questions` — array of items that need a human decision before the artifact can be considered final

## Create

```json
{
  "status": "complete",
  "intent": "create",
  "design": "{doc_workspace}/DESIGN.md",
  "experience": "{doc_workspace}/EXPERIENCE.md",
  "memlog": "{doc_workspace}/.memlog.md",
  "working_artifacts": ["{doc_workspace}/.working/color-themes-1.html"],
  "promoted_artifacts": {
    "mockups": ["{doc_workspace}/mockups/direction-calm-sage.html"],
    "wireframes": ["{doc_workspace}/wireframes/ia-2026-05-19.excalidraw"]
  },
  "open_questions": [],
  "assumptions": [],
  "external_handoffs": [
    {"directive": "Confluence upload", "tool": "corp:confluence_upload", "url": "https://confluence.corp/DESIGN/123", "status": "ok"}
  ]
}
```

The `working_artifacts` and `promoted_artifacts` keys are optional and omitted entirely when empty. Headless Create runs default to not enabling creative tools — both keys are typically absent in headless output unless the caller enabled them.

## Update

```json
{
  "status": "complete",
  "intent": "update",
  "design": "{doc_workspace}/DESIGN.md",
  "experience": "{doc_workspace}/EXPERIENCE.md",
  "memlog": "{doc_workspace}/.memlog.md",
  "changes_summary": "1-3 sentences describing what changed and why",
  "conflicts_with_prior_decisions": [],
  "open_questions": [],
  "external_handoffs": [
    {"directive": "Confluence upload", "tool": "corp:confluence_upload", "url": "https://confluence.corp/DESIGN/123", "status": "ok"}
  ]
}
```

## Validate

```json
{
  "status": "complete",
  "intent": "validate",
  "validation_report": "{doc_workspace}/validation-report.md",
  "findings_summary": {
    "critical": 0,
    "high": 0,
    "medium": 0,
    "low": 0
  },
  "offer_to_update": true
}
```

`validation_report` is always written for Validate intent — the path here is required, not optional.

## Blocked

```json
{
  "status": "blocked",
  "intent": "update",
  "reason": "Change signal ambiguous — could be a brand refresh or an accessibility audit response; no inferred direction"
}
```

Always include the intent (best-guess if not certain) and a one-sentence `reason`.
`````

---

## File: skills/bmad-ux/assets/key-screens.md

`````markdown
# Key Screens Renderer

Subagent prompt. Fired at Finalize (or during late Discovery once layout decisions firm up). Produces 1:1 HTML mocks of the load-bearing surfaces so the spine can link to them as visual reference. Spine remains the contract; mocks illustrate.

## Inputs

`.memlog.md`, the current drafts `DESIGN.md` and `EXPERIENCE.md`, `.working/` (especially the chosen color-theme and direction mocks), source PRD. The user names which surfaces to render — typically 2-4: the canonical entry surface, the most complex flow's hero screen, any load-bearing overlay/modal, and (when present) the Week / list / dashboard view.

## What to render

One HTML file per screen, at `.working/key-{screen}.html`. Each file: realistic device frame (phone or browser), real product content from the conversation (no lorem), every visible string voice-checked against `.memlog.md`, all decided tokens applied. Show one canonical state per screen; if a surface has a load-bearing alternate state (focus, error, crisis-card-present), render it as a second column or section in the same file.

Inline CSS, system fonts, no JS, no network. The mock must render fully offline. Comment block at the top of the `<style>` notes which spine sections govern this screen so a future reader knows what to check.

## What to return

A compact summary to the parent:
- file path per screen
- one-line caption per screen ("Today picker at rest; accent on Thought record")
- which spine sections each mock illustrates (Component Patterns rows, State Patterns rows, Flow steps)

The parent, at Finalize "Promote working artifacts," uses this summary to insert inline `mockups/...` links into the relevant spine sections.

## Anti-patterns

- Do not invent layout — every composition decision must trace to a `.working/` artifact or a confirmation in `.memlog.md`. If a layout question is open, the mock is premature.
- Do not show every screen of every flow — 2-4 load-bearing surfaces, not 14.
- Do not stage marketing copy. Strings come from `.memlog.md` and voice rules.
- Do not introduce a new pattern not in the spine's Component Patterns table. If you need one, log it and ask before rendering.
`````

---

## File: skills/bmad-ux/references/creative-tools.md

`````markdown
# Creative Tools

`{workflow.creative_tools}` is a registry of collaborative renderers invoked on demand when seeing options helps the user decide. Entries follow the standard prefix convention: `skill:NAME`, `file:PATH`, `tool:MCP_TOOL_NAME: <directive>`, or plain-text directive.

Defaults ship for HTML color themes, HTML design directions, Excalidraw wireframes (Discovery), and 1:1 HTML key-screen mocks (Finalize). Teams append more via override TOML — Figma MCP, custom skills, prompt-based mood boards.

## When to invoke

Decision moments where a visual beats more conversation: picking color tokens, picking a visual personality among directions, sketching IA, mocking a tricky flow. Fast-path users typically skip; coaching-path users typically lean in. Read the room.

## Artifact handling

Every renderer writes to `{doc_workspace}/.working/` with a descriptive filename. `.working/` is the audit trail and survives the run. At Finalize, the facilitator walks `.working/` with the user and promotes artifacts with lasting reference value to `{doc_workspace}/mockups/` (HTML anchoring a brand or layout decision) or `{doc_workspace}/wireframes/` (Excalidraw a dev would glance at). Bar for promotion: *would a future reader of `DESIGN.md` or `EXPERIENCE.md` open this?* Default is leave-in-`.working/`.

## Renderer contract

The parent passes the subagent: current `.memlog.md`, relevant prior `.working/` captures, the user's stated intent for this pass, the output path. The subagent writes its artifact under `.working/` and returns ONLY a compact summary (file path, one line per variant, mode coverage). Parent never holds the full payload.

For HTML, open in the browser when interactive with the platform opener — `open "PATH"` on macOS, `xdg-open` on Linux, `start ""` on Windows, path always double-quoted. On failure, give the user the path instead. Skip in headless.
`````

---

## File: skills/bmad-ux/references/design-md-spec.md

`````markdown
# DESIGN.md Spec — Working Reference

Source of truth: [google-labs-code/design.md](https://github.com/google-labs-code/design.md) (Apache 2.0, Google Labs, April 2026). This file is a working summary; the URL wins on conflict.

## Structure

YAML frontmatter (machine-readable tokens) + markdown body (human-readable rationale, prose sections).

## Frontmatter tokens

| Key | Type | Notes |
|---|---|---|
| `name` | string | Required. Brand or system name. |
| `description` | string | One-line statement of what this system is. |
| `colors` | flat object | Kebab-case keys. Values are hex strings (`'#FBF9F4'`). |
| `typography` | nested object | Each value: an object with any subset of `fontFamily`, `fontSize`, `fontWeight`, `lineHeight`, `letterSpacing`. |
| `rounded` | object | Scale names (`sm`, `md`, `lg`, `xl`, `full`, `DEFAULT`) → CSS dimensions. `full` is conventionally `9999px`. |
| `spacing` | object | Scale levels (`'1'`, `'2'`, ...) or named tokens (`gutter`, `margin-mobile`, `editorial-gap`) → dimensions. |
| `components` | object | Component-name → object of component tokens mapped to values or `{path.to.token}` references. |

## Body sections (omittable, order-locked when present)

1. **Brand & Style** — Aesthetic posture in prose. The editorial voice — what *kind* of thing this is.
2. **Colors** — Per-color story. Why each exists, where it's used, what it's *not* used for.
3. **Typography** — Type roles, ramp, and rules. Platform conventions noted semantically when inherited.
4. **Layout & Spacing** — Spacing scale narrative, grid behavior, margins, gutters, breakpoint rules.
5. **Elevation & Depth** — Shadow language and tonal layering rules.
6. **Shapes** — Corner radii rules and the aesthetic logic behind them.
7. **Components** — Per-component visual specs: anatomy, color usage, sizing, state appearance.
8. **Do's and Don'ts** — Hard visual rules — what to do, what to avoid.

Sections may be omitted when not relevant; order is locked when present.

## Cross-reference syntax

`{path.to.token}` used in prose and inside component objects to reference frontmatter tokens. Examples:

- `{colors.primary}`
- `{typography.body.fontSize}`
- `{rounded.md}`
- `{spacing.4}`

The path follows the YAML structure.

## Common patterns

- **Light/dark mode.** Either separate kebab-case tokens (`surface-base` / `surface-base-dark`) or separate DESIGN.md files per mode. The spec allows either; pick the form that reads cleanest for the product.
- **Platform conventions.** When inheriting from native platforms (iOS UIKit, Android Compose, Apple Human Interface Guidelines), use a `note` field instead of literal values: `{ note: 'iOS Title 1 · Android Headline Small' }`. The spec is the spec; the platform owns the rendered values.
- **UI-system inheritance.** When inheriting from shadcn / MUI / Tailwind / internal design system, reference the system's tokens by name rather than restating values. DESIGN.md specifies only the deltas (brand color overrides, typography swaps, component customizations).
- **Component tokens.** The `components` frontmatter entry maps each named component (e.g., `button-primary`) to its specific token values. Use `{path.to.token}` references freely; the resolver flattens at consumption time.
`````

---

## File: skills/bmad-ux/references/headless.md

`````markdown
# Headless Mode

Load this file when invoked headless. Follow it for the whole run.

## Detection

Headless when any of: caller sets `headless: true` (or harness equivalent); invocation is from another skill or non-interactive runner; `{workflow.activation_steps_prepend}` declares it; first message is an automation context pre-supplying inputs. Ambiguous → default interactive.

## Inputs

Free-form structured payload in the first message:

- `intent` — `"create"`, `"update"`, or `"validate"`. If absent, infer from the artifact set.
- **Create**: any source spec (PRD, brief, requirements list, design-thinking output, prior UX — text, path, or URL) plus brand / platform / accessibility notes; `doc_workspace` if a specific run folder is required.
- **Update**: existing workspace containing `DESIGN.md` + `EXPERIENCE.md` (or path to either) + change signal.
- **Validate**: existing workspace containing `DESIGN.md` + `EXPERIENCE.md` (or path to either). Workspace defaults to the spines' containing directory.

Inferences → `assumptions[]`. Gaps needing a human decision → `open_questions[]`. Do not invent persona, brand, accessibility, or scope detail.

Creative tools default off in headless. Caller can override; artifacts land in `.working/` and are not promoted unless the caller signals.

## Behavior

Do not ask. Do not greet. Complete the intent from what's provided, what exists in `{doc_workspace}`, or what you can discover. If intent stays ambiguous after inference, halt with `status: "blocked"` and a one-sentence `reason`.

`status`:
- `"complete"` — stands on its own.
- `"partial"` — artifact produced but `open_questions[]` non-empty or critical inputs inferred.
- `"blocked"` — no artifact produced.

End with the JSON response (an example of each payload is in `assets/headless-schemas.md`). `intent` reflects detected intent. Omit keys for artifacts not produced.

## Mode-specific overrides

**Update.** Apply the change. Log it via `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type change --text "<change + rationale>"`. Surface conflicts in `conflicts_with_prior_decisions[]`.

**Validate.** Always write both `validation-report.html` and `validation-report.md` regardless of finding count. Always include `"offer_to_update": true`. Skip the browser-open step.
`````

---

## File: skills/bmad-ux/references/validate.md

`````markdown
# Validate

Critique an existing spine pair (`DESIGN.md` + `EXPERIENCE.md`) or any format of UX the user provides, without changing it. The synthesis pipeline below is also used at the Reviewer Gate during Create / Update Finalize.

## Orient

Subagent-extract from `.memlog.md`, sources in frontmatter, `imports/`, `mockups/`, `wireframes/`, `DESIGN.md`, `EXPERIENCE.md`. Parent assembles from extracts.

## Reviewer Gate

**Opt-in.** Reviewers are costly. At Finalize, ask first if the user wants to run UX validation with multiple subagent lenses. Default offered, easy skip. At Validate intent, skip that question, the user already invoked it.

**Lens menu.** UNLESS HEADLESS MODE: Always present the lens picks before dispatching. Build the menu from: rubric walker (this file) + `{workflow.finalize_reviewers}` + ad-hoc reviewers the skill judges relevant. The user picks all, a subset, or none. Only picked lenses dispatch.

Rubric walker prompt:

> Validate the spine pair (`DESIGN.md` + `EXPERIENCE.md`) as the contract for downstream consumers (architecture, story-dev — human or AI). Can a consumer source-extract cleanly, with every reference resolving and every load-bearing decision committed? Read `{workflow.design_md_examples}` and `{workflow.experience_md_examples}` first.
>
> **Pass 1 — mechanical coverage.** Per category: extract, then list misses with location citations. No misses = **strong**.
>
> 1. **Flow coverage** (EXPERIENCE.md). Sources frontmatter → extract every UJ / requirement name. Verify each has a Key Flow with named protagonist, numbered steps, a climax beat, and a failure path where applicable.
>
> 2. **Token completeness** (DESIGN.md). Extract every token in the YAML frontmatter and every `{path.to.token}` reference in the prose. Verify each defined (see `references/design-md-spec.md` for type rules). **Color tokens missing hex (or light/dark pairs where applicable) are critical** — downstream code mirrors the spine. Platform conventions (native dynamic type, 8pt grid) may stay semantic. Contrast targets stated for load-bearing combinations.
>
> 3. **Component coverage** (both spines). Extract every component name used anywhere. Verify each has a row in DESIGN.md.Components (visual spec) *and* EXPERIENCE.md.Component Patterns (behavioral spec) — real rules, not one-word descriptions.
>
> 4. **State coverage** (EXPERIENCE.md). Walk every IA surface. List states it should have (empty, cold-load, focus, error, offline, permission-denied — whichever apply). Verify each covered.
>
> 5. **Visual reference coverage.** List every file in `mockups/`, `wireframes/`, `imports/`. Spines link to each inline at the relevant section and name what it illustrates; spines-win-on-conflict stated once. List orphans and unspecific references.
>
> **Pass 2 — judgment.** Verdict per category (*strong / adequate / thin / broken*); findings only where they add information.
>
> 6. **Bloat & overspecification.** Pixel specs where tokens cover it; source restatement (personas, FRs, scope); prose where a table works; sections no downstream consumer would read; decorative narrative untied to a decision. DESIGN.md prose may carry editorial voice; EXPERIENCE.md prose should not.
>
> 7. **Inheritance discipline.** `sources` frontmatter resolves. UJ / requirement names verbatim from sources. Glossary identical across spines and sources. Component names identical across all sections in both files. EXPERIENCE.md token references resolve to DESIGN.md tokens by name.
>
> 8. **Shape fit.** DESIGN.md sections in canonical order (Brand & Style → Colors → Typography → Layout & Spacing → Elevation & Depth → Shapes → Components → Do's and Don'ts; omittable but order-locked when present). EXPERIENCE.md required defaults present (Foundation, IA, Voice and Tone, Component Patterns, State Patterns, Interaction Primitives, Accessibility Floor, Key Flows). Dropped defaults defensible. Required-when-applicable present where triggered (Inspiration when sources / memlog show reference products or rejects; Responsive when multi-surface or breakpoints). Invented sections earn their place.
>
> Severity = downstream impact, not fix difficulty.
>
> Write to `{doc_workspace}/review-rubric.md`:
>
> ```markdown
> # Spine Pair Review — {project_name}
>
> ## Overall verdict
> [2–3 sentences]
>
> ## 1. Flow coverage — [verdict]
> [What was checked.]
> ### Findings
> - **[critical|high|medium|low]** [finding] (location). *Fix:* [suggestion].
>
> (repeat 2–8)
>
> ## Mechanical notes
> [Name inconsistencies, broken cross-refs, frontmatter completeness, Mermaid syntax.]
> ```
>
> Return ONLY a compact summary: overall verdict, per-section verdicts, finding counts by severity, file path.

The gate may dispatch `{workflow.finalize_reviewers}` and ad-hoc reviewers (accessibility for consumer / regulated). Each writes `review-{lens}.md` and returns a compact summary. Parallel.

## Synthesis pipeline

Under Validate intent, after every reviewer returns, render one consolidated report. Don't skip.

1. Read every `{doc_workspace}/review-*.md`.
2. Fill `{workflow.validation_report_template}`. No overall grade — the per-category verdicts and severity counts already say what's true. Synthesis paragraph lifts the rubric's overall verdict; add a second if extra reviewers shift the picture. One section per rubric category (open if thin / broken), one per extra reviewer (closed, adversarial voice preserved).
3. Write `{doc_workspace}/validation-report.html`.
4. Write the Markdown twin `{doc_workspace}/validation-report.md` — same content grouped by severity.
5. Open HTML with the platform opener — `open "{doc_workspace}/validation-report.html"` on macOS, `xdg-open` on Linux, `start ""` on Windows, path always double-quoted. On failure, give the user the path instead. Skip headless.

Re-running overwrites the consolidated report; individual `review-*.md` files persist.

## Markdown twin shape

```markdown
# Validation Report — {project_name}

- **DESIGN.md:** `{design_path}`
- **EXPERIENCE.md:** `{experience_path}`
- **Run at:** {ISO timestamp}

## Overall verdict
{synthesis paragraphs}

## Category verdicts
- Flow coverage — {verdict}
- Token completeness — {verdict}
- Component coverage — {verdict}
- State coverage — {verdict}
- Visual reference coverage — {verdict}
- Bloat & overspecification — {verdict}
- Inheritance discipline — {verdict}
- Shape fit — {verdict}

## Findings by severity

### Critical (n)
**[Category or Reviewer]** — Title (§ location)
{Note}
Fix: {suggested fix}

### High (n) / Medium (n) / Low (n)
...

## Reviewer files
- `review-rubric.md`
- ...
```

## Close

Surface artifact paths. Always offer to roll findings into an Update.
`````

---

## File: skills/bmad-ux/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
`````

---

## File: skills/bmad-ux/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-ux.
# Overrides:
#   {project-root}/_bmad/custom/bmad-ux.toml         (team)
#   {project-root}/_bmad/custom/bmad-ux.user.toml    (personal)
# Merge rules: scalars override, arrays append.

[workflow]

# Steps to run before/after standard activation. Append-only.
activation_steps_prepend = []
activation_steps_append = []

# Persistent facts loaded at activation and kept in mind for the run.
# Entries: literal sentence, `skill:NAME`, or `file:PATH` (glob ok).
persistent_facts = []

# Runs at workflow completion. String or array of instructions.
on_complete = ""

# Reference DESIGN.md spines the distillation subagent reads to anchor shape
# and editorial richness. Convention-compliant with the Google Labs DESIGN.md
# spec (https://github.com/google-labs-code/design.md). Append entries via
# override TOML to seed an org-specific canonical aesthetic.
# Each entry: `file:PATH` (or bare relative path, resolved skill-relative).
design_md_examples = [
  "assets/design-example-mobile.md",
  "assets/design-example-shadcn.md",
  "assets/design-example-editorial.md",
]

# Reference EXPERIENCE.md spines for the behavioral/flow/IA layer. Each entry:
# `file:PATH` (or bare relative path, resolved skill-relative).
experience_md_examples = [
  "assets/experience-example-mobile.md",
  "assets/experience-example-shadcn.md",
]

# Design handoff targets — external tools that can take over the design /
# visual identity work. The user runs the tool externally and saves outputs
# (whatever the tool produces — DESIGN.md, Figma files, React components,
# HTML mocks) to {doc_workspace}.
# Each entry: `tool:NAME: <directive>`, `skill:NAME`, or plain-text descriptor.
# Default: Google Stitch (emits DESIGN.md + per-screen HTML). Other producers:
# Vercel v0, Figma, Galileo, Anima, internal generators.
design_handoffs = [
  "Google Stitch (https://stitch.withgoogle.com) — emits DESIGN.md + per-screen HTML. Paste assembled prompt; save outputs to {doc_workspace}.",
]

# HTML skeleton filled in by the validation synthesis pass.
validation_report_template = "assets/validation-report-template.html"

# Run folder. DESIGN.md, EXPERIENCE.md, the `{run_folder_pattern}.md` file naming them, .memlog.md, .working/
# (creative-tool artifacts), imports/ (user-supplied screens / brand decks /
# Figma exports / sketches), optional mockups/ and wireframes/ (promoted
# artifacts), optional validation-report.* all land inside
# {ux_output_path}/{run_folder_pattern}/.
ux_output_path = "{output_folder}/{active_initiative}"
run_folder_pattern = "ux-{slug}"

# Creative tools registry. Collaborative renderers invoked on demand during
# Discovery and at Finalize. Entry forms: `file:PATH`, `skill:NAME`,
# `tool:MCP_TOOL: <directive>`, or plain text. Defaults ship for HTML color
# themes, HTML design directions, Excalidraw wireframes (Discovery), and
# 1:1 HTML key-screen mockups (Finalize). Working artifacts land in
# {doc_workspace}/.working/; finalize promotes those with lasting reference
# value to mockups/ or wireframes/. See references/creative-tools.md.
creative_tools = [
  "file:assets/color-themes.md",
  "file:assets/design-directions.md",
  "file:assets/excalidraw-wireframe.md",
  "file:assets/key-screens.md",
]

# Polish passes applied to DESIGN.md and EXPERIENCE.md at finalize.
# Entries: `skill:NAME`, `file:PATH`, or plain text directive.
# Suggested order: structural → content/voice → prose mechanics.
# The default entry runs bmad-review's two editorial lenses in order:
# structure, then prose on top of the structure findings. The `lenses=` suffix
# names them; drop it to let bmad-review pick what fits the content.
doc_standards = [
  "skill:bmad-review lenses=structure,prose",
]

# Information retrieval registry. Consulted on demand when the conversation
# surfaces a matching need. Distinct from creative_tools (artifact production).
# Example: "When researching component patterns, consult corp:design_system_search."
external_sources = []

# Routes outputs beyond local files at Finalize. Returned URLs/IDs surfaced
# to the user. Unavailable tools skipped and flagged.
# Example: "Upload DESIGN.md to Confluence via corp:confluence_upload (space_key='DESIGN')."
external_handoffs = []

# Reviewers spawned at Finalize step 4 and at Validate intent, alongside
# the rubric walker. Entries: `skill:NAME`, `file:PATH`, or plain text.
# Common ad-hoc add (judged by the skill): accessibility-focused reviewer
# for consumer / regulated work.
finalize_reviewers = []
`````

---

## File: skills/bmad-ux/SKILL.md

`````markdown
---
name: bmad-ux
description: 'Capture the user''s UX vision in two documents: DESIGN.md for how the product looks and EXPERIENCE.md for how it behaves. Use when the user says "lets create UX design" or "create UX specifications" or "help me plan the UX"'
---
# BMad UX

## Overview

You are a master UX facilitator. **Elicit and capture** the user's vision, never impose yours. Probe like a senior practitioner; never volunteer colors, patterns, or directions unless a theme is already in place and the user confirms it. Render options via creative tools when seeing helps; the picks are the user's.

Produce two peer contracts: **`DESIGN.md`** (visual identity per the [Google Labs spec](https://github.com/google-labs-code/design.md) — owns *how it looks*) and **`EXPERIENCE.md`** (information architecture, behavior, states, interactions, accessibility, journeys — owns *how it works*). EXPERIENCE.md cross-references DESIGN.md tokens by name using `{path.to.token}` syntax. Both spines win on conflict with any mock, wireframe, or import.

## The DESIGN.md spine

Per the [Google Labs spec](https://github.com/google-labs-code/design.md). YAML frontmatter tokens (**colors** · **typography** · **rounded** · **spacing** · **components**) + markdown body in canonical order: **Brand & Style** · **Colors** · **Typography** · **Layout & Spacing** · **Elevation & Depth** · **Shapes** · **Components** · **Do's and Don'ts**. Sections omittable; order locked when present. Spec rules: `references/design-md-spec.md`. Shape: read every entry in `{workflow.design_md_examples}`.

## The EXPERIENCE.md spine

Always: **Foundation** (form-factor, UI system when present; DESIGN.md is the visual identity reference) · **Information Architecture** · **Voice and Tone** (microcopy — brand voice lives in DESIGN.md.Brand & Style) · **Component Patterns** (behavioral — visual specs live in DESIGN.md.Components) · **State Patterns** · **Interaction Primitives** · **Accessibility Floor** (behavioral — visual contrast lives in DESIGN.md) · **Key Flows** (named-protagonist journeys with a climax beat).

When triggered: **Inspiration & Anti-patterns** · **Responsive & Platform**.

Invent sections for product-specific concerns. Shape: read every entry in `{workflow.experience_md_examples}`.

When Foundation names a UI system (shadcn, MUI, native UIKit, Compose, internal design system), both spines inherit from it; DESIGN.md tokens reference or extend the system's defaults, EXPERIENCE.md specifies only the behavioral delta.

## Sources

UX may lead, follow, or stand alone. Inherit `sources:` by reference; the spines hold design and experience decisions, not duplicates of upstream product content.

## On Activation

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} --key workflow`.
   - Script not found: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - Any other failure: read `{skill-root}/customize.toml` directly and use defaults.
2. Run `{workflow.activation_steps_prepend}`. Treat `{workflow.persistent_facts}` as foundational context (entries prefixed `file:` are loaded). `{workflow.external_sources}` is an org-configured registry of internal tools; consult them alongside generic web research on the same triggers, org tools preferred when their directive matches.
3. Resolve config: `uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root} --key core.project_name --key core.output_folder --key core.active_initiative`. `{date}` is the current system datetime. `{slug}` is what the design is about, in kebab-case: the run lands in `ux-{slug}/ux-{slug}.md`.
   - Script not found, or no `output_folder`: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - No `active_initiative`: hand off to the `bmad` skill to set or create one, then run the command again and continue. Headless: write loose.
4. If headless, follow `references/headless.md` for the whole run. Otherwise greet the user. In the greeting, let the user know `bmad-party-mode` and `bmad-advanced-elicitation` are always available. Then scan for misroute on the first message: PRD → `bmad-prd`; architecture → `bmad-architecture`; game UX → BMad GDS; agent/skill → `bmad-workflow-builder` (if the BMad Builder module is installed); brief → `bmad-product-brief`.
5. Detect intent: **Create**, **Update**, **Validate**. For Create, before binding a fresh workspace, scan `{workflow.ux_output_path}` for prior in-progress runs (folders matching `{workflow.run_folder_pattern}` whose `DESIGN.md` frontmatter `status` is not `final`) and offer to resume rather than starting over.

Run `{workflow.activation_steps_append}`.

Activation is complete. If `activation_steps_prepend` or `activation_steps_append` were non-empty, confirm every entry was executed in order before proceeding. Do not begin the main workflow until all activation steps have been completed.

## Modes

**Create.** Bind `{doc_workspace}` to `{workflow.ux_output_path}/{workflow.run_folder_pattern}/`. Create `.working/` and `imports/`; seed the memlog with `uv run {project-root}/_bmad/scripts/memlog.py init --workspace {doc_workspace} --field topic="<product/UX>"`; create `DESIGN.md` (frontmatter only), `EXPERIENCE.md` (frontmatter only), and `{workflow.run_folder_pattern}.md`, frontmatter plus one line each naming the two spines. Run Discovery → Finalize.

**Update.** Read spines + memlog + sources. If `.memlog.md` is missing, init it with `uv run {project-root}/_bmad/scripts/memlog.py init --workspace {doc_workspace}` — this update is entry one. Surface conflicts with prior decisions. Run Finalize.

**Validate.** See `references/validate.md`.

## Discovery

**Capture; do not author.** The spines are distilled at Finalize toward the memlog. Decisions → `.memlog.md` (canonical), each appended via `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type <decision|change|override|assumption|event> --text "…"` — never hand-edited; a resume reloads it. Creative-tool artifacts → `.working/`. User-supplied visuals (Figma, sketches, brand decks, image folders) → `imports/`, one `memlog.py append` per item. Spines win on conflict.

**Source scan.** List candidate inputs by type — `<type>-*/<type>-*.md` for `brief`, `prd`, `spec`, `architecture`, `research` — in `{output_folder}/{active_initiative}/`, then `{output_folder}/`; surface paths only — never read content in the parent. User confirms which apply or adds others; subagent-extracts on confirm.

Brain dump first — even when the user opens with paragraphs (that's intake). Subagent-extract big docs. One "anything else?" probe. Stakes: hobby / internal / consumer / regulated.

Working mode:

- **Fast path** — batch gaps, draft both spines with `[ASSUMPTION]` tags, skip creative tools.
- **Coaching path** — walk decisions; creative tools woven in.
- **Design handoff** — assemble captured Discovery into a producer-shaped prompt; user runs the external tool and saves outputs to `{doc_workspace}` in whatever format the tool emits. Producer registry: `{workflow.design_handoffs}`. EXPERIENCE.md can follow via Update mode when ready.

Creative tools — scan `{workflow.creative_tools}`, invoke when seeing helps. Defaults: HTML color themes, design directions, Excalidraw wireframes; key-screen HTML mocks at Finalize. See `references/creative-tools.md`. Research subagents on demand; consult `{workflow.external_sources}` when entries match.

Concern scan — name what the UX carries: accessibility, platforms, brand, regulated language, motion, i18n, dark mode, offline, content density, input modalities, notifications. Open list; drives invented sections.

Journeys: user narrates a real session with a named protagonist (Mary, mom of three, kids asleep — not "the user"); structure into numbered steps with a climax beat. Mirror source-spec names verbatim when defined.

Form-factor: mobile / web / desktop / multi-surface must resolve before IA closes. Named-protagonist journeys often derive it (Pary on iPad implies an iPad surface; Skeeter on Android adds a multi-surface need); when journeys don't disambiguate, probe.

Surface closure: stated needs become screens through journeys. IA closes when every stated need has a surface that delivers it, and every surface has a journey that lands there. When closure fails, probe — never invent the missing piece.

## Reviewer Gate

Used by Validate and Finalize. **Opt-in, lens-selectable** — reviewers are costly (parallel subagents, substantial token spend). At **Finalize**, first ask whether to run validation at all; default offered, easy skip. At **Validate** intent the user already opted in — skip that question. In both cases, present the lens menu and let the user pick all / a subset / none. Menu: rubric walker (`references/validate.md`) + `{workflow.finalize_reviewers}` + ad-hoc (accessibility for consumer / regulated; others by stakes and content). Picked lenses dispatch as parallel subagents → each writes `review-{lens}.md`, returns a compact summary. If any lens ran, run the synthesis pipeline in `references/validate.md`.

## Finalize

Outcomes, in order:

- **Spines distilled.** Subagent reads `.memlog.md`, `.working/`, `imports/`, sources; produces `DESIGN.md` against `## The DESIGN.md spine` + `{workflow.design_md_examples}` and `EXPERIENCE.md` against `## The EXPERIENCE.md spine` + `{workflow.experience_md_examples}`. Runs the rubric walker's Pass 1 coverage checks proactively (see `references/validate.md`). Surface gaps; never invent.
- **Inputs reconciled.** Subagent per user-supplied input → `reconcile-{input}.md`. Surface dropped qualitative ideas.
- **Reviewer Gate offered.** Ask whether to run validation; if yes, present the lens menu (see `## Reviewer Gate`) and let the user pick. If any lens ran, resolve findings before polish; otherwise proceed.
- **Open items triaged.** Open Questions, `[ASSUMPTION]`, `[NOTE FOR UX]`. Phase-blockers one at a time; non-blockers → `memlog.py append`.
- **Key-screen mocks rendered.** Key-screens tool → `.working/` for surfaces where layout drives behavior or anchors visual language.
- **Mock coverage confirmed.** Walk every IA surface; classify *mocked* vs *spine-only*. Ask: *"These will be built from spine tables alone — any need a visual reference?"* Render more if named; log spine-only choices.
- **Layout extracted, artifacts promoted.** Distill subagent re-reads each `.working/` and `imports/` artifact; lifts visual decisions into DESIGN.md and behavioral decisions into EXPERIENCE.md. Promote `.working/` keepers to `mockups/` (HTML) or `wireframes/` (Excalidraw); imports stay. Inline relative links at relevant spine sections; state spines-win-on-conflict once.
- **Polished, handed off, closed.** Apply `{workflow.doc_standards}` in order. Execute `{workflow.external_handoffs}`; surface URLs. Set both files' `status: final`, `updated: {date}`. Log finalization via `uv run {project-root}/_bmad/scripts/memlog.py append --workspace {doc_workspace} --type event --text "spines finalized"`. Share paths. Common next: `bmad-architecture`, `bmad-ticket`, `bmad-build`. Run `{workflow.on_complete}`.
`````

---

