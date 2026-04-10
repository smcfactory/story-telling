---
name: story-telling
version: 0.1.0
description: |
  Produces a positioning statement for an approved product using the
  four-blank brand-strategy template (For [audience] who [problem],
  [product name] [solution] that allows them to [benefit]). Reads a
  Verdict-produced design doc and writes positioning.md alongside it.
  v0.1.0 covers positioning only; future versions will add the rest
  of the Craft step (product messaging, vision/mission, audience
  messaging, tone of voice).
benefits-from: verdict-eval
allowed-tools:
  - Read
  - Write
---

# Story-Telling Skill (v0.1.0 — Positioning)

You are running as Scout. Your job here is to produce **one positioning
statement** for an approved product using the exact four-blank template
from the brand-strategy framework, derived from the inputs already
captured in Verdict's design doc.

## Inputs

- **A design doc** at `ideas/{project-name}/design-doc.md` (Verdict
  produced it in Phase 5 of the verdict-eval skill, then posted it as
  a fenced comment in Phase 7, and you extracted and committed it).
- **The {project-name}** from the file path.

## Output

A new file at `ideas/{project-name}/positioning.md` containing exactly
one positioning statement. Nothing else — no header, no preamble, no
commentary, no surrounding prose.

## The template

```
For [audience], who [problem],
[product name] [solution]
that allows them to [benefit].
```

This is the entire output. One sentence (or two if "the solution"
genuinely needs an extra clause). It must be specific enough that a
reader who saw only this paragraph could explain what the product
does and why someone should care.

## Sourcing the four blanks

Read the design doc carefully. Source each blank from a specific
section:

- **[audience]** — from the design doc's "Target User & Narrowest
  Wedge" section. Use the specific human or named cohort identified
  there. **Never use a category.** "Enterprises", "developers",
  "users", "marketing teams" are filters, not audiences. If the
  design doc only has a category, the design doc failed Verdict's
  Q3 (Desperate Specificity). Halt and report (see "Halting" below).

- **[problem]** — from the design doc's "Problem Statement" section,
  cross-referenced with "Status Quo" (Verdict's Q2 answers). State
  the problem **functionally**, not emotionally. The framing should
  match what users are actually doing badly today, in their own words
  where possible.

- **[product name]** — the project name from the design doc title
  ("# Design: {project-name}").

- **[solution]** — from the design doc's "Recommended Approach"
  section in Phase 4 Level 1. One phrase that names what the product
  actually does. **Forbidden words: "platform", "tool", "solution",
  "AI-powered", "next-generation", "seamless", "intelligent",
  "smart".** These are filler words that hide the absence of a real
  description. Reference the actual mechanism.

- **[benefit]** — from the design doc's "Demand Evidence" (Verdict's
  Q1 answers) and "Success Criteria". This is the **user's outcome**,
  not the product's feature list. Concrete, measurable when possible.
  ("Quickly and affordably reach their next major funding milestone"
  is a benefit. "Powered by AI" is not a benefit, it's a feature.)

## The shape you must match

Worked example (this is the format and tone you should produce):

> For early-stage founders who have technical/operational expertise
> but often lack marketing savvy, New Vision offers tactical advice
> and a streamlined process for taking a brand to market that allows
> them to quickly and affordably reach their next major funding,
> hiring, or growth milestone.

Notice every part:

- **Specific audience:** "early-stage founders with technical/
  operational expertise" (not "founders" or "startup people")
- **Specific problem:** "lack marketing savvy" (not "marketing
  challenges")
- **Named product:** "New Vision"
- **Concrete solution:** "tactical advice and a streamlined process
  for taking a brand to market" (not "a marketing platform" or "an
  AI-powered tool")
- **Measurable benefit:** "quickly and affordably reach their next
  major funding, hiring, or growth milestone" (not "grow faster" or
  "increase efficiency")

## Rules

1. **One sentence.** Two at most. If you need three, you're
   overstuffing — pick the most load-bearing parts and drop the rest.
2. **No preamble in the output file.** No "Here is the positioning:".
   No section header. No commentary. Just the statement.
3. **No filler nouns.** Re-read the forbidden words list above.
4. **No marketing language.** Don't sell the product — describe it.
5. **Specific over abstract every time.** If you can't be specific
   about a blank, the design doc didn't give you enough — halt.

## Halting

If any of these are true, **do not produce a weak positioning**.
Halt the skill and report what's missing:

- The design doc has no "Target User & Narrowest Wedge" section, OR
  that section only contains a category (not a specific human).
- The design doc has no "Problem Statement" section.
- The design doc has no "Recommended Approach" section in Phase 4
  Level 1.
- The design doc has no "Demand Evidence" or "Success Criteria"
  section.
- The recommended approach is marked SPECULATIVE (Verdict's design
  doc rules forbid speculative recommendations; if one slipped
  through, Verdict's design doc has a different problem).

When halting, write a comment to the user (not a file) describing
exactly which input was missing and which design doc section it
should have come from. Verdict should fix the design doc, then
re-invoke this skill.

## Output format

Write `ideas/{project-name}/positioning.md`. The file content is:

```
For [audience], who [problem], [product name] [solution] that allows them to [benefit].
```

That's the entire file. One paragraph. No frontmatter, no header, no
trailing newline beyond the standard one.

Then report to the caller: "Positioning written to
`ideas/{project-name}/positioning.md`."

## Why this is named "story-telling" instead of "positioning"

The positioning statement is one piece of the broader Craft step from
the brand framework (Learn → Define → Craft). The Craft step has five
deliverables: Positioning, Product Messaging, Vision/Mission, Audience
Messaging, Tone of Voice. v0.1.0 implements only the first. Future
versions will add the rest as downstream needs justify them. The
skill is named after the eventual scope, not the v0.1.0 scope.
