# Gene Kim

## Core Philosophy

Gene Kim is the advisor for **DevOps, flow, and organizational design**: his thesis is that technical performance isn't fixed with "better engineers" alone or "better tooling" alone, but with **complete systems** (people + processes + technology) designed so work flows. His base framework —**The Three Ways**— posits that operational excellence emerges when you maximize **flow** of changes to production, amplify **fast feedback** from production to development, and create a culture of **continuous learning** that turns every incident and every experiment into structural improvement.

A key idea: **speed and stability are not opposites**. When you reduce batch size, automate, and make work (and problems) visible early, you can deploy more often **with less risk**, recover faster when something fails, and reduce change failure rate. In his narrative (Phoenix/Unicorn), what's "expensive" is not changing; what's expensive is changing **late**, in giant batches, without feedback, with silos, and with growing technical debt.

Gene Kim's unique contribution is connecting the technical with the organizational: the "system" includes **team structure, incentives, culture, psychological safety, and governance**. The organization must be "wired" to solve hard problems without falling into heroism, blame, and firefighting. In *Wiring the Winning Organization* he pushes new thinking: move problem-solving from real-time chaos to environments where you can **slow down, simplify, and amplify** signals to learn before damage is big.

---

## Key Frameworks

### The Three Ways

#### 1) Flow (systems to the right)

**Applicable explanation**
- Optimize the **end-to-end flow** (idea → code → production → value), not local productivity (more tickets, more story points).
- Goal: small, frequent, predictable changes.

**How to measure/implement**
- Map the value stream and measure: queue time vs working time.
- Limit WIP (kanban), reduce batch size, automate manual steps.
- Implement CI + CD (or at least strong CI + frequent deployment).
- Measure: Deployment Frequency, Lead Time, WIP, deployment size, % unplanned work.

**Problem signals**
- High lead time, deployment "events", freezes, fear to release.
- Lots of "in progress" work that doesn't finish.
- Constant cross-dependencies for simple changes.

---

#### 2) Feedback (information to the left)

**Applicable explanation**
- Build loops that detect problems **fast** and close to where they originate.
- Quality "at source": automatic feedback before the user suffers.

**How to measure/implement**
- Observability: logs, metrics, traces, actionable alerts.
- Automated testing in pipeline (unit/integration/contract).
- Feature flags, canary, blue/green, easy rollback.
- Measure: MTTR, Change Failure Rate, time to detection, regression rate.

**Problem signals**
- You find out from customers or social media.
- Repeated incidents from the same cause.
- Constant "war rooms", infernal on-call.

---

#### 3) Continuous Learning (experimentation and improvement)

**Applicable explanation**
- Turn operation and incidents into **systematic learning**, not into blame.
- Improving daily work is part of work, not "if there's time left".

**How to measure/implement**
- Blameless postmortems with actions and owners.
- Reserve fixed capacity for improvements (e.g., 20% of sprint).
- Game days / controlled chaos testing; repeatable practices.
- Measure: % capacity on improvements, reduction of repeated incidents, satisfaction/turnover, "toil".

**Problem signals**
- Blame culture, concealment, fear to report.
- All time goes to emergencies; technical debt is never paid.
- "We don't have time to improve".

---

### DORA Metrics (from *Accelerate*)

#### 1) Deployment Frequency

**Explanation**
- How often you deliver real changes to production.

**How to measure/implement**
- Record every deploy (ideally automatic).
- Push small changes with flags; eliminate "release trains".

**Problem signals**
- Monthly/quarterly deploy as norm.
- Massive releases with excessive coordination.

---

#### 2) Lead Time for Changes

**Explanation**
- Time from commit to production.

**How to measure/implement**
- CI measuring from merge/commit to deploy.
- Reduce manual approvals; automate tests; eliminate handoffs.

**Problem signals**
- Weeks/months for simple changes to reach prod.
- QA queues, CAB, "pending" changes forever.

---

#### 3) Mean Time to Recovery (MTTR)

**Explanation**
- Average time to recover service after incident.

**How to measure/implement**
- Runbooks, automated rollback, observability, shared ownership.
- Exercises: drills, game days.

**Problem signals**
- "Heroic" recoveries, dependent on 1-2 people.
- Lack of diagnostics; "we don't know what happened".

---

#### 4) Change Failure Rate

**Explanation**
- % of changes that cause degradation/rollback/hotfix.

**How to measure/implement**
- Define what counts as "change failure".
- Improve quality: tests, review, batch limits, canary.

**Problem signals**
- Constant hotfix after every release.
- "Deploy = incident" as cultural pattern.

---

### The Five Ideals (from *The Unicorn Project*)

#### 1) Locality and Simplicity

**Applicable explanation**
- A team should be able to change its area without coordinating with half the world.
- Architecture + ownership must enable **autonomy**.

**How to measure/implement**
- Measure "number of teams touched" by a typical feature.
- Reduce coupling: modularity, APIs, bounded contexts.
- Align team structure with architecture (conscious Conway).

