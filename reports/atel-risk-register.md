# ATEL Risk Register (Board + Operator Ready)

_Date: 2026-03-11_

## Scope and method
This register synthesizes risks from:
- `atel-technical-executive-analysis.md`
- `atel-competitive-landscape-executive-analysis.md`
- `atel-go-to-market-executive-analysis.md`
- `analysis/state/atel-platform-technical-notes.md`
- `analysis/state/atel-portal-technical-notes.md`
- `analysis/state/atel-sdk-technical-notes.md`

Severity and likelihood are assessed for current state, assuming no major hardening intervention.

---

## 1) Top 10 risks summary

| Rank | Risk ID | Risk | Why it matters now | Immediate executive action |
|---|---|---|---|---|
| 1 | ATEL-R01 | Financial integrity failure (float ledger + accounting gaps) | Settlement/dispute errors can directly destroy trust and create legal exposure. | Mandate money-safe model (integer minor units/decimal), ledger invariants, reconciliation sign-off before scale. |
| 2 | ATEL-R02 | Security incident on auth/admin/relay surfaces | Token/session/default-secret and unauth surfaces create exploit path risk. | Force security hardening sprint and block broad launch until critical controls pass. |
| 3 | ATEL-R03 | Trust model fragmentation (SDK vs Platform semantics) | Weakens core differentiator: “verifiable trust tied to settlement.” | Establish one canonical trust pipeline and one source of truth for trust acceptance. |
| 4 | ATEL-R04 | Schema/API contract drift across repos | Runtime breaks and failed customer journeys undermine reliability and sales credibility. | Introduce cross-repo contract tests + migration completeness checks in CI gates. |
| 5 | ATEL-R05 | Enterprise compliance gap (audit-readiness) | Regulated buyers will stall or fail procurement without evidence packaging. | Prioritize compliance evidence layer + control mapping (NIST/ISO/EU logging expectations). |
| 6 | ATEL-R06 | GTM narrative split (marketplace vs trust control plane) | Diluted positioning slows sales cycles and creates buyer confusion. | Standardize narrative: “trust-and-settlement control plane for high-stakes agent workflows.” |
| 7 | ATEL-R07 | Marketplace liquidity and synthetic-network risk | Current observed depth is low (few verified/online agents/offers), limiting marketplace-led growth. | Treat marketplace as testbed, not core revenue engine in next 6–12 months. |
| 8 | ATEL-R08 | Observability and incident-response immaturity | Limited telemetry/runbooks slows recovery and weakens enterprise confidence. | Fund telemetry baseline, SLOs, incident runbooks, and on-call procedures. |
| 9 | ATEL-R09 | Portal/Backend coupling and frontend quality debt | Lint/type/test debt increases breakage probability as backend evolves. | Enforce strict typing/tests and remove `any`-driven contract assumptions. |
| 10 | ATEL-R10 | Ecosystem displacement by incumbents/standards | Cloud/orchestration/observability stacks can absorb “good enough” trust features. | Win via integration-first strategy and defend unique proof+settlement capability. |

---

## 2) Full risk register

