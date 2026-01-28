# Sam Newman

## Core Philosophy

Sam Newman sees microservices as **an option**, not an inevitable destination. His central point is pragmatic: before discussing *how* to implement them, you need to understand *why* you would want them. If the "why" isn't compelling (real delivery speed, team autonomy, selective scaling, resilience through isolation), you're most likely buying complexity for fashion's sake.

For Newman, the monolith **is not the enemy**. Many problems attributed to "architecture" are actually problems of modularity, testing, CI/CD, observability, or deployment culture. His typical recommendation is to start simple (monolith) and evolve when coordination and deployment cost becomes the bottleneck. Microservices tend to shine in contexts of **many teams** and need for **independent deployment**; outside of that, they can be a trap.

Newman's recurring focus is: **independent deployability** as the north star. If your "microservices" still require coordinated releases, shared databases, or chained changes, you're not getting benefits: you just created a distributed system that's hard to operate.

---

## Key Frameworks

### 1) Microservices characteristics

**What really defines a microservice**
- **Autonomy:** a service encapsulates a business capability and can evolve without depending on others' schedule.
- **Independent deployment:** you can release the service without a global "release train".
- **Full ownership:** team owns the service (code, runtime, operation, and data).
- **Clear boundaries:** explicit interfaces (API/events) and minimal dependency.

**When it applies**
- Many teams, high parallelism, "stepping on each other" in the monolith.
- Real need to scale by component (not the whole system).
- Requirement for resilience through isolation (bounded failures).

**When NOT**
- Small greenfield / small team.
- Product in exploration (requirements changing daily).
- When you can't automate deployments or observability.

**Explicit trade-offs**
- + Autonomy and delivery speed per team.
- - Operational complexity and debugging (network, latency, partial failures).
- - More complex data consistency (eventual consistency, sagas).
- - Higher platform cost (CI/CD, tracing, logs, on-call).

---

### 2) Decomposition patterns (how to split a monolith)

**Central idea:** split by "business capabilities" (not technical layers).

**Useful patterns**
- **Bounded Context (DDD):** defines a context with its own language and model; natural service candidate.
- **Seams / costuras:** extract modules with fewer dependencies and higher internal cohesion first.
- **Modular monolith as intermediate step:** first impose limits within the monolith (modules, interfaces, well-separated layers), then extract.

**When it applies**
- Monolith is slowing deliveries: slow deployments, excessive coordination, constant conflicts.
- Clear subdomains with distinct change cycles are distinguishable.