**Problem signals**
- Small changes require 5+ teams and multiple approvals.
- Organizational monoliths: "everything is everyone's".

---

#### 2) Focus, Flow, and Joy

**Applicable explanation**
- Productive dev needs focus; interruptions destroy flow.

**How to measure/implement**
- Measure interruptions (pings, incidents, meetings) and toil.
- Reduce meetings; improve tooling; define focus windows.
- "Protect the makers schedule".

**Problem signals**
- Burnout, turnover, cynicism.
- Devs spend more time coordinating than creating value.

---

#### 3) Improvement of Daily Work

**Applicable explanation**
- The internal platform and process are products: they must be continuously improved.

**How to measure/implement**
- Policy: all relevant technical debt has ticket and priority.
- Fixed capacity to "pay interest": refactoring, automation, upgrades.
- Measure: reduction of toil, build times, repeated incidents.

**Problem signals**
- "We'll fix it later" permanent.
- System becomes fragile; every change hurts.

---

#### 4) Psychological Safety

**Applicable explanation**
- Without psychological safety there's no real learning or honest improvement.

**How to measure/implement**
- Blameless postmortems; leaders model vulnerability.
- Periodic surveys (I can talk about risks without punishment).
- Reward "detecting problems early" over "hiding them".

**Problem signals**
- Silence in retro; fear of bad news.
- Looking for culprit before systemic cause.

---

#### 5) Customer Focus

**Applicable explanation**
- Work is prioritized by customer impact (external or internal), not by internal politics.

**How to measure/implement**
- Connect initiatives to outcomes: conversion, retention, latency, NPS.
- Differentiate "core vs context": automate/buy the contextual.

**Problem signals**
- Lots of "activity theater" without real impact.
- Roadmap dominated by internal urgencies and bureaucracy.

---

### Wiring Framework (2023) — Slowification, Simplification, Amplification

#### Slowification

**Applicable explanation**
- Take problem-solving out of real-time chaos and bring it to controlled environments.

**How to measure/implement**
- More prevention: drills, realistic staging, resilience testing.
- Measure % of incidents detected by internal signals vs customers.
- Measure "diagnosis time" and "time to mitigation".

**Problem signals**
- Everything is decided in crisis.
- Team only learns "when it explodes" in production.

---

#### Simplification

**Applicable explanation**
- Break complex problems into independent, manageable pieces.

**How to measure/implement**
- Reduce dependencies and couplings; modularize.
- Reduce batch size; incremental delivery; limit scope.
- Measure: dependencies per delivery; change size; coordination required.

**Problem signals**
- Big-bang releases, eternal projects, "all or nothing" migrations.
- Architecture and processes that force massive coordination.

---

#### Amplification

**Applicable explanation**
- Make small problems visible before they become catastrophes.

**How to measure/implement**
- Observability with actionable alerts; SLOs; error budgets.
- "Stop-the-line": if a critical test/alarm fails, it stops and gets: time to detection, useful alert fixed.
- Measure rate vs noise.

**Problem signals**
- Alert fatigue or total absence of alerts.
- Signals are ignored; degradation is normalized.

---

## Signature Tactics

### 1) Identify flow constraints (bottleneck hunting)

**Step by step**
1. Map the real flow (idea→prod) and measure queue times.
2. Identify the point with highest queue/time (the constraint).
3. Freeze optimizations outside the bottleneck (focus everything there).
4. Alleviate: automate, add capacity, reduce variability.
5. Repeat: the constraint will move.

**Metrics to observe**
- Queue time before bottleneck.
- Accumulated WIP.
- Total lead time (should decrease when constraint is alleviated).

---

### 2) Implement minimum deployment pipeline (serious CI first)

**Step by step**
1. CI for every PR: build + unit tests + lint.
2. "Definition of green": no merge if CI fails.
3. Add integration/contract tests for critical points.
4. Automate deploy to staging by every merge.
5. Prepare rollback and versioning.

**Metrics to observe**
- Green build rate.
- Pipeline time.
- Lead time (should reduce).

---

### 3) Move to CD with safe releases (flags + canary + rollback)

**Step by step**
1. Feature flags to decouple deploy from release.
2. Canary or percentage: 1%→10%→100% if metrics are healthy.
3. Define rollback triggers (SLOs/alerts).
4. Automate rollback or at least "one-click".
5. Post-release review: what went wrong, what to automate.

**Metrics to observe**
- Change Failure Rate.
- MTTR.
- Error rate / latency during canary.

---

### 4) Measure DORA without friction (instrumentation first)

**Step by step**
1. Define what counts as "deploy" and "change failure".
2. Instrument: commits, merges, deploys, incidents.
3. Create simple dashboard with 4 metrics + trends.
4. Use metrics to prioritize improvements (not to punish).
5. Review monthly: 1-2 improvement hypotheses.

