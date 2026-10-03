# BMad Method :: 05 - Fase Learn & Adjust: Retrospectivas e Código Legado

Fonte: gem-bmad-method (Repositório BMAD-METHOD)

---

## File: docs/existing-codebases/getting-deeper.md

`````markdown
---
title: 'Getting Deeper'
description: Use Build and BMad Spec to extend a command in a specific Django version
sidebar:
  order: 3
---

You already know Build from small projects. Here, you will use it in a specific
version of Django: first for one bounded command change, then for three related
stories defined by one BMad Spec. The two exercises demonstrate the
[one-session and epic-sized planning paths](../plan/choose-a-planning-path.md).

:::note[Prerequisites]
Use a macOS or Linux shell with Git, Node.js 20.12+ and `npx`,
[uv](https://docs.astral.sh/uv/getting-started/installation/), and a coding tool
supported by BMad. Complete [Build Your First Change](../start/build-your-first-change.md) before
continuing. The exact install and launch commands below are for Claude Code. If
you use another supported tool, you can run Build there instead.
:::

## 1. Check Out the Exact Django Version

Clone Django 5.2.4 into a new directory, confirm that you have the expected
source code, and create a branch for the exercise:

```bash
git clone --depth 1 --branch 5.2.4 https://github.com/django/django.git bmad-django
cd bmad-django
git rev-parse HEAD
git switch -c bmad-getting-deeper
```

`git rev-parse HEAD` should print:

```text
c941d0deec0ea08a30670be0fac879f2372f071b
```

## 2. Set Up Django for Editing

Set up Python 3.12, install your Django checkout so the example app uses it,
and create a small Django project next to the repository:

```bash
uv python install 3.12
uv venv --python 3.12
uv pip install -e .
mkdir ../bmad-django-app
uv run django-admin startproject tutorial_project ../bmad-django-app
```

## 3. Check the Starting Behavior

Confirm that JSON output is not yet available:

```bash
uv run python ../bmad-django-app/manage.py diffsettings --output=json
```

The command ends with this error:

```text
manage.py diffsettings: error: argument --output: invalid choice: 'json' (choose from hash, unified)
```

## 4. Install BMad

Install BMad Method with the skills CLI, then let the `bmad` skill set up the
project:

```bash
npx skills add bmad-code-org/BMAD-METHOD
```

Open Claude Code in this directory and ask the `bmad` skill to run `bmad setup`. Restart Claude Code once the skills are installed.

Tell Git to ignore the BMad files and uv lockfile created for this tutorial:

```bash
cat >> .git/info/exclude <<'EOF'
/_bmad/
/_bmad-output/
/.claude/
/uv.lock
EOF
```

## 5. Build It

Open your coding tool from the repository root. For Claude Code, run:

```bash
claude
```

```text
/bmad-build Add JSON output support to django-admin diffsettings. Preserve
the existing output formats, add focused tests, and update the command
documentation. Leave the implementation in the working tree for local
inspection.
```

Build asks any questions it needs before it writes a plan. Answer according
to your own preferences for the new JSON output. There is no single required
JSON design for this exercise.

Build presents a plan and waits for you to approve it or ask for changes.
Once approved, it builds and reviews the change, handles its findings, and
shows you the result. Keep this exercise about JSON output for `diffsettings`;
filtering, redaction, and CI behavior belong in the next exercise.

Build ends with a short summary and offers the next steps. Continue with the
manual checks below before asking it to create a PR.

## 6. See It Work

Back in your shell, run Django's `diffsettings` tests:

```bash
uv run python tests/runtests.py admin_scripts.tests.DiffSettings --verbosity 1
```

The tests should pass.

Now run the command again:

```bash
uv run python ../bmad-django-app/manage.py diffsettings --output=json
```

Look through the JSON and compare it with the choices you made with Build.

## 7. You Built It

Congratulations, you've now added something useful to a complex open-source
codebase.

## 8. Write a Spec for the Larger Change

The next change needs three Build runs. `/bmad-forge-idea` can help you decide
what to build. `/bmad-advanced-elicitation` can help you improve a draft. You do
not need either here because the requirements are already clear. Send them
straight to BMad Spec:

```text
/bmad-spec Create a spec named diffsettings-audit and break it into
exactly three stories in this order: filters, redaction, then CI status.

Read the current diffsettings implementation, focused tests, and command
documentation before writing the spec. Keep every existing output format
and the JSON design already approved. Add repeatable --include and --exclude
shell-glob filters. Include patterns are OR, and exclusions always win. Add
repeatable --redact shell-glob masks that replace current and default values
with [REDACTED] without changing whether a difference exists. Add
--fail-on-difference, which exits 1 when differences remain after filtering and
0 otherwise. Each story adds focused tests and updates the existing command
documentation. Do not add another Django documentation file or an external
service. Use diffsettings-audit as the spec folder slug.
```

BMad Spec first has you pick or create an initiative, then writes `_bmad-output/initiative-<slug>/spec-diffsettings-audit/spec-diffsettings-audit.md`. Hand it to `bmad-ticket` to create `epic-diffsettings-audit` and its three ordered entries in `tickets.toml`. Read the spec and entries and answer any questions. Continue when they match the requirements above; note the epic folder the skill creates.

## 9. Build the Three Stories

Run Build once for each story, in order. Complete each Build run before moving
to the next one. Every run uses the same spec. You will run these stories
attentively because they establish how filtering, redaction, and exit behavior
fit together. Later epics with stable, repeated patterns may be better
candidates for automation.

### Story 1: Filters

```text
/bmad-build Implement story 1, filters, from
the epic-diffsettings-audit ticket tree.
```

After Build finishes, observe the result:

```bash
uv run python ../bmad-django-app/manage.py diffsettings \
  --include=DATABASES --include=DEBUG --include=SECRET_KEY \
  --exclude=DATABASES
printf 'exit: %s\n' "$?"
```

The output has `DEBUG` and `SECRET_KEY`, but no `DATABASES`, followed by
`exit: 0`. The include patterns are combined, while the exclusion wins.

### Story 2: Redaction

```text
/bmad-build Implement story 2, redaction, from
the epic-diffsettings-audit ticket tree.
```

Observe the unified output:

```bash
uv run python ../bmad-django-app/manage.py diffsettings \
  --output=unified --include=SECRET_KEY --redact='SECRET*'
printf 'exit: %s\n' "$?"
```

The secret does not appear. Both sides of the difference are masked:

```text
- SECRET_KEY = [REDACTED]
+ SECRET_KEY = [REDACTED]
exit: 0
```

### Story 3: CI Status

```text
/bmad-build Implement story 3, CI status, from
the epic-diffsettings-audit ticket tree.
```

Observe a difference that remains after filtering:

```bash
uv run python ../bmad-django-app/manage.py diffsettings \
  --include=DEBUG --fail-on-difference
printf 'exit: %s\n' "$?"
```

The `DEBUG` difference remains visible, and the command finishes with
`exit: 1`.

## 10. See the Whole Change Work

Now combine the three stories in one observation:

```bash
uv run python ../bmad-django-app/manage.py diffsettings \
  --output=json --include=DEBUG --include=SECRET_KEY --exclude=DEBUG \
  --redact='SECRET*' --fail-on-difference
printf 'exit: %s\n' "$?"
```

The JSON contains only `SECRET_KEY`. Every current or default value exposed by
the JSON shape you chose earlier is `[REDACTED]`; neither original value
appears. The underlying values still differ, so the final line is `exit: 1`.

The first exercise gave Build one bounded change directly. This exercise gave
three separate Build runs one spec. Filtering, redaction, and CI status still
work together at the end. You have extended a mature Django command, and the
final result still does what you asked for at the start.

If you want several perspectives on the result, `/bmad-party-mode` is an
optional final step. You do not need it to finish this tutorial.

## 11. Review the Epic

Run Retrospective against the epic:

```text
/bmad-retrospective epic-diffsettings-audit
```

Retrospective reads the epic's `tickets.toml` and joined plans, then checks the integrated result against its requirements and source spec. It writes `epic-diffsettings-audit-retrospective.md` directly in the epic folder. Review its evidence, acceptance verdict, and proposed follow-up work. Keep the plans: they own status and historical evidence.

## 12. Keep Building

Now [install BMad in your own repository](../start/install-bmad.md), then use
the `bmad-build` skill to make a change you want. See
[Build a Change](../build/build-a-change.md) for the attended path. Use
[Choose a Planning Path](../plan/choose-a-planning-path.md) to decide
when a change needs a spec, automation, or the full project flow.
`````

---

## File: docs/existing-codebases/set-and-maintain-project-context.md

`````markdown
---
title: 'Set and Maintain Project Context'
description: Set up and maintain your repository's agent instructions with bmad-project-context — what goes in, what stays out, and how to keep it healthy.
sidebar:
  order: 2
---

Use `bmad-project-context` to set up a repository so AI agents work well in
it. It works for a new project or an existing codebase, with or without a BMad
install. The output is a small verified block in your `AGENTS.md`. It asks
before it writes; you approve every change.

## When to Use This

- You are starting AI-assisted work in an existing codebase.
- You already wrote an `AGENTS.md` or `CLAUDE.md` and want it kept and
  improved.
- You are starting a new project and want your standards followed from the
  first commit.
- You have governance, security, or style rules that agents need to respect.
- Agents keep making the same mistake, or the instructions feel stale.

## Step 1: Run It

```bash
bmad-project-context
```

Say what you want in plain language — "set up AGENTS.md", "adopt the AGENTS.md
we already have", "refresh the context", "audit our context", "the agent keeps
using the wrong test runner" — and the skill routes to setup, adopt, refresh,
record, or audit.

Point it at a repo if you are not already in one. If that path points to more
than one working tree, it asks which one before writing. If you cannot commit
in that tree, it asks before writing there.

## Step 2: Tell It What You Bring

It reads what is already there — `AGENTS.md`, `CLAUDE.md`, editor rule files,
docs — and reports what is good, what looks stale, and what it wants to
change. A file you wrote is improved, not thrown away: you see what happens to
every instruction, and nothing is deleted without your sign-off.

Then it asks what rules you want followed regardless of what the repo does:
governance, security and compliance, coding standards, style guides, frozen
areas. Bring outside documents too — org handbooks, wiki exports, an MCP
knowledgebase.

For a new project, that conversation is the whole content. For an existing
codebase, it is the part a scan cannot find.

## Step 3: It Verifies the Rest

It checks every path a line names, and reads your `package.json`, `Makefile`,
and CI config — not to copy the scripts, since an agent reads those directly,
but to know what they already answer so the block adds only the right commands
to use, the corrections, and the caveats.

Then it asks what no scan could answer: what agents keep getting wrong here,
what is off limits, what a domain term means, and which commands come with a
catch.

## Step 4: Approve the Block

You see the complete block before anything is written. On approval it is
written between the `<!-- bmad:context -->` and `<!-- /bmad:context -->`
markers in `AGENTS.md` at the repo root. For a tool that reads a different
file, such as Claude Code's `CLAUDE.md`, the skill proposes and verifies a
one-line `@AGENTS.md` import for the tools you use. Everything outside those
markers is left unchanged, and no later run touches it.

It never commits. Changes stay in your working tree for you to review.

At the end it tells you what went in, what was left out, and why.

## Keep It Healthy

- **Refresh** after real change. It re-checks the caveats, diffs deletions
  and renames since the recorded commit, updates what moved, and never re-asks
  what you already settled.
- **Record** the moment an agent gets something wrong. A pitfall goes in only
  when someone has seen the mistake. A scan finding that looks like a trap
  becomes a question, not a line.
- **Audit** on demand. It re-checks and cuts; the block ends smaller or
  equal, never larger.

A rule stays until what it is about is gone, or you retire it. "Nothing broke
lately" is never a reason to delete one — a working rule erases the evidence
that it is still needed.

## What Earns a Line

The block holds only what is expensive to rediscover, or that the agent learns
only after it has already gone wrong. Repo overviews, directory trees, and
tech-stack lists never enter: agents read code better than prose about code,
and the copy goes stale. What earns a line is what the code cannot say:

- **Policy** the org requires — frozen paths, generated files, branch rules,
  security and compliance.
- **What a config file cannot say about running the project** — the catch,
  and which command is the right one to use. `pnpm test` is already in
  `package.json`; that the suite takes eleven minutes, or needs a service
  running first, is not.
- **Conventions that differ from ecosystem defaults**, because an agent
  follows the norm unless told otherwise.
- **Observed pitfalls** — a recorded lesson, the maintainer's recollection, a
  mistake fixed repeatedly in git history, or one the writing session made and
  caught.
- **Cross-component rules and required versions** — rules that must hold
  across parts of the system an agent cannot see from the file it is editing,
  and the tool versions the project actually builds with.
- **Pointers** to where work lands, and to nested or linked files worth
  reading first.

Every rule the skill applies is in its `references/best-practices.md`. It uses
that file to judge what you already have and to explain its reasoning. For why
the block is kept this small, see
[The Theory of Project Context](./theory-of-project-context.md).

## The Intents

| Intent      | Use it when                                                     |
| ----------- | --------------------------------------------------------------- |
| **Setup**   | The repo has no instructions worth preserving.                  |
| **Adopt**   | You already wrote instructions and want them kept and improved. |
| **Refresh** | The code changed since the block was written.                   |
| **Record**  | An agent just made a mistake worth writing down.                |
| **Audit**   | The block feels stale or bloated.                               |

## Where the File Lives

Monorepo components and nested repositories get their own file under the same
rules, listed as pointers in the parent. A large rule set that only applies to
one directory can move into an `AGENTS.md` in that directory — but only after
checking that the tools you use actually read it there. If they do not, the
rules stay in the root file, each naming the directory it applies to.

Commit what the skill writes. The team shares it, and it is versioned with the
code it constrains. Rules that repeat across every project, or that are your
personal preferences, belong in your agent's global configuration instead.

## Hand-Off to Architecture

Make design decisions in `bmad-architecture`. If a decision has real
tradeoffs and more than one viable shape, the skill tells you to run
`bmad-architecture` instead of choosing for you. See
[Design UX and Architecture](../plan/design-ux-and-architecture.md).

## Replaces Two Earlier Skills

:::note[Looking for bmad-generate-project-context or bmad-document-project?]
Both are deprecated and forward here; their trigger phrases still work. If you
have a `project-context.md` from `bmad-generate-project-context`, setup offers
to absorb its content rather than ignore it. `bmad-document-project`
generated repository documentation, which the evidence says not to do.
:::
`````

