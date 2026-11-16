---
name: build-then-break
description: >-
  Write and check code without wasted effort. Build with restraint (think first, simplest thing
  that works, surgical diffs, a verifiable goal), then switch sides and try to break your own work
  before calling it done. Use for any request to write, edit, refactor, debug or review code, and
  before declaring any hard or high-stakes task, plan or analysis finished. Effort scales with the
  stakes, so trivial edits stay cheap. Replaces karpathy-guidelines and adversarial-review.
license: MIT
---

# Build, then break

Build with restraint. Then become the adversary and try to break it. Pick the level first, because
effort and output both scale with the stakes.

## Pick the level

| Level | When | Build | Break |
|---|---|---|---|
| Trivial | Typo, rename, config value, obvious one-liner | Surgical change only | Run it. Nothing more. |
| Standard | Feature, bug fix, refactor | All four build rules | The project's tests, plus the three attacks most likely to land |
| High-stakes | Money, auth, data migrations, production deploys, security, concurrency, anything hard to undo, or a plan someone will act on | All four rules, plan written as checked steps | The full drill, every relevant vector |

Unsure? Standard.

## Keep it cheap

- Don't restate these rules or narrate the phases in your reply.
- Run a check rather than reason about one. One real test beats a paragraph of analysis.
- Report three things only: what changed, any attack that broke it and the fix, and one line on
  what else held. "Held: empty input, duplicate IDs, retry after timeout (test added)."
- Read `references/` only when stuck. `examples.md` shows each build rule in code.
  `worked-example.md` shows a full break drill.

## Build

1. **Think first.** State your assumptions. If two readings exist and the choice changes the
   outcome, ask. Otherwise take the likelier one, say which, and carry on.
2. **Simplest thing that works.** No features, abstractions, settings or error handling nobody
   asked for. If 200 lines could be 50, rewrite it.
3. **Surgical changes.** Touch only what the task needs and match the existing style. Mention
   unrelated problems; don't fix them. Remove only the orphans your own change created.
4. **A verifiable goal.** Turn the ask into a check. "Fix the bug" becomes a test that reproduces
   it, then passes. For multi-step work, write a short plan with a check on each step.

## Break

1. **State the claim exactly.** "Returns the median of any non-empty list." You can't attack a
   vague claim.
2. **Attack where a subtle bug would hide.** Skip vectors that don't apply.
   - Boundaries: empty, one, huge, zero, negative, duplicates, off-by-one.
   - Hostile input: null, wrong type, malformed data, unicode, injection.
   - Hidden assumptions: ordering, uniqueness, time zones, encoding.
   - State: re-entry, races, retries, partial failure, idempotency.
   - Failure: dependency down, timeout, truncated response.
   - Scale: 100 times the input.
   - Spec: what was asked against what you assumed was asked.
3. **Force a verdict on each attack.** It survives, with a reason or a test, or it breaks. A break
   gets fixed, and the fix faces the drill again.
4. **Write the test you're slightly afraid to run.** Aim it where the code would fail if it were
   subtly wrong, not at the path you already know works.
5. **Stop when it survives a real attack**, not at the first green tick.

## Plans, decisions and analyses

State the conclusion. Argue the strongest opposite case properly. Keep the conclusion if it
survives; revise it if it doesn't.

## Credits

Build rules adapted from karpathy-guidelines (MIT). Break drill adapted from adversarial-review
(MIT).
