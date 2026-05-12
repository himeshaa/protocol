# Contributing to the Himeshaa Protocol

Thank you for your interest in improving HMP. This document describes how to propose changes to the protocol specification.

## Guiding Principles

1. **The spec is the contract.** Every change affects every implementation. Proposals must be precise, justified, and backward-compatible whenever possible.
2. **Implementations drive evolution.** Spec changes should emerge from real implementation experience, not theoretical design.
3. **Simplicity is a feature.** HMP's power comes from its minimal surface area. New RPCs, fields, or schemas carry a permanent maintenance cost. The bar for additions is high.

## Types of Contributions

| Type | Process | Example |
|------|---------|---------|
| **Typo / Clarification** | Pull Request | Fix ambiguous wording, correct a JSON example |
| **Editorial** | Pull Request | Improve structure, add cross-references |
| **Spec Gap** | Issue → Discussion → PR | Missing error codes, undefined behavior |
| **New Feature** | RFC Process | New RPC method, new file schema, new extension |
| **Breaking Change** | RFC Process | Rename a field, change the confidence formula |

## Pull Requests

For typos, clarifications, and editorial improvements:

1. Fork the repository.
2. Create a branch from `main` with a descriptive name (e.g., `fix/pagination-cursor-gap`).
3. Make your changes to `README.md` or other spec files.
4. Ensure all JSON examples remain valid.
5. Open a Pull Request with a clear description of **what** changed and **why**.

## RFC Process

For any change that affects protocol behavior, wire format, or schemas:

### Step 1 — Open an Issue

Create a GitHub Issue using the title format:

```
RFC: <Short description>
```

The issue body must include:

- **Summary** — One paragraph describing the change.
- **Motivation** — Why is this needed? What implementation problem does it solve?
- **Proposal** — The specific spec change, including any new or modified JSON examples.
- **Backward Compatibility** — Does this break existing implementations? If so, what is the migration path?
- **Alternatives Considered** — What other approaches were evaluated and why were they rejected?

### Step 2 — Discussion

The issue will be labeled `rfc` and open for community discussion. RFCs require a minimum **14-day discussion period** before moving forward.

During discussion, the proposal may be revised based on feedback. The issue author is responsible for maintaining an up-to-date proposal in the issue body.

### Step 3 — Decision

After the discussion period, the maintainers will label the RFC with one of:

| Label | Meaning |
|-------|---------|
| `rfc/accepted` | Approved for implementation. Author or a maintainer opens a PR. |
| `rfc/deferred` | Valid proposal, but not the right time. Will be reconsidered in a future version. |
| `rfc/rejected` | Declined with rationale. |

### Step 4 — Implementation

Once accepted, submit a Pull Request that:

1. Updates the spec (`README.md` and/or schema files).
2. Updates the `CHANGELOG.md` under `[Unreleased]`.
3. Adds or updates test vectors if the change affects wire format or computation.

## Versioning

The protocol follows [Semantic Versioning](https://semver.org/):

- **Patch** (`0.1.1`) — Clarifications and editorial fixes. No behavioral change.
- **Minor** (`0.2.0`) — Backward-compatible additions (new optional fields, new extensions, new RPC methods).
- **Major** (`1.0.0`, `2.0.0`) — Breaking changes (renamed fields, removed methods, formula changes).

The protocol will remain at `0.x.x` until at least two independent implementations demonstrate interoperability.

## Spec Maturity Progression

| Stage | Meaning |
|-------|---------|
| **Working Draft** | Active development. Breaking changes expected. |
| **Candidate** | Feature-complete. Seeking implementation feedback. Breaking changes unlikely. |
| **Standard** | Stable. Two or more interoperable implementations exist. Breaking changes require a new major version. |

## Code of Conduct

All participants are expected to be respectful and constructive. Proposals are evaluated on technical merit.

## License

By contributing, you agree that your contributions will be licensed under the [Apache License 2.0](LICENSE).
