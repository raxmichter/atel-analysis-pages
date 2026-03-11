# Atel Pilot Readiness Checklist (Executive + Operator)

**Purpose:** Define the exact conditions that must be true before Atel runs **serious commercial pilots**.

**Scope used:**
- `atel-technical-executive-analysis.md`
- `atel-competitive-landscape-executive-analysis.md`
- `atel-go-to-market-executive-analysis.md`
- `analysis/state/atel-platform-technical-notes.md`
- `analysis/state/atel-portal-technical-notes.md`
- `analysis/state/atel-sdk-technical-notes.md`

**Decision rule:** All **Critical** gates must be checked before launch. Any red-flag blocker in Section 10 means **no pilot launch**.

---

## 1) Executive summary

Atel has a strong strategic architecture (trust + settlement + governance + portal UX), but current evidence indicates **pre-hardening maturity**. The stack is **not ready for serious pilots today** without targeted fixes.

### Executive call (current)
- [ ] **Ready now**
- [x] **Ready after fixes** (recommended)
- [ ] **Not ready**

### What must be true before launch
- [ ] Trust verification and settlement logic are canonical, tested, and consistent across SDK/Platform/Portal.
- [ ] Security posture is hardened (no permissive defaults, no unauthenticated sensitive paths).
- [ ] Financial integrity is money-safe and reconciliation-clean.
- [ ] Reliability, incident response, and evidence exports meet enterprise pilot expectations.
- [ ] Pilot is sold as **B2B trust control plane** (not broad open-marketplace scale motion).

---

## 2) Pilot gating criteria overview

> **Launch rule:** Every gate below must be marked ✅.

### Gate A — Product correctness (Critical)
- [ ] Cross-repo API/schema contract tests pass in CI for SDK ↔ Platform ↔ Portal.
- [ ] Known drift issues are closed (e.g., missing `offers` migration, `agents.online` schema mismatch, SDK acceptance export mismatch).
- [ ] Pilot-critical user journeys pass end-to-end (register → execute/prove → settle/dispute → report).

### Gate B — Security controls (Critical)
- [ ] No default/shared secrets in any environment.
- [ ] Relay/admin/sensitive routes require auth + authorization.
- [ ] Admin session handling upgraded to secure server-side/session-cookie pattern (no localStorage bearer for production admin).

### Gate C — Financial integrity (Critical)
- [ ] Monetary logic uses decimal/integer-minor-unit model (no float-based accounting in settlement ledger paths).
- [ ] Double-entry or equivalent invariant checks enforce ledger correctness.
- [ ] Daily reconciliation and exception handling are operationalized.

### Gate D — Compliance evidence (Critical)
- [ ] Evidence pack can be generated on demand for pilot customers (identity, policy, trace, proof, settlement, dispute history).
- [ ] Controls map exists to NIST AI RMF / ISO 42001 / relevant EU AI Act traceability expectations.

### Gate E — Reliability + observability (Critical)
- [ ] SLOs defined for pilot-critical APIs and async jobs.
- [ ] Dashboarding/alerting exists for trust verification, settlement, disputes, and relay delivery.
- [ ] Incident runbooks tested in tabletop or live drill.

### Gate F — Support + operations (High)
- [ ] On-call ownership and escalation matrix are documented.
- [ ] Runbooks exist for payment incidents, proof verification failures, and rollback/freeze operations.

### Gate G — Customer integration readiness (High)
- [ ] Reference integrations and onboarding guides are complete for top target frameworks/workflows.
- [ ] Security and architecture review packet is ready for customer procurement.

### Gate H — Commercial readiness (High)
- [ ] Pilot contracts include success criteria, scope boundaries, and risk controls.
- [ ] Clear pilot-to-paid conversion plan exists.

---

## 3) Product readiness checklist

### Core functionality
- [ ] Canonical trust pipeline is defined and implemented once (no conflicting score/proof semantics across repos).
- [ ] Proof verification is enforced in completion/settlement path for pilot workflows.
- [ ] Dispute workflow is fully operational with deterministic states and audit trails.
- [ ] Offer/order lifecycle endpoints are complete and consistent with frontend usage.

