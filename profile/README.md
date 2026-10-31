# Exchange8 - Securities Exchange Platform

**Secure Post-Quantum-Safe Securities Exchange Platform That Heals and Scales Itself.**

Exchange8 is the securities exchange vertical of the Sentin8 platform - a complete trading venue: from market data ingestion and order matching through clearing, margining, settlement, and regulatory surveillance. Built as independent Rust services composed on top of the Sentin8 platform for identity, events, workflows, scheduling, and storage.

> **A note on our history.** If you found your way here looking for
> **PRIME** - `authPrime`, `exchangePRIME`, and family - you're in the
> right place. Those projects are this one: our open-source
> work is now rebranded to **Sentin8** (this vertical is **Exchange8**) to
> avoid confusion with other brand names and products. At the same
> time we left our self-hosted **Apache SVN** behind and moved
> everything to GitHub. The lineage goes back further than you might
> expect - here's a fun LinkedIn read from 2017, when the platform was
> still called PRIME and still lived on SVN:
> [Advanced Hybrid Securities Exchange](https://www.linkedin.com/pulse/advanced-hybrid-securities-exchange-hamid-jahanzad/).

## Services

| Service | Domain | What it does |
|---|---|---|
| **Exchange8:Gateway** | Market Access | Order routing, session management, market data distribution to participants |
| **Exchange8:ME** | Member Management | Member onboarding, permissions, role assignment, session controls |
| **Exchange8:OMS** | Order Management System | Order lifecycle (create/cancel/replace), order books, matching engine interface |
| **Exchange8:MD** | Market Data | Consolidated quotes, ladders, depth of book, SSE streaming to subscribers |
| **Exchange8:VFeeds** | External Feeds | Market data vendor adapters (Bloomberg, Refinitiv, etc.), normalization |
| **Exchange8:SOR** | Smart Order Router | Order routing logic, smart order execution, venue distribution |
| **Exchange8:Clearing** | Clearing | Trade confirmation, netting, clearinghouse interfaces, trade validation |
| **Exchange8:Margin** | Margin Management | Initial/variation margin calculation, margin calls, collateral management |
| **Exchange8:Settlement** | Settlement | Delivery vs. payment (DVP), cash settlement, reconciliations, corporate actions |
| **Exchange8:RefData** | Reference Data | Instruments, markets, symbols, fee schedules, holiday calendars |
| **Exchange8:RFQ** | Request for Quote | RFQ workflow, quote negotiation, block trading, voice/chat integration |
| **Exchange8:Risk** | Pre-Trade Risk | Real-time risk checks, exposure limits, position limits, credit limits |
| **Exchange8:Regulatory** | Regulatory Reporting | Trade reporting, regulatory filings, compliance workflows |
| **Exchange8:Surveillance** | Market Surveillance | Trade surveillance, pattern detection, anomaly monitoring, alert management |

## Flutter Apps

| Repository | Description |
|---|---|
| `exchange8-eexchange` | Member/trader-facing exchange portal |
| `exchange8-admin` | Vertical admin console (RFW) |

## Platform Dependencies

Exchange8 depends on these Sentin8 platform services for cross-cutting concerns:

| Platform Service | What Exchange8 gets from it | Dependency direction |
|---|---|---|
| **Sentin8:Auth** | Identity, sign-in, token issuance (user + M2M), JWKS, RBAC authorization | exchange8 → Auth (verification + M2M identity) |
| **Sentin8:BFF** | Proxy, delegation gateway, client session custody, token attach | exchange8 ← BFF (user tokens proxied to services) |
| **Sentin8:Bus** | Durable event transport, producer outbox relay, consumer DLQ, SSE push | exchange8 → Bus (produce + consume domain events) |
| **Sentin8:Flow** | Multi-step workflow orchestration with retries and wait states | exchange8 → Flow (workflow execution) |
| **Sentin8:Cron** | Recurring job scheduling (EOD processing, margin calls) | exchange8 → Cron (schedule registration) |
| **Sentin8:Func** | Tenant-authored executable hooks | exchange8 ← Func (proxy-back handlers for hooks) |
| **Sentin8:Storage** | Object/file storage (trade confirmations, settlement documents) | exchange8 → Storage (document PUT/GET) |
| **Sentin8:Stat** | Business metrics aggregation and telemetry | exchange8 → Stat (event ingestion) |
| **Sentin8:Sys** | Organization/environment lifecycle, service enablement | exchange8 → Sys (env-init events) |

## Key Invariants

- **Complete regulatory traceability** - who acted on which order, when, why it was denied (itemized traces), every wire message, every fill with preserved broker tags
- **Capital protection** - exposure limits enforced before acceptance; market data access bought exactly as consumed
- **One source of truth** - all trading state lives in the exchange8 database with hard tenant isolation
- **Zero infrastructure operation** - the vertical never operates infrastructure; the platform group does
- **Composable enrollment** - any subset of services enrolls independently via `vertical enroll`

---

**Architecture & Design:** exchange8 and its underlying concepts were designed by [Hamid Jahanzad](https://www.linkedin.com/in/hamidjahanzad/).
