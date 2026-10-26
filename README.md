# The Legend Track — APIs, TypeScript, Node.js & React

> Goal, stated honestly: **not** "I can build a CRUD app and a dashboard."
> The goal is: **when anyone in the room has an API, TypeScript, Node, or React question, you are the person they turn to** — and when an interviewer at Google/Amazon/Stripe pushes three levels deeper than the original question, you're still standing.

This sits alongside your [Spring Boot track](https://github.com/Pawar-Pratik/SpringBoot) and follows the same rules.

---

## The four tracks

| Track | Repo | Lessons | What it makes you |
|---|---|---|---|
| **APIs & REST** | [API-mastery](https://github.com/Pawar-Pratik/API-mastery) | 32 | The person who designs the contract everyone else builds against |
| **TypeScript** | [TypeScript-mastery](https://github.com/Pawar-Pratik/TypeScript-mastery) | 22 | The person who makes wrong states *unrepresentable*, not just caught |
| **Node.js** | [Node-mastery](https://github.com/Pawar-Pratik/Node-mastery) | 38 | The person who knows what the event loop was doing when p99 spiked, and why the pod was OOMKilled |
| **React** | [React-mastery](https://github.com/Pawar-Pratik/React-mastery) | 30 | The person who knows *why* the UI re-rendered, and makes it stop |

**Recommended order:** API → TypeScript → Node → React.

Why that order: APIs teach you the boundary of every system, and you're already server-side from Spring so it lands immediately. TypeScript is the language both Node and modern React are written in here — learning either first means learning it *twice*, once wrong. Node comes next because the API track already had you writing Node handlers: now you learn the runtime underneath them, and it's where server-side reasoning from Spring transfers most directly. React last, and it feels like a small library instead of a mountain.

If you're impatient: run **API and TypeScript in parallel** (API in the morning, TS in the evening), then **Node and React in parallel** once TypeScript is done. Never start Node or React before TypeScript Module 4: every Node lesson is TypeScript that Node runs directly.

---

## One product, four tracks

Most curricula give you four disconnected toy apps. This one gives you **one real product**, built across all four tracks.

### Ledger — a payments API (Stripe, but yours)

A merchant payments platform: merchants create **payment intents**, customers pay, money lands in a **wallet ledger**, and merchants receive **webhooks**. Plus refunds, payouts, and an audit trail.

**Why payments and not another to-do app:** a payments API *cannot be built badly and still appear to work*. The domain forces every hard API concept on you for real reasons — not because a tutorial said so:

| The domain forces… | …which teaches you |
|---|---|
| "Never charge twice, even if the client retries" | **Idempotency keys** — the single most common senior API interview topic |
| "Merchants must be notified when a payment succeeds" | **Webhooks**, HMAC signatures, retries with backoff, dead-letter queues |
| "Money must never be a float" | Data representation, minor units, currency, precision bugs |
| "A payment can't go from `refunded` back to `pending`" | **State machines** in API design, 409 Conflict, optimistic concurrency |
| "Merchant A must never see Merchant B's payments" | **Multi-tenancy**, authorization, BOLA — the #1 real-world API vulnerability |
| "A merchant has 4 million payments" | **Cursor pagination**, indexing, and why `OFFSET 4000000` is a bug |
| "We can't break integrations written in 2019" | **Versioning & evolution**, tolerant readers, deprecation policy |
| "One merchant's runaway script can't take down everyone" | **Rate limiting**, quotas, fair queueing, bulkheads |

### Ledger Engine — the Node runtime underneath

The processes that run Ledger in production, plus the two artefacts every payments platform ships to developers: a webhook dispatcher on a Postgres `SKIP LOCKED` queue, a statement exporter that streams 4M rows in constant memory, a payout-file ingester with backpressure end to end, a reconciler on a worker-thread pool, an SSE live feed fed by `LISTEN/NOTIFY`, and a published SDK (`@ledger/node`) and CLI (`ledger listen --forward-to`).

**Why this and not a chat app:** a payments engine makes every runtime concern real — constant memory because the export is 4 million rows, backpressure because the upload is 2 GB, graceful shutdown because a dropped request might be a payment, worker threads because reconciliation is CPU-bound and the API can't stall.

### Ledger Console — the React + TypeScript dashboard

The merchant-facing dashboard for Ledger: a payments table (virtualized, filterable, 4M rows), a live payment feed, a refund flow with optimistic updates, API-key management, a webhook delivery log with retry, and revenue charts.

**Why this and not a to-do list:** it forces every React concept for an actual reason — server-state caching, optimistic updates with rollback, virtualization, Suspense and streaming, forms with real validation, race conditions in search-as-you-type, auth token refresh mid-request, code splitting, and measurable Core Web Vitals.

You end with **one hosted product** you can demo, and every interview answer becomes *"here's how I handled that"* instead of *"I think you'd probably…"*. That difference is the whole game.

> Blueprints: [Ledger API blueprint](https://github.com/Pawar-Pratik/API-mastery/blob/main/07-project/28-ledger-blueprint.md) · [Ledger Engine blueprint](https://github.com/Pawar-Pratik/Node-mastery/blob/main/09-project/34-ledger-engine-blueprint.md) · [Ledger Console blueprint](https://github.com/Pawar-Pratik/React-mastery/blob/main/08-project/26-console-blueprint.md)
> Skim them early. Don't start coding them until the lessons say so.

---

## The rules I follow in every lesson

| Rule | What it means |
|---|---|
| **Mechanism before syntax** | You learn the problem, then the wire format, then the framework's helper. Never the reverse |
| **The protocol is the truth** | Frameworks change; HTTP doesn't. Learn what's on the wire and every framework becomes readable |
| **Production reality** | What breaks at 3am and what it costs, not just what compiles |
| **Interview traps** | Every lesson ends with the questions people confidently get *wrong*, and why |
| **Real systems as evidence** | How Stripe, GitHub, Slack, AWS and Netflix actually did it — including where they disagree with each other |
| **Build it** | Each lesson advances Ledger, the Engine or the Console |
| **No padding** | Depth for what matters; a table for trivia. I won't inflate the lesson count |

### The lesson shape

Every lesson follows the same beats, so you always know where you are:

1. **The idea in one sentence** — what you must never forget
2. **Why it exists** — the problem, before the solution
3. **Mechanism** — how it actually works, down to bytes / event-loop phases / renders / types
4. **Code** — production quality, not tutorial quality
5. **Production rules** — do / never-do, each with a reason
6. **Interview** — questions, strong answers, and the follow-up they'll ask next
7. **Build & break** — do it, then break it on purpose

---

## How to actually study this (read this part — it matters more than any lesson)

Reading is the *least* effective part of learning. The loop, per lesson:

1. **Read once, fast.** No notes. Just build the map.
2. **Hit the interview section and answer *before* reading the answers.** Failing to recall is what builds memory. Being *shown* the answer builds recognition, which feels identical and is nearly useless under pressure.
3. **Type the code. Never copy-paste.** Typing forces you to read every token.
4. **Break it on purpose.** Every lesson has one. Delete the `Cache-Control`. Ignore `write()` returning `false`. Remove the `key`. Widen the type. Watch the exact failure. *"I've seen that happen"* is the most credible sentence in an interview.
5. **Explain it out loud in 60 seconds** to an imaginary colleague. If you stumble, you don't know it yet.

### The rule about magic
Any time you think *"this just works somehow"* — **stop**. That's debt. Write it in `QUESTIONS.md` and hunt it down. Mastery is mostly an unusually low tolerance for unexplained behaviour.

### The three artifacts you maintain

| File | Purpose |
|---|---|
| `QUESTIONS.md` | Every "why does that work?" you couldn't answer. Empty it weekly |
| `PROGRESS.md` | Lesson checkboxes + dates. Pace keeps you honest |
| `RECALL.md` | Questions you got wrong in the interview sections. Re-test yourself on these every Sunday, cold |

### Pacing

| Intensity | All four tracks |
|---|---|
| **Sprint** (4–5 h/day) | ~14 weeks |
| **Serious** (2–3 h/day) | ~26 weeks — the sweet spot |
| **Steady** (1 h/day) | ~13 months (keep the project moving weekly or you'll lose context) |

Go slowest on: **API Modules 1–2** (HTTP + REST design), **TypeScript Module 3** (the type-level engine), **Node Module 2 and Lesson 13** (the event loop + backpressure), **React Modules 1–2** (rendering + effects). Those are where all later depth comes from.

---

## Language choice, and why

**API track:** concepts are protocol-first and language-neutral; code is **TypeScript (Node + Express/Fastify)**, with a `> **Spring equivalent**` callout wherever your Spring knowledge maps across. You get double reinforcement — the same idea in two ecosystems is what makes you framework-independent, which is exactly what senior interviews probe for.

**Node:** TypeScript that **Node 24+ runs directly** — type stripping, so erasable syntax only (no enums, no parameter properties), ESM throughout — with `> **Spring equivalent**` callouts like the API track. It's deliberately the same language as the API track's examples: the handlers you wrote there become the subject here, one layer down.

**TypeScript / React:** TypeScript 5.x strict mode, React 19, Vite. Next.js App Router gets dedicated lessons (RSC, Server Actions, caching) because interviews now assume it.

---

## The 20 things that make you sound senior in these four areas

Bookmark this. If you can speak fluently on all twenty — mechanism, trade-offs, and what you'd do differently — you are, functionally, an expert.

**APIs**
1. **Idempotency** — why every retryable write needs a key, and how you'd store it (API L18)
2. **Safe vs idempotent vs cacheable** — and why `PUT` doesn't mean "update" (API L03)
3. **Unknown fields** — must an API reject an extra parameter? The tolerant-reader answer (API L11)
4. **Cache invalidation over HTTP** — ETag, `stale-while-revalidate`, and why `no-cache` ≠ `no-store` (API L17)
5. **BOLA** — why *authorization*, not authentication, is where APIs actually get breached (API L15)
6. **JWT honestly** — what you give up (revocation) for what you gain (statelessness) (API L13)
7. **Pagination at 4M rows** — why `OFFSET` dies and cursors don't (API L08)
8. **REST vs gRPC vs GraphQL** — chosen by constraint, not by fashion (API L24)

**TypeScript**
9. **Structural typing & variance** — why your callback type is unsafe and TS allowed it (TS L06, L12)
10. **`unknown` at every boundary** — parse, don't assume; types vanish at runtime (TS L15)
11. **Making illegal states unrepresentable** — discriminated unions over optional-field soup (TS L09)

**Node**
12. **The event loop, phase by phase** — and why `setTimeout(0)` vs `setImmediate` depends on where you call it (Node L05)
13. **The thread pool nobody sees** — why slow DNS makes file reads slow (Node L07)
14. **Backpressure** — what happens when you ignore `write()` returning `false`, and why `pipeline` beats `pipe` (Node L13)
15. **Graceful shutdown** — readiness first, drain, and the PID 1 signal trap (Node L24)
16. **Heap vs RSS** — why the pod was OOMKilled while the heap looked fine (Node L27)

**React**
17. **Render vs commit, and reconciliation** — why your list lost its input state (React L03, L04)
18. **Effects are synchronization, not lifecycle** — and the four cases where you shouldn't use one (React L08)
19. **Server state ≠ client state** — why `useEffect` + `fetch` is the wrong default (React L17)
20. **Why it re-rendered** — measured with the Profiler, then fixed at the right layer (React L20)

---

## Progress tracker

Copy into `PROGRESS.md`:

```
API
[ ] M1 Foundations      01 [ ] 02 [ ] 03 [ ] 04 [ ] 05 [ ]
[ ] M2 REST design      06 [ ] 07 [ ] 08 [ ] 09 [ ] 10 [ ] 11 [ ]
[ ] M3 Security         12 [ ] 13 [ ] 14 [ ] 15 [ ] 16 [ ]
[ ] M4 Production       17 [ ] 18 [ ] 19 [ ] 20 [ ]
[ ] M5 Beyond REST      21 [ ] 22 [ ] 23 [ ] 24 [ ]
[ ] M6 Craft            25 [ ] 26 [ ] 27 [ ]
[ ] M7 Project          28 [ ] 29 [ ]
[ ] M8 Interview        30 [ ] 31 [ ] 32 [ ]

TYPESCRIPT
[ ] M1 Foundations      01 [ ] 02 [ ] 03 [ ] 04 [ ]
[ ] M2 Type system      05 [ ] 06 [ ] 07 [ ] 08 [ ]
[ ] M3 Type-level       09 [ ] 10 [ ] 11 [ ] 12 [ ]
[ ] M4 Real code        13 [ ] 14 [ ] 15 [ ] 16 [ ]
[ ] M5 Ecosystem        17 [ ] 18 [ ] 19 [ ]
[ ] M6 Interview        20 [ ] 21 [ ] 22 [ ]

NODE
[ ] M1 Foundations      01 [ ] 02 [ ] 03 [ ] 04 [ ]
[ ] M2 Event loop       05 [ ] 06 [ ] 07 [ ] 08 [ ] 09 [ ] 10 [ ]
[ ] M3 Data & I/O       11 [ ] 12 [ ] 13 [ ] 14 [ ] 15 [ ]
[ ] M4 Networking       16 [ ] 17 [ ] 18 [ ] 19 [ ] 20 [ ]
[ ] M5 Concurrency      21 [ ] 22 [ ] 23 [ ] 24 [ ]
[ ] M6 Data & jobs      25 [ ] 26 [ ]
[ ] M7 Internals        27 [ ] 28 [ ] 29 [ ]
[ ] M8 Shipping         30 [ ] 31 [ ] 32 [ ] 33 [ ]
[ ] M9 Project          34 [ ] 35 [ ]
[ ] M10 Interview       36 [ ] 37 [ ] 38 [ ]

REACT
[ ] M1 Mental model     01 [ ] 02 [ ] 03 [ ] 04 [ ]
[ ] M2 State & effects  05 [ ] 06 [ ] 07 [ ] 08 [ ]
[ ] M3 Hooks in full    09 [ ] 10 [ ] 11 [ ] 12 [ ]
[ ] M4 Composition      13 [ ] 14 [ ] 15 [ ] 16 [ ]
[ ] M5 Data             17 [ ] 18 [ ] 19 [ ]
[ ] M6 Performance      20 [ ] 21 [ ] 22 [ ]
[ ] M7 Server React     23 [ ] 24 [ ] 25 [ ]
[ ] M8 Project & scale  26 [ ] 27 [ ] 28 [ ]
[ ] M9 Interview        29 [ ] 30 [ ]
```

---

## One promise and one warning

**The promise:** do the loop — read, self-test, type, break, explain out loud — and you won't be "someone who knows REST, Node and React." You'll be someone who can explain *why they are the way they are*, which is rarer and worth far more.

**The warning:** you cannot read your way to this. The people who fail aren't the ones lacking intelligence; they're the ones who mistake *understanding a lesson while reading it* for *producing it under pressure*. Close the file and explain it out loud. That discomfort is the learning.

Start here → **[API: Lesson 01 — What an API really is](https://github.com/Pawar-Pratik/API-mastery/blob/main/01-foundations/01-what-an-api-really-is.md)**