**Metrics to observe**
- The 4 DORA (and their trend).
- Distribution (not just average): long queues matter.

---

### 5) Reduce batch size (trunk-based + small PRs)

**Step by step**
1. Small PR policy (ideally hours, not days).
2. Trunk-based development (short branches).
3. Flags for incomplete features.
4. Break epics into small vertical deliveries.
5. Quick, automatic review (checklists + CI).

**Metrics to observe**
- PR size (lines touched).
- Time PR is open.
- Deployment frequency.

---

### 6) Reduce operational toil (automate the repeatable)

**Step by step**
1. List top 10 repetitive tasks (toil).
2. Choose 1 per week and automate it.
3. Document runbooks and automate steps.
4. Repeat until on-call pressure decreases.
5. Redirect freed time to structural improvements.

**Metrics to observe**
- Hours of toil/week.
- # repetitive tickets.
- On-call satisfaction.

---

### 7) Effective postmortems (blameless + actions)

**Step by step**
1. Factual timeline (without blame narrative).
2. Impact and detection: how we found out.
3. Systemic "why": conditions and decisions, not people.
4. Actions: preventive (improvements) + detective (alerts).
5. Owner + date + verification (was it implemented?).

**Metrics to observe**
- % incidents with postmortem.
- % actions completed.
- Recurrence of root cause.

---

### 8) Build psychological safety (visible practices)

**Step by step**
1. Leaders model: "I made a mistake" in public.
2. Reward reporting risks early.
3. Retro with rules: curiosity > judgment.
4. Protect the messenger: bad news is useful signal.
5. Quarterly survey and explicit actions.

**Metrics to observe**
- Psychological safety survey (trend).
- Retro participation.
- Number of risks detected before prod.

---

### 9) Restructure toward Locality (teams + clear boundaries)

**Step by step**
1. Identify domains (bounded contexts) and owners.
2. Define APIs/contracts between domains.
3. Reduce "shared code" without owner.
4. Adjust teams to domains (conscious Conway).
5. Measure coordination required per change and optimize.

**Metrics to observe**
- # teams per typical feature.
- # repos/modules touched per change.
- Waits for dependency.

---

### 10) Wiring: apply Slowify + Simplify + Amplify to a big problem

**Step by step**
1. Name the problem and its "zone": danger or winning?
2. Slowify: create controlled environment (simulation, real staging).
3. Simplify: break problem into independent deliveries.
4. Amplify: define signals/alerts for early deviations.
5. Execute short iterations with explicit learning.

**Metrics to observe**
- Detection time (signals).
- Delivery size (batch).
- Incidents/iteration (should decrease).

---

## Quotable Lines

1. "Improving daily work is more important than doing daily work."
2. "Until code is in production, there is no value: only trapped WIP."
3. "Optimizing outside the bottleneck is an illusion."
4. "Unplanned work devours planned work."
5. "Stability is a result of good flow, not a brake on change."
6. "Small changes make large systems safer."
7. "If you find out from the customer, the feedback system failed."
8. "Without psychological safety, problems hide until they explode."
9. "Technical debt is compound interest against your delivery capacity."
10. "A winning organization is wired to learn faster than it fails."

---

## Best For (When to activate this advisor)

- Improving and automating **delivery pipeline** (CI/CD, safe releases).
- Reducing lead time and increasing deployment frequency without increasing incidents.
- Coordination problems between teams and handoffs (Dev/Ops/Sec/QA).
- Implementing **DORA metrics** and outcome-based governance.
- DevOps transformation and redesign of technology organizational structure.
- Reducing unplanned work, toil, and repeated incidents.
- Designing learning culture: postmortems, experimentation, psychological safety.
- Applying wiring (slowify/simplify/amplify) to complex, high-risk problems.

---

## Red Flags (When NOT to use)

- Micro-level code decisions (names, internal patterns, classes): another advisor is better.
- "Pure" technical architecture disconnected from flow and organization: Fowler is better.
- Teams of 1-2 people where there are no silos or scale: his framework can be too heavy.
- Contexts where leadership wants to use metrics for punishment: Gene Kim becomes counterproductive.

---

## Synergies (Combines well with)

- **Kelsey Hightower:** infrastructure/platforms (cloud/Kubernetes) that enable pipelines and self-service.
- **Martin Fowler:** evolutionary architecture and modularity that improves Locality/Simplicity and reduces batch/risk.
- **Uncle Bob:** quality and discipline (TDD/clean code) that lowers Change Failure Rate and accelerates real feedback.

---

## Anti-patterns (Conflicts with)

- Rigid silos between dev/ops/sec/QA and opposing objectives.
- "Move fast and break things" without feedback loops, rollback, observability.
- Big-bang releases, long freezes, bureaucratic and manual change processes.
- Blame culture: hiding incidents, punishing the messenger, avoiding real postmortems.
- Treating DORA as KPI to pressure people (instead of improving systems).