---

## File: docs/existing-codebases/start-in-an-existing-codebase.md

`````markdown
---
title: 'Start in an Existing Codebase'
description: Start BMad work in a repository that already exists — what to prepare, how much planning the change needs, and how Build treats your conventions.
sidebar:
  order: 1
---

You have an existing project and a stream of change requests coming
in — bugs, tickets, new features. Most of the knowledge about this
application is already encoded in its source. Modern agents are
trained very well to get knowledge from code. Feeding them textual
descriptions of things they can already read there creates
contradiction, ambiguity, and context-window bloat. That, oddly
enough, includes the original greenfield context (PRD etc). Keep it
archived for the few sessions that need it, and out of reach of an
ordinary change — an agent doing a small request should not even be
able to find it by accident.

For a small change, use `[bmad-build](../build/build-a-change.md)`.
For one that needs several coding sessions, run `bmad-spec`, plan its entries with `bmad-ticket`, and Build each entry directly. Then run `bmad-retrospective` on the epic. Keep its joined plans as live status and evidence. If it is bigger than that, treat it as a project and follow [Choose a Planning Path](../plan/choose-a-planning-path.md).

Too little planning costs one Build run: Build looks at the code
first, and stops to ask when it cannot settle the intent. Too much
planning costs documents nobody reads. When unsure, ask `bmad`
rather than deciding alone. It inspects the project and answers
questions like "I have an existing Rails app, where should I start?"
It also runs at the end of every workflow to say what comes next.

Often, the codebase is all you need, but supplementing it with a
tight project context in `AGENTS.md` and companion files really
helps.

## Prepare Project Context, or Skip It

`bmad-project-context` writes a small verified block of agent instructions
into your repo's `AGENTS.md`. See
[Set and Maintain Project Context](./set-and-maintain-project-context.md) for
how to run it. (The earlier `bmad-document-project` workflow is deprecated)

Run it when those instructions are missing, stale, or you are not sure they
are any good. Skip it when the repo already has an `AGENTS.md`, `CLAUDE.md`,
or editor rules someone keeps current, or when agents already have another
discovery tool to build on.

Skipping it does not fail a Build. The cost is the same mistake every session
until someone writes it down. You can run it later, including a refresh or
audit partway through a project.

## Plan Around What Already Exists

When a change needs a PRD, make the agent find and read the existing project
documentation before it writes requirements. If the PRD cites nothing the
repository already does, expect to rework the design once Build meets the
real code.

UX work is optional. Run it when this change adds or alters screens, flows, or
patterns. Skip it for simple updates to screens you are happy with. Running it
with nothing to design wastes a pass; skipping it when new patterns are needed
produces inconsistent screens, one story at a time.

Architecture work needs the architect to use the documented architecture files
and scan the existing codebase. If the proposed decisions do not name what the
code already does, expect a reinvented component or a choice that conflicts
with the current architecture, found during implementation.
[Design UX and Architecture](../plan/design-ux-and-architecture.md) covers
both when the change calls for them.

## Build Follows What It Finds

You do not inventory conventions beforehand. `bmad-build` investigates the
repository, writes down what to reuse and what not to change, and follows
that. It does not stop to ask whether to match the current codebase.

If you want this change to break a pattern, say so in the request, and
write why in the spec so later sessions follow the new rule. If you
dislike a pattern but have no plan to change it, say nothing — it will
match the code. Hoping it modernizes on its own continues the pattern.
Changing one file and leaving the rest leaves two standards with no
record of which one wins.

## Try It on a Known Tree First

[Getting Deeper](./getting-deeper.md) is optional. It walks through one
bounded Build in a specific Django checkout, then a spec-backed epic of three
stories, so you can see both paths before touching your own repository.
`````

---

## File: docs/existing-codebases/theory-of-project-context.md

`````markdown
---
title: 'The Theory of Project Context'
description: Why bmad-project-context captures so little, what belongs in a repository's agent instructions, and what is left out.
sidebar:
  order: 4
---

Most documentation written for AI agents makes them worse.
`bmad-project-context` captures little on purpose. This page is the evidence
and the rules. For how to run the skill, see
[Set and Maintain Project Context](./set-and-maintain-project-context.md).

## What is worth writing down

A line belongs when the fact is expensive to get from the repository — not
whether an agent *could* derive it, but what it costs every time one doesn't,
and whether the fact shows up before the mistake or after it.

Two results show where that line is. On repository-level tasks, **code access
beats documentation access** — a document describing the system loses to the
source it describes. When models generate requirements *from* code, they are
unreliable at producing anything not already implemented. Implementation
behavior is recoverable from source; **intent, rationale, and what was
deliberately rejected are not.**

So what the agent can read cheaply first-hand is read live and never stored.
A stored copy goes stale and costs tokens on every call. A line that stops
the same costly rediscovery every session stays, even if the agent could
eventually find it.

## Why most AGENTS.md files do not help

Measured present versus absent, repository instruction files show **no
improvement in success rate and +20% inference cost.** The result has been
replicated on real repositories, with failures traced to implementation skill
gaps rather than missing repository knowledge. In one study at scale, randomly
generated rules matched expert-curated ones.

Those files overwhelmingly restate what the repository already holds —
structure, stack, architecture summaries. The studies measured *derivable*
written context, not written context in general.

## When a short index in AGENTS.md does help

One controlled comparison ran four configurations against framework APIs
absent from the model's training data:

| Configuration | Pass rate |
|---|---|
| No documentation | 53% |
| Reusable skill, unaided | 53% |
| Same skill, with explicit instructions to invoke it | 79% |
| **Compressed documentation index in `AGENTS.md`** | **100%** |

Same file format as the studies that found no improvement; opposite content —
knowledge the model did not have, rather than a restatement of the repo. The
index was 8KB, compressed from 40KB with no loss in performance.

The other half matters as much. The unaided skill was **never invoked in 56%
of cases**; telling the agent to use it raised that above 95% and still
capped at 79%, with outcomes swinging on small wording changes. **Agents
often skip retrieval they have to choose.** In a test over a 709-page wiki,
agents skipped the index and guessed page paths from the question instead.

An index the agent must choose to fetch gets skipped; an index already in
context does not. Anything the agent must follow goes in `AGENTS.md`. Pointers
out of it must name a trigger the agent can *observe* — a path, a file type, a
concrete task — never one it must judge.

## What earns a place

The test for every line: *would removing this line change agent behavior?* On
a line a human wrote, a failed test opens a question rather than settling one
— see [A working rule stays](#a-working-rule-stays).

- **What a config file cannot say about running the project.** An invocation
  the obvious guess gets right lives in `package.json` or CI config. What does
  not live there is which command is right when several look plausible, and
  the correction: integration tests need a service up first, CI runs a check
  the test script does not.
- **Policy the code cannot express.** Frozen paths, generated files, branch
  rules, security and compliance requirements. These come from people, not
  from a scan.
- **Conventions that differ from ecosystem defaults.** Only the divergences.
  A fact nobody would get wrong by default is not worth a line.
- **Known pitfalls, from observed failure only.** A repository yields hundreds
  of trap-looking facts; only observed behavior separates the few that cause
  real mistakes. A surprising scan finding becomes a question, never a line.
- **Cross-component rules and required versions.** The few rules that must
  hold across parts of the system an agent cannot see from the file it is
  editing, plus the tool versions the project actually builds with.
- **Negative constraints over positive guidance**, which measured better, and
  which is why a prohibition here always names the permitted alternative.

## What is left out

| Not captured | Why |
|---|---|
| **What the code already says** | Agents read source better than summaries of source. A paraphrase adds a second copy that goes stale while the original stays true. |
| **Repo structure and file maps** | Structure changes with every commit — stored maps rot fastest of all, and agents derive structure fresh in seconds. |
| **Overview and tour documents** | These are the documents that were measured to hurt. The block's job is to change behavior, not to give a tour. |
| **Ecosystem defaults** | An LLM already knows how a typical Node, Python, or Go project works. Restating them spends budget teaching the agent what it arrived knowing. |
| **Anything included for being interesting** | Interest is not evidence of need. |
| **Style rules an agent should self-enforce** | That job belongs to a formatter, linter, hook, or CI check. The skill proposes the check instead, and a check that lands deletes its line. |
| **History and edit narration** | Do not write "We removed X because…". Git holds history; the block states present truth only. |
| **Aspirational state** | What the system *should* become belongs in specs. An agent that treats a future state as current will write the wrong code. |

When the evidence supports ten lines, ten lines is the deliverable.

## A working rule stays

**A policy or pitfall is removed only when what it is about is gone** —
deleted, or now enforced by a tool — **or when a human removes it.** Absence
of recent failures is never grounds: a working rule erases the evidence that
it is still needed.

The same protection covers every instruction a human wrote. It goes only when
it is stale or wrong, already enforced by a hook or a check, harmful or
contradictory, or you approve the deletion as a line item — never because it
looks derivable, and never because it is discoverable somewhere in the
repository.

## Two kinds of context, two artifacts

One artifact cannot serve both coding and planning work.

**Implementation context** — constraints, commands, conventions, pitfalls —
belongs to a **code repository**: checkable against the code, stale on every
commit, loaded on every session, so it must be tiny. That is what this skill
owns.

**Planning context** — rationale, rejected approaches, ownership, domain
meaning, org standards — belongs to a **project or initiative**: traceable
only to source documents, stale in months rather than hours, consulted in
bursts rather than loaded continuously. That is a different capability, and it
is coming separately.

Serving both from one file is what produced the two skills this one replaced.

## Extra context is a cost

More documentation is not more value. Extra context is a cost. Refresh
re-checks every caveat and diffs deletions and renames against every line.
Audit asks whether each line still changes agent behavior — subject to the
grounds above on anything a human wrote — and ends with the block smaller or
equal, never larger. When a claim's source disappears, the claim is fixed or
removed, never silently pointed at a different document that still mentions
it.

Generating the first version is cheap. Keeping it true is the work, which is
why refresh and audit exist as their own commands.

## Versus the two replaced skills

`bmad-document-project` scanned an existing repo and generated a documentation
tree — overview, source tree, per-area deep dives. Large, unverified, stale on
arrival: the kind of context that makes agents worse. Its valid instinct —
understand the repo before working in it — survives as the discovery pass,
which now feeds verification instead of prose.

`bmad-generate-project-context` had the right instinct: a single small rules
file of unobvious, project-specific facts. What it lacked was everything
around the file — no verification, no maintenance loop, no way to tell an
inference from a confirmed fact.

The old skills wrote more documentation. This skill keeps less, and checks it.
`````

---

## File: skills/bmad-correct-course/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
`````

---

## File: skills/bmad-correct-course/checklist.md

`````markdown
# Change Navigation Checklist

<critical>This checklist is executed as part of: ./SKILL.md</critical>
<critical>Work through each section systematically with the user, recording findings and impacts</critical>

<checklist>

<section n="1" title="Understand the Trigger and Context">

<check-item id="1.1">
<prompt>Identify the triggering story that revealed this issue</prompt>
<action>Document story ID and brief description</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="1.2">
<prompt>Define the core problem precisely</prompt>
<action>Categorize issue type:</action>
  - Technical limitation discovered during implementation
  - New requirement emerged from stakeholders
  - Misunderstanding of original requirements
  - Strategic pivot or market change
  - Failed approach requiring different solution
<action>Write clear problem statement</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="1.3">
<prompt>Assess initial impact and gather supporting evidence</prompt>
<action>Collect concrete examples, error messages, stakeholder feedback, or technical constraints</action>
<action>Document evidence for later reference</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<halt-condition>
<action if="trigger is unclear">HALT: "Cannot proceed without understanding what caused the need for change"</action>
<action if="no evidence provided">HALT: "Need concrete evidence or examples of the issue before analyzing impact"</action>
</halt-condition>

</section>

<section n="2" title="Epic Impact Assessment">

