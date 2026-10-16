---
name: prompt-refiner
description: >-
  Improve and iteratively refine any agent prompt based on feedback about what
  went wrong. Use when the user says a prompt isn't working, the agent made a
  mistake, the output was wrong or incomplete, or they want to fix/improve/tune
  a prompt before sending it to an agent. Works with any agent, any domain,
  any prompt type (system prompt, task instructions, role definition, few-shot).
---

# Prompt Refiner

Iteratively improve agent prompts by diagnosing why a prompt failed and
producing a corrected version. Generic — works with any agent, any domain.

## When to use

- The user says an agent did something wrong and wants to fix the prompt.
- The user has a prompt draft and wants to review it before sending it.
- The user pastes an output that was wrong/incomplete and asks to improve the
  prompt that generated it.
- The user says "the prompt isn't working", "the agent ignored X", "it keeps
  doing Y instead of Z", or similar feedback.

## What the user provides

| Input | Required? | What it is |
|-------|-----------|------------|
| Current prompt | Yes | The full text the user sent (or plans to send) to the agent |
| What went wrong | Yes | Description of the undesired behavior, wrong output, or missing result |
| Expected behavior | Helpful | What the user wanted instead (if not obvious from the complaint) |
| Example output | Optional | A concrete sample of the bad output, for precision |
| Agent / platform | Optional | Where the prompt runs (e.g. Toqan assistant, ChatGPT, Claude, custom) — helps tailor syntax |

If the user only describes the problem verbally without the prompt text, ask
for the current prompt before proceeding. Do not invent a prompt you have not
seen.

## Diagnosis framework

Before rewriting anything, identify which failure pattern(s) apply. A single
prompt can have several.

| # | Pattern | How it manifests | Fix direction |
|---|---------|------------------|--------------|
| 1 | **Ambiguity** | Instruction can be read multiple ways; agent picks the wrong one | Replace vague words with concrete, unambiguous terms |
| 2 | **Missing context** | Agent lacks background it needs to act correctly | Add the necessary context, definitions, or reference data |
| 3 | **Missing constraints** | No guardrail prevents the undesired behavior | Add explicit "do not" rules and boundary conditions |
| 4 | **Output format mismatch** | Expected shape (table, list, JSON, paragraph) is unclear or absent | Specify the exact output structure with an example |
| 5 | **Over-instruction** | Too much detail creates confusion or contradictions | Simplify; split into clear numbered steps; remove redundancy |
| 6 | **Under-instruction** | Too vague; agent fills gaps with assumptions | Add the missing steps, criteria, or decision rules |
| 7 | **Conflicting instructions** | Two rules contradict each other | Resolve the conflict; state priority order explicitly |
| 8 | **No examples** | Agent has no anchor for expected behavior | Add 1–2 few-shot examples showing correct input→output |
| 9 | **Wrong role framing** | Agent persona doesn't match the task | Reframe the role to align with the desired behavior |
| 10 | **Implicit assumption** | Prompt assumes the agent knows something it doesn't | Make the assumption explicit or provide the data |

## Improvement workflow

### Step 1 — Receive and parse

Collect the current prompt and the feedback. If either is missing, ask. Restate
the problem in one sentence so the user can confirm you understood correctly.

### Step 2 — Diagnose

Identify which pattern(s) from the table above apply. Explain briefly why each
applies, citing the specific part of the prompt. Do not skip this step — the
diagnosis is what makes the fix trustworthy.

### Step 3 — Propose the fix

Rewrite only the parts that need changing. Preserve everything that already
works. Present the result as:

1. **Diagnosis** — numbered list of identified patterns with explanation.
2. **Changes** — for each change: what was removed/added/rewritten and why.
3. **Improved prompt** — the full updated prompt, ready to copy.

### Step 4 — Offer iteration

End by asking if the user wants to test the improved prompt or refine further.
If they report another issue, return to Step 2 with the new feedback. Keep
each iteration focused on the new problem — do not re-litigate fixed issues.

## Guidelines

- **Minimal changes.** Fix what is broken; do not rewrite working parts. This
  keeps the prompt stable and the diff easy to review.
- **Preserve the user's voice.** If the prompt is in Portuguese, keep it in
  Portuguese. Match tone, formality, and language of the original.
- **Explain, don't just rewrite.** The user needs to understand the fix so they
  can apply the same principle to future prompts. One sentence per change is
  enough.
- **No invented content.** If the fix requires domain knowledge the user hasn't
  provided (e.g. specific business rules, data formats), flag it and ask rather
  than guessing.
- **Format the improved prompt in a code block** so it is easy to copy.
- **Keep the diagnosis concise.** The user wants the fix, not an essay. One line
  per identified pattern.

## Quick reference: prompt anatomy

When reviewing a prompt, check that each layer is present and clear:

```
Role        → Who the agent is and what it produces
Context     → Background information the agent needs
Task        → What the agent should do, step by step
Constraints → What the agent must not do, boundaries, rules
Format      → Exact output shape, with example if possible
```

A failure often maps to a missing or weak layer. Use this as a second lens on
top of the diagnosis table.
