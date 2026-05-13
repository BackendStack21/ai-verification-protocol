# The AI Verification Protocol

> **Diagnose, repair, and measure** — a multi-agent pipeline for AI verification that quantifies verification debt, derives η from observable signals, tracks Ci/Cv ratios, and produces machine-readable certificates with in-toto attestation.

[![Version](https://img.shields.io/badge/version-5.2.5-blue)](ai-verification-protocol.md)
[![Website](https://img.shields.io/badge/website-vprotocol.21no.de-38bdf8)](https://vprotocol.21no.de)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

---

## The Problem

AI generates code 16,000× cheaper than we can verify it. Generation costs dropped 100–150× since 2023. Human review is still capped at ~200 LOC/hour. The **Cv/Ci ratio** — cost-to-verify divided by cost-to-implement — has exploded from ~33:1 to **~3,300:1**, and it's degrading exponentially.

## The Answer

This protocol is the operational companion to the whitepaper [*The AI Verification Debt*](https://21no.de/viewer.html?file=publications/the-verification-trap.md&label=Whitepaper). It defines a **five-agent pipeline** that:

1. **Classifies** the PR (KnownGroundTruth, NovelBehavior, GeneratedCode, GeneratedTests)
2. **Runs 9 verification axes** — semantic correctness, behavioral contract, security surface, structural integrity, adversarial surface, documentation coverage, and more
3. **Derives η** from 7 observable signals (mutation kill rate, oracle agreement, branch coverage, fuzz survival, SAST clean rate, static depth, doc coverage)
4. **Computes ρ** — the correlation penalty that quantifies how dependent verification artifacts are on the generator
5. **Tracks Ci/Cv ratio** per PR, per module — including a human Ci floor for non-AI-generated code
6. **Auto-repairs** documentation gaps, missing tests, and type mismatches (behavior-changing fixes are human-only)
7. **Signs an in-toto attestation** — machine-readable JSON certificates for CI gates, deploy pipelines, and audit dashboards

The human reviews the **certificate**, not the code.

## Key Metrics

| Metric | Symbol | What it measures |
|--------|--------|-----------------|
| Automated filtering efficiency | η | Fraction of potential defects caught by automated filters (∈ [0,1]) |
| Correlation penalty | ρ | How dependent verification artifacts are on the generator (∈ [0, 0.30]) |
| Verification debt | ΔDebt | `(1 − η) × Cv_raw × LOC_filtered` in hours |
| Cost ratio | Cv/Ci | Cost-to-Verify ÷ Cost-to-Implement — tracked per PR, per module |

## Quick Start

The protocol is **self-contained** — it functions as both a specification and a system prompt.

```bash
# Read the full protocol
curl -s https://vprotocol.21no.de/ai-verification-protocol.md | less

# Load it as a system prompt into any capable AI model
# The model will follow the pipeline roles defined in §0.1

# Or clone the repo
git clone https://github.com/BackendStack21/ai-verification-protocol.git
```

## Pipeline Roles

| Role | Agent | Responsibility |
|------|-------|---------------|
| **A** | Generator | Wrote the PR. Out of scope. |
| **B** | Reviewer | Classifies PR, runs 9 axes, drives repair loop |
| **C** | Contract formalizer | Extracts behavioral contract from spec (NEVER implementation) |
| **D** | Fuzzer / sandbox | Runs deterministic replay + property/fuzz suites |
| **E** | Certificate compiler | Derives η + ρ, computes ΔDebt, signs in-toto attestation |

Minimum two distinct provider families across B/C/D. Same-family pipelines pay the full ρ price.

## Version History

- **v5.2.x** — 13 patches hardening internal consistency (axes count, temporal paradoxes, invalidation loop, Gate 2/3 deadlock, nomenclature unification)
- **v5.1** — Spec independence recalibrated: contributes to ρ, flags axis 2.2, no mechanical floor
- **v5.0** — Spec bar universal, axis 2.9 (doc coverage) added, auto-correction mandatory
- **v4.0** — Measurement loop closed: η derived from signals, Ci/Cv per PR, meta-audit
- **v3.x** — Active Repair Mode, structural hardening

Full protocol: [ai-verification-protocol.md](ai-verification-protocol.md)

## Website

**[vprotocol.21no.de](https://vprotocol.21no.de)** — landing page with the Five Whys, pipeline overview, and whitepaper citations.

## License

MIT — see [LICENSE](LICENSE) file.

---

*Part of the [BackendStack21](https://github.com/BackendStack21) ecosystem — zero-dependency infrastructure tools for the agent-first era.*