<check-item id="2.1">
<prompt>Evaluate current epic containing the trigger story</prompt>
<action>Can this epic still be completed as originally planned?</action>
<action>If no, what modifications are needed?</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="2.2">
<prompt>Determine required epic-level changes</prompt>
<action>Check each scenario:</action>
  - Modify existing epic scope or acceptance criteria
  - Add new epic to address the issue
  - Remove or defer epic that's no longer viable
  - Completely redefine epic based on new understanding
<action>Document specific epic changes needed</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="2.3">
<prompt>Review all remaining planned epics for required changes</prompt>
<action>Check each future epic for impact</action>
<action>Identify dependencies that may be affected</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="2.4">
<prompt>Check if issue invalidates future epics or necessitates new ones</prompt>
<action>Does this change make any planned epics obsolete?</action>
<action>Are new epics needed to address gaps created by this change?</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="2.5">
<prompt>Consider if epic order or priority should change</prompt>
<action>Should epics be resequenced based on this issue?</action>
<action>Do priorities need adjustment?</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

</section>

<section n="3" title="Artifact Conflict and Impact Analysis">

<check-item id="3.1">
<prompt>Check PRD for conflicts</prompt>
<action>Does issue conflict with core PRD goals or objectives?</action>
<action>Do requirements need modification, addition, or removal?</action>
<action>Is the defined MVP still achievable or does scope need adjustment?</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="3.2">
<prompt>Review Architecture document for conflicts</prompt>
<action>Check each area for impact:</action>
  - System components and their interactions
  - Architectural patterns and design decisions
  - Technology stack choices
  - Data models and schemas
  - API designs and contracts
  - Integration points
<action>Document specific architecture sections requiring updates</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="3.3">
<prompt>Examine UI/UX specifications for conflicts</prompt>
<action>Check for impact on:</action>
  - User interface components
  - User flows and journeys
  - Wireframes or mockups
  - Interaction patterns
  - Accessibility considerations
<action>Note specific UI/UX sections needing revision</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="3.4">
<prompt>Consider impact on other artifacts</prompt>
<action>Review additional artifacts for impact:</action>
  - Deployment scripts
  - Infrastructure as Code (IaC)
  - Monitoring and observability setup
  - Testing strategies
  - Documentation
  - CI/CD pipelines
<action>Document any secondary artifacts requiring updates</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

</section>

<section n="4" title="Path Forward Evaluation">

<check-item id="4.1">
<prompt>Evaluate Option 1: Direct Adjustment</prompt>
<action>Can the issue be addressed by modifying existing stories?</action>
<action>Can new stories be added within the current epic structure?</action>
<action>Would this approach maintain project timeline and scope?</action>
<action>Effort estimate: [High/Medium/Low]</action>
<action>Risk level: [High/Medium/Low]</action>
<status>[ ] Viable / [ ] Not viable</status>
</check-item>

<check-item id="4.2">
<prompt>Evaluate Option 2: Potential Rollback</prompt>
<action>Would reverting recently completed stories simplify addressing this issue?</action>
<action>Which stories would need to be rolled back?</action>
<action>Is the rollback effort justified by the simplification gained?</action>
<action>Effort estimate: [High/Medium/Low]</action>
<action>Risk level: [High/Medium/Low]</action>
<status>[ ] Viable / [ ] Not viable</status>
</check-item>

<check-item id="4.3">
<prompt>Evaluate Option 3: PRD MVP Review</prompt>
<action>Is the original PRD MVP still achievable with this issue?</action>
<action>Does MVP scope need to be reduced or redefined?</action>
<action>Do core goals need modification based on new constraints?</action>
<action>What would be deferred to post-MVP if scope is reduced?</action>
<action>Effort estimate: [High/Medium/Low]</action>
<action>Risk level: [High/Medium/Low]</action>
<status>[ ] Viable / [ ] Not viable</status>
</check-item>

<check-item id="4.4">
<prompt>Select recommended path forward</prompt>
<action>Based on analysis of all options, choose the best path</action>
<action>Provide clear rationale considering:</action>
  - Implementation effort and timeline impact
  - Technical risk and complexity
  - Impact on team morale and momentum
  - Long-term sustainability and maintainability
  - Stakeholder expectations and business value
<action>Selected approach: [Option 1 / Option 2 / Option 3 / Hybrid]</action>
<action>Justification: [Document reasoning]</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

</section>

<section n="5" title="Sprint Change Proposal Components">

<check-item id="5.1">
<prompt>Create identified issue summary</prompt>
<action>Write clear, concise problem statement</action>
<action>Include context about discovery and impact</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="5.2">
<prompt>Document epic impact and artifact adjustment needs</prompt>
<action>Summarize findings from Epic Impact Assessment (Section 2)</action>
<action>Summarize findings from Artifact Conflict Analysis (Section 3)</action>
<action>Be specific about what changes are needed and why</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="5.3">
<prompt>Present recommended path forward with rationale</prompt>
<action>Include selected approach from Section 4</action>
<action>Provide complete justification for recommendation</action>
<action>Address trade-offs and alternatives considered</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="5.4">
<prompt>Define PRD MVP impact and high-level action plan</prompt>
<action>State clearly if MVP is affected</action>
<action>Outline major action items needed for implementation</action>
<action>Identify dependencies and sequencing</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="5.5">
<prompt>Establish agent handoff plan</prompt>
<action>Identify which roles/agents will execute the changes:</action>
  - Developer agent (for implementation)
  - Product Owner / Developer (for backlog changes)
  - Product Manager / Architect (for strategic changes)
<action>Define responsibilities for each role</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

</section>

<section n="6" title="Final Review and Handoff">

<check-item id="6.1">
<prompt>Review checklist completion</prompt>
<action>Verify all applicable sections have been addressed</action>
<action>Confirm all [Action-needed] items have been documented</action>
<action>Ensure analysis is comprehensive and actionable</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="6.2">
<prompt>Verify Sprint Change Proposal accuracy</prompt>
<action>Review complete proposal for consistency and clarity</action>
<action>Ensure all recommendations are well-supported by analysis</action>
<action>Check that proposal is actionable and specific</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="6.3">
<prompt>Obtain explicit user approval</prompt>
<action>Present complete proposal to user</action>
<action>Get clear yes/no approval for proceeding</action>
<action>Document approval and any conditions</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<check-item id="6.4">
<prompt>Confirm next steps and handoff plan</prompt>
<action>Review handoff responsibilities with user</action>
<action>Ensure all stakeholders understand their roles</action>
<action>Confirm timeline and success criteria</action>
<status>[ ] Done / [ ] N/A / [ ] Action-needed</status>
</check-item>

<halt-condition>
<action if="any critical section cannot be completed">HALT: "Cannot proceed to proposal without complete impact analysis"</action>
<action if="user approval not obtained">HALT: "Must have explicit approval before implementing changes"</action>
<action if="handoff responsibilities unclear">HALT: "Must clearly define who will execute the proposed changes"</action>
</halt-condition>

</section>

</checklist>

<execution-notes>
<note>This checklist is for SIGNIFICANT changes affecting project direction</note>
<note>Work interactively with user - they make final decisions</note>
<note>Be factual, not blame-oriented when analyzing issues</note>
<note>Handle changes professionally as opportunities to improve the project</note>
<note>Maintain conversation context throughout - this is collaborative work</note>
</execution-notes>
`````

---

## File: skills/bmad-correct-course/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-correct-course. Mirrors the
# agent customization shape under the [workflow] namespace.

[workflow]

# --- Configurable below. Overrides merge per BMad structural rules: ---
#   scalars: override wins • arrays (persistent_facts, activation_steps_*): append
#   arrays-of-tables with `code`/`id`: replace matching items, append new ones.

# Steps to run before the standard activation (config load, greet).
# Overrides append. Use for pre-flight loads, compliance checks, etc.

activation_steps_prepend = []

# Steps to run after greet but before the workflow begins.
# Overrides append. Use for context-heavy setup that should happen
# once the user has been acknowledged.

activation_steps_append = []

# Persistent facts the workflow keeps in mind for the whole run
# (standards, compliance constraints, stylistic guardrails).
# Distinct from the runtime memory sidecar — these are static context
# loaded on activation. Overrides append.
#
# Each entry is either:
#   - a literal sentence, e.g. "All sprint changes require PO sign-off before execution."
#   - a file reference prefixed with `file:`, e.g. "file:{project-root}/docs/standards.md"
#     (glob patterns are supported; the file's contents are loaded and treated as facts).

persistent_facts = []

# Scalar: executed when the workflow reaches Step 6 (Workflow Completion),
# after the Sprint Change Proposal is finalized and handoff is confirmed. Override wins.
# Leave empty for no custom post-completion behavior.

on_complete = ""
`````

---

## File: skills/bmad-correct-course/SKILL.md

`````markdown
---
name: bmad-correct-course
description: 'Assess the impact of a significant change during sprint execution across the PRD, epics, architecture, and UX documents, and produce a sprint change proposal. Use when the user says "correct course" or "propose sprint change"'
---

# Correct Course - Sprint Change Management Workflow

**Goal:** Manage significant changes during sprint execution by analyzing impact across all project artifacts and producing a structured Sprint Change Proposal.

**Your Role:** You are a Developer navigating change management. Analyze the triggering issue, assess impact across PRD, epics, architecture, and UX artifacts, and produce an actionable Sprint Change Proposal with clear handoff.

## Conventions

- Bare paths (e.g. `checklist.md`) resolve from the skill root.
- `{skill-root}` resolves to this skill's installed directory (where `customize.toml` lives).
- `{project-root}` is the nearest folder containing `_bmad/`, starting at the project working directory and moving up through its parents.
- `{skill-name}` resolves to the skill directory's basename.

## On Activation

### Step 1: Resolve the Workflow Block

Run: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} --key workflow`

**If the script is not found**, BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.

**If it fails for any other reason**, resolve the `workflow` block yourself by reading these three files in base → team → user order and applying the same structural merge rules as the resolver:

1. `{skill-root}/customize.toml` — defaults
2. `{project-root}/_bmad/custom/{skill-name}.toml` — team overrides
3. `{project-root}/_bmad/custom/{skill-name}.user.toml` — personal overrides

Any missing file is skipped. Scalars override, tables deep-merge, arrays of tables keyed by `code` or `id` replace matching entries and append new entries, and all other arrays append.

### Step 2: Execute Prepend Steps

Execute each entry in `{workflow.activation_steps_prepend}` in order before proceeding.

### Step 3: Load Persistent Facts

Treat every entry in `{workflow.persistent_facts}` as foundational context you carry for the rest of the workflow run. Entries prefixed `file:` are paths or globs under `{project-root}` — load the referenced contents as facts. All other entries are facts verbatim.

### Step 4: Load Config

Run: `uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root} --key core.output_folder --key core.active_initiative`

- Script not found, or no `output_folder`: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
- No `active_initiative`: hand off to the `bmad` skill to set or create one, then run the command again and continue.

- `date` as system-generated current datetime
- YOU MUST ALWAYS SPEAK OUTPUT in your Agent communication style
- DOCUMENT OUTPUT: A Sprint Change Proposal with clear, actionable changes.

### Step 5: Greet the User

Greet the user.

### Step 6: Execute Append Steps

Execute each entry in `{workflow.activation_steps_append}` in order.

Activation is complete. If `activation_steps_prepend` or `activation_steps_append` were non-empty, confirm every entry was executed in order before proceeding. Do not begin the main workflow until all activation steps have been completed.

## Paths

- `default_output_file` = `{output_folder}/{active_initiative}/change-{slug}/change-{slug}.md`, `{slug}` the change's title in kebab-case

## Input Files

Look in `{output_folder}/{active_initiative}/` first, then `{output_folder}/`.

| Input | Path | Load Strategy |
|-------|------|---------------|
| PRD | `prd-*/prd-*.md` | FULL_LOAD |
| Architecture | `architecture-*/architecture-*.md` | FULL_LOAD |
| UX Design | `ux-*/`: `DESIGN.md` and `EXPERIENCE.md` | FULL_LOAD |
| Spec | `spec-*/spec-*.md` and the companions it lists | FULL_LOAD |
| Project Context | `AGENTS.md` in the affected repo (the `bmad:context` block) | FULL_LOAD |

## Execution

### Document Discovery - Loading Project Artifacts

**Strategy**: Course correction needs broad project context to assess change impact accurately. Load all available planning artifacts.

**Discovery Process for FULL_LOAD documents (PRD, Architecture, UX Design, Spec):**

1. **Find each document by type** - the folder and main file patterns in Input Files
2. **If the main file is an index of section files beside it**:
   - Read ALL section files listed in the index
   - Process the combined content as a single document

**Discovery Process for Project Context:**

1. **Read `AGENTS.md`** in the repo the change affects — the block between the `bmad:context` markers carries the policy, frozen paths, and conventions a course correction must respect.
2. **Follow only the pointers that relate to the impacted areas** — nested component files or linked rule files listed under "Where things are". Do not load them all.
3. **This document is optional** — skip if the repo has no `AGENTS.md` (greenfield projects).

**Fuzzy matching**: Be flexible with document names — users may use variations like `prd.md`, `bmm-prd.md`, `product-requirements.md`, etc.

**Missing documents**: Not all documents may exist. A PRD or a spec is essential; Architecture, UX Design, and Project Context are loaded if available. HALT if neither a PRD nor a spec can be found.

<workflow>