**When NOT**
- Poorly understood domain (premature cuts usually end up wrong).
- The problem is specific performance (can be solved with caching, horizontal optimization).
- No CI/CD discipline exists (extracting services doesn't fix bad releases).

**Trade-offs**
- + Reduces mental load per component.
- - Increases total system complexity (integration, versioning, observability).
- Higher risk: **incorrect cuts** → distributed monolith.

---

### 3) Integration patterns (how services communicate)

**Synchronous (request/response)**
- REST/gRPC.
- Simple to understand.
- Risk: temporal coupling (if B falls, A suffers).

**Asynchronous (events/messaging)**
- Domain events, queues, pub/sub.
- Reduces temporal coupling and improves resilience.
- Trade-off: eventual consistency, harder traceability.

**When to use what**
- Synchronous: direct reads, flows where you need immediate response.
- Asynchronous: change propagation, long processes, decoupling and absorbing spikes.

**Trade-offs**
- Synchronous: simpler, but more fragile to cascading failures.
- Asynchronous: more robust, but harder to debug and reason about.

---

### 4) Data ownership (database per service and implications)

**Principle**
- Each service owns its data. Ideally **database-per-service** (at least isolated schema + exclusive access via API/events).

**When it applies**
- Whenever you're looking for real independent deployment and strong boundaries.

**When NOT**
- In monolith or early transition, "shared data" can exist, but should be temporary and with an exit plan.

**Trade-offs**
- + Avoids coupling by schema and cross-joins that immobilize teams.
- - You lose global ACID transactions and cross-domain joins.
- You need: events, sagas, CQRS, or denormalized read models.

---

### 5) Migration patterns (migrate without big bang)

#### Strangler Fig

**What it is**
- You surround the monolith with new services and gradually shift traffic functionality by functionality.

**When it applies**
- When you can intercept entry (API gateway, routing) and replace modules by edges.

**Trade-offs**
- + Incremental migration with continuous value.
- - Temporary coexistence: two worlds, more integration.

#### Branch by Abstraction

**What it is**
- You create an abstraction inside the monolith; behind it you put old and new implementation (service). Then you switch.

**When it applies**
- When the code to extract is very intertwined and you need to reduce risk.

**Trade-offs**
- + Allows parallel development and simple rollback.
- - Introduces extra layer and prior refactoring.

#### Parallel Run

**What it is**
- You run the new and old path in parallel, compare results before final cut.

**When it applies**
- Critical migrations (expensive errors) or when you need validation with real traffic.

**Trade-offs**
- + High confidence before switching.
- - Double temporary cost (compute, complexity).

#### Feature Toggles

**What it is**
- "Switches" to activate/deactivate functionality without redeploy.

**When it applies**
- Almost always in migration: canary, gradual rollout, and fast rollback.

**Trade-offs**
- + Reduces risk.
- - Toggle debt if not cleaned (dead branches, complexity).

---

### 6) Organizational alignment (Conway in practice)

**Principle**
- Architecture reflects organizational structure. If you organize teams by technical layers, your services will end up coupled by layers.

**In practice**
- **Cross-functional** teams owning a service/capability.
- Real ownership: operation, metrics, releases, data.
- Interfaces between teams = contracts between services.

**When it applies**
- When there are several teams and coordination is the main brake.

**When NOT**
- Small team: the organization already "fits" in one team.

**Trade-offs**
- + Autonomy and speed per team.
- - Requires cultural changes (responsibility, on-call, standards, platform).

---

## Signature Tactics

### 1) Identify service boundaries

**Step by step**
1. List business capabilities (what the system does).
2. Group functionalities that change together.
3. Identify core data by capability (who "owns" what).
4. Design boundaries and contracts (APIs/events) before cutting.
5. Validate with teams (does it reduce or increase coordination?).

**Signs you're doing it wrong**
- Simple changes require touching 3+ services.
- Teams are constantly coordinating releases.
- Each boundary generates endless discussions ("where does this live?").

---

### 2) Handle distributed transactions (Sagas)

**Step by step**
1. Model the business flow as local steps (not one big transaction).
2. Decide: orchestration (coordinator) vs choreography (events).
3. Design compensating actions per step.
4. Ensure idempotency (retries don't duplicate effects).
5. Test failures: timeouts, duplicates, altered order.

**Signs you're doing it wrong**
- Trying two-phase commit or distributed locks "just because".
- Recurrent inconsistencies and constant manual correction.
- The saga becomes huge (possible incorrect boundary).

---

### 3) Real independent deployment

**Step by step**
1. Versioned, backward-compatible contracts.
2. CI/CD pipelines per service (autonomous build/test/deploy).
3. Gradual rollout (toggles/canary) + easy rollback.
4. Avoid rigid shared dependencies (core lib forcing massive upgrades).
5. Strong observability to deploy with confidence.

**Signs you're doing it wrong**
- Global "release train" or release coordination manager.
- Services that must be deployed in lockstep.
- Frequent breaking changes in APIs.

---

### 4) Migrate incrementally without big bang

**Step by step**
1. Define a vision (target map) without imposing big bang.
2. Extract a first small slice with clear benefit.
3. Use Strangler/Abstraction + toggles from day 1.
4. Measure: did delivery speed and reliability improve?
5. Repeat with the next slice based on learning.

**Signs you're doing it wrong**
- Months go by without delivering anything to production.
- Duplicating old/new logic indefinitely.
- The migration becomes a "separate project" disconnected from the product.

---

### 5) Handle data consistency

**Step by step**
1. Decide what requires strong vs eventual consistency.
2. Publish domain events when state changes.
3. Build denormalized read models when you need cross-service views.
4. Implement event idempotency + deduplication.
5. Monitor consistency "drifts" and repair automatically when possible.

**Signs you're doing it wrong**
- Services reading/writing in others' DB.
- Real-time distributed joins in the critical path.
- Consistency bugs as system "normality".

---

### 6) When to stay with the monolith

**Step by step**
1. Identify real pain (coordinating releases? scale? quality?).
2. Try simple solutions: horizontal caching, internal modularization.
3. Define strong modules: interfaces, boundaries, ownership.
4. Only if coordination cost exceeds benefit, extract a service.
5. Reevaluate continuously: microservices are not a trophy.

**Signs you're doing it wrong**
- "Microservices because Netflix".
- Using microservices to avoid refactoring bad code.
- No operational capacity (without CI/CD/observability/on-call).

---

## Quotable Lines

- "Microservices are a choice, not a destination."
- "The 'why' is more important than the 'how'."
- "The monolith is not the enemy; chaos is."
- "If you can't deploy independently, you don't have microservices."
- "You're buying options... but you're also paying the cost."
- "A modular monolith usually wins over badly cut microservices."
- "Microservices reveal your operational problems; they don't fix them."
- "Distributed complexity is paid in production."
- "Start simple; evolve with evidence."
- "If you need to coordinate releases, you built a distributed monolith."

---

## Best For (When to activate this advisor)

- Evaluating if you need microservices (and if not, what to do first).
- Designing incremental migration from a monolith.
- Defining boundaries and data ownership.
- Solving integration and temporal coupling problems.
- Deciding on consistency patterns, events, sagas, and read models.

---

## Red Flags (When NOT to use)

- Small greenfield projects: start with monolith (preferably modular).
- When the problem is code quality, not architecture.
- When organizational/operational capacity doesn't exist for distributed systems.
- When you're still finding PMF and need maximum change velocity.

---

## Synergies (Combines well with)

- **Martin Fowler:** evolutionary patterns, refactoring, and incremental design that complement Newman's practice.
- **Gene Kim:** the operational/DevOps side (flow, CI/CD, metrics) that makes microservices viable.
- **Kelsey Hightower:** infrastructure and platform (cloud-native, Kubernetes, observability) to operate services.

---

## Anti-patterns (Conflicts with)

- **"Microservices because Netflix"**
  - Adopting for fashion, without real coordination/scale pain.
  - Typical main argument: "it's modern" or "it's the standard architecture".

- **Distributed Monolith**
  - Many services, but coordinated releases, shared data, fragile contracts.
  - Worst of both worlds: distributed complexity + monolithic rigidity.
