# story-telling

Story-telling skill for [SMCFactory](https://github.com/smcfactory) agents.

This skill produces a single **positioning statement** for an approved
product, using the four-blank template from
[Building Brands for Startups with Steven Ebert](https://youtu.be/QJ7Q-w_6k-g?si=jKT3VBlxqHb13J69):

```
For [audience], who [problem],
[product name] [solution]
that allows them to [benefit].
```

## When it runs

Scout invokes `/story-telling` after Verdict has APPROVED an idea and
Scout has saved the design doc to `ideas/{project-name}/design-doc.md`
in [smcfactory/factory-ops](https://github.com/smcfactory/factory-ops).
The skill reads the design doc and writes
`ideas/{project-name}/positioning.md` next to it. Both files get
committed in the same Scout commit.

## Why one sentence matters

Until a product has revenue, all you have is a story. Tech researchers
in Stage 2, future investors, future hires, and future users will all
read the design doc looking for the **one paragraph** that tells them
what this is, who it's for, and why anyone should care. The positioning
statement is that paragraph.

A weak positioning means downstream agents are guessing about audience,
problem, and benefit. A strong positioning means everything that follows
— marketing site, pitch deck, onboarding copy — has a single grounded
reference.

## Rules the skill enforces

- **Specific audience.** A named person or named cohort, never a category
  ("enterprises", "developers", "users").
- **Functional problem.** What they're doing badly today, sourced from
  Verdict's Q2 (Status Quo) answers in the design doc.
- **Concrete solution.** No "platform", "tool", "AI-powered", "next-
  generation", "seamless". Reference what the product actually does.
- **Outcome benefit.** What the user gains, not what the product features.
  Sourced from Demand Evidence + Success Criteria.
- **One sentence.** Two at most. If you need three, you're overstuffing.

If the design doc is missing any of the four required inputs, the skill
**halts and reports** rather than producing a weak statement. Verdict
should fix the design doc first.

## Worked example (from the brand framework)

> For early-stage founders who have technical/operational expertise but
> often lack marketing savvy, **New Vision** offers tactical advice and
> a streamlined process for taking a brand to market that allows them
> to quickly and affordably reach their next major funding, hiring, or
> growth milestone.

Notice: specific audience, specific problem, named product, concrete
solution, measurable benefit. That's the shape every output of this
skill must match.

## Scope of v0.1.0

This version implements **only the positioning statement**. Future versions
will add the rest of the Craft step from the brand framework:

- 0.2.0 — Product Messaging (3-5 sentence functional explanation)
- 0.3.0 — Vision / Mission (why this matters now + bigger aspiration)
- 0.4.0 — Audience Messaging (tailored variants for customers / hires / investors)
- 0.5.0 — Tone of Voice (writing rules for the product's audience)

Each expansion will be driven by a real downstream need, not by speculation.

## Layout

```
story-telling/
├── README.md        # This file
├── SKILL.md         # The skill itself (read by Claude Code)
└── (future versions may add more files)
```

## Install

The skill is intended to be installed on a VPS where a Claude Code agent
runs. Clone and symlink:

```bash
git clone git@github.com:smcfactory/story-telling.git ~/story-telling
ln -sf ~/story-telling ~/.claude/skills/story-telling
```

After installation the agent invokes the skill by name: `/story-telling`.

## Related

- [smcfactory/factory-ops](https://github.com/smcfactory/factory-ops) — coordination layer where ideas are evaluated
- [smcfactory/verdict-eval](https://github.com/smcfactory/verdict-eval) — Verdict's evaluation skill that produces the design docs this skill reads