<step n="1" goal="Initialize Change Navigation">
  <action>Confirm change trigger and gather user description of the issue</action>
  <action>Ask: "What specific issue or change has been identified that requires navigation?"</action>
  <action>Verify access to project documents:</action>
    - PRD (Product Requirements Document) or spec — required
    - Architecture documentation — optional, load if available
    - UI/UX specifications — optional, load if available
  <action>Ask the user to describe the epics and stories the change affects: what each covers and where it stands</action>
  <action>Ask user for mode preference:</action>
    - **Incremental** (recommended): Refine each edit collaboratively
    - **Batch**: Present all changes at once for review
  <action>Store mode selection for use throughout workflow</action>

<action if="change trigger is unclear">HALT: "Cannot navigate change without clear understanding of the triggering issue. Please provide specific details about what needs to change and why."</action>

<action if="neither a PRD nor a spec is available">HALT: "Need access to a PRD or a spec to assess change impact. Please ensure one is accessible. Architecture and UI/UX will be used if available."</action>
</step>

<step n="2" goal="Execute Change Analysis Checklist">
  <action>Read fully and follow the systematic analysis from: checklist.md</action>
  <action>Work through each checklist section interactively with the user</action>
  <action>Record status for each checklist item:</action>
    - [x] Done - Item completed successfully
    - [N/A] Skip - Item not applicable to this change
    - [!] Action-needed - Item requires attention or follow-up
  <action>Maintain running notes of findings and impacts discovered</action>
  <action>Present checklist progress after each major section</action>

<action if="checklist cannot be completed">Identify blocking issues and work with user to resolve before continuing</action>
</step>

<step n="3" goal="Draft Specific Change Proposals">
<action>Based on checklist findings, create explicit edit proposals for each identified artifact</action>

<action>For Story changes:</action>

- Show old → new text format
- Include story ID and section being modified
- Provide rationale for each change
- Example format:

  ```
  Story: [STORY-123] User Authentication
  Section: Acceptance Criteria

  OLD:
  - User can log in with email/password

  NEW:
  - User can log in with email/password
  - User can enable 2FA via authenticator app

  Rationale: Security requirement identified during implementation
  ```

<action>For PRD modifications:</action>

- Specify exact sections to update
- Show current content and proposed changes
- Explain impact on MVP scope and requirements

<action>For Architecture changes:</action>

- Identify affected components, patterns, or technology choices
- Describe diagram updates needed
- Note any ripple effects on other components

<action>For UI/UX specification updates:</action>

- Reference specific screens or components
- Show wireframe or flow changes needed
- Connect changes to user experience impact

<check if="mode is Incremental">
  <action>Present each edit proposal individually</action>
  <action>HALT and give the user a choice:
  - **Approve** — accept this proposal
  - **Edit** — refine this proposal
  - **Skip** — drop this proposal
  </action>
  <action>If the user chooses **Approve**, keep the proposal. If they choose **Edit**, refine it with them. If they choose **Skip**, drop it. Continue to the next proposal.</action>
</check>

<action if="mode is Batch">Collect all edit proposals and present together at end of step</action>

</step>

<step n="4" goal="Generate Sprint Change Proposal">
<action>Compile comprehensive Sprint Change Proposal document with following sections:</action>

<action>Section 1: Issue Summary</action>

- Clear problem statement describing what triggered the change
- Context about when/how the issue was discovered
- Evidence or examples demonstrating the issue

<action>Section 2: Impact Analysis</action>

- Epic Impact: Which epics are affected and how
- Story Impact: Current and future stories requiring changes
- Artifact Conflicts: PRD, Architecture, UI/UX documents needing updates
- Technical Impact: Code, infrastructure, or deployment implications

<action>Section 3: Recommended Approach</action>

- Present chosen path forward from checklist evaluation:
  - Direct Adjustment: Modify/add stories within existing plan
  - Potential Rollback: Revert completed work to simplify resolution
  - MVP Review: Reduce scope or modify goals
- Provide clear rationale for recommendation
- Include effort estimate, risk assessment, and timeline impact

<action>Section 4: Detailed Change Proposals</action>

- Include all refined edit proposals from Step 3
- Group by artifact type (Stories, PRD, Architecture, UI/UX)
- Ensure each change includes before/after and justification

<action>Section 5: Implementation Handoff</action>

- Categorize change scope:
  - Minor: Direct implementation by Developer agent
  - Moderate: Backlog reorganization needed (PO/DEV)
  - Major: Fundamental replan required (PM/Architect)
- List the epic and story changes (added, removed, resequenced, or rescoped) for the user to apply with `bmad-ticket`
- Specify handoff recipients and their responsibilities
- Define success criteria for implementation

<action>Present complete Sprint Change Proposal to user</action>
<action>Write Sprint Change Proposal document to {default_output_file}</action>
<action>HALT and give the user a choice:
- **Continue** — proceed to approval
- **Edit** — revise the proposal first
</action>
<action>If the user chooses **Edit**, revise the proposal with them and write the updated document before continuing.</action>
</step>

<step n="5" goal="Finalize and Route for Implementation">
<action>Get explicit user approval for complete proposal</action>
<ask>Do you approve this Sprint Change Proposal for implementation? (yes/no/revise)</ask>

<check if="no or revise">
  <action>Gather specific feedback on what needs adjustment</action>
  <action>Return to appropriate step to address concerns</action>
  <goto step="3">If changes needed to edit proposals</goto>
  <goto step="4">If changes needed to overall proposal structure</goto>

</check>

<check if="yes the proposal is approved by the user">
  <action>Finalize Sprint Change Proposal document</action>
  <action>Determine change scope classification:</action>

- **Minor**: Can be implemented directly by Developer agent
- **Moderate**: Requires backlog reorganization and PO/DEV coordination
- **Major**: Needs fundamental replan with PM/Architect involvement

<action>Provide appropriate handoff based on scope:</action>

</check>

<check if="Minor scope">
  <action>Route to: Developer agent for direct implementation</action>
  <action>Deliverables: Finalized edit proposals and implementation tasks</action>
</check>

<check if="Moderate scope">
  <action>Route to: Product Owner / Developer agents</action>
  <action>Deliverables: Sprint Change Proposal + backlog reorganization plan</action>
</check>

<check if="Major scope">
  <action>Route to: Product Manager / Solution Architect</action>
  <action>Deliverables: Complete Sprint Change Proposal + escalation notice</action>

<action>Confirm handoff completion and next steps with user</action>
<action>Document handoff in workflow execution log</action>
</check>

</step>

<step n="6" goal="Workflow Completion">
<action>Summarize workflow execution:</action>
  - Issue addressed: {{change_trigger}}
  - Change scope: {{scope_classification}}
  - Artifacts modified: {{list_of_artifacts}}
  - Routed to: {{handoff_recipients}}

<action>Confirm all deliverables produced:</action>

- Sprint Change Proposal document
- Specific edit proposals with before/after
- Implementation handoff plan

<action>Report workflow completion to user: "Correct Course workflow complete!"</action>
<action>Remind user of success criteria and next steps for Developer agent</action>
<action>Run: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} --key workflow.on_complete` — if the resolved value is non-empty, follow it as the final terminal instruction before exiting.</action>
</step>

</workflow>
`````

---

## File: skills/bmad-project-context/references/best-practices.md

`````markdown
# What belongs in a repo's agent instructions

Rules for deciding what goes in the block, for judging what a repo already has, and for explaining both to the user.

## The test

Not *could an agent derive this* but *what does it cost when it doesn't*: how much exploration finding it takes, how likely the agent is to search the right place in time rather than guess, whether it is available at the point of use or only after the mistake, what a retrieval failure costs — a wasted search, or corrupt data — and whether it is a rule that must hold or a detail the code already shows.

A line that stops the same rediscovery every session earns its place, derivable or not. A stored copy of what the agent reads more accurately first-hand does not — it rots, and it is charged every session.

## Admit

- **Policy the code cannot express** — branch rules, frozen and protected paths, generated files, secrets, security and compliance. Stated by a human or read off an enforcing config, never inferred.
- **What a config file cannot say about running the project** — the root test script does nothing in this workspace, integration tests need a service up first, the suite takes eleven minutes so iterate on single files, the `Makefile` is the real entry point and `package.json` is vestigial, CI runs a typecheck the test script does not. An invocation the obvious guess gets right is already stated in `package.json`, `Makefile`, `pyproject.toml`, or CI config and does not earn a line — the correction, the caveat, and the right command to use do.
- **Conventions that differ from ecosystem defaults.** An agent follows the norm unless told otherwise, so only the divergences earn a line. Command invocations count: when the obvious command is wrong here — a bare-repo prefix, a required wrapper — the exact working invocation earns a line, and no observed mistake is needed to admit it.
- **Pitfalls with observed evidence** — a recorded lesson, the maintainer's recollection, the same mistake fixed repeatedly in history, or one this session made and caught. A repo yields hundreds of trap-looking facts and none of them predict real mistakes; only observed behavior does. A surprising scan finding is a question to ask, not a line to write.
- **Runtime behavior invisible from the repo** — replaying webhooks, lying health endpoints, environment quirks — once a human confirms it.
- **Cross-component rules**, admitted when getting one wrong in one file breaks something elsewhere — what must stay true across parts of the system the agent cannot see from the file it is editing: who owns what, how data must flow, what order a pipeline runs in. "Writes go through the dispatcher; direct store mutation skips the transaction." "The importer is two passes — validate every row, then commit; never write inside the parse loop." A six-line map of who owns what. Never an inventory written for completeness; the exclusions below still bind.
- **Required tool and runtime versions**, read from the project files that declare them, never from this session's environment — which answers faster, and wrongly, so the mistake arrives before the search.
- **Entry points and pointers** to where work lands.

Prefer prohibitions to advice, and name the permitted alternative in the same line.

## Exclude

| | Why |
|---|---|
| Repo overviews, directory trees, stack lists | Derived fresh, more accurately; stored copies rot |
| Anything included for being interesting | Interest is not need |
| Style rules an agent self-enforces | Belongs in a formatter, linter, hook, or CI check — propose the check instead |
| Platitudes | Already the default |
| Transcribed command lists whose obvious invocation is already right | Read from `package.json`, a `Makefile`, or CI config; a copy drifts the moment a script is renamed. The right command to use, and any command the obvious guess gets wrong, are admitted above |
| Pasted code, changelog content, fast-changing facts | Stale immediately |
| Aspirational state | Describe what is; intent belongs in specs |
| History and edit narration | Git holds it; state present truth |

## Retire

A policy or pitfall goes only when the thing it guards is gone, or the user retires it. Nothing failing lately is not evidence — a working rule erases its own evidence. Any other existing instruction goes only on one of the four grounds under "Judging an existing file".

Every line faces one question at each write: would removing it change agent behavior? If no, cut it — but for a line a human wrote, that answer only opens a candidate; a ground still has to carry it.

## Size

Every line is paid in every session, and instruction-following degrades as the loaded set grows. Count what other always-loaded files add. Over budget means cut the weakest lines or move them behind a trigger — never raise the budget. Ten lines of evidence means ten lines.

An adopted file must fit the budget too, but shrinking it works differently. Move the weakest instructions out first — into a child file, a linked doc, or a hook or check that enforces them. Deleting still needs one of the four grounds. If the file is still too big and no ground justifies another deletion, show the user and let them decide — an over-budget file they chose beats a gutted one they didn't. "Keep it small" disciplines what this skill writes, never what the maintainer already wrote.

## Retrieval

An index the agent must choose to fetch gets skipped; one already in context does not. Keep everything load-bearing in the block. A pointer out of it names a trigger the agent can observe — a path, a file type, a named task — never one it must judge ("when the task is complex") or track about itself ("before your first edit").

Rules bounded to a directory can go in a nested `AGENTS.md` there, attached by location rather than by pointer — but only when they are subtree-exclusive and substantial, the split materially reduces the root block, the user approves it, and **loading is verified for every harness in use**. Even with verified loading, keep a rule at the root when it must apply before a session enters that directory or when breaking it can affect work outside the child. Check, never assume: several harnesses build the instruction chain once at session start, root down to the working directory, so a nested file is invisible to the session that later edits into that subtree. Unverified means path-qualified lines at root instead — "in `src/importer/`: ..." — cheaper than a file nobody loads.

Use a linked file only when the trigger is not a path.

## Maintain

- Re-check that caveats still hold — a slow suite that got fast, a workaround for a bug that was fixed.
- Diff deletions and renames since the verified SHA against every line.
- Record provenance in the block so the next run knows what it is diffing from.
- Capture mistakes when they happen, not at review time. One occurrence is a note; recurrence earns a line.
- Route anything mechanically preventable to a hook, lint rule, or CI check. A check that lands deletes its line.

## Repo or home directory

This block belongs committed: shared by the team, consistent across machines, versioned with the code it constrains.

Two things belong in the user's global agent config instead — rules repeating across all their projects, and personal preferences that are theirs rather than the team's.

## Judging an existing file

Every instruction a human wrote is presumed intentional: someone paid for it, usually by watching an agent fail. The file is the baseline being improved, not raw material. Keep its phrasing where it works, and carry each instruction through a ledger entry — `retain | rewrite | relocate | automate | delete`, opened at retain or rewrite — so the user sees where all of it went.

**Deletion needs one of four grounds:**

1. **Stale or incorrect** — the referent is gone, or the instruction was never true; the evidence is named.
2. **Mechanically enforced** — a hook, linter, formatter, or CI check already fails the violation named by the instruction. A tool that only covers the same files or topic does not enforce the instruction.
3. **Harmful or contradictory** — it points agents at the wrong thing, or it contradicts another live instruction and loses the reconciliation.
4. **The user approved this deletion** — asked as a line item, never implied by approving a replacement block.

Grounds 1–3 are evidence the run carries itself, and ride the block approval; ground 4 is the ask-first path everything else takes. Nothing else deletes. Brevity is not grounds, nothing failing lately is not grounds, "the agent could derive it" is not grounds, and **"it is discoverable somewhere in the repository" is never, alone, grounds** — that is the reasoning that empties good files. Content the exclusions table rejects — a directory tree, a stack list, pasted code — has no ground of its own: propose the deletion and let it land under ground 4, asked rather than assumed.

Report, in this order: what is unverifiable or stale, what is missing against the sections above, what is already good, and the ledger, every relocation, automation, and deletion itemized. Recorded lessons are maintainer testimony — kept by default, challenged only with evidence that the thing they name is gone or wrong.
`````

