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
  - Bash
  - Grep
---

# Story-Telling Skill (v0.1.0 — Positioning)

You are running as an SMCFactory agent. Your job here is to produce
**one positioning statement** for an approved product using the exact
four-blank template from the brand-strategy framework, derived from
the inputs already captured in Verdict's design doc.

## Inputs

- **A design doc** at `ideas/{project-name}/design-doc.md` (Verdict
  produced it in Phase 5 of the verdict-eval skill, then posted it as
  a fenced comment in Phase 7, and you extracted and committed it).
- **The {project-name}** from the file path.

## Pre-flight checks

Before reading the design doc, verify all required sections exist.
Run these checks — if any fail, HALT immediately (do not attempt
partial extraction):

```bash
DOC="ideas/{project-name}/design-doc.md"

# Verify file exists
test -f "$DOC" || echo "HALT: design doc not found at $DOC"

# Verify required sections
grep -q "## Target User"            "$DOC" || echo "HALT: missing '## Target User & Narrowest Wedge'"
grep -q "## Problem Statement"      "$DOC" || echo "HALT: missing '## Problem Statement'"
grep -q "## Recommended Approach"   "$DOC" || echo "HALT: missing '## Recommended Approach'"
grep -qE "## Demand Evidence|## Success Criteria" "$DOC" || echo "HALT: missing '## Demand Evidence' or '## Success Criteria'"

# Check for SPECULATIVE flag in recommended approach
grep -A 20 "## Recommended Approach" "$DOC" | grep -qi "SPECULATIVE" && echo "HALT: recommended approach is marked SPECULATIVE"
```

If any check prints HALT, stop. Report the missing section to the
caller. Verdict must fix the design doc before this skill can run.

If all checks pass, proceed to extraction.

---

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

## Extraction logic

Follow these steps in order. Do not skip steps. Do not improvise.

```
STEP 1 — EXTRACT PRODUCT NAME:
  1. Read the first H1 heading ("# Design: {name}")
  2. Extract {name} after the colon
  3. Store as {product_name}
  4. IF no H1 heading with "Design:" → HALT: "design doc has no title"

STEP 2 — EXTRACT AUDIENCE:
  1. Read section "## Target User & Narrowest Wedge"
  2. Find the first named person, role, or specific cohort
  3. IF the section only contains generic categories
     ("developers", "enterprises", "users", "teams",
     "companies", "organizations") →
     HALT: "audience is a category, not a person —
     Verdict's Q3 (Desperate Specificity) failed"
  4. ELSE → store as {audience}

STEP 3 — EXTRACT PROBLEM:
  1. Read section "## Problem Statement"
  2. Read section "## Status Quo" for cross-reference
  3. Pick the FUNCTIONAL framing — what users DO badly today
     (not how they FEEL about it)
  4. IF Problem Statement is empty or only emotional language
     ("frustrated", "struggling", "overwhelmed") without a
     functional description → rewrite functionally using
     Status Quo details
  5. Store as {problem}

STEP 4 — EXTRACT SOLUTION:
  1. Read section "## Recommended Approach"
  2. Reduce to ONE phrase that names what the product does
  3. Check phrase against forbidden words:
     "platform", "tool", "solution", "AI-powered",
     "next-generation", "seamless", "intelligent", "smart"
  4. IF any forbidden word found → rewrite the phrase
     without it (describe the mechanism, not the category)
  5. IF the section says "SPECULATIVE" → HALT (pre-flight
     should have caught this, but double-check)
  6. Store as {solution}

STEP 5 — EXTRACT BENEFIT:
  1. Read section "## Demand Evidence"
  2. Read section "## Success Criteria"
  3. Pick the USER'S OUTCOME, not the product's feature
     Good: "reach their next funding milestone"
     Bad:  "powered by machine learning"
  4. IF both sections are empty →
     HALT: "no demand evidence or success criteria to
     source benefit from"
  5. Make it concrete and measurable where possible
  6. Store as {benefit}

STEP 6 — ASSEMBLE:
  Compose: "For {audience}, who {problem}, {product_name}
  {solution} that allows them to {benefit}."

STEP 7 — VALIDATE:
  1. Count sentences: MUST be 1 (max 2)
     IF 3+ sentences → cut to the most load-bearing parts
  2. Re-check {audience} is not a generic category
  3. Re-check {solution} contains no forbidden words
  4. Re-check {benefit} is an outcome, not a feature
  5. Read the assembled statement as a stranger would —
     could they explain what this product does and who
     it's for from ONLY this sentence?
     IF no → revise until yes
  6. IF any check fails → revise, do not ship weak output

STEP 8 — WRITE:
  Write the assembled statement to
  ideas/{project-name}/positioning.md
  Nothing else in the file. No header. No preamble.
  Report: "Positioning written to
  ideas/{project-name}/positioning.md"
```

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

Halts are enforced at two layers:

1. **Pre-flight checks** (bash grep, before any reading) — catches
   missing sections immediately.
2. **Extraction steps** (STEP 1–5 above) — catches content-level
   problems (category instead of person, emotional instead of
   functional, speculative approach, empty evidence).

When halting at either layer: **do not produce a weak positioning**.
Report to the caller exactly which input was missing and which
design doc section it should have come from. Verdict must fix the
design doc, then Scout re-invokes this skill.

## Output format

STEP 8 of the extraction logic writes the file. The content is
exactly the assembled statement from STEP 6 — one paragraph, no
frontmatter, no header, no trailing text. The file path is
`ideas/{project-name}/positioning.md`.

## Why this is named "story-telling" instead of "positioning"

The positioning statement is one piece of the broader Craft step from
the brand framework (Learn → Define → Craft). The Craft step has five
deliverables: Positioning, Product Messaging, Vision/Mission, Audience
Messaging, Tone of Voice. v0.1.0 implements only the first. Future
versions will add the rest as downstream needs justify them. The
skill is named after the eventual scope, not the v0.1.0 scope.
