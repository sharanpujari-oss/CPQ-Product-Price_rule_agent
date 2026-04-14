# CPQ Product Rule — Request Form

---

**Rule Name:**

**What problem does this rule solve?**
[Describe the business scenario — e.g. "Reps were quoting hardware without the required software license"]

**What should the rule do when it fires?**
- [ ] Block the quote from saving (Validation)
- [ ] Show a warning but still allow saving (Alert)
- [ ] Automatically add/remove a product (Selection)

**Which quotes/quote line does this apply to?**
- [ ] New quotes
- [ ] Renewal quotes
- [ ] Amendment quotes
- [ ] All quotes

**Error / Alert message shown to the user:**
[What message should the rep see? e.g. "This product requires an active software entitlement."]

---

## When should the rule fire?

> List each condition that must be true for the rule to trigger.

| # | What to check | Condition | Value | Plain English explanation |
|---|---|---|---|---|
| 1 | | equals / greater than / less than / contains / is blank | | e.g. "Quote has at least 1 hardware line" |
| 2 | | equals / greater than / less than / contains / is blank | | |
| 3 | | equals / greater than / less than / contains / is blank | | |

**All conditions must be true, or just one?** All / Any

---

## What to count on the quote

> Used when a condition needs to count how many lines of a certain product exist.

| What are you counting? | Which field to count | Filter — which product(s)? |
|---|---|---|
| e.g. Number of hardware lines | Quantity | e.g. RSCP HW SKUs |
| | | |

---

## Automatic actions when rule fires *(only for Selection type)*

| # | What should happen? | Which field / product? | Set it to |
|---|---|---|---|
| 1 | Add product / Remove product / Set field value | | |
| 2 | | | |
