# The Legend Track — APIs, TypeScript & React

> Goal, stated honestly: **not** "I can build a CRUD app and a dashboard."
> The goal is: **when anyone in the room has an API, TypeScript, or React question, you are the person they turn to** — and when an interviewer at Google/Amazon/Stripe pushes three levels deeper than the original question, you're still standing.

This sits alongside your [Spring Boot track](spring%20boot/README.md) and follows the same rules.

---

## The three tracks

| Track | Folder | Lessons | What it makes you |
|---|---|---|---|
| **APIs & REST** | [API/](https://github.com/Pawar-Pratik/TypeScript-mastery/blob/14ce11db44d23479228747dbd659ae255e95ba4d/README.md) | 32 | The person who designs the contract everyone else builds against |
| **TypeScript** | [TypeScript/](TypeScript/README.md) | 22 | The person who makes wrong states *unrepresentable*, not just caught |
| **React** | [React/](React/README.md) | 30 | The person who knows *why* the UI re-rendered, and makes it stop |

**Recommended order:** API → TypeScript → React.

Why that order: APIs teach you the boundary of every system, and you're already server-side from Spring so it lands immediately. TypeScript is the language modern React is written in — learning React first means learning React *twice*, once wrong. React last, and it feels like a small library instead of a mountain.

If you're impatient: run **API and TypeScript in parallel** (API in the morning, TS in the evening). Never start React before TypeScript Module 4.

---

## One product, three tracks

Most curricula give you three disconnected toy apps. This one gives you **one real product**, built across all three tracks.

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

### Ledger Console — the React + TypeScript dashboard

The merchant-facing dashboard for Ledger: a payments table (virtualized, filterable, 4M rows), a live payment feed, a refund flow with optimistic updates, API-key management, a webhook delivery log with retry, and revenue charts.

**Why this and not a to-do list:** it forces every React concept for an actual reason — server-state caching, optimistic updates with rollback, virtualization, Suspense and streaming, forms with real validation, race conditions in search-as-you-type, auth token refresh mid-request, code splitting, and measurable Core Web Vitals.

You end with **one hosted product** you can demo, and every interview answer becomes *"here's how I handled that"* instead of *"I think you'd probably…"*. That difference is the whole game.

> Blueprints: [Ledger API blueprint](API/07-project/28-ledger-blueprint.md) · [Ledger Console blueprint](React/08-project/26-console-blueprint.md)
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
| **Build it** | Each lesson advances Ledger or the Console |
| **No padding** | Depth for what matters; a table for trivia. I won't inflate the lesson count |

### The lesson shape

Every lesson follows the same beats, so you always know where you are:

1. **The idea in one sentence** — what you must never forget
2. **Why it exists** — the problem, before the solution
3. **Mechanism** — how it actually works, down to bytes / renders / types
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
4. **Break it on purpose.** Every lesson has one. Delete the `Cache-Control`. Remove the `key`. Widen the type. Watch the exact failure. *"I've seen that happen"* is the most credible sentence in an interview.
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

| Intensity | All three tracks |
|---|---|
| **Sprint** (4–5 h/day) | ~10 weeks |
| **Serious** (2–3 h/day) | ~18 weeks — the sweet spot |
| **Steady** (1 h/day) | ~9 months (keep the project moving weekly or you'll lose context) |

Go slowest on: **API Modules 1–2** (HTTP + REST design), **TypeScript Module 3** (the type-level engine), **React Modules 1–2** (rendering + effects). Those are where all later depth comes from.

---

## Language choice, and why

**API track:** concepts are protocol-first and language-neutral; code is **TypeScript (Node + Express/Fastify)**, with a `> **Spring equivalent**` callout wherever your Spring knowledge maps across. You get double reinforcement — the same idea in two ecosystems is what makes you framework-independent, which is exactly what senior interviews probe for.

**TypeScript / React:** TypeScript 5.x strict mode, React 19, Vite. Next.js App Router gets dedicated lessons (RSC, Server Actions, caching) because interviews now assume it.

---

## The 15 things that make you sound senior in these three areas

Bookmark this. If you can speak fluently on all fifteen — mechanism, trade-offs, and what you'd do differently — you are, functionally, an expert.

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

**React**
12. **Render vs commit, and reconciliation** — why your list lost its input state (React L03, L04)
13. **Effects are synchronization, not lifecycle** — and the four cases where you shouldn't use one (React L08)
14. **Server state ≠ client state** — why `useEffect` + `fetch` is the wrong default (React L17)
15. **Why it re-rendered** — measured with the Profiler, then fixed at the right layer (React L20)

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

**The promise:** do the loop — read, self-test, type, break, explain out loud — and you won't be "someone who knows REST and React." You'll be someone who can explain *why they are the way they are*, which is rarer and worth far more.

**The warning:** you cannot read your way to this. The people who fail aren't the ones lacking intelligence; they're the ones who mistake *understanding a lesson while reading it* for *producing it under pressure*. Close the file and explain it out loud. That discomfort is the learning.

Start here → **[API: Lesson 01 — What an API really is](API/01-foundations/01-what-an-api-really-is.md)**
