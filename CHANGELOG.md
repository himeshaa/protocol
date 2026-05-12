# Changelog

All notable changes to the Himeshaa Protocol specification will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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

[Unreleased]: https://github.com/vinkius-labs/himeshaa-protocol/compare/v0.1.0-draft...HEAD
[0.1.0-draft]: https://github.com/vinkius-labs/himeshaa-protocol/releases/tag/v0.1.0-draft