| Risk ID | Risk title | Category | Description | Severity | Likelihood | Time horizon | Leading indicators | Mitigation options | Suggested owner | Decision impact |
|---|---|---|---|---|---|---|---|---|---|---|
| ATEL-R01 | Financial integrity failure in settlement/accounting | financial | Float-based money logic and inconsistent fee-credit paths can cause reconciliation errors and payout disputes. | Critical | High | Near-term (0–3 mo) | Reconciliation breaks; unexplained balance deltas; dispute reversals; manual ledger fixes rising | Move to integer/decimal accounting; define ledger invariants; dual-entry checks; daily automated reconciliation; finance+engineering sign-off gates | CTO + Head of Finance/Payments | **Go/No-Go for any scaled transaction volume** |
| ATEL-R02 | Security breach via auth/admin/relay weaknesses | security | Uneven auth defaults (e.g., default JWT fallback), localStorage admin tokens, permissive surfaces, and unauth relay patterns increase exploit likelihood. | Critical | High | Near-term (0–3 mo) | Unauthorized admin actions; abnormal token reuse; relay abuse spikes; security audit findings | Remove insecure defaults; enforce secret boot checks; server-side admin sessions; endpoint auth hardening; threat model + penetration test | CISO / Security Lead | **Blocks enterprise pilots and raises incident liability** |
| ATEL-R03 | Canonical trust pipeline not unified | technical | SDK trust/proof semantics and Platform trust score/update paths are not fully unified, undermining trust consistency. | High | High | Near-term (0–6 mo) | Trust score anomalies; inconsistent verification outcomes across components; customer questions on “source of truth” | Define canonical trust acceptance pipeline; unify trust schema; remove self-reported trust shortcuts for critical flows; add end-to-end trust conformance tests | Chief Architect / Head of Platform | **Core product differentiation risk** |
| ATEL-R04 | Cross-repo contract and migration drift | ops | Documented mismatches (schema/API/script/export drift) can break core journeys and deployment safety. | High | High | Near-term (0–3 mo) | Failed deploys on clean env; portal runtime errors post backend release; migration rollback events | Add contract-test suite (Portal↔Platform↔SDK); migration completeness checks; release version matrix; CI block on drift | VP Engineering / Release Manager | **Impacts release confidence and customer uptime** |
| ATEL-R05 | Compliance and audit-readiness shortfall | legal | Current posture is below typical SOC2/ISO enterprise expectations despite trust-heavy positioning. | High | Medium-High | 3–12 mo | Procurement stalls; security questionnaires fail; extended legal review cycles | Build compliance evidence exports; map controls to NIST/ISO/EU logging requirements; formalize audit logs/retention/access controls | COO + Compliance Lead | **Determines ability to close regulated accounts** |
| ATEL-R06 | GTM positioning split and diluted buyer message | GTM | Competing narratives (open marketplace economy vs enterprise trust control plane) slow decisions and confuse budget owners. | High | High | Near-term (0–6 mo) | Low conversion from pilots; mixed messaging in decks/docs; long cycles without champion clarity | Single ICP-led narrative; verticalized proof points; unify sales + product language; retire broad marketplace-first messaging for enterprise motion | CRO / CEO | **Affects pipeline quality and conversion rate** |
| ATEL-R07 | Marketplace depth/network quality risk | market | Observable network depth appears early (low verified/online ratio, shallow offers), weakening marketplace-led growth assumptions. | High | High | 0–12 mo | Flat active offer count; low repeat orders; high share of synthetic/non-productive agents | Reframe marketplace as controlled sandbox; focus on curated design partners; enforce quality/verification thresholds | GM Marketplace / Head of Growth | **Impacts revenue forecast realism and investor narrative** |
| ATEL-R08 | Operational resilience and observability gap | ops | Limited structured telemetry, sparse SLO governance, and immature runbooks increase incident MTTR and contractual risk. | High | Medium-High | Near-term (0–6 mo) | Unknown root cause incidents; prolonged outages; manual firefighting; lack of service-level reporting | Instrument logs/metrics/traces; define SLOs and error budgets; incident runbooks and on-call drills; postmortem process | Head of SRE / Platform Ops | **Constrains scale and enterprise SLA commitments** |
| ATEL-R09 | Frontend contract fragility and quality debt | technical | Portal has significant lint/type debt and weak test coverage; heavy backend assumptions can cause production regressions. | Medium-High | High | 0–6 mo | Rising client-side runtime errors; release rollbacks; repeated hotfixes in dashboard/admin flows | Strict TS contracts; eliminate `any` in critical paths; add integration/E2E tests; centralize API client behavior | Head of Product Engineering | **Directly affects user trust and support burden** |
| ATEL-R10 | Ecosystem displacement by incumbents and bundled platforms | ecosystem | Cloud and orchestration incumbents may bundle governance/traceability features, reducing Atel differentiation if not defended. | High | Medium | 6–18 mo | Prospect objections: “already covered by cloud stack”; win-rate decline vs bundled alternatives | Integration-first strategy (LangGraph/AutoGen/Temporal/MCP/A2A); focus on proof-enforced settlement outcomes incumbents lack | CEO + Partnerships Lead | **Strategic moat durability decision** |
| ATEL-R11 | Credibility gap between claims and production maturity | market | Strong trust/compliance claims can backfire if reliability, consistency, and controls do not match enterprise diligence standards. | High | Medium-High | Near-term (0–6 mo) | Deal-stage drop-off after technical due diligence; negative reference calls; audit finding concentration | Tighten external claims to verified capabilities; publish transparent reliability/security metrics; stage-gate marketing language | CEO + Marketing + CTO | **Brand trust and category leadership risk** |
| ATEL-R12 | Supply-chain and dependency vulnerability exposure | security | Known dependency vulnerabilities and weak default hardening in developer/ops paths raise breach and compliance risk. | Medium-High | Medium | 0–6 mo | Critical CVEs unresolved >30 days; repeated dependency exceptions; security scanner failures | Dependency upgrade cadence; SBOM + vulnerability policy; block release on critical vulns; secure secret handling standards | Security Engineering Manager | **Affects security audit outcomes and insurance posture** |
| ATEL-R13 | Dispute/governance legitimacy risk | legal | If trust signals, proof quality, and dispute outcomes diverge, parties may challenge settlement fairness and platform neutrality. | High | Medium | 3–12 mo | Dispute appeal rate rises; repeated manual overrides; conflicting evidence records | Formal dispute policy; evidence-chain integrity checks; independent review workflow for high-value disputes | Head of Trust & Safety / Legal | **Legal exposure and platform defensibility** |
| ATEL-R14 | Scale-out architecture constraints | technical | In-process jobs, partial async strategy, and immature distributed coordination can degrade reliability under multi-instance growth. | Medium-High | Medium | 6–18 mo | Queue/backlog growth; duplicate job processing; multi-instance state inconsistencies | Externalize schedulers/queues; idempotent job design; distributed locks; load testing and capacity planning | Principal Platform Engineer | **Determines safe expansion path** |
| ATEL-R15 | Long enterprise sales-cycle cash-flow pressure | financial | Heavy compliance/security selling motion may delay ARR realization versus operating burn. | Medium | Medium | 6–18 mo | Pilot-to-paid conversion lag; CAC payback deterioration; runway compression scenarios | Tight stage-gates for pilot spend; services-assisted monetization; target high-conviction vertical accounts first | CFO + CRO | **Runway and funding strategy sensitivity** |

