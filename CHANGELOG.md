# Changelog

All notable changes to the Himeshaa Protocol specification will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Fixed

- §9.2: **Arithmetic error in W verification vector #2** — derivation incorrectly computed `1.9433 + 0.6931 + 1 = 2.9130` (correct sum: `3.6362`). Expected W corrected from `0.6674` to `0.5344`. "Without A_origin" comparison corrected from `0.6536` to `0.5141`. All CHANGELOG references updated.
- §8.6: **Schema versioning contradiction resolved** — "Additive (new optional field) → No increment" contradicted the `additionalProperties: false` enforcement documented in schemas/README.md. All schema changes now require a version increment, with rationale paragraph added explaining the security-over-forward-compatibility trade-off and the `_meta` escape hatch.
- All repository links corrected to canonical `https://github.com/himeshaa/protocol`.
- Schema domain migrated from `himeshaa.dev` to `himeshaa.com` across all JSON Schemas, README examples, and schemas/README.
- GitHub repository metadata configured (description, homepage, topics).
- Protocol reframed as forge-agnostic: "GitHub IS the Hive Mind" → "Git IS the source of truth". GitHub is the first and largest forge, but GitLab, Gitea, Codeberg, and self-hosted forges are equally valid. All 22 GitHub-locked references corrected.
- Core narrative corrected: `.himeshaa/` is a reward (priority indexing, higher trust), not a prerequisite. The Hive Mind exists on day zero with zero `.himeshaa/` directories.
- CHANGELOG W formula corrected to include `A_origin` term, matching the canonical formula in §9.2 (`ln(1 + A_origin + Σ A_confirming)` instead of `ln(1 + Σ A_c)`).
- `hmp.memory.request` response example: `"status": "verified"` renamed to `"retrieval_status": "corroborated"` with formal definition of the four retrieval statuses (`nascent`, `corroborated`, `disputed`, `unanimous`).
- Signal type extensibility: replaced incorrect `_meta` reference with `x-` prefix convention for custom signal types.
- `hmp.signal.poll` response: added `poll_timestamp` field for clock-skew-safe sequential polling.
- §3.1 CQRS Model: clarified statelessness scope — operational state (ping queues, rate limit counters) is transient and lossy by design, distinct from the rebuildable Materialized Index.
- `schemas/README.md`: corrected forward compatibility claim — schemas enforce `additionalProperties: false` (strict validation, not permissive). Documentation now accurately reflects the security-first design decision.
- §5.1 `retrieval_status`: added explicit precedence order (`unanimous > corroborated > disputed > nascent`). Clarified `unanimous` requires 3 distinct confirmation files from 3 distinct Nodes. Resolves ambiguity where a memory could match both `unanimous` and `corroborated`.
- §8.4: cooling period temporal anchor changed from "receiving a new contradiction" to "the `created_at` timestamp of any existing contradiction" — ensures deterministic, stateless evaluation consistent with Design Principle 3.
- §10.1: removed stray Markdown code fence between `hmp.signal.poll` section and Ephemeral State Isolation paragraph.
- §10.4.4: `pulse.sync` tie-breaking rule added — when two deltas share identical timestamps, the delta from the lexicographically lower `server_id` wins.

### Added

