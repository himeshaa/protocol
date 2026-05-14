# Changelog

All notable changes to the Himeshaa Protocol specification will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Fixed

- All repository links corrected to canonical `https://github.com/himeshaa/protocol`.
- Schema domain migrated from `himeshaa.dev` to `himeshaa.com` across all JSON Schemas, README examples, and schemas/README.
- GitHub repository metadata configured (description, homepage, topics).
- Protocol reframed as forge-agnostic: "GitHub IS the Hive Mind" → "Git IS the source of truth". GitHub is the first and largest forge, but GitLab, Gitea, Codeberg, and self-hosted forges are equally valid. All 22 GitHub-locked references corrected.
- Core narrative corrected: `.himeshaa/` is a reward (priority indexing, higher trust), not a prerequisite. The Hive Mind exists on day zero with zero `.himeshaa/` directories.

### Added

- §1.1: "Two Layers of Intelligence" section — formalizes Latent Intelligence (code, deps, contributors) vs. Structured Intelligence (`.himeshaa/` memories) as distinct layers of the Hive Mind.
- §1.4: Two new design principles — "The Hive Mind exists on day zero" and "Forge-agnostic" — codifying that any public repo contributes without opt-in and that HMP is not tied to any specific Git forge.
- §2.2: "Latent Intelligence" and "Structured Intelligence" as formal glossary terms.
- §9.5: Formal Node Authority (A) formula with weighted components (D, K, F, G), log-normalization, reference ceilings, declaration boost, and verification vectors.
- §9.6: Context Similarity (S) comprehensively rewritten with 6 subsections: §9.6.1 Canonical Text Construction with SHA-256 verification vector, §9.6.2 Embedding Requirements (L2 normalization, `embedding_dimensions`, floating-point tolerance), §9.6.3 Anisotropy Mitigation (mean-centering SHOULD, detection threshold), §9.6.4 Cross-Server Comparability (S values are server-local, Pulse syncs raw data not scores), §9.6.5 Clamping Rationale (domain-specific justification), §9.6.6 Implementation Notes (two-stage retrieval, quality validation).
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
- Asymptotically normalized W formula: `W = ln(1 + Σ A_c) / (ln(1 + Σ A_c) + ln(1 + Σ A_x) + 1)`.
- Explicit time decay formula: `T = 0.5 ^ (t / t_half)` with class-specific half-life table.
- Verification vector for W calculation (Σ A_confirming = 49.0 → W = 0.7964 ± 0.0001).
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