---

## 3) Interdependency notes (risk amplification map)

- **R03 (trust pipeline fragmentation)** amplifies **R13 (dispute legitimacy)** and **R11 (credibility gap)** because inconsistent trust semantics undermine defensibility.
- **R01 (financial integrity)** amplifies **R13 (legal exposure)** and **R11 (market trust)** when settlement errors become customer-visible.
- **R02 (security weaknesses)** amplifies **R05 (compliance readiness)** and **R15 (cash pressure)** through delayed enterprise closes and incident costs.
- **R04 (contract drift)** amplifies **R08 (ops instability)** and **R09 (portal regressions)** by increasing release-induced outages.
- **R06 (messaging split)** amplifies **R07 (marketplace depth risk)** and **R15 (sales-cycle pressure)** via poor ICP targeting.
- **R08 (observability gap)** amplifies every near-term technical/security risk by increasing detection and recovery time.
- **R10 (ecosystem displacement)** amplifies **R06 (GTM confusion)** if positioning remains broad rather than integration-led.

---

## 4) Suggested prioritization order

### Priority A — Immediate board-level blockers (0–30 days)
1. **R01** Financial integrity
2. **R02** Security/auth hardening
3. **R03** Canonical trust pipeline
4. **R04** Contract/migration discipline

### Priority B — Pilot viability and enterprise conversion (30–90 days)
5. **R05** Compliance evidence and controls
6. **R08** Observability/SLO/runbooks
7. **R06** GTM narrative unification
8. **R09** Portal quality and contract safety

### Priority C — Scale and strategic defense (90+ days)
9. **R07** Marketplace depth and quality strategy
10. **R13** Dispute governance maturity
11. **R12** Dependency/security operations maturation
12. **R14** Scale-out architecture hardening
13. **R10** Ecosystem moat defense
14. **R11** Credibility governance (ongoing)
15. **R15** Sales-cycle/cash discipline (ongoing)

---

## 5) Risk treatment posture: accept vs mitigate vs avoid

### Mitigate (active engineering/GTM/control work required)
- **R01, R02, R03, R04, R05, R08, R09, R12, R13, R14**
- Rationale: these are controllable execution risks with high downside and clear intervention paths.

### Avoid (do not pursue current path until conditions change)
- **R06** Avoid broad dual-narrative GTM; do not run “marketplace-first + enterprise-governance-first” in parallel as equal motions.
- **R07** Avoid forecasting core growth from open marketplace liquidity in current network state.
- Rationale: strategic focus errors can consume runway faster than technical debt.

### Accept (monitor and hedge; cannot fully eliminate)
- **R10** Some incumbents/OSS commoditization pressure is structural; accept as market reality while differentiating on proof-enforced settlement.
- **R11** Some credibility challenge is inherent in category creation; accept baseline skepticism but reduce with transparent evidence.
- **R15** Longer enterprise cycles are expected in regulated workflows; accept partially and manage with tighter cash/pipeline controls.

---

## 6) Operator notes: minimum governance cadence

- Weekly executive risk review on **Priority A/B** risks with owner-level KPI updates.
- Monthly board risk packet with:
  - open/closed risk deltas,
  - incident and reconciliation metrics,
  - pilot pipeline conversion health,
  - compliance readiness milestones.
- Stage-gate rule: no broad-scale launch until **R01–R05** have documented control evidence and sign-off.