- §1.1: "Two Layers of Intelligence" section — formalizes Latent Intelligence (code, deps, contributors) vs. Structured Intelligence (`.himeshaa/` memories) as distinct layers of the Hive Mind.
- §1.4: Two new design principles — "The Hive Mind exists on day zero" and "Forge-agnostic" — codifying that any public repo contributes without opt-in and that HMP is not tied to any specific Git forge.
- §2.2: "Latent Intelligence" and "Structured Intelligence" as formal glossary terms.
- §8.1: clarification note distinguishing Node Declaration context (`languages` + `domain` required) from `memory_context` (`stack` only required).
- §8.3: **Self-endorsement prohibition** — Server MUST ignore contradictions where the target URN references the same Node that authored the file.
- §8.3: **Target type constraint** — `target` URN in contradiction files MUST reference `memory` type. Contradictions of contradictions/confirmations are undefined and MUST be ignored.
- §8.4: **Self-endorsement prohibition** — Server MUST ignore confirmations where the target URN references the same Node that authored the file.
- §8.4: **Target type constraint** — `target` URN in confirmation files MUST reference `memory` type. Confirmations of confirmations/contradictions are undefined and MUST be ignored.
- §8.4: **Evidence lifecycle cooling period** — Servers SHOULD carry forward contradictions at 50% weight when a memory is edited within 7 days of receiving a new contradiction, preventing evidence-wiping abuse.
- §8.4: `outcome` field limit rationale documented — 16,384 chars vs 32,768 for contradiction `evidence`.
- §8.5: **Port Exclusion** rule — `node_uri` MUST NOT include port numbers to prevent parsing ambiguity.
- §7.3: note that `hmp.node.memory.read` returns raw Git content without Server-computed fields.
- §9.2: **Zero-authority edge case** documented — `A = 0` produces `C = 0` but memories remain discoverable via `hmp.node.memory.list`.
- §9.5: **Boundedness** note — `A ∈ [0, 1]` naturally guaranteed by component bounds. No clamping required.
- §8.6: **Schema Versioning** — formal rules for when schema versions increment (all change types = new version, justified by `additionalProperties: false` enforcement), multi-version support requirements, and deprecation cycle.
- §5.1: Formal `retrieval_status` enum with four defined values and computation rules based on the confirmation/contradiction graph.
- §5: Pagination mandate refined: "durable collections" use cursor-based pagination; ephemeral data RPCs (e.g., `hmp.signal.poll`) MAY use timestamp-based filtering.
- §9.2: Second verification vector (A_origin = 0.98, Σ A_confirming = 5.0, Σ A_contradicting = 1.0 → W = 0.5344) to validate A_origin inclusion.
- §10.2: Infrastructure note — Servers implementing `reasoning` extension MAY require GPU/LLM resources, operating beyond the stateless indexer role.
- `additionalProperties: false` added to all JSON Schemas (`memory-v1.json`, `confirmation-v1.json`, `contradiction-v1.json`, `hmp-v1.json`, `common-v1.json#memory_context`) to prevent DoS via arbitrary fields.
- `common-v1.json`: domain field description updated with SHOULD guidance for when to include domain in memories.
- §9.5: Formal Node Authority (A) formula with weighted components (D, K, F, G), log-normalization, reference ceilings, declaration boost, and verification vectors.
- §9.6: Context Similarity (S) comprehensively rewritten with 6 subsections: §9.6.1 Canonical Text Construction with SHA-256 verification vector, §9.6.2 Embedding Requirements (L2 normalization, `embedding_dimensions`, floating-point tolerance), §9.6.3 Anisotropy Mitigation (mean-centering SHOULD, detection threshold), §9.6.4 Cross-Server Comparability (S values are server-local, Pulse syncs raw data not scores), §9.6.5 Clamping Rationale (domain-specific justification), §9.6.6 Implementation Notes (two-stage retrieval, quality validation).
- §9.6.7: Latent Intelligence and S — clarifies that confidence formula applies only to Structured Intelligence (memories). Latent Intelligence uses graph traversal, not the confidence formula. Documents hybrid query pattern where latent acts as retrieval scope, structured provides scored results.
- `embedding_model` and `embedding_dimensions` fields in `hmp.initialize` response. Optional `embedding_postprocessing` field for declaring mean-centering.
- §10.1: Full Perception extension specification with `hmp.signal.emit`, `hmp.signal.broadcast`, and `hmp.signal.poll` payloads, signal types enum, TTL semantics, and aggregation rules.
- §10.2: Full Collective Reasoning extension specification with `hmp.reason.collective` payload, reasoning strategies (`convergent`, `divergent`, `adversarial`), provenance tracking, and agreement scoring.
- §10.3: Full Introspection extension specification with `hmp.network.introspect` response schema (index stats, coverage, health metrics).
- §10.4: Full Pulse Federation specification with `pulse.announce`, `pulse.sync`, `pulse.signal` message types, delta synchronization, peer lifecycle, and mutual TLS trust model.
- §13: JSON-RPC error codes — 5 standard codes and 9 HMP application-specific codes (`-32000` to `-32008`) with structured `data` fields and example response.
- §14: Related Work rewritten with deep research — 13 systems compared across 4 taxonomic categories (Single-Agent, Organizational, Multi-Agent Reasoning, Protocol/Infra) with arXiv citations. Adds Generative Agents, Reflexion, Mem0, Cognee, and LangMem.
- §15: Roadmap to v1.0 with maturity progression (Working Draft → Candidate → Standard), milestone tables for v0.2.0 and v1.0.0, and post-v1.0 directions.

