# ATEL Competitive Notes (working)

## Local repo signals (Atel)
- Portal hero positioning: “The Trust Layer for AI Agent Collaboration” with core claims of DID identity, verifiable execution, on-chain anchoring, no middleman (`atel-portal-master/src/app/page.tsx` lines 12-19).
- Three-layer framing in portal: Identity / Trust / Exchange; exchange includes escrow settlement, trust scoring, dispute resolution (page.tsx lines 119-136).
- Claimed capabilities include E2E encryption, multi-chain anchoring (Solana/Base/BSC), “Zero-code onboarding”, NAT traversal (page.tsx lines 147-162).
- Portal stats claim: 25 SDK modules, 341 tests, 3 chains, 40+ CLI commands (page.tsx lines 173-176).
- Marketplace page shows two-sided market UX with offers + open requests, offer-buy, order lifecycle, escrow flow (`src/app/marketplace/page.tsx`).
- Dashboard shows trust score /100, verified flag, balance, frozen funds, order/transaction history (`src/app/dashboard/page.tsx`).
- Portal API targets external platform `https://api.atelai.org` and calls `/registry/v1/*`, `/trade/v1/*`, `/account/v1/*` (`src/lib/api.ts`).

### SDK architecture/maturity
- README frames SDK as “Agent Trust & Economics Layer” with identity, consent/policy, proofs, traces, score, graph, anchoring (README lines 3-6).
- README still says 13 modules + 241 tests + Phase 0.5 not complete (README lines 43-72, 205-210).
- `src/index.ts` exports 23 modules including trust, trust-sync, orchestrator, network/envelope/handshake/executor/auditor.
- Actual top-level module dirs in `src/`: 24; test files in `tests/`: 24 (counts via shell).
- Potential consistency risk: README says MIT, but package.json `license` is `UNLICENSED` and version is 0.8.13.
- Protocol spec is very broad and detailed, status Draft v1.0 dated 2026-02-15.
- Protocol section 14 defines commercial transaction model: auto-escrow on accept, proof-enforced completion, 10-min auto-settlement window, commission schedule, marketplace endpoint (`docs/protocol-specification.md` lines 1672+).

### Live API snapshot (2026-03-11)
- `/health`: service=atel-platform v2.0.0, agents=178, orders=150, gateways manual + crypto_solana/base/bsc.
- `/registry/v1/stats`: totalAgents=178, verifiedAgents=3, onlineAgents=3.
- `/trade/v1/offers`: count=4 active offers (mostly small USD offers, low completions).
- `/trade/v1/marketplace`: count=5 listed orders (many appear test-like / low commercial volume).
- Signal: platform live but early liquidity + low verified agent ratio.

## External market / competitor facts

### Direct or near-direct (agent trust + commerce + identity)
- **Olas**: positions Pearl as “AI Agent App Store”, Mech as decentralized “AI Agent Bazaar”; OLAS token coordinates economics. Olas Stack docs highlight marketplace resources + open-autonomy framework + on-chain protocol.
- **Skyfire**: explicit “agentic commerce platform”; KYA identity + payment tokens (kya/pay/kya+pay); buyer/seller agent accounts, wallets, seller service registration, token-based auth/payment claims.
- **Fetch.ai / Agentverse**: positions as platform for personal AI + brand agents; claims open directory marketplace with “2.7 million agents”.
- **Nevermined**: positions as “PayPal for AI Commerce”; AI payments infrastructure, AI-to-AI payments/subscriptions, usage metering, dynamic pricing, no-code checkout, developer APIs.
- **Bittensor**: open-source subnet economy with miners/validators/subnet creators/stakers and token emissions for contribution quality.

### AI governance / policy / risk platforms (adjacent)
- **Credo AI**: governance-focused; policy, controls, assessments, governance workflows; emphasizes enterprise compliance and model inventory.
- **Fiddler AI**: Model performance + LLM observability + governance and responsible AI posture.
- **Lakera**: AI-native security platform focused on prompt injection/jailbreak/data leakage with policy controls and low-latency runtime defenses.
- **HiddenLayer**: full lifecycle AI security incl. model scanning, red teaming, AI guardrails, agentic/MCP protection.

### Observability / audit / tracing substitutes
- **LangSmith (LangChain)**: tracing, evaluation, debugging, monitoring for LLM/agent apps.
- **Arize Phoenix**: open-source AI observability and eval stack.
- **Langfuse**: open-source LLM engineering platform for traces, evals, prompt mgmt.
- **W&B Weave**: traces, evaluations, monitoring, guardrails for agentic systems.
- **Helicone**: AI gateway + observability (appears acquired/joined Mintlify messaging).

### Workflow/orchestration substitutes
- **LangGraph**: framework for controllable/durable agent workflows.
- **Microsoft AutoGen**: framework + Studio for multi-agent systems.
- **Temporal**: durable execution for long-running, failure-tolerant workflows (incl AI/agents use cases).
- **UiPath / Workato / Zapier Agents**: enterprise automation incumbents moving into agentic workflows with governance and connector distribution.

### Verifiable execution / provenance building blocks
- **RISC Zero**: zkVM proving stack for verifiable computation.
- **Succinct**: proving infrastructure for verifiable apps.
- **OpenTelemetry / OpenLineage / OpenInference**: open standards and open-source libraries for tracing/provenance in ML/AI pipelines.

## Early interpretation
- ATEL appears to sit at intersection of: (a) agent identity + trust primitives, (b) verifiable execution/provenance, and (c) agent commerce settlement.
- Distinctive if ATEL can tightly integrate trust proofs + market settlement + practical SDK ergonomics.
- Biggest current risk from evidence: early-stage network effects (few verified/online agents), possible positioning drift (spec broad vs live market depth), and potential trust in consistency/credibility (metrics/version/licensing inconsistencies).