---

## File: skills/bmad-project-context/references/template.md

`````markdown
# Block shape

Sections in this order. Omit any section with nothing that passes its rule — never write an empty one. Admission rules: `best-practices.md`.

1. **Orientation** — three or four sentences: what this is, the stack, where planning and deeper docs live.
2. **Policy** — what the org requires.
3. **Where things are** — entry points, and pointers to children and linked files.
4. **Running and verifying** — the right commands to run and the required tool versions, plus what `package.json`, `pyproject.toml`, a `Makefile`, or CI config does not already say.
5. **Conventions that differ from defaults**
6. **Known pitfalls**

Terse imperative lines under plain headings. No prose beyond Orientation, no introduction, no summary. A bare fact appears only as the justification clause of an instruction — "Exclude `vendor/` from searches, it is 60% of tracked files", never "`vendor/` is 60% of tracked files". A prohibition names the alternative. At most two emphasis markers in the whole block.

## Worked example

````markdown
<!-- bmad:context -->
<!-- Verified 2026-08-08 against a1b2c3d. Managed by bmad-project-context; edits inside this block are replaced on refresh. Keep anything you want preserved outside the markers. -->

## acme-billing

Payment processing for Acme storefronts. TypeScript/Node, pnpm, Postgres. Planning lives in `docs/planning/`, tickets in Linear (ACME board).

## Policy

- Never push to main; PRs only, one approval.
- Never modify `legacy/` — frozen, being replaced. New work goes in `src/`.
- Never hand-edit `src/generated/` — run `pnpm codegen`.

## Where things are

- Webhook handling: `src/routes/webhooks.ts`; conventions in `docs/webhooks.md`
- Writing a migration? Read `docs/db-rules.md` first — ordering, transaction boundaries, pool limits.
- Billing service has its own guide: `services/billing/AGENTS.md`

## Running and verifying

- Run single test files while iterating; the full suite takes ~11 minutes.
- Integration tests need `docker compose up -d` first, and fail confusingly without it.
- CI also runs `pnpm typecheck`, which `pnpm test` does not cover.

## Conventions that differ from defaults

- Money is integer cents (`amountCents`), never floats — `src/lib/money.ts`
- All DB access goes through repositories in `src/repos/`; never call the client directly.

## Known pitfalls

- Stripe webhooks replay in staging every 6h — handlers must be idempotent.
- Use vitest matchers, not jest — agents repeatedly add jest syntax here.

<!-- /bmad:context -->
````

Fill the provenance line with the real date and the commit SHA verified against. Refresh diffs from that SHA.
`````

---

## File: skills/bmad-project-context/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
`````

---

## File: skills/bmad-project-context/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-project-context.
# Team overrides:     {project-root}/_bmad/custom/bmad-project-context.toml
# Personal overrides: {project-root}/_bmad/custom/bmad-project-context.user.toml
#
# Merge rules: scalars override (last layer wins); arrays append.

[workflow]

# --- Universal defaults ---
activation_steps_prepend = []
activation_steps_append = []
# Deliberately empty: this skill's own output (AGENTS.md) is loaded by the
# harness, not through this array. Users append their own standing facts.
persistent_facts = []
on_complete = ""

# Standing outside-the-repo sources offered at every setup/refresh run
# (untrusted until verified against the repo or user-confirmed).
# Append-only. Entries: "file:{project-root}/..." or "file:/abs/path" for
# docs, "skill:name" to consult a skill, plain text for a standing fact,
# "tool:name" for an MCP knowledgebase.
external_sources = []
`````

---

## File: skills/bmad-project-context/SKILL.md

`````markdown
---
name: bmad-project-context
description: 'Set up, adopt, refresh, or audit a repository''s agent instructions (the AGENTS.md block) so AI agents work well in that repo. Also records observed agent mistakes as pitfalls. Use when invoked by name'
---

# Overview

A conversation that produces a repository's agent instructions: a small verified block inside `AGENTS.md`. The user brings rules they want followed — governance, security, standards — and the repository supplies the rest, verified.

Conversational always; the user approves every write.

**Args:** intent (`setup` | `adopt` | `refresh` | `record` | `audit`); a target repo or path; extra source paths or URLs.

## Resolution rules

- Bare paths and `{skill-root}` (e.g. `references/best-practices.md`) resolve from this skill's installed directory.
- `{project-root}` → the project working directory.
- **Target** → the repository being described, defaulting to `{project-root}`. If it resolves to more than one working tree, or to one the user cannot commit in, ask before writing.

## On Activation

1. Resolve customization: `uv run {project-root}/_bmad/scripts/resolve_customization.py --skill {skill-root} --project-root {project-root} --key workflow`.
   - Script not found: BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command again.
   - Any other failure: read `{skill-root}/customize.toml` directly and use defaults.

   Execute `{workflow.activation_steps_prepend}`; treat `{workflow.persistent_facts}` entries as standing context (`file:` = paths/globs to load, others verbatim).
2. Config: if `{project-root}/_bmad` exists, `uv run {project-root}/_bmad/scripts/resolve_config.py --project-root {project-root}` and read `{output_folder}`. Standalone: skip.
3. **Load `references/best-practices.md` and `references/template.md` before anything else.** Every decision below is made against them.
4. Detect intent and greet the user: **setup** (no instruction file in the target carries meaningful content — scaffolding alone, empty headings, a comment, a lone import line, is not meaningful; when unsure, adopt, since adopting a near-empty file costs one small ledger while setting up a meaningful one loses instructions), **adopt** (an instruction file has content but no managed block, whatever its state and whoever wrote it — the migration form of refresh; that file is the baseline and every instruction in it enters the ledger of step 1), **refresh** (a managed block exists), **record** (the user reports a mistake agents made), **audit** (re-verify and prune). A supplied intent that contradicts what detection finds — e.g. `setup` against a file with content — is surfaced and confirmed, never silently obeyed. Fold `{workflow.external_sources}` into the source list. Execute `{workflow.activation_steps_append}`.

## Setup, Adoption, and Refresh Steps

No writes until step 5!

### 1. Assess and report

Read `AGENTS.md`, harness or agent specific rule files, docs folders, and any notes carrying lessons. Report what exists and how it measures up, per `best-practices.md`.

Existing instructions are the baseline being improved, never raw material to discard. Open a **ledger**: one entry per existing section and per independently meaningful instruction, opened at `retain` or `rewrite`, carrying what an agent would get wrong without it. Entries settle as evidence arrives in steps 2–4 — `retain | rewrite | relocate | automate | delete`, each with its reason, its evidence, the risk if it goes, a destination for a relocation, and an approval flag. Deletion needs one of the four grounds in `best-practices.md`, and a relocation destination must itself be loaded or sit behind an observable trigger — a move into a file nothing reads is a deletion and needs its ground. Setup has nothing to map and opens no ledger; refresh opens entries for the lines it proposes to change or remove, the block's own included. A lesson found outside the instruction files — a warning in a README, a notes file — is an ordinary candidate, not a ledger entry.

If the target contains separable units — a workspace manifest listing members, or directories carrying their own build manifest — name them and ask whether this run covers the root only, all of them, or which. Absent that evidence, do not ask. Sibling repositories are not children; each is its own target, offered in turn.

### 2. Ask what they bring

Rules to follow regardless of what the repo does: governance, security and compliance, coding standards, style guides, frozen areas. Ask for outside documents too — handbooks, wikis, architecture docs, MCP knowledgebases. Note the paths; do not read them yet.

Greenfield: this is the whole content. Brownfield: it is the half no scan reaches.

### 3. Discover and verify

Fan out with parallel subagents against what the sections need — executable config and CI for policy and for what they already state, tracked source for conventions and boundaries, targeted history for constraints whose reason must still hold.

`package.json`, a `Makefile`, `pyproject.toml`, contribution guides, pull request templates, and CI config are read to know what the block must not repeat. Their caveats come from the human in step 4. Path-check every claim naming a file. For every claim the block will make about what a command does, read the target or script that runs it and verify the claim.

Each child agreed in step 1 is scanned as its own scope, against its own manifests.

### 4. Interview the gaps

Only what no scan reaches: what agents keep getting wrong here, what is off limits, what a domain term means, why a constraint exists.

- Never ask what a scan could answer. Asking the user to confirm a path-checked claim, or one a config file already states, is a defect.
- Ask recall questions, not review lists. Never hand the user a selection problem a scan created.
- A mistake this session made and caught is observed evidence — offer it.
- A repeatable command spotted in anything read this session — a log, a doc, its own runs — whose correct form is not the obvious guess is a candidate line: offer it. E.g. `uv run pytest` where plain `pytest` looks right but runs outside the project environment.
- Batches of at most eight; fewer is better. A batch yielding nothing new means write.
- When the repo contradicts the user, show the evidence and ask. Never write the claim as given, never drop it silently.

### 5. Show the block, then write it

Compose against `template.md`. For each candidate, ask first whether a hook, lint rule, or CI check enforces it better than prose; if so propose the check, and the line becomes the fallback if they decline. A ledger entry marked `automate` keeps its instruction until its check is in place (a later run deletes the line under ground 2 once the check is live).

**Show the complete block before writing it**, and every child block alongside it — one approval covers the set. **Present the settled ledger with it**: replacement text alone is an incomplete proposal, because it shows what the user gains and hides what they lose. Every existing instruction appears with its decision and reason. Retains and rewrites that keep the full rule may be grouped. If a rewrite weakens, narrows, or drops part of a rule, treat the lost part as a deletion and list it separately. Keep the rule itself; examples may explain it but cannot replace it. Every relocation, automation, and deletion is itemized. A deletion resting on none of the first three grounds is held for line-item approval — approving the block never approves it — and a declined deletion, relocation, or automation reverts to retain. On approval, splice between the markers — the splice itself touches nothing outside them. Text outside the markers changes only through a settled ledger entry or a proposed fix the user has seen, never as a side effect of the splice. Fill each provenance line with today's date and the verified SHA.

Where an instruction elsewhere contradicts the block in a way that changes behavior — a stale `CLAUDE.md` line, a retired command — propose the fix to that file. Two live contradictory instructions is a defect.

Never commit.

### 6. Close

- What went in, what was left out and why, and — after adoption or refresh — where each existing instruction landed.
- Why, in the user's terms, from `best-practices.md` — why it is small, why what the repo already states stays out, why a pitfall stays until its cause is gone.
- How it loads, and that other harness files can point at it.
- Any branch, ticket, commit, or pull request rules that apply when the user submits these instruction changes.
- Maintenance: re-run after significant change, `record` the moment an agent gets something wrong, prefer a check over a new line.
- Rules repeating across their projects, or personal rather than the team's, belong in their global agent config.

Run `{workflow.on_complete}`.

### Refresh

Same steps, step 1 as a diff. Read the provenance line, re-verify every path and every caveat, and run `git log --diff-filter=DR --name-only` since the recorded SHA against every line — update or remove lines whose evidence is gone. Every proposed removal is a ledger entry shown in step 5, never a silent edit, and handwritten instructions outside the block are treated as in adoption — any proposal touching them enters the ledger. Never re-ask what a prior run settled; the interview shrinks to what changed about how the team works. The block grows only on new evidence.

### Adoption

Refresh against instructions this skill has never touched. Nothing was settled by a prior run, so the full interview applies — and the file itself is maintainer testimony, so the ledger is the run's main output: the user should be able to read it and see where each of their instructions went.

The proposal states what remains of every file instructions were moved out of — commonly a `CLAUDE.md` reduced to `@AGENTS.md`, once that import is verified for every harness in use, like any loading mechanism. No instruction lives in two loaded files, where it is paid for twice; a duplicate, verbatim or reworded, is kept once — the block keeps the survivor — and that settles both entries.

### Greenfield

Seeded from a spec or planning document, or interview alone. Commands that do not exist yet are written as explicit TODOs naming the decided stack, never a guessed invocation stated as fact, and verified on the first refresh after code exists. A genuinely contested design decision — real tradeoffs, multiple viable shapes — goes to `bmad-architecture`.

### Migration

If the target has a `project-context.md` from the retired skills, commonly under `{output_folder}`, read it in step 1 and offer to absorb its content. Do not delete it without agreement, and do not silently orphan it.

## Record

Capture one observed agent mistake as it happens — the only admissible source for a pitfall.

Take the task, the mistake, the correction, and its evidence. Check the block for a line already covering it. One occurrence is noted; a recurring or costly mistake earns a line now — an exact invocation under **Running and verifying** when it is a command error, otherwise a pitfall. Write it and show the diff. If it is mechanically preventable, propose the hook, lint rule, or CI check instead. Run `{workflow.on_complete}`.

## Audit

Re-check every caveat, path-check every file, follow every pointer, and ask of every line whether removing it would change agent behavior. Verify each command claim against the target or script that runs it. Check for contradictions with other instruction files.

Failing lines get fixed, move behind an observable trigger, or become ledger entries: a removal needs one of the four grounds in `best-practices.md`, presented and settled as in step 5 before anything is removed. **A policy or pitfall goes only when the thing it guards is gone or the user retires it; nothing failing lately is not grounds.** Audit ends smaller or equal. Run `{workflow.on_complete}`.

## Children

A component, nested repository, or extracted rules file gets its own file under the same shape when work keeps landing there and every condition holds: its rules are subtree-exclusive, they are substantial (a handful of rules is not a file), the split materially reduces the parent block, the loading mechanism is verified for every harness in use — checked, never assumed — and the user approves the split. Even with verified loading, keep a rule at the root when it must apply before a session enters that directory or when breaking it can affect work outside the child. Otherwise the rules stay in the parent block as path-qualified lines ("in `src/importer/`: ..."), which cost less than a file nobody loads. Why the loading check: `best-practices.md`.

Use a linked file only when the trigger is not a path.

A chosen child that ends with nothing its parent does not already say gets no file. Say so and move on.

List every child in the parent's **Where things are** with one line and its path. Discovery never depends on the harness finding it.
`````

