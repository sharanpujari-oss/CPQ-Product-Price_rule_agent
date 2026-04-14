# CPQ Price Rule — Request Form

---

**Rule Name:**

**What does this price rule do?**
[Describe the business scenario — e.g. "When a customer buys more than 100 licenses, apply a 10% discount automatically"]

**What does it change on the quote?**
- [ ] Sets a discount
- [ ] Sets a price / net price
- [ ] Sets a field value on the quote line
- [ ] Other:

**Which quotes does this apply to?**
- [ ] New quotes
- [ ] Renewal quotes
- [ ] Amendment quotes
- [ ] All quotes

---

## When should the rule fire?

> List each condition that must be true for the rule to trigger.

| # | What to check | Condition | Value | Plain English explanation |
|---|---|---|---|---|
| 1 | | equals / greater than / less than / contains / is blank | | e.g. "Quantity is greater than 100" |
| 2 | | equals / greater than / less than / contains / is blank | | |
| 3 | | equals / greater than / less than / contains / is blank | | |

**All conditions must be true, or just one?** All / Any

---

## What should change when the rule fires?

> Describe what field gets updated and what value it should be set to.

| # | What field to update | Set it to | Source of value |
|---|---|---|---|
| 1 | e.g. Discount | e.g. 10 | Fixed value / Another field / Formula |
| 2 | | | |

---

## What to count on the quote *(if needed)*

> Only fill this if a condition needs to count or sum lines on the quote.

| What are you counting / summing? | Which field? | Filter — which product(s)? |
|---|---|---|
| e.g. Total quantity of a specific SKU | Quantity | e.g. SKU-1234 |
| | | |