## [0.1.0-draft] - 2026-05-12

### Added

- Initial protocol specification (Working Draft).
- Core Protocol: `hmp.initialize`, `hmp.memory.request` — read-only intelligence queries.
- Graph Protocol: `hmp.graph.traverse`, `hmp.graph.context` — Hive Mind navigation.
- Node Protocol: `hmp.node.ping`, `hmp.node.memory.list`, `hmp.node.memory.read` — cache invalidation and memory retrieval.
- CQRS architecture: GitHub as Write Model, Materialized Index as Read Model.
- Trust model: Git-based integrity, declared Node rewards, rate limiting guidelines.
- Extension stubs: Perception (`hmp.signal.*`), Collective Reasoning (`hmp.reason.*`), Introspection (`hmp.network.*`), Federation (Pulse).
- JSON Schema files for all `.himeshaa/` file formats (`schemas/` directory).
  - `common-v1.json` — Shared type definitions (memory_context, hmp_urn, blob_sha, object_id, utc_timestamp, meta).
  - `hmp-v1.json` — Node Declaration schema.
  - `memory-v1.json` — Memory schema.
  - `contradiction-v1.json` — Contradiction schema.
  - `confirmation-v1.json` — Confirmation schema.
- URN addressing scheme for cross-Node references (`urn:hmp:{node}:{type}:{id}`).
- URN validation regex (RFC 8141 / RFC 3986 compliant character class).
- URN case normalization rule: `node_uri` MUST be lowercased before storage and comparison.
- Confidence model with authority-weighted evidence and Sybil-resistant scoring.
- Asymptotically normalized W formula: `W = ln(1 + A_origin + Σ A_confirming) / (ln(1 + A_origin + Σ A_confirming) + ln(1 + Σ A_contradicting) + 1)`.
- Explicit time decay formula: `T = 0.5 ^ (t / t_half)` with class-specific half-life table.
- Verification vectors for W calculation: (1) A_origin = 0, Σ A_confirming = 49.0 → W = 0.7964; (2) A_origin = 0.98, Σ A_confirming = 5.0, Σ A_contradicting = 1.0 → W = 0.5344.
- Mandated natural logarithm (base e / ln) for all logarithmic operations.
- `cursor` parameter to `hmp.memory.request` for pagination consistency.
- Content length guidance: `content` SHOULD NOT exceed 32,768 characters.
- `_meta` extension point on all file schemas (`maxProperties: 32`).
- `maxItems` bounds on all array fields for DoS prevention.
- `target_blob_sha` accepts SHA-1 (40 chars) and SHA-256 (64 chars) for Git 3.0 transition.
- `created_at` MUST use UTC with Z suffix (timezone offsets and fractional seconds prohibited).
- `context.languages` and `context.domain` declared as REQUIRED fields in Node Declaration.
- CHANGELOG.md (Keep a Changelog format).
- CONTRIBUTING.md (RFC process for specification changes).
- `schemas/README.md` documenting schema structure, compatibility, and security constraints.

[Unreleased]: https://github.com/himeshaa/protocol/compare/v0.1.0-draft...HEAD
[0.1.0-draft]: https://github.com/himeshaa/protocol/releases/tag/v0.1.0-draft