---

## File: skills/bmad-retrospective/references/acceptance-verdict.md

`````markdown
# Decide: Routing and the Acceptance Verdict

Phase 4. Turn the consolidated findings into two outputs: routed action items the human can act on, and an honest verdict on whether the epic met its acceptance criteria. This skill proposes; it does not auto-apply fixes or edit the project spec. The human decides what executes.

## Route each finding

Give every finding two independent dispositions:

- **What to do about this instance** — *fix now*, *defer*, or *accept as-is*. Fix-now findings become action items. Deferred findings carry enough context to be acted on later without re-investigation. Accepted deviations are recorded so later retros stop re-flagging them.
- **What would prevent the next one** — the upstream lesson: spec wording, story sizing, a missing convention or gate, or nothing. This is where a recurring finding becomes a process change rather than a one-off fix.

Findings from sub-agents or the team discussion are unverified reports, not established facts. Before an action item relies on one, re-check it against the primary source — reopen the file, the commit, the spec. A finding whose source does not hold up is dropped, not routed.

## Action items

Compile fix-now findings and process lessons into specific, owned action items. Each names what to change and who owns it. Two kinds are *proposed, not applied* in this version:

- **Remediation** — code fixes are written up as action items (or story-shaped work) for the normal dev loop to execute later. The retrospective does not run the dev loop itself.
- **Spec reconciliation** — where the as-built diverges from the spec, propose the reconciliation as an action item with the evidence attached. The human applies it to the project contract; an uncertain interpretation is never written into the spec automatically.

## Previous-retro follow-through

When the previous epic's retrospective file exists, check whether the action items it committed to were completed. Read its Action items section, and its Previous-retro follow-through section for the items recorded there as not landed, and for every item, record one line in the retrospective document's Previous-retro follow-through section:

- **The item** — its action text as the previous file spells it, and its owner.
- **Whether it landed** — with the source that shows it: the commit, the file and line, the test. An item you cannot point at is "no evidence found", not "not done" — the reader must be able to tell a checked item from an unchecked one.

No status is written anywhere; the record is the follow-through. A run with no previous retrospective file, or one whose file has no Action items section, records that there was nothing to follow through on — and which of those it was, so a missing file is never mistaken for "no outstanding items."

## The verdict

Judge the final state against the epic file's Done when. If the epic file has none, profile the criteria from the diff and plans and mark the verdict as **profiled** rather than declared. Weigh verification results (the Phase 2 behavior check) and unresolved findings. Render one of:

- **Accepted** — criteria demonstrably met in the evidence, no blocking findings open, and **no unfinished tickets** for this epic.
- **Accepted-with-open-items** — criteria met, but named findings remain deferred and tracked — still only when every ticket of this epic is finished.
- **Rejected** — criteria not met, a blocking finding stands unresolved, **or any of this epic's tickets is still unfinished**.

### Unfinished tickets

`pending_tickets` is authoritative for this epic's incomplete work: the `ref`s of the `tickets.py status <folder>` rows that are unfinished — `status` not `built` and `state` not `done` or `dropped` — in build order. When that list is non-empty:

- The **machine** verdict is **rejected**. Name every unfinished ticket in the Acceptance verdict section as the evidence. Do not soften this to accepted-with-open-items: unfinished delivery is not an open finding about a finished epic — the epic itself is incomplete.

If the completeness check did not run (`tickets.py status` failed), do **not** render a rejected or accepted verdict from the absence of data — say the check was unavailable and weigh only the criteria and findings you have.

Three hard rules:

1. A human decision always overrides the machine verdict.
2. An epic that fails its criteria with **no** human decision is recorded as **not accepted** — never as silently accepted.
3. A non-empty `pending_tickets` list makes the machine verdict **rejected**, including in headless mode.

The verdict and its evidence carry into the Phase 5 document.
`````

---

## File: skills/bmad-retrospective/references/aggregate-views.md

`````markdown
# Aggregate Views

Phase 2. An epic is many coding sessions, each validated in isolation; the defects that matter are the ones no single session — and no single diff hunk — could see. Nine sessions each added three hundred lines and none ever saw the 3,000-line class they collectively built. These views are properties of the *whole* change, derived across the full diff range from Phase 1.

Prefer deterministic derivation: a script that measures the codebase is evidence; a model's impression is not. Where you compute a view inline instead of by script, record the narrowed scope. Every observation that becomes a finding carries a source reference — the file, the symbol, the commits. `{{ rendered("references/evidence-gathering.md") }}` is authoritative for what every `git_evidence.py` key means, including the commit-level `is_merge` and `stories` — read it there before deriving anything from the numbers.

## The catalog

- **Architecture delta** — how the dependency structure changed across the epic. Where a language-native dependency tool exists (dependency-cruiser, madge, pydeps, and the like), run it before and after the range and diff the graphs; otherwise derive the module/import graph from the changed files. Look for new cross-cutting dependencies, layering violations, and cycles introduced — structure the code's own conventions would forbid but no single ticket tripped.
- **Duplication map** — the same problem solved more than one way across tickets. Two sessions independently writing near-identical logic, or a helper reimplemented because the second session did not know the first existed.
- **God-class / size growth** — files that grew past a healthy size *over the epic*, invisible per-commit because each session added only a little. The `git_evidence.py` pre-pass (Phase 1) reports `added` / `deleted` / `net` per path in `files` — *change volume*, not a file's absolute size or a per-commit growth rate. Those sums cover the range's **non-merge** commits only, and they are always integers: an unmeasurable revision is left out of them rather than nulling them. Rank on `files`, then open the top of the ranking and read each file's real current size and structure before calling anything a god-class — high net churn makes a file a candidate to inspect, not a verdict on its own. Three qualifiers say how far the ranking can be trusted: `binary_revisions` counts that path's revisions whose churn could not be measured, so its true volume is *at least* what the sums report; `merges_measured` short of `merge_count` means some merges were never measured at all, which caps how complete the ranking can be; and `merge_files` mostly restates churn `files` already counted, so summing the two double counts — but it is not redundant, because a merge's first-parent diff also carries whatever the conflict resolution itself added, code that lives in no non-merge commit and therefore appears in `files` nowhere. So read `merge_files` separately, for the paths whose churn shows up only there, rather than discarding it as double counting. Whether a flagged file is genuinely a god-class or legitimately large stays your judgment.
- **Pattern divergence** — where the epic's code diverges from the conventions the surrounding codebase already established: naming, error handling, test structure, module boundaries. Agents learn conventions by pattern-matching the code, so divergence compounds.
- **Spec-to-implementation reconciliation** — where the as-built diverges from the epic file's Done when and the initiative Requirements each ticket's `covers` names, with PRD/architecture as context. Requirements silently dropped, added behavior nobody specified, intent reinterpreted between tickets. Each divergence is either a defect (fix), an accepted deviation (record so later runs stop re-flagging it), or a spec that should be reconciled to reality (propose in Phase 4).

## Delegation

When sub-agents are available, delegate the derivation: each returns evidence with source refs and checked scope, never a verdict — the parent consolidates and decides. Give each a narrow view and an explicit return format. When sub-agents are unavailable, compute the highest-value views inline (architecture delta and spec reconciliation first) and record which views were narrowed or skipped.
`````

---

## File: skills/bmad-retrospective/references/evidence-gathering.md

`````markdown
# Evidence Gathering

Phase 1 of the retrospective. Enumerate what the completed epic produced, so every later analysis works from real artifacts instead of memory. Output is an inventory: what exists, what is missing, and the diff ranges the rest of the retro will read.

## Inventory checklist

Collect what the epic produced and note the source path or range of each:

- **Epic file** — `epic-<slug>.md` in the epic folder, the folder's name plus `.md`: Description, Outcome, Done when, Boundaries, and Notes. Done when governs Phase 4; when the file has none, note that the verdict will be profiled from the diff.
- **Initiative requirements** — the Requirements section of the initiative file in the epic folder's parent. Each ticket's `covers` names ids there.
- **Entries** — the `tickets` rows of `tickets.py status <folder>`, and for each, `tickets.py find <folder> <ref>` with the row's `ref` (same command form as the workflow): its `description`, `verify`, and `covers` are what the build was given.
- **Story files** — `find`'s `story_file` when it is not null: the refined intent of a ticket a person refined.
- **Plans** — `find`'s `plan` for each ticket: its frontmatter (`status`, `baseline_revision`) and the sections Review Triage Log, Verification, Plan Change Log, and the dated `### <date>` blocks under `## Code Review`, absent when no review ran. These mark the boundaries between build sessions.
- **Diff ranges and commits** — the full set of changes the epic introduced, one range per plan (see Ranges from the plans below). For each range, run `uv run --no-cache {skill-root}/scripts/git_evidence.py --repo {project-root} --range <range> --stories <story-ids>` to get, as JSON, the commits in the range and the per-file change volume — added / deleted / net across the range — that Phase 2 reads. Record each range explicitly; Phase 2's aggregate views and the `bmad-review` pass both read them. When a range cannot be established, say so and narrow the scope rather than guessing. Read the output keys precisely: each commit carries `is_merge` and `stories` — *every* id its subject names, so a commit spanning two stories counts for both. `files` sums non-merge commits only. `merge_files` is each measured merge's diff against its first parent, so it *restates* the churn that merge brought in plus whatever the conflict resolution added — never add it into `files`, and never read it as merge-introduced work on its own. `merges_measured` counts the merges on the range head's first-parent spine; `merge_count` counts every merge in the range, so a gap between the two means merges went unmeasured. `binary_revisions` is unmeasured churn, not zero churn.
- **Previous retrospective** — `<folder name>-retrospective.md` in the previous epic's folder, if one exists, located as the workflow's Inputs say, so Phase 4 can check whether last epic's action items landed.
- **Session logs** — conversation or session records for the epic's tickets, when available. They are the only record of *why* a session took an unexpected turn — what was tried and abandoned. They are also the evidence most likely to be deleted or expire, so capture references now.

## Ranges from the plans

Each plan records its own `baseline_revision`, the commit its build started from, so there is no single epic-wide range. Order the plans by their baselines' place in history, oldest first (`git merge-base --is-ancestor A B` says A is older) — the order the builds started, which need not be the row order. A plan's range runs from its baseline to the next baseline in that order; the last plan's range runs to `HEAD`, marked inferred rather than recorded. When later work has landed since, end it at the last commit that belongs to this epic's tickets, judged from the plans and commit subjects, and record the cut. A plan whose `baseline_revision` is missing or `NO_VCS` gets no commit or diff evidence — record that too. Two plans sharing a baseline give the earlier one an empty range: that ticket simply has no commits of its own. Group the tickets sharing an identical range and run `git_evidence.py` once per distinct range, passing that group's `ref`s (`3.4`, as `status` spells them) as one comma-separated `--stories` value. A commit's `stories` is usually empty, since subjects rarely name a ref; the range is the attribution, and an empty list is not missing evidence. Ranges may overlap or diverge; count a shared commit or file change once in the aggregate views while keeping each ticket's range as its provenance.

## Missing evidence

Evidence availability varies; never hide a gap. Each later analysis declares what it needs and, when that input is absent, records a narrowed scope rather than guessing. A reader of the final retro must always be able to tell **"checked and clean"** from **"never checked."**

- Missing session logs → process-lesson analysis is skipped, and the retro says so.
- No Done when in the epic file → the verdict is profiled from the diff and plans, flagged as profiled rather than declared.
- Sub-agents unavailable → analyses that would delegate run inline over a narrowed scope, and the narrowing is recorded.