### Engineering quality gates
- [ ] CI blocks release on: build + tests + lint + contract checks.
- [ ] Portal lint/type debt is reduced to agreed threshold (target: 0 critical lint/type errors on pilot-critical pages).
- [ ] SDK acceptance/deploy script path works in clean environment.
- [ ] Migrations are versioned, reproducible, and validated in CI against empty + existing DB states.

### Release discipline
- [ ] Single authoritative API/version matrix is published.
- [ ] Backward compatibility policy exists for pilot APIs.
- [ ] Change management and rollback criteria are documented.

---

## 4) Security readiness checklist

### Identity, auth, access
- [ ] DID auth validation includes replay protection beyond in-memory windows for production paths.
- [ ] Admin actions require role-based authorization and are server-enforced.
- [ ] Relay endpoints are authenticated and rate-limited.

### Secret/config hardening
- [ ] Default JWT secret fallback removed; boot fails hard when secrets are missing.
- [ ] Secrets are stored in managed secret system (not plaintext config artifacts).
- [ ] Environment-specific CORS policies are least-privilege.

### App and dependency security
- [ ] Security headers/CSP are configured for portal.
- [ ] Dependency audit baseline: **0 critical, 0 high** unresolved vulnerabilities in pilot runtime paths.
- [ ] Threat model completed for trust, payment, admin, and relay surfaces.
- [ ] Pen test or focused security review completed with remediation plan signed off.

---

## 5) Financial/settlement integrity checklist

### Monetary correctness
- [ ] Replace float-based money arithmetic with fixed-precision model.
- [ ] Explicit fee accounting paths credit/debit all platform and user accounts consistently.
- [ ] Settlement and dispute calculations have deterministic rounding policy.

### Ledger controls
- [ ] Ledger invariant checks run on every financial state transition.
- [ ] Idempotency keys protect all payment and settlement write operations.
- [ ] Reconciliation jobs are scheduled and alert on variance immediately.

### Operations and auditability
- [ ] Daily close process documented and owned.
- [ ] Exception queue exists for unmatched/failed settlements.
- [ ] Manual override procedures require dual-control and are fully logged.

---

## 6) Compliance/audit evidence checklist

### Evidence model
- [ ] For each pilot transaction, evidence includes: actor identity, consent/policy context, execution trace hash chain, proof bundle, settlement result, and dispute state.
- [ ] Evidence artifacts are immutable/tamper-evident and timestamped.
- [ ] Retention and retrieval policy meets pilot contractual requirements.

### Control mapping
- [ ] Controls-to-framework mapping is documented (NIST AI RMF, ISO 42001, customer-required controls).
- [ ] EU AI Act logging/traceability expectations are explicitly addressed where applicable.
- [ ] Data handling matrix exists (PII, sensitive data, residency, retention, deletion).

### Audit operations
- [ ] Evidence export is one-click/API-accessible for auditors/customers.
- [ ] Sample audit packet validated internally before pilot launch.
- [ ] Compliance owner assigned for customer audit requests.

---

## 7) Reliability/observability checklist

### SLOs and monitoring
- [ ] SLOs published for: API availability, proof verification latency/success, settlement completion latency/success, relay delivery success.
- [ ] Structured logs + metrics + tracing are enabled in all pilot-critical services.
- [ ] Golden signals and business KPIs are visible in shared dashboards.

### Alerting and incident response
- [ ] Alert thresholds defined with pager routing.
- [ ] Sev1/Sev2 incident playbooks are documented and tested.
- [ ] Status communication templates exist for pilot customers.

### Resilience
- [ ] Backup/restore tested for core data stores.
- [ ] Failure injection/tabletop completed for at least: payment gateway outage, proof verification service degradation, and DB migration rollback.
- [ ] Multi-instance behavior validated for background jobs and nonce/session handling.

---

## 8) Support/ops/runbook checklist

### Team readiness
- [ ] Named DRI per area: platform backend, SDK trust pipeline, portal/admin UX, payments, security/compliance.
- [ ] On-call schedule and escalation path approved.
- [ ] Customer support SLA targets and support hours defined.

### Runbooks (must exist before launch)
- [ ] Payment reconciliation incident runbook
- [ ] Dispute escalation and adjudication runbook
- [ ] Trust/proof verification failure runbook
- [ ] Security incident containment runbook
- [ ] Rollback/hotfix release runbook

