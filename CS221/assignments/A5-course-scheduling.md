# CS221 Assignment 5: Course Scheduling

## Problem Statement

This assignment turns Stanford course planning into a weighted constraint satisfaction problem. Instead of hand-picking a schedule, you encode quarter availability, prereqs, unit limits, repeating-course restrictions, and personal preferences, then let a backtracking solver search for the best valid plan.

The assignment starts small and then builds up. First you rehearse CSP basics and heuristics on toy graphs, then you implement course scheduling itself. The final task is the real target: convert a profile plus bulletin data into a CSP that can produce an optimal schedule, not just any schedule.

## Key Techniques

- **CSP modeling.** The main skill is translating scheduling rules into variables and factors.
- **Weighted preferences.** Requests are soft constraints with weights, so the solver can prefer one valid schedule over another.
- **Backtracking with heuristics.** The assignment leans on MCV and AC-3 to keep search from exploding.
- **Auxiliary sum variables.** Unit constraints are handled by building a sum variable with `get_sum_variable()`.
- **Unary and binary factor reduction.** The assignment includes a helper exercise for turning higher-arity structure into factors the solver can handle.
- **Domain pruning by construction.** A lot of speed comes from making illegal assignments impossible instead of merely unlikely.

## Implementation Walkthrough

1. **Use the warm-up problems to calibrate the solver.** The early exercises cover CSP basics, chain CSPs with XOR factors, n-queens, MCV, and AC-3. They are not decorative. They train the solver behavior the scheduler depends on.
2. **Understand the core variable shape.** In `SchedulingCSPConstructor`, the main variables are `(request, quarter)` pairs. The value is either a course ID from that request or `None` if nothing is scheduled there.
3. **Add hard constraints first.** `add_bulletin_constraints()` keeps courses inside their offered quarters, and `add_norepeating_constraints()` prevents the same course from appearing more than once.
4. **Encode profile rules as extra factors.** `add_quarter_constraints()` handles allowed-quarter filters. Request modifiers like `or`, `in`, `after`, and `weight` turn user intent into either exclusions or weighted preferences.
5. **Handle units explicitly.** `add_unit_constraints()` introduces the per-quarter unit totals and the `(courseId, quarter)` auxiliary variables that let the grader recover how many units each course carries in the final plan.
6. **Let the heuristics do the heavy lifting.** Once the CSP is assembled, MCV and AC-3 are what make the search usable. Without them, the schedule space is too large to explore blindly.

## What You Learn

- Scheduling is not a special problem. It is a CSP with a lot of domain knowledge encoded correctly.
- The hard part is not search, it is representation.
- Soft preferences can coexist with hard constraints if you separate weights from feasibility.
- Good heuristics are not optional once the variable count grows.
- Real planning problems are usually optimization problems wearing a feasibility mask.

## Sources

- Assignment page: `https://raw.githubusercontent.com/stanford-cs221/autumn2019/gh-pages/assignments/scheduling/index.html`
- Course home page: `https://raw.githubusercontent.com/stanford-cs221/autumn2019/gh-pages/index.html`
- Public repo: `https://github.com/stanford-cs221/autumn2019`