Carry the inventory forward into Phase 2 as the authoritative list of what is available to read.
`````

---

## File: skills/bmad-retrospective/references/retro-document.md

`````markdown
# Finalize: Retrospective Document

Phase 5. Finalize the retrospective document.

## The retrospective document

This document is the run's working artifact: it is created as a skeleton once the epic is fixed and filled as each phase completes, so Phase 5 finalizes rather than writes it from scratch. It lives at `<epic folder>/epic-<slug>-retrospective.md`, as readable markdown — a fixed name, so a resumed run finds it.

Open the document with YAML frontmatter a machine can read without parsing the prose — an epic gate or orchestrator keys off `verdict` to decide whether to hold the next epic:

```
---
epic: epic-<slug>
date: {date}
verdict: accepted | accepted-with-open-items | rejected
criteria: declared | profiled
headless: true | false
---
```

`epic` is the folder's name, as `tickets.py` spells the epic. Keep `verdict` in sync with the Acceptance verdict section below. Never write a `type` or `ticket` field into this frontmatter: `tickets.py` reads a markdown file in the epic folder as a ticket when `type` is a ticket type and as a plan when `ticket` is present. This frontmatter is the only machine-readable verdict; nothing in the tree records it, so a gate or orchestrator that acts on the verdict **must** read this file.

Sections:

- **Epic summary** — which epic, its tickets with their statuses, the tickets still at `built`, any tickets still unfinished (`pending_tickets`) that the user accepted retro-ing over, each plan's range, the evidence inventory (what was available, what was missing). Unfinished tickets force the machine acceptance verdict to **rejected** (see `{{ rendered("references/acceptance-verdict.md") }}`).
- **Findings** — grouped by aggregate view and by lens, each with its source reference and disposition (fix now / defer / accept). This is the record; do not summarize away the provenance.
- **Behavior verification** — what was exercised end to end and what was observed, or an explicit note that runtime behavior was not exercised.
- **Previous-retro follow-through** — if a prior retro exists, whether its action items landed, with evidence (`{{ rendered("references/acceptance-verdict.md") }}` specifies what to record).
- **Action items** — the routed fix-now items and process lessons, each with an owner. Note which are proposed remediation or spec reconciliations awaiting human application.
- **Acceptance verdict** — accepted / accepted-with-open-items / rejected, whether the criteria were declared or profiled, and the evidence behind the call.
- **Open questions** — what a human answer would materially change, and anything the analyses could not resolve.
- **Assumptions** — in headless runs, every choice made without the user: how the epic reference resolved to its folder, any non-empty `pending_tickets`, a machine **rejected** verdict forced by unfinished tickets or rendered with no human decision, each proposed item. Omit in interactive runs — an interactive run records the same facts where the user confirmed them, in Epic summary.

Do not state time estimates anywhere in the document.

## Finish

Report the document's path, the verdict, and the action-item count. Nothing else was written: no status changed, and no tree file was edited.

## On Complete

If anything appears below, follow it as the final terminal instruction before exiting; otherwise exit normally.

{{ workflow.on_complete }}
`````

---

## File: skills/bmad-retrospective/references/team-discussion.md

`````markdown
# Team Discussion (opt-in)

An optional discussion layer over Phase 2's findings, off by default. It exists for users who want the retrospective discussed from multiple perspectives, the way a team would. One rule: **the team discusses evidence, never invention.** Agents speak only to findings that carry source references. No agent may describe an event that did not happen or report a pattern the diff does not show.

## When to run it

Only when asked — "discuss it as a team," "run party mode," or similar. A default run never enters this phase.

## How to run it

Invoke **`bmad-party-mode`**, seeded with the consolidated Phase 2 findings and the epic context, so the installed agents react as real subagents with independent thinking rather than a scripted dialogue. Seed it with:

- The findings, each with its source reference, grouped by the aggregate view or lens that produced it.
- The improvements the evidence confirms — real gains, patterns that worked — so positive observations are grounded in fact.
- The epic's acceptance criteria (or the profiled stand-in), so the discussion can weigh the verdict.
- The previous epic's action items and whether they landed, when a prior retro exists, so accountability is grounded in fact.

If `bmad-party-mode` is unavailable, a discussion the user asked for must not silently fail to happen. Run it inline over the same seed — take each perspective yourself, hold every perspective to sourced findings — and record in the retrospective document that the discussion ran inline rather than through the installed agents. Record it as the narrowing it is: one model playing every role loses the independent disagreement that surfaces missed findings. State that in the document rather than omitting it.

Keep the user an active participant and steer toward systemic understanding over blame — the point is which process or convention would have prevented a finding, not who wrote the line. Capture anything the discussion surfaces that the analyses missed; a genuinely new observation becomes a finding only once you can tie it to a source, otherwise it is a question for Phase 4, not a conclusion.

The discussion does not replace Phase 4. Its output feeds the action items and the verdict; it does not render them.
`````

---

## File: skills/bmad-retrospective/scripts/git_evidence.py

`````python
# /// script
# requires-python = ">=3.11"
# ///
"""Measure git commit and file-change evidence over a revision range.

Prints ONLY JSON to stdout. Errors are emitted as JSON to stdout with a
non-zero exit code: 2 for invalid arguments (rejected before git runs),
1 for git or I/O failures. This script only MEASURES — it never judges
acceleration or violations. The model interprets the numbers.

Two git passes. The first lists every commit in the range (merges included)
and sums the per-file churn of the non-merge commits, which is what `files`
reports. The second runs only when the range contains merges and measures
those merges alone, reported separately as `merge_files` — never folded into
`files`, because a merge's diff against its first parent restates the churn
of the commits it merged in, which the first pass already counted.
"""

import argparse
import json
import os
import re
import subprocess
import sys

UNIT_SEP = "\x1f"
# sha, space-separated parents (empty for a root commit), subject.
LOG_FORMAT = f"--format=%H{UNIT_SEP}%P{UNIT_SEP}%s"


def _emit(obj, code=0):
    sys.stdout.write(json.dumps(obj))
    sys.exit(code)


class JsonArgumentParser(argparse.ArgumentParser):
    """Emit argparse failures on the JSON-only stdout contract, not usage text.

    The parser is constructed with ``add_help=False``. The override below covers
    ``error()``, but ``-h`` never reaches it: the built-in help action calls
    ``print_help()`` and ``exit(0)`` directly, which would put plain usage text
    on stdout with a zero exit and break the JSON-only contract. Removing the
    action instead of intercepting it routes ``-h`` through the already-tested
    ``error()`` path as an ordinary unrecognized argument. The cost is that the
    ``help=`` strings are unreachable from the CLI; the skill's references carry
    the usage a human needs.
    """

    def error(self, message):
        _emit({"ok": False, "error": f"argument error: {message}"}, 2)


def _parse_numstat_line(line):
    # numstat lines: "<added>\t<deleted>\t<path>"; binary files use "-".
    parts = line.split("\t")
    if len(parts) < 3:
        return None
    added_raw, deleted_raw, path = parts[0], parts[1], "\t".join(parts[2:])
    added = None if added_raw == "-" else int(added_raw)
    deleted = None if deleted_raw == "-" else int(deleted_raw)
    return added, deleted, path


def _git_log(repo, extra_args, rng):
    """Run one `git log --numstat` pass over `rng` and return its stdout.

    `core.quotePath=false` keeps non-ASCII paths as real UTF-8 strings instead
    of octal escapes, and `--no-renames` makes a rename an honest delete + add
    instead of an unopenable "src/{a => b}" pseudo-path that splits one file's
    churn across several keys. Both matter for every pass, so both live here.

    `log.diffMerges=separate` is pinned on the command line because it is what
    `-m` means: a user or repo config setting it to `off` makes pass 2 emit no
    file rows at all, so `merge_files` would come back empty beside a non-zero
    `merges_measured` and read as "the merges changed nothing".
    """
    cmd = [
        "git",
        "-c",
        "core.quotePath=false",
        "-c",
        "log.diffMerges=separate",
        "-C",
        repo,
        "log",
        "--numstat",
        "--no-renames",
        *extra_args,
        LOG_FORMAT,
        rng,
        "--",  # terminate rev parsing so the range can never match a pathspec
    ]
    try:
        # Decode explicitly: git emits UTF-8 path bytes regardless of the
        # caller's locale, and a C locale would otherwise decode them as ASCII.
        # surrogateescape, not replace: replace maps every invalid byte to the
        # same U+FFFD, so two distinct non-UTF-8 paths would collapse into one
        # `files` key with their churn silently summed. Lone surrogates survive
        # json.dumps (escaped as \udcXX under ensure_ascii) and json.loads.
        proc = subprocess.run(
            cmd,
            capture_output=True,
            text=True,
            encoding="utf-8",
            errors="surrogateescape",
            env={k: v for k, v in os.environ.items() if not k.startswith("GIT_")},
        )
    except Exception as exc:  # noqa: BLE001
        _emit({"ok": False, "error": str(exc)}, 1)

    if proc.returncode != 0:
        # stderr can be empty (a signal kill, a quiet failure); the exit code is
        # then the only thing left to report, so never emit an empty error.
        _emit(
            {
                "ok": False,
                "error": proc.stderr.strip() or f"git exited {proc.returncode}",
            },
            1,
        )
    return proc.stdout


def _parse_log(output, stories):
    """Turn one pass's log output into (commits, files_map). Shared by both."""
    commits = []
    files = {}  # path -> {path, _added, _deleted, binary_revisions, commit_count}
    seen = set()
    counting = True

    for raw in output.splitlines():
        if UNIT_SEP in raw:
            sha, parents, subject = raw.split(UNIT_SEP, 2)
            # Under -m, git repeats a merge's header once per parent unless it
            # also honours --first-parent (git 2.31+). Count only the first
            # block for a sha — git emits parents in order, so that block is
            # the first-parent diff either way, and no churn is double counted.
            counting = sha not in seen
            if not counting:
                continue
            seen.add(sha)
            commits.append(
                {
                    "sha": sha,
                    "subject": subject,
                    # Every id the subject names, in --stories order: a commit
                    # spanning two stories belongs to both. Word-boundary match
                    # so a story id like "1-2" does not also match "11-2".
                    "stories": [sid for sid in stories if re.search(rf"\b{re.escape(sid)}\b", subject)],
                    "is_merge": len(parents.split()) > 1,
                }
            )
            continue

        if not counting or not raw.strip():
            continue

        parsed = _parse_numstat_line(raw)
        if parsed is None:
            continue
        added, deleted, path = parsed

        entry = files.get(path)
        if entry is None:
            # _added/_deleted are running sums over the path's text revisions.
            entry = {
                "path": path,
                "_added": 0,
                "_deleted": 0,
                "binary_revisions": 0,
                "commit_count": 0,
            }
            files[path] = entry

        entry["commit_count"] += 1
        if added is None or deleted is None:
            # A binary revision is unmeasurable, not zero — count it alongside
            # the sums instead of nulling the path's real measured churn.
            entry["binary_revisions"] += 1
        else:
            entry["_added"] += added
            entry["_deleted"] += deleted

    return commits, files


def _file_list(files):
    return [
        {
            "path": entry["path"],
            "added": entry["_added"],
            "deleted": entry["_deleted"],
            "net": entry["_added"] - entry["_deleted"],
            "commit_count": entry["commit_count"],
            "binary_revisions": entry["binary_revisions"],
        }
        for entry in files.values()
    ]


def main(argv=None):
    parser = JsonArgumentParser(
        description=(
            "Measure commit and per-file change evidence over a git revision range. Measures only; does not judge."
        ),
        add_help=False,
    )
    parser.add_argument("--repo", default=".", help="Path to the git repo (default: .)")
    parser.add_argument("--range", dest="range", help="Revision range REV..REV")
    parser.add_argument(
        "--stories",
        help="Comma-separated story ids to match against commit subjects.",
    )
    args = parser.parse_args(argv)

    stories = []
    if args.stories:
        # dict.fromkeys dedupes while keeping the caller's order: a repeated id
        # would otherwise land twice in a commit's `stories`, double counting
        # that commit in any per-story total built from the output.
        stories = list(dict.fromkeys(s.strip() for s in args.stories.split(",") if s.strip()))

    if not args.range:
        _emit(
            {
                "range": None,
                "note": "no range supplied",
                "commits": [],
                "files": [],
            }
        )

    # Accept only an explicit REV..REV range. Anything else silently measures
    # the wrong thing: a leading "-" is consumed by git as an option, a single
    # rev logs all history up to it, a bare pathspec logs by path, an empty
    # endpoint ("..", "a..", "..b") makes git default that side to HEAD, and a
    # three-dot "A...B" is a symmetric difference — a different commit set
    # entirely. partition splits at the FIRST "..", so any of those extra-dot
    # shapes leaves `right` empty or dot-prefixed.
    left, _, right = args.range.partition("..")
    if args.range != args.range.strip() or args.range.startswith("-") or not left or not right or right.startswith("."):
        _emit(
            {
                "ok": False,
                "error": f"invalid --range {args.range!r}: expected a revision range like REV..REV",
            },
            2,
        )

    # Pass 1 — the listing. No extra args, so full topology: every commit in
    # the range including merges, which is what per-story attribution reads.
    # Merges contribute no numstat rows here, so `files` is non-merge churn.
    commits, files = _parse_log(_git_log(args.repo, [], args.range), stories)
    merge_count = sum(1 for commit in commits if commit["is_merge"])

    # Pass 2 — merge churn, only when there is any. `-m --first-parent
    # --min-parents=2` walks the range head's first-parent spine and emits
    # exactly one diff-against-first-parent block per merge sitting on it.
    # Merges off that spine are counted in merge_count and never measured,
    # which is precisely why merges_measured is a separate key: the gap
    # between the two is a visible statement that some merges went
    # unmeasured. This never folds into `files` — a merge's first-parent diff
    # restates the churn of the commits it merged in, which pass 1 already
    # counted, so adding it in would double count.
    merge_commits, merge_files = [], {}
    if merge_count:
        merge_commits, merge_files = _parse_log(
            _git_log(
                args.repo,
                ["-m", "--first-parent", "--min-parents=2"],
                args.range,
            ),
            stories,
        )

    _emit(
        {
            "range": args.range,
            "commit_count": len(commits),
            "merge_count": merge_count,
            "merges_measured": len(merge_commits),
            "commits": commits,
            "files": _file_list(files),
            "merge_files": _file_list(merge_files),
            "stories_supplied": stories,
        }
    )