### Operational hygiene
- [ ] Pilot launch checklist dry-run completed.
- [ ] Change freeze windows defined during go-live.
- [ ] Post-incident review template and cadence established.

---

## 9) Integration/customer onboarding checklist

### Technical onboarding
- [ ] Integration guides for top target stack(s) are complete (e.g., LangGraph/AutoGen/Temporal/MCP-adjacent flows).
- [ ] Sample code and reference architectures are validated against current APIs.
- [ ] Sandbox environment mirrors production-critical behavior.

### Customer enablement
- [ ] Security packet ready (architecture, auth model, data flow, threat model, control mapping).
- [ ] Pilot scope doc defines in-scope use cases, excluded features, and rollback criteria.
- [ ] Joint success plan agreed with each pilot customer.

### Commercial onboarding
- [ ] Contract terms include incident obligations, data handling, and evidence access rights.
- [ ] Pilot-to-paid decision framework and timeline are explicit.
- [ ] Executive sponsor assigned on both Atel and customer sides.

---

## 10) Red-flag blockers that should halt pilot launch

If **any** item below is true at launch time, stop pilot launch:

- [ ] Unresolved schema/runtime mismatches in production migration path.
- [ ] Float-based money logic still active in settlement ledger.
- [ ] Any unauthenticated sensitive endpoint (admin/relay/payment-affecting).
- [ ] Default secrets or insecure secret handling still enabled in deployed environments.
- [ ] No tested incident runbook for payment, trust verification, and security events.
- [ ] No auditable evidence export for pilot transactions.
- [ ] Contract/API drift still causes failed end-to-end critical journeys.
- [ ] Critical/high security vulnerabilities unresolved in runtime-critical dependencies.
- [ ] No owner assigned for reconciliation and dispute adjudication operations.

---

## 11) Minimum viable pilot success metrics

> Pilot is “successful” only if both **technical** and **commercial** thresholds are hit.

### Technical success metrics (minimum)
- [ ] **Availability:** ≥ 99.5% uptime for pilot-critical APIs during pilot window.
- [ ] **Proof integrity:** ≥ 99.0% successful proof verification for in-scope transactions.
- [ ] **Settlement correctness:** 0 unreconciled financial discrepancies above agreed threshold at pilot close.
- [ ] **Incident quality:** 0 Sev1 incidents caused by known pre-launch blockers; all Sev2 incidents resolved within SLA.
- [ ] **Dispute handling:** 100% disputes have complete auditable timeline and disposition within contractual SLA.

### Security/compliance success metrics (minimum)
- [ ] 100% pilot transactions retain complete evidence bundle.
- [ ] 100% customer audit evidence requests fulfilled within agreed turnaround.
- [ ] 0 critical security findings left unmitigated beyond agreed exception window.

### Commercial success metrics (minimum)
- [ ] At least 2–3 design-partner pilots complete full workflow from onboarding to outcome review.
- [ ] At least 1 pilot converts to paid expansion/annual agreement (or signed expansion commitment).
- [ ] Customer-reported value achieved in at least one quantified dimension (e.g., audit prep time reduction, faster incident forensics, lower dispute cycle time).

---

## 12) Recommendation: ready now / ready after fixes / not ready

### Current recommendation: **READY AFTER FIXES**

- [ ] Ready now
- [x] Ready after fixes
- [ ] Not ready

### Rationale
Atel has credible product architecture and differentiation, but present findings show critical readiness gaps in cross-repo contract discipline, security hardening consistency, and financial correctness safeguards. These are fixable in a focused hardening program.

### Launch condition to switch to “Ready now”
- [ ] All Critical gates (A–E) in Section 2 are complete and evidenced.
- [ ] No Section 10 red-flag blockers remain.
- [ ] Minimum metrics instrumentation in Section 11 is live before first customer pilot start.

---

## Pilot sign-off block

- **Engineering Lead:** [ ] Approved
- **Security Lead:** [ ] Approved
- **Finance/Operations Lead:** [ ] Approved
- **Compliance/Legal Lead:** [ ] Approved
- **GTM/Customer Success Lead:** [ ] Approved
- **Executive Sponsor:** [ ] Approved
- **Target pilot start date:** __________________
- **Go/No-Go meeting date:** __________________
