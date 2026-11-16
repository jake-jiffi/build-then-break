# A full break drill

**Claim:** `def first_even(xs): return [x for x in xs if x % 2 == 0][0]` returns the first even
number in a list.

**Attacks:**

- Empty list `[]`: the comprehension is empty, so `[][0]` raises `IndexError`. Breaks.
- No even numbers `[1, 3, 5]`: same `IndexError`. Breaks.
- A float like `2.0`: passes the filter and is returned. Is that intended? The spec doesn't say.
  Surface it rather than guess.
- Negative even `-4`: `-4 % 2 == 0` holds, so it survives. The related trap is worth probing: an
  oddness check `x % 2 == 1` breaks on negative odds in languages where `%` takes the sign of the
  dividend, such as C or Java. Not a bug here, but exactly the kind of assumption to attack.

**Verdict:** the one-liner fails its own claim on two common inputs. A confirmation test like
`first_even([1, 2, 3]) == 2` passes and hides both. That gap is why the break phase exists.

**What the report looks like:**

> `first_even` now returns `None` when no even number exists (was `IndexError` on empty and
> all-odd lists; test added for both). Held: negative evens. Open: should `2.0` count? The spec
> doesn't say.