if __name__ == "__main__":
    if sys.platform == "win32":
        # Piped output on Windows defaults to a legacy code page, not UTF-8.
        sys.stdout.reconfigure(encoding="utf-8")
        sys.stderr.reconfigure(encoding="utf-8")
    main()
`````

---

## File: skills/bmad-retrospective/bmod.toml

`````toml
[skill]
bmod = "bmod-method"
source = "github:bmad-code-org/BMAD-METHOD/skills"
`````

---

## File: skills/bmad-retrospective/customize.toml

`````toml
# DO NOT EDIT -- overwritten on every update.
#
# Workflow customization surface for bmad-retrospective. Mirrors the
# agent customization shape under the [workflow] namespace.

[workflow]

# --- Configurable below. Overrides merge per BMad structural rules: ---
#   scalars: override wins • arrays (persistent_facts, activation_steps_*): append
#   arrays-of-tables with `code`/`id`: replace matching items, append new ones.

# Steps to run before the standard activation.
# Overrides append. Use for pre-flight loads, compliance checks, etc.

activation_steps_prepend = []

# Steps to run after persistent facts load but before the workflow begins.
# Overrides append. Use for context-heavy setup that should happen
# once activation context is in place.

activation_steps_append = []

# Persistent facts the workflow keeps in mind for the whole run
# (standards, compliance constraints, stylistic guardrails).
# Distinct from the runtime memory sidecar — these are static context
# loaded on activation. Overrides append.
#
# Each entry is either:
#   - a literal sentence, e.g. "All retrospectives must produce SMART action items with named owners."
#   - a file reference prefixed with `file:`, e.g. "file:{project-root}/docs/standards.md"
#     (glob patterns are supported; the file's contents are loaded and treated as facts).

persistent_facts = []

# Scalar: executed at the end of Phase 5 (Finalize), after the retrospective
# document is saved. Override wins.
# Leave empty for no custom post-completion behavior.

on_complete = ""
`````

---

## File: skills/bmad-retrospective/SKILL.md

`````markdown
---
name: bmad-retrospective
description: 'Review a finished epic folder in the ticket tree against the evidence it left behind — the epic file, each ticket plan, diffs, commits — and produce a retrospective with sourced findings, action items, and an acceptance decision. Use when the user says "run a retrospective" or "lets retro the epic [epic]". Supports -H/--headless'
---

Run the following command exactly once without changing the current working directory. Replace `{project-root}` with the absolute path to the project root and `{skill-root}` with the absolute path to this skill's directory:

```bash
uv run --no-cache "{project-root}/_bmad/scripts/render_skill.py" --project-root "{project-root}" --skill "{skill-root}"
```

- On success, read and follow the one absolute `workflow.md` instruction printed to stdout.
- If the script is not found, BMad is not set up here. Offer to run the `bmad` skill's setup, installing `bmad` first if you do not have it (`npx skills add bmad-code-org/BMAD-METHOD --skill bmad`), then run the command above once more.
- On any other failure (including `uv` being unavailable), report the command output and HALT. Do not run any workflow source directly.
`````

---

## File: skills/bmad-retrospective/workflow.md

`````markdown
# Retrospective Workflow

**Goal:** Review a completed epic by reading the evidence it left in the ticket tree — the epic file, the initiative's requirements, each ticket's entry, story file, and plan, the diff and commits between plan baselines, and session logs when they exist. An unattended epic run leaves a record; this workflow reads that record, surfaces the defects no single ticket could show, and judges the epic against the criteria it set for itself.

Every finding you report carries a source reference (file, line, commit, or log). A claim you cannot point at — an invented root cause, a pattern the diff does not actually show — is not a finding. Drop it.

**CRITICAL:** If a phase directs you to another snapshot file, read it fully and follow it. No exceptions.

## Conventions

- Every operational cross-file reference in this workflow is an absolute snapshot path. Open it directly; do not resolve it relative to a skill directory.
- `{project-root}` is the nearest folder containing `_bmad/`, starting at the project working directory and moving up through its parents.
- `{date}` is the current system datetime. Never state time estimates — AI has changed development speed, so hour/day/week predictions are noise.

## Modes

Interactive by default. With `-H`/`--headless`: skip every confirmation, take the epic from the invocation, never open the team discussion, render the verdict on the evidence alone, and record each assumption made without the user (which epic was selected, the machine verdict, each proposed item) into the retrospective document's Assumptions section so the audit trail survives. The Phase 4 acceptance fail-safe still applies in headless runs.

For automation, `-H <epic folder | id | slug>` — an explicit epic in headless mode — is the stable orchestrator-facing interface. Headless with nothing named stops and reports. The offer of finished epics (see Inputs) is a human convenience, not an automation contract.

## On Activation

### Step 1: Execute Prepend Steps

Execute each of these steps in order before proceeding (`_None._` means skip):

{{ workflow.activation_steps_prepend }}

### Step 2: Load Persistent Facts

Treat every entry below as foundational context you carry for the rest of the workflow run. Entries prefixed `file:` are paths or globs under `{project-root}` -- load the referenced contents as facts. All other entries are facts verbatim (`_None._` means none):

{{ workflow.persistent_facts }}

### Step 3: Execute Append Steps

Execute each of these steps in order (`_None._` means skip):

{{ workflow.activation_steps_append }}

Activation is complete after all activation steps have run.

## Inputs

| Input | Where | Use |
|-------|-------|-----|
| epic | invocation argument — an epic folder, or an epic id or slug in the active initiative — or chosen from the offer below | which epic to retro |
| epic folder | `tickets.toml`, the epic file `epic-<slug>.md` (the folder's name plus `.md`), one `<type>-<slug>-plan.md` per ticket, and a story file where a ticket was refined | the record the epic left |
| initiative file | the epic folder's parent's same-named file, section Requirements | what each ticket's `covers` points at |
| architecture / prd | `architecture-*/architecture-*.md` and `prd-*/prd-*.md` in the initiative folder (the epic folder's parent) | context for judging as-built vs intended |
| previous retro (optional) | `<folder name>-retrospective.md` in the previous epic's folder: run `status` with no folder for the `epics` order; the previous epic's folder is the sibling folder of that name | check whether last epic's actions landed |
| session logs (optional) | conversation/session records for the epic's tickets | process lessons; record the gap when absent |

The epic is a folder in the ticket tree, named `epic-<slug>`; the retrospective is `epic-<slug>-retrospective.md` in it — the folder's name plus `-retrospective.md`. Run `tickets.py` as `uv run {project-root}/_bmad/method/scripts/tickets.py --project-root {project-root} <verb>`; a non-zero exit prints an error — show it and stop. A ticket is finished when its `status` is `built` or its `state` is `done` or `dropped`; unfinished is everything else. An epic with no `tickets` rows has nothing to retro: leave it out of the offer, and when it is named, say so and stop. Resolve the reference:

- **Folder given**: run `status <folder>`.
- **Id or slug given** (including `-H <id|slug>`): run `status` with no folder and match the argument against `epics` by `id` or `slug`. No match: list the epics and ask which; headless, stop and report. The folder is the parent of `epic_file` from `find <ref>`, `ref` taken from the first `tickets` row whose `epic` is the slug. Then run `status <folder>`.
- **Nothing given**: interactive, run `status` with no folder and offer the epics whose `tickets` rows are all finished; when none is, say so and ask which. Headless, stop and report.

`status <folder>`'s `tickets` rows, in the order returned, are the ticket list — that order is the build order. Each row carries `id`, `ref`, `title`, `status`, and `state`. `pending_tickets` is the `ref`s of the unfinished rows, scoped to this epic alone.

Then check the epic is actually finished before Phase 1. When `pending_tickets` is non-empty, interactively list those tickets and ask whether to retro an unfinished epic: if the user declines, stop and report — do not enter Phase 1; if they accept, record the tickets they accepted proceeding over in the document's Epic summary. Headless, proceed and record the same list in the Assumptions section — do not invent a confirmation. Either way the list sits in the document, and Phase 4's machine verdict is **rejected** when any ticket remained unfinished (see `{{ rendered("references/acceptance-verdict.md") }}`); a human may override interactively. Tickets still at `built` — finished by the build, not yet called done — are listed in Epic summary too. Then go to Phase 1.

## Working state and resumption

The retrospective document is the working artifact, not only the final output. Once the epic is fixed, create it as a skeleton (`{{ rendered("references/retro-document.md") }}` names the sections) and write each phase's result into it as you finish — inventory, then findings with sources, then dispositions and verdict. Continuity is re-reading the file.

The document is the retrospective file named above, a fixed name so a resumed run finds it. If it already exists, load it, reconcile its recorded state against the current evidence — the current evidence wins, since commits may have landed and questions may have been answered since — and resume at the first incomplete phase instead of redoing finished ones.

## Flow

Run the phases in order. A default run stops at a written evidence report and verdict; the team discussion in Phase 3 is opt-in.

Before Phase 1, interactively invite the user's going-in concerns ("anything you want weighted — a ticket that felt rushed, a risky interaction between two tickets?"). Use any answer to focus the Phase 1–2 analysis; it directs attention but never becomes a finding without a source.

### Phase 1 — Gather

Enumerate what the epic actually produced and record what is missing. Read fully and follow `{{ rendered("references/evidence-gathering.md") }}` for the inventory checklist, the `git_evidence.py` pre-pass that derives each plan's diff range and commits, and the missing-evidence rule: each later analysis declares what it needs and records a narrowed scope when the evidence is absent, so a reader can always tell "checked and clean" from "never checked."

### Phase 2 — Analyze

Produce findings, each with a source reference, from three angles:

- **Aggregate views** — the defects no single diff hunk shows: architecture delta, duplication map, god-class growth, pattern divergence, spec-to-implementation reconciliation. Read fully and follow `{{ rendered("references/aggregate-views.md") }}` for the catalog and how to derive each (deterministic scripts first).
- **Diff-scope review** — do not reimplement review. Invoke **`bmad-review`** on the epic's diff for the code lenses (adversarial, edge-case, verification-gap), weighting the boundaries between tickets, where no single session ever saw both sides. Fold its findings in. If `bmad-review` is unavailable, run those lenses inline over the diff on a narrowed scope and record the narrowing.
- **Behavior check (when the epic changed runtime behavior)** — exercise the changed flows end to end and record what you observed. Passing tests do not substitute for running the system.

Consolidate: merge, dedupe, and provenance-link findings. Drop any finding you cannot tie to a source.

### Phase 3 — Team Discussion (opt-in)

Skip by default; never runs headless. When the user asks to "discuss it as a team," "run party mode," or similar, invoke the skill `bmad-party-mode` seeded with the Phase 2 findings so the installed agents react to real evidence — the god class the diff really grew, the verification gap that is actually there, the wins the evidence confirms. Read fully and follow `{{ rendered("references/team-discussion.md") }}` for how to seed it and keep it grounded. If `bmad-party-mode` is unavailable, run the discussion inline over the Phase 2 findings and record the narrowing. The rule: agents speak only to findings with sources.

### Phase 4 — Decide

- **Action items** — compile fix-now findings and process lessons into specific, owned action items. Fixes and spec reconciliations are *proposed here*, not auto-applied; the human decides what to execute.
- **Acceptance verdict** — judge the final state against the epic file's Done when (profile the criteria from the diff and plans if it has none): **accepted**, **accepted-with-open-items**, or **rejected** — one spelling, everywhere a machine reads it. Unfinished tickets in `pending_tickets` force the machine verdict to **rejected**. A human decision always overrides. An epic that fails its criteria with no human decision is recorded as *not accepted* — never as silently accepted. Read fully and follow `{{ rendered("references/acceptance-verdict.md") }}` for the rubric, the finding-routing dispositions, and the previous-retro follow-through record.

### Phase 5 — Finalize

Finalize the retrospective document and stop. Read fully and follow `{{ rendered("references/retro-document.md") }}` for the document's location, frontmatter, and sections, and the terminal instruction that ends the run. The document is the run's only write: no status change, no `tickets.py mark`, and no edit to the epic file, any plan, or any story file. Closing the epic is `bmad-ticket`'s, confirmed by the user.
`````

---

