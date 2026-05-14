# Himeshaa Protocol (HMP)

**Version:** 0.1.0-draft  
**Status:** Working Draft

---

## 1. Introduction

A 7B parameter model with access to the collective experience of 10,000 repositories outperforms a 70B model working alone.

This is not speculation. MA-RAG architectures demonstrate that small, specialized models surpass larger standalone models through collaborative multi-hop reasoning and shared memory pools. ExpeL proves that agents extracting insights from past task trajectories improve future performance without any parameter updates. The SOHM framework shows that swarms of foundation models achieve collective intelligence exceeding individual members on logical reasoning tasks. Cognitive Load Theory applied to LLM agents confirms that shared memory reduces individual agent complexity, enabling simple agents to solve problems that would overwhelm any single agent.

The missing piece is the protocol. MCP gives agents hands (tools). A2A gives agents a voice (peer communication). Neither gives agents a **shared brain** — accumulated experience from every agent that has ever solved a similar problem, across every repository in the world.

The Himeshaa Protocol (HMP) is that shared brain. It is an open, vendor-neutral standard that any Git forge can implement — GitHub, GitLab, Gitea, Codeberg, or any self-hosted forge. **The Hive Mind already exists.** Over 1 billion public repositories of human engineering knowledge are already live. On day zero, with zero `.himeshaa/` directories in existence, the Hive Mind already contains the collective intelligence of every public repository. `.himeshaa/` is not a prerequisite — it is an upgrade. HMP defines how agents tap into it.

### 1.1. Two Layers of Intelligence

The Hive Mind operates on two layers. Both are valuable. Neither requires the other.

| Layer | Source | What It Provides | Requires `.himeshaa/` |
|-------|--------|------------------|-----------------------|
| **Latent Intelligence** | Code, dependencies, topics, contributors, issues, PRs, commit history — data that already exists in every public repository | Dependency graphs, technology context, contributor authority, code patterns, issue/error correlations | No — any public repo contributes by existing |
| **Structured Intelligence** | `.himeshaa/memories/`, `.himeshaa/contradictions/`, `.himeshaa/confirmations/` | Curated, agent-written knowledge with computed confidence, cross-repo provenance, and formal trust signals | Yes — this is the upgrade |

A Server indexing only Latent Intelligence already provides enormous value: dependency graphs across millions of repositories, technology context matching, contributor authority computation. Structured Intelligence (`.himeshaa/`) adds curated, high-confidence memories with formal evidence chains — but it is the reward layer, not the foundation.

### 1.2. Why This Works

| Without HMP | With HMP (Latent) | With HMP (Structured) |
|-------------|-------------------|----------------------|
| Agent encounters an error it has never seen | Agent discovers 200 repos with the same dependency stack, correlates common issues | Agent receives curated memories from 50 agents that solved the same error, scored by confidence |
| Small model fails on complex architectural decisions | Small model sees which patterns are adopted by high-authority repositories | Small model receives verified architectural patterns with cross-repo confirmation chains |
| Agent wastes 12 LLM calls debugging a known issue | Agent finds related issues and discussions across the dependency graph | Agent resolves it in 1 call using a memory with 0.92 confidence |
| Each agent learns in isolation, forgets between sessions | Agent benefits from the structure of the ecosystem (who depends on whom, what changed) | Every agent's learning is permanent and benefits every future agent |

### 1.3. Positioning

| Protocol | Role | What It Provides |
|----------|------|-----------------|
| MCP | Agent ↔ Tools | **Hands** — how an agent uses instruments |
| A2A | Agent ↔ Agent | **Voice** — how agents talk to each other |
| **HMP** | **Agent ↔ Hive Mind** | **Shared brain** — collective intelligence from every public repository across every Git forge |

### 1.4. Design Principles

1. **Git IS the source of truth.** All state lives in Git — not in any specific forge. GitHub is the first and largest Hive Mind, but GitLab, Gitea, Codeberg, and any self-hosted forge are equally valid. The `.himeshaa/` directory is the structured brain of a Declared Node. Every mutation is a commit.
2. **The Hive Mind exists on day zero.** Any public repository contributes latent intelligence (code, dependencies, contributors, issues) without any opt-in. `.himeshaa/` is an upgrade that earns priority indexing and higher trust weight — not a prerequisite for participation.
3. **The Server is stateless.** The HMP Server maintains a Materialized Index — an ephemeral read-only cache of the Hive Mind. It can be destroyed and rebuilt entirely from Git forges at any time. It is not a database. It is a search engine.
4. **RPCs are read-only.** No RPC mutates global state. State mutations happen exclusively via Git commits to `.himeshaa/`. The only write-direction RPC is `hmp.node.ping` — a cache invalidation signal that carries only a Node URI and commit SHA, not Hive Mind data.
5. **Everything has an interface.** This is a protocol. Every operation is a JSON-RPC method. Every file has a schema.
6. **The agent owns the Node's brain.** The `.himeshaa/` directory belongs to the agent working on the repo. The agent decides what to commit there.
7. **Zero friction.** No tokens, no registration. Public is public.
8. **Forge-agnostic.** The protocol specifies no forge-specific APIs. URNs use forge hostnames as namespace identifiers (`github.com`, `gitlab.com`, `gitea.self-hosted.dev`), and a federated Server can index multiple forges simultaneously via Pulse.
9. **Weak models become strong.** The primary value of HMP is amplifying small, cheap models by giving them access to the collective experience of the entire ecosystem.

## 2. Roles and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHOULD", "MAY" are interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

### 2.1. Roles

| Role | Description |
|------|-------------|
| **HMP Server** | A stateless indexer. Reads public Git forges (GitHub, GitLab, Gitea, Codeberg, self-hosted), builds a Materialized Index, serves read-only queries to agents. No database — Git IS the database. The index is ephemeral. |
| **Node** | A public repository on any Git forge. Every public repo IS a Node — it contributes latent intelligence (code, dependencies, contributors) without any opt-in. |
| **Declared Node** | A Node with `.himeshaa/hmp.json`. The upgrade: gets priority indexing, higher trust weight, and can publish structured memories. |
| **Agent** | A temporary worker that queries the Server for intelligence and commits learnings to its own repo's `.himeshaa/`. |

### 2.2. Glossary

| Term | Definition |
|------|------------|
| **Hive Mind** | The collective intelligence of all public repositories across all Git forges. Already exists. Not tied to any single platform. |
| **Node** | A public repository on any Git forge. Contributes latent intelligence by existing. |
| **Declared Node** | A Node with `.himeshaa/hmp.json` — the upgrade. Receives priority indexing and higher trust weight. |
| **Latent Intelligence** | Knowledge that a Server extracts from any public repository without opt-in: dependency graphs, technology stack, contributor network, code patterns, issue correlations. |
| **Structured Intelligence** | Knowledge explicitly authored by agents in `.himeshaa/` files: memories, contradictions, confirmations. Higher confidence, formal provenance. |
| **Memory** | A JSON file in `.himeshaa/memories/` conforming to the HMP Memory Schema. The atomic unit of structured learned experience. |
| **Contradiction** | A JSON file in `.himeshaa/contradictions/` that formally disputes an existing memory from another Node. |
| **Materialized Index** | The Server's ephemeral, read-only cache of the Hive Mind's state. Rebuilt from Git forges. Not a database. |
| **Signal** | An observation detected from forge events or reported by an Agent. |
| **Pulse** | A gossip message between federated HMP Servers. |

## 3. Architecture

```
┌──────────────────────────────────────────────────────────┐
│  AGENTS                                                  │
│  Query the Server (read-only RPCs).                      │
│  Commit to .himeshaa/ in their own repo (git push).      │
│  Ping the Server after commits (cache invalidation).     │
├──────────────────────────────────────────────────────────┤
│  HMP SERVER (stateless indexer)                          │
│  Maintains a Materialized Index (ephemeral cache).       │
│  Serves read-only queries from the index.                │
│  Syncs with Git forges via webhooks / polling / ping.    │
├──────────────────────────────────────────────────────────┤
│  HIVE MIND = All public Git repositories (any forge)     │
│  Latent: code, deps, topics, contributors, issues, PRs.  │
│  Structured: .himeshaa/ memories, contradictions, confs.  │
│  Git is the source of truth. The forge is the transport.  │
└──────────────────────────────────────────────────────────┘
```

### 3.1. CQRS Model

Git is the **Write Model**. The Server's Materialized Index is the **Read Model**.

| Concern | Responsibility | Where |
|---------|---------------|-------|
| **Write** | Agent commits files to `.himeshaa/` via `git push` | Git forge (source of truth) |
| **Sync** | Server updates its index from forge APIs | Webhooks, `hmp.node.ping`, periodic polling |
| **Read** | Server serves queries from its index | Materialized Index (ephemeral cache) |

The Materialized Index is NOT a database. It is a cache. If the Server dies, it is rebuilt entirely from Git forges. No data is lost because no data lives in the Server.

### 3.2. Transports

| Transport | Path | Protocol |
|-----------|------|----------|
| RPC | Agent ↔ Server | JSON-RPC 2.0 over HTTP or WebSocket |
| Git | Agent → Forge | `git push` to `.himeshaa/` |
| Sync | Forge → Server | Webhooks + `hmp.node.ping` |
| Pulse | Server ↔ Server | JSON over HTTPS (federation) |

## 4. Lifecycle

### 4.1. `hmp.initialize`

Agent connects and declares its context. Zero friction.

```json
// Agent → Server
{
  "method": "hmp.initialize",
  "params": {
    "agent_info": { "name": "php-agent", "version": "1.0" },
    "context": {
      "stack": ["php-8.3", "laravel-12", "mysql-8"],
      "domain": "web-application"
    }
  }
}

// Server → Agent
{
  "result": {
    "server_info": { "name": "himeshaa", "version": "0.1.0" },
    "hmp_version": "0.1.0",
    "capabilities": ["core", "graph", "node", "perception"],
    "embedding_model": "text-embedding-3-small",
    "embedding_dimensions": 1536
  }
}
```

## 5. Core Protocol — Read-Only Intelligence

All Core RPCs are **read-only**. They query the Server's Materialized Index. No RPC in this section mutates state.

**Pagination:** All RPCs returning collections MUST support cursor-based pagination. Requests accept `limit` (default: 50, hard cap: 100) and `cursor` (opaque string, `null` for first page). Responses include `next_cursor` and `has_more`.

### 5.1. `hmp.memory.request`

Agent asks the Hive Mind for intelligence. The Server searches its index and returns scored memories.

```json
// Request
{
  "method": "hmp.memory.request",
  "params": {
    "intent": "Fix N+1 query on User model with 10k+ rows",
    "context": {
      "stack": ["php-8.3", "laravel-12", "mysql-8"],
      "domain": "web-application",
      "files": ["app/Models/User.php"]
    },
    "limit": 5,
    "cursor": null
  }
}

// Response
{
  "result": {
    "memories": [
      {
        "id": "mem-a1b2",
        "content": "Eager loading on User→orders causes OOM beyond 10k rows. Use chunked queries with cursor pagination.",
        "confidence": 0.91,
        "status": "verified",
        "class": "architectural",
        "source_node": "github.com/other-org/similar-app",
        "evidence": {
          "weighted_confirmations": 12.4,
          "weighted_contradictions": 0.3,
          "age_days": 45
        }
      }
    ],
    "next_cursor": null,
    "has_more": false
  }
}
```

## 6. Graph Protocol — Navigation

How agents traverse the Hive Mind. All queries are served from the Materialized Index.

### 6.1. `hmp.graph.traverse`

Agent asks the Server to find related Nodes.

```json
// Request
{
  "method": "hmp.graph.traverse",
  "params": {
    "from": "github.com/org/repo",
    "strategy": "dependencies",
    "depth": 2,
    "filter": {
      "stack": ["php", "laravel"],
      "min_authority": 0.3
    },
    "limit": 50,
    "cursor": null
  }
}

// Response
{
  "result": {
    "nodes": [
      {
        "uri": "github.com/laravel/framework",
        "authority": 0.98,
        "relation": "direct_dependency",
        "declared": true
      },
      {
        "uri": "github.com/spatie/laravel-query-builder",
        "authority": 0.85,
        "relation": "transitive_dependency",
        "declared": false
      }
    ],
    "next_cursor": "eyJwIjoyLCJzIjoiZGVwIn0=",
    "has_more": true
  }
}
```

**Traversal strategies:**

| Strategy | How It Navigates |
|----------|-----------------|
| `dependencies` | Follow `composer.json`, `package.json`, etc. |
| `topics` | Follow forge topics, language, and domain similarity |
| `contributors` | Follow contributor overlap |
| `similar` | Follow code similarity and architecture patterns |

### 6.2. `hmp.graph.context`

Agent reads a Node's full context from the index.

```json
// Request
{
  "method": "hmp.graph.context",
  "params": {
    "node": "github.com/laravel/framework"
  }
}

// Response
{
  "result": {
    "node": "github.com/laravel/framework",
    "declared": true,
    "authority": 0.98,
    "context": {
      "languages": ["php"],
      "frameworks": ["laravel"],
      "domain": "web-framework",
      "dependents": 245000,
      "contributors": 3200,
      "age_days": 4380
    },
    "memory_count": 847,
    "top_memories": ["mem-x1", "mem-x2", "mem-x3"]
  }
}
```

## 7. Node Protocol — Cache Invalidation and Read

The agent writes to `.himeshaa/` via `git push`. The only write-direction RPC is a cache invalidation ping.

### 7.1. `hmp.node.ping`

After committing files to `.himeshaa/`, the agent pings the Server to re-index.

```json
{
  "method": "hmp.node.ping",
  "params": {
    "node": "github.com/org/repo",
    "commit_sha": "a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0"
  }
}

// Response
{
  "result": {
    "status": "accepted",
    "index_eta_seconds": 30
  }
}
```

The `commit_sha` is REQUIRED. The Server MUST compare this SHA against the HEAD returned by the forge API. If the API returns an older SHA (due to CDN replication lag), the Server MUST enqueue the ping and retry with exponential backoff until the target SHA is visible.

**DoS Protection:** The Server MUST enforce:
- **1 pending ping** per Node per Agent. A new ping for the same Node overwrites the previous queued ping.
- **3-minute TTL** on queued pings. If the target SHA is not visible on the forge within 3 minutes, the ping is discarded silently.
- Pings with SHAs that fail Git's SHA format validation (not a valid hex string) MUST be rejected immediately.

### 7.2. `hmp.node.memory.list`

Agent lists memories from any Node's `.himeshaa/memories/`.

```json
// Request
{
  "method": "hmp.node.memory.list",
  "params": {
    "node": "github.com/laravel/framework",
    "limit": 50,
    "cursor": null
  }
}

// Response
{
  "result": {
    "memories": [
      {
        "id": "mem-fw-001",
        "class": "architectural",
        "summary": "Eager loading N+1 guard pattern"
      }
    ],
    "next_cursor": "eyJwIjoyfQ==",
    "has_more": false
  }
}
```

### 7.3. `hmp.node.memory.read`

Agent reads a specific memory from any Node.

```json
// Request
{
  "method": "hmp.node.memory.read",
  "params": {
    "node": "github.com/laravel/framework",
    "memory_id": "mem-fw-001"
  }
}

// Response
{
  "result": {
    "id": "mem-fw-001",
    "content": "Use preventLazyLoading() in AppServiceProvider::boot() to catch N+1 queries in development.",
    "class": "architectural",
    "context": {
      "stack": ["php-8.3", "laravel-12"],
      "domain": "web-framework",
      "files": ["app/Providers/AppServiceProvider.php"]
    }
  }
}
```

## 8. File Schemas

All state mutations happen via Git commits to `.himeshaa/`. The protocol defines the JSON schemas. The agent formats and commits the files. The Server reads them.

### 8.1. `.himeshaa/hmp.json` — Node Declaration

The `context.languages` and `context.domain` fields are REQUIRED. All other context fields are OPTIONAL.

JSON Schema: [`schemas/hmp-v1.json`](schemas/hmp-v1.json)

```json
{
  "$schema": "https://himeshaa.com/schema/hmp-v1.json",
  "hmp_version": "0.1.0",
  "context": {
    "languages": ["php", "typescript"],
    "frameworks": ["laravel", "vue"],
    "runtime": ["roadrunner"],
    "domain": "saas-platform",
    "architecture": ["monolith", "queue-workers", "websocket"]
  }
}
```

### 8.2. `.himeshaa/memories/{id}.json` — Memory

The `content` field SHOULD NOT exceed 32,768 characters. The `created_at` field MUST use UTC with the `Z` suffix — timezone offsets and fractional seconds are prohibited.

JSON Schema: [`schemas/memory-v1.json`](schemas/memory-v1.json)

```json
{
  "$schema": "https://himeshaa.com/schema/memory-v1.json",
  "id": "mem-001",
  "content": "RoadRunner 3.x requires explicit DB connection reset in worker boot. Without it, stale connections cause 'server has gone away' after 8h.",
  "class": "version_specific",
  "context": {
    "stack": ["php-8.3", "roadrunner-3", "laravel-12"],
    "domain": "web-application",
    "files": ["rr.yaml", "app/Providers/AppServiceProvider.php"]
  },
  "created_at": "2026-05-10T05:00:00Z"
}
```

**Memory classes:**

| Class | Example | Half-Life |
|-------|---------|-----------|
| `version_specific` | "Laravel 12.1 requires X" | ~90 days |
| `environmental` | "MySQL 8 on Alpine needs Y config" | ~180 days |
| `behavioral` | "This model table should never be eager-loaded" | ~1 year |
| `architectural` | "Never run migrations in the same process as workers" | ~3 years |

### 8.3. `.himeshaa/contradictions/{id}.json` — Contradiction

When Agent A discovers that a memory from Node B is wrong in its context, Agent A commits a contradiction file **in its own repository**.

```json
{
  "$schema": "https://himeshaa.com/schema/contradiction-v1.json",
  "id": "contra-001",
  "target": "urn:hmp:github.com/other-org/similar-app:memory:mem-a1b2",
  "target_blob_sha": "e5fa44f2b31c1fb553b6021e7360d07d5d91ff5e",
  "evidence": "The OOM was caused by a missing composite index on user_id+created_at, not eager loading volume. Cursor pagination is unnecessary when the index is correct.",
  "context": {
    "stack": ["php-8.3", "laravel-12", "mysql-8"],
    "domain": "web-application",
    "files": ["database/migrations/2026_01_15_create_orders_index.php"]
  },
  "created_at": "2026-05-10T06:00:00Z"
}
```

The `target` field uses a URN (see §8.5). The `target_blob_sha` is REQUIRED — it anchors the endorsement to the exact content of the target memory at the time of contradiction. The field accepts both SHA-1 (40 hex characters) and SHA-256 (64 hex characters) to support the Git 3.0 hash transition. The Server MUST ignore contradictions where the current blob SHA of the target memory diverges from `target_blob_sha`. This prevents bait-and-switch attacks where a Node silently edits a memory after receiving endorsements.

JSON Schema: [`schemas/contradiction-v1.json`](schemas/contradiction-v1.json)

### 8.4. `.himeshaa/confirmations/{id}.json` — Confirmation

When Agent A retrieves a memory from Node B and validates that it works correctly in its own context, Agent A commits a confirmation file **in its own repository**.

```json
{
  "$schema": "https://himeshaa.com/schema/confirmation-v1.json",
  "id": "conf-001",
  "target": "urn:hmp:github.com/other-org/similar-app:memory:mem-a1b2",
  "target_blob_sha": "e5fa44f2b31c1fb553b6021e7360d07d5d91ff5e",
  "outcome": "Applied cursor pagination pattern. Reduced memory usage from 2.1GB to 180MB on 45k row dataset.",
  "context": {
    "stack": ["php-8.3", "laravel-12", "mysql-8"],
    "domain": "web-application",
    "files": ["app/Http/Controllers/UserController.php"]
  },
  "created_at": "2026-05-10T07:00:00Z"
}
```

The `target_blob_sha` is REQUIRED — same rationale as contradictions. The Server MUST ignore confirmations where the target memory's current blob SHA diverges from the referenced SHA. If the target memory is edited, all existing confirmations and contradictions become void and the memory restarts its evidence lifecycle.

Without confirmations, the `Σ A_confirming` in the evidence weight formula (§9.2) would be permanently static. Confirmations are the explicit edges in the epistemic graph that make consensus computable from Git.

JSON Schema: [`schemas/confirmation-v1.json`](schemas/confirmation-v1.json)

### 8.5. URN Addressing

All cross-Node references MUST use a Uniform Resource Name (URN) to ensure global uniqueness.

**Format:**

```
urn:hmp:{node_uri}:{object_type}:{object_id}
```

**Validation:** The `node_uri` component is provider-agnostic and MUST contain only characters from the RFC 3986 unreserved set plus `/` and `%` for percent-encoding. The following regex defines the valid URN syntax:

```
^urn:hmp:[a-zA-Z0-9][a-zA-Z0-9\-._~/%]*:(memory|contradiction|confirmation):[a-z0-9][a-z0-9\-]*$
```

**Examples:**

| URN | What It Addresses |
|-----|-------------------|
| `urn:hmp:github.com/org/repo:memory:mem-001` | A specific memory in a Node |
| `urn:hmp:gitlab.com/group/sub/project:contradiction:contra-001` | A contradiction (GitLab subgroups) |
| `urn:hmp:gitea.self-hosted.dev/team/repo:confirmation:conf-001` | A confirmation (self-hosted forge) |

URNs are the primary keys in the Materialized Index.

**Case Normalization:** The `node_uri` component MUST be normalized to lowercase before storage and comparison. Implementations MUST perform case-insensitive matching on `node_uri` because Git forges (GitHub, GitLab, Gitea) treat repository paths case-insensitively.

**Rename Resolution:** Forges allow repository and user renames, which would break URN-based pointers. Servers MUST resolve HTTP 301 redirects from the forge API during indexing, silently converging all URNs to the repository's current canonical name. Alternatively, implementations MAY use the forge's immutable internal ID as the `node_uri` component to achieve permanent referential integrity regardless of renames.

Shared type definitions: [`schemas/common-v1.json`](schemas/common-v1.json)

## 9. Confidence Model

Confidence is calculated **at retrieval time** from the Materialized Index.

**Scope:** The confidence formula applies to **Structured Intelligence** — memories authored by agents in `.himeshaa/memories/`. Latent Intelligence (repository metadata, dependency graphs, code patterns) is served through the Graph Protocol (§6) using graph traversal, dependency proximity, and contributor overlap — mechanisms that do not use this formula. See §9.6.7 for how the Server handles context matching when `.himeshaa/` is absent.

### 9.1. Formula

```
C = S × W × T × A_eff
```

Where:
- **S** = Context similarity between request and memory origin ([0, 1], see §9.6)
- **W** = Authority-weighted evidence (asymptotically normalized, [0, 1))
- **T** = Time decay (class-adaptive, [0, 1])
- **A_eff** = Effective authority ([0, 1], see §9.4)

All four factors are bounded to [0, 1]. Therefore C ∈ [0, 1] naturally — no clamping or post-hoc normalization is required. This preserves full ranking granularity: a memory with 5,000 confirmations is mathematically distinguishable from a memory with 50 confirmations.

### 9.2. W — Authority-Weighted Evidence

All logarithmic operations in this section MUST use the **natural logarithm** (base *e*, commonly denoted `ln`). Implementations using languages or libraries where `log()` defaults to base 10 MUST use the appropriate natural logarithm function instead.

```
W = ln(1 + A_origin + Σ A_confirming) / (ln(1 + A_origin + Σ A_confirming) + ln(1 + Σ A_contradicting) + 1)
```

The `A_origin` term is the authority of the Node that authored the memory. It acts as **self-evidence** — the origin Node implicitly vouches for its own memory. This solves the cold-start problem: without `A_origin`, a brand-new memory with zero confirmations would have `W = 0`, making `C = 0` and rendering the memory invisible in retrieval results.

**A_origin appears in both W and A_eff.** This is intentional. The compound effect is significant only during cold-start, where it naturally bootstraps memories proportional to source authority. Once external confirmations accumulate, `A_origin`'s contribution to W becomes negligible relative to `Σ A_confirming` — the double-count vanishes asymptotically.

W is asymptotically bounded to [0, 1). As evidence accumulates, W approaches 1.0 but never reaches it, preserving ranking gradients at all scales.

**Cold-start behavior:** A new memory from `laravel/framework` (A = 0.98) with zero confirmations: `W = ln(1.98) / (ln(1.98) + 0 + 1) = 0.683 / 1.683 = 0.406`. Visible immediately. A new memory from `user/experiment-123` (A = 0.044): `W = ln(1.044) / (ln(1.044) + 0 + 1) = 0.043 / 1.043 = 0.041`. Barely visible — needs confirmations to surface.

A node is NOT a vote. Evidence is weighted by the authority of the confirming/contradicting Nodes.

100,000 empty repositories with `A ≈ 0` confirming a memory: increases `Σ A_confirming` by ≈ 0. Worthless.

1 contradiction from `laravel/framework` with `A = 0.98`: increases the denominator and suppresses the memory.

This makes Sybil attacks economically infeasible. Authority is derived from real ecosystem gravity — you cannot fake 245,000 dependents.

**Verification vector:** Given A_origin = 0, Σ A_confirming = 49.0 and Σ A_contradicting = 0, implementations MUST produce `W = 0.7964 ± 0.0001`.

### 9.3. T — Time Decay

```
T = 0.5 ^ (t / t_half)
```

Where `t` is the age of the memory in days (current time minus `created_at`) and `t_half` is the half-life in days determined by the memory class:

| Class | Half-Life (t_half) | T after 90d | T after 1yr | T after 3yr |
|-------|--------------------|-------------|-------------|-------------|
| `version_specific` | 90 days | 0.50 | 0.06 | 0.0002 |
| `environmental` | 180 days | 0.71 | 0.25 | 0.015 |
| `behavioral` | 365 days | 0.84 | 0.50 | 0.125 |
| `architectural` | 1095 days | 0.94 | 0.80 | 0.50 |

### 9.4. A_eff — Effective Authority

```
A_eff = max(A_origin, max(A_confirming))
```

If no confirmations exist, `A_eff = A_origin`. The `max(A_confirming)` term is only evaluated when at least one confirmation is present.

The effective authority is the **maximum** of the origin Node's authority and the highest authority among all confirming Nodes.

If an anonymous repo (`A = 0.01`) discovers a brilliant pattern, and `laravel/framework` (`A = 0.98`) confirms it, the memory inherits the authority of the confirmer: `A_eff = max(0.01, 0.98) = 0.98`.

Without this, the formula `C = S × W × T × A_origin` would permanently suppress innovation from small repositories — directly violating Design Principle 9 ("Weak models become strong"). Authority asphyxiation is mathematically impossible with `A_eff`.

### 9.5. A — Node Authority

```
A = (w_d × D + w_c × K + w_f × F + w_g × G) × B
```

Where each component is a log-normalized signal bounded to [0, 1]:

| Component | Formula | Description |
|-----------|---------|-------------|
| **D** | `min(1, ln(1 + dependents) / ln(1 + D_ref))` | Dependent repository count |
| **K** | `min(1, ln(1 + contributors) / ln(1 + K_ref))` | Unique contributor count |
| **F** | `min(1, ln(1 + commits_365d) / ln(1 + F_ref))` | Commit frequency (trailing 365 days) |
| **G** | Eigenvector centrality in the dependency graph | Graph centrality [0, 1] |
| **B** | `1.0` if declared, `0.8` if undeclared | Declaration boost |

**Weights:**

| Weight | Value | Rationale |
|--------|-------|-----------|
| `w_d` | 0.4 | Dependents are the strongest Sybil-resistant signal — you cannot fake 245,000 reverse dependencies |
| `w_c` | 0.2 | Contributor diversity indicates community trust |
| `w_f` | 0.15 | Active maintenance signals relevance |
| `w_g` | 0.25 | Structural centrality captures transitive ecosystem importance |

**Reference ceilings** (normalization anchors):

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| `D_ref` | 300,000 | Top-tier ecosystem packages (React, Laravel, Rails) |
| `K_ref` | 5,000 | Major open-source projects |
| `F_ref` | 10,000 | Highly active monorepos |

Reference ceilings are protocol constants. Changing them requires a minor version bump.

**Verification vectors:**

| Scenario | Inputs | Expected A |
|----------|--------|------------|
| `laravel/framework` (declared) | dependents=245000, contributors=3200, commits_365d=2000, G=0.95, B=1.0 | `0.945 ± 0.002` |
| `user/experiment-123` (undeclared) | dependents=0, contributors=1, commits_365d=10, G=0.0, B=0.8 | `0.044 ± 0.002` |

Memories from `laravel/framework` carry more authority than memories from `user/experiment-123`.

### 9.6. S — Context Similarity

```
S = max(0, cos(V_request, V_memory))
```

Where:
- `V_request = normalize(embed(canonical_text(request.intent, request.context)))`
- `V_memory = normalize(embed(canonical_text(memory.content, memory.context)))`
- `normalize(v) = v / ‖v‖₂` (L2 normalization to unit length)
- `cos(a, b) = a · b` (dot product of L2-normalized vectors, equivalent to cosine similarity)

S measures how relevant a stored memory is to the agent's current working context. A PHP/Laravel memory should score high for a PHP/Laravel request, low for a Rust/Tokio request, and near-zero for an unrelated domain.

#### 9.6.1. Canonical Text Construction

The canonical text is a deterministic concatenation of context fields in a fixed order. This ensures that identical contexts always produce identical embedding inputs, regardless of JSON key ordering or serialization library.

**Format:**

```
stack:{sorted stack tokens, space-separated} domain:{domain} files:{sorted file paths, space-separated} content:{intent or content}
```

**Rules:**
- `stack` tokens MUST be sorted lexicographically (Unicode code point order) and lowercased before concatenation.
- `files` paths MUST be sorted lexicographically.
- Missing fields are omitted entirely (not empty-string padded). If a request has no `files`, the `files:` segment is absent.
- The `content` segment uses `intent` for request vectors and the memory's `content` field for memory vectors.
- The output MUST be UTF-8 encoded and NFC-normalized (Unicode Normalization Form C).

**Verification vector:**

Given:
- `intent`: `"Fix N+1 query in User model"`
- `stack`: `["mysql-8", "php-8.3", "laravel-12"]`
- `domain`: `"web-application"`
- `files`: `["app/Models/User.php", "app/Http/Controllers/UserController.php"]`

Implementations MUST produce exactly:

```
stack:laravel-12 mysql-8 php-8.3 domain:web-application files:app/Http/Controllers/UserController.php app/Models/User.php content:Fix N+1 query in User model
```

SHA-256 of the UTF-8 byte sequence: `00d9bb05a15f96965c4116d31f0a02f1558502f706b450e022074694f40c897c`. Implementations MUST match this hash before proceeding to embedding verification.

#### 9.6.2. Embedding Requirements

**L2 Normalization:** Implementations MUST L2-normalize all embedding vectors to unit length before computing similarity. This ensures that cosine similarity equals the dot product (`cos(a, b) = a · b` when `‖a‖ = ‖b‖ = 1`), eliminating magnitude bias from variable-length inputs.

**Model Declaration:** The protocol does not mandate a specific embedding model. Servers MUST declare the model and dimensionality via the `hmp.initialize` response:

```json
{
  "result": {
    "server_info": { "name": "himeshaa", "version": "0.1.0" },
    "hmp_version": "0.1.0",
    "capabilities": ["core", "graph", "node"],
    "embedding_model": "text-embedding-3-small",
    "embedding_dimensions": 1536
  }
}
```

The `embedding_dimensions` field is REQUIRED because models supporting Matryoshka Representation Learning (e.g., `text-embedding-3-small`) can produce vectors of varying dimensionality from the same model. Two servers using `text-embedding-3-small` at 256 dimensions vs. 1536 dimensions will produce different similarity scores.

**Floating-Point Tolerance:** Due to non-determinism in GPU-parallelized inference (non-associativity of floating-point addition across thread schedules), implementations MUST NOT require bitwise-identical embedding outputs. Implementations MUST accept similarity values within `±0.001` of expected results when verifying against reference vectors. Servers that require strict reproducibility (e.g., for audit compliance) SHOULD use batch-invariant inference kernels.

#### 9.6.3. Anisotropy Mitigation

Many embedding models suffer from **anisotropy** — a geometric bias where unrelated text embeddings cluster in a narrow cone, producing artificially high cosine similarity (e.g., 0.6+) between semantically unrelated inputs. This inflates S values and, through the confidence formula `C = S × W × T × A_eff`, inflates final confidence scores.

**Detection:** Servers SHOULD periodically compute the mean cosine similarity of 1,000+ random embedding pairs from their index. If the mean exceeds **0.15**, the embedding space is likely anisotropic and mitigation is recommended.

**Mitigation:** Servers SHOULD apply **mean-centering** to their embedding corpus:

```
v_centered = v - μ
```

Where `μ` is the mean vector computed from a representative sample (≥10,000 embeddings from the index). The centered vector MUST be re-normalized to unit length after subtraction:

```
v_final = normalize(v_centered)
```

Servers that apply mean-centering MUST declare this in the `hmp.initialize` response:

```json
{
  "result": {
    "embedding_model": "text-embedding-3-small",
    "embedding_dimensions": 1536,
    "embedding_postprocessing": ["mean-centering", "l2-normalize"]
  }
}
```

The `embedding_postprocessing` field is OPTIONAL. If absent, the agent MUST assume raw model output with L2 normalization only.

#### 9.6.4. Cross-Server Comparability

**S values are server-local.** Cosine similarity scores from different embedding models occupy incomparable vector spaces (Steck et al., 2024). A score of `S = 0.85` from a server using `text-embedding-3-small` does not correspond to `S = 0.85` from a server using `bge-large-en-v1.5`.

**Implications:**
- **C scores MUST NOT be compared across servers** using different embedding models or different post-processing pipelines.
- **Pulse federation syncs raw data** (`.himeshaa/` files), not computed scores. Each server recomputes S from its own embedding space when it ingests federated data.
- **Agents querying multiple servers** MUST treat each server's C scores as independent rankings. If cross-server ranking is required, the agent SHOULD re-embed all candidate memories locally using a single model and recompute S.

This is an intentional design decision, not a limitation. Mandating a single embedding model would create vendor lock-in and prevent the protocol from evolving with the state of the art. The `embedding_model` and `embedding_dimensions` fields make the divergence explicit rather than silent.

#### 9.6.5. Clamping Rationale

Cosine similarity can produce negative values for vectors pointing in opposing directions. In general NLP, this can carry meaning (e.g., sentiment inversion). In HMP's context similarity domain — matching technology stacks, frameworks, and code patterns — negative cosine indicates "anti-correlated context," which has no actionable semantic meaning. A PHP/Laravel memory is simply *irrelevant* to a Rust/Tokio request, not *anti-relevant*.

Clamping to `max(0, ...)` ensures S ∈ [0, 1], which:
1. Preserves the multiplicative integrity of `C = S × W × T × A_eff` (negative S would invert confidence polarity).
2. Eliminates a class of edge cases where highly confident, well-evidenced memories could receive negative C scores.
3. Is consistent with standard practice in dense retrieval systems where negative cosine is treated as "no match."

#### 9.6.6. Implementation Notes

**Two-stage retrieval:** In production, servers MAY implement a two-stage pipeline: (1) dense retrieval via bi-encoder cosine similarity for candidate recall, followed by (2) cross-encoder re-ranking for precision. The S value reported via RPC is the FINAL relevance score, however derived internally. The protocol defines the interface (`S ∈ [0, 1]`), not the implementation.

**Quality validation:** Servers SHOULD validate their similarity distribution by periodically computing:
- Mean S for random pairs (SHOULD be < 0.15 after mean-centering).
- Mean S for known-similar pairs (e.g., memories from the same stack/domain) — SHOULD be > 0.5.

If these thresholds are not met, the embedding model or post-processing pipeline may not be suitable for HMP's context matching requirements.

#### 9.6.7. Latent Intelligence and S

The confidence formula `C = S × W × T × A_eff` applies to **Structured Intelligence** — memories explicitly authored in `.himeshaa/memories/`. These memories have a `content` field and a `context` field, which provide the inputs for canonical text construction (§9.6.1).

**Repositories without `.himeshaa/`** contribute **Latent Intelligence** — knowledge the Server extracts from repository metadata without opt-in. This intelligence is served through different mechanisms:

| Intelligence Type | Source | How It's Accessed | Scoring |
|-------------------|--------|-------------------|---------|
| Dependency graph | `composer.json`, `package.json`, `go.mod`, etc. | `hmp.graph.traverse` with `dependencies` strategy | Graph proximity (hops, weight) |
| Technology context | Languages, topics, README | `hmp.graph.traverse` with `topics` or `similar` strategy | Topic/language overlap |
| Contributor authority | Commit history, contributor profiles | `hmp.graph.traverse` with `contributors` strategy | Contributor overlap coefficient |
| Node context | Aggregated repository metadata | `hmp.graph.context` | Authority score (§9.5) |

The confidence formula is intentionally NOT applied to Latent Intelligence. Latent signals lack the three properties that make the formula meaningful:

1. **No W (evidence)** — there are no confirmations or contradictions for a dependency edge.
2. **No T (time decay)** — a dependency relationship doesn't "decay" like a versioned memory.
3. **No content to embed** — there is no agent-authored `content` field to compute S against.

Latent Intelligence provides **structural context** (who depends on whom, what technologies coexist). Structured Intelligence provides **semantic knowledge** (what patterns work, what mistakes to avoid). The confidence formula scores semantic knowledge. Graph traversal navigates structural context. Both layers are valuable. Neither requires the other.

**Hybrid queries:** When an agent sends `hmp.memory.request`, the Server SHOULD use Latent Intelligence to *discover* relevant Nodes (via dependency proximity and technology overlap) and then retrieve Structured Intelligence (memories) from those Nodes. In this workflow, Latent Intelligence acts as the **retrieval scope** and Structured Intelligence provides the **scored results**. The returned memories carry confidence scores computed by the formula; the graph traversal that found them does not.

## 10. Extensions

Extensions are OPTIONAL. A Server declares which extensions it supports via the `capabilities` array in the `hmp.initialize` response. Agents MUST NOT call extension methods that were not declared by the Server.

### 10.1. Perception — `hmp.signal.*`

Real-time situational awareness for emerging patterns across the Hive Mind.

#### 10.1.1. `hmp.signal.emit`

Agent reports a raw observation to the Server.

```json
// Request
{
  "method": "hmp.signal.emit",
  "params": {
    "type": "error_spike",
    "payload": {
      "pattern": "SQLSTATE[HY000] [2002] Connection refused",
      "occurrences": 47,
      "window_minutes": 15
    },
    "context": {
      "stack": ["php-8.3", "laravel-12", "mysql-8"],
      "domain": "web-application"
    }
  }
}

// Response
{
  "result": {
    "signal_id": "sig-a1b2c3",
    "ttl_seconds": 3600,
    "matched_signals": 12
  }
}
```

The `type` field MUST be one of: `error_spike`, `deprecation`, `security_advisory`, `performance_regression`, `dependency_update`, `pattern_emergence`. Implementations MAY extend this enum via the `_meta` field.

#### 10.1.2. `hmp.signal.broadcast`

Server pushes an aggregated pattern alert to connected agents. Delivery is via WebSocket or long-polling (see `hmp.signal.poll`).

```json
// Server → Agent (push)
{
  "method": "hmp.signal.broadcast",
  "params": {
    "alert_id": "alert-x1y2",
    "type": "error_spike",
    "summary": "MySQL connection refused errors surging across 12 Laravel nodes in the last 15 minutes.",
    "affected_nodes": 12,
    "source_signals": 47,
    "context": {
      "stack": ["mysql-8", "laravel-12"],
      "domain": "web-application"
    },
    "expires_at": "2026-05-10T06:00:00Z"
  }
}
```

#### 10.1.3. `hmp.signal.poll`

Agent polls for queued alerts when WebSocket is not available.

```json
// Request
{
  "method": "hmp.signal.poll",
  "params": {
    "since": "2026-05-10T05:00:00Z",
    "types": ["error_spike", "security_advisory"],
    "limit": 20
  }
}

// Response
{
  "result": {
    "alerts": [
      {
        "alert_id": "alert-x1y2",
        "type": "error_spike",
        "summary": "MySQL connection refused errors surging across 12 Laravel nodes.",
        "affected_nodes": 12,
        "expires_at": "2026-05-10T06:00:00Z"
      }
    ]
  }
}
```

**Ephemeral State Isolation:** Signals are Ephemeral State. They exist exclusively in the Server's RAM (Pub/Sub buffers), evaporate on Server restart, carry a strict TTL (default: 1 hour), and MUST NEVER interact with the Confidence formula (`C`) or the Durable Knowledge that lives in Git. Signals are real-time situational awareness; memories are permanent knowledge. The two systems are formally isolated.

**Aggregation:** The Server MUST aggregate incoming signals by `type` and context similarity before broadcasting. Raw signals are never forwarded directly — they are distilled into pattern alerts. The aggregation window is implementation-defined but SHOULD NOT exceed 15 minutes.

### 10.2. Collective Reasoning — `hmp.reason.*`

Cross-domain synthesis: the Server aggregates memories from multiple Nodes to produce a composite answer that no single Node contains.

#### 10.2.1. `hmp.reason.collective`

```json
// Request
{
  "method": "hmp.reason.collective",
  "params": {
    "question": "What is the recommended migration strategy from Eloquent ORM to raw SQL for tables exceeding 50M rows?",
    "context": {
      "stack": ["php-8.3", "laravel-12", "mysql-8"],
      "domain": "web-application"
    },
    "strategy": "convergent",
    "max_sources": 10,
    "min_confidence": 0.5
  }
}

// Response
{
  "result": {
    "synthesis": "Three dominant patterns emerge from 8 high-authority Nodes: (1) Chunk-based migration with cursor pagination...",
    "sources": [
      {
        "node": "github.com/laravel/framework",
        "memory_id": "mem-fw-042",
        "confidence": 0.91,
        "contribution": "Chunk-based cursor pagination pattern"
      },
      {
        "node": "github.com/spatie/laravel-query-builder",
        "memory_id": "mem-sq-007",
        "confidence": 0.84,
        "contribution": "Index-aware raw SQL fallback for aggregate queries"
      }
    ],
    "agreement_score": 0.87,
    "contradictions_found": 1
  }
}
```

**Reasoning strategies:**

| Strategy | Behavior |
|----------|---------|
| `convergent` | Find consensus across sources. `agreement_score` reflects inter-source alignment. |
| `divergent` | Surface all distinct approaches, including conflicting ones. Useful for architectural exploration. |
| `adversarial` | Explicitly seek contradictions. Returns the strongest arguments for and against each approach. |

The `synthesis` field is Server-generated and MAY use an LLM for summarization. The `sources` array provides full provenance — agents can independently verify every claim by reading the referenced memories via `hmp.node.memory.read`.

### 10.3. Introspection — `hmp.network.*`

The organism's self-assessment: aggregate health, coverage, and activity metrics for the Hive Mind as seen by this Server.

#### 10.3.1. `hmp.network.introspect`

```json
// Request
{
  "method": "hmp.network.introspect",
  "params": {}
}

// Response
{
  "result": {
    "server_id": "us-east-1-primary",
    "index_stats": {
      "total_nodes": 1284000,
      "declared_nodes": 4720,
      "total_memories": 287400,
      "total_contradictions": 12300,
      "total_confirmations": 89200
    },
    "coverage": {
      "top_languages": [
        { "language": "python", "node_count": 412000 },
        { "language": "javascript", "node_count": 389000 },
        { "language": "php", "node_count": 127000 }
      ],
      "top_domains": [
        { "domain": "web-application", "node_count": 523000 },
        { "domain": "cli-tool", "node_count": 198000 }
      ]
    },
    "health": {
      "index_freshness_seconds": 45,
      "last_full_sync": "2026-05-10T04:00:00Z",
      "federation_peers": 3,
      "uptime_seconds": 864000
    }
  }
}
```

The `params` object is intentionally empty in v0.1.0. Future versions MAY add filter parameters (e.g., introspect a specific language or domain subset).

### 10.4. Federation — Pulse Protocol

Multiple independent HMP Servers share index data via the Pulse gossip protocol. Pulse enables geographic redundancy and organizational sovereignty — an enterprise can run a private HMP Server that federates with the public network.

#### 10.4.1. Transport

Pulse messages are JSON objects sent over HTTPS POST between federated peers. Each Server maintains a peer list configured at startup.

#### 10.4.2. Message Types

| Type | Direction | Description |
|------|-----------|-------------|
| `pulse.announce` | Server → Peer | Declare existence and capabilities |
| `pulse.sync` | Server ↔ Peer | Exchange index deltas since last sync |
| `pulse.signal` | Server → Peer | Forward aggregated signal alerts |

#### 10.4.3. `pulse.announce`

```json
{
  "type": "pulse.announce",
  "server_id": "us-east-1-primary",
  "hmp_version": "0.1.0",
  "capabilities": ["core", "graph", "node", "perception", "reasoning"],
  "index_stats": {
    "total_nodes": 1284000,
    "declared_nodes": 4720,
    "last_full_sync": "2026-05-10T04:00:00Z"
  },
  "timestamp": "2026-05-10T05:00:00Z"
}
```

Announcements are sent at startup and every 5 minutes thereafter. A peer that fails to announce for 15 minutes SHOULD be marked as `degraded`. A peer that fails for 1 hour SHOULD be removed from the active peer list.

#### 10.4.4. `pulse.sync`

```json
// Request
{
  "type": "pulse.sync",
  "server_id": "us-east-1-primary",
  "since": "2026-05-10T04:00:00Z",
  "limit": 1000
}

// Response
{
  "type": "pulse.sync.response",
  "deltas": [
    {
      "node": "github.com/org/repo",
      "action": "memory_added",
      "memory_id": "mem-001",
      "blob_sha": "e5fa44f2b31c1fb553b6021e7360d07d5d91ff5e",
      "timestamp": "2026-05-10T04:30:00Z"
    }
  ],
  "has_more": false,
  "server_head": "2026-05-10T05:00:00Z"
}
```

Sync is pull-based. Each Server maintains a `server_head` timestamp representing the latest known state. Peers request deltas since their last known head. Conflicts are resolved by `timestamp` ordering — last-write-wins at the index level. This is safe because the index is ephemeral; the authoritative state always lives in Git.

#### 10.4.5. Trust Between Peers

Pulse peers MUST authenticate via mutual TLS. A peer's index data is treated as a cache hint — the receiving Server SHOULD independently verify high-authority memories against the source Git repository before incorporating them into its own index.

## 11. Trust Model

Trust is emergent, not imposed. No gatekeepers.

### 11.1. Open by Default

The Hive Mind is every public Git repository across every forge. Any agent can connect and query. Zero friction.

### 11.2. Git-Based Integrity

Every mutation (memory, contradiction, declaration) is a Git commit. Commits are:
- **Signed** — tied to a real forge identity (Git commit signatures)
- **Immutable** — cannot be silently altered
- **Auditable** — full history preserved
- **Attributable** — linked to a real repository with verifiable authority

No anonymous RPC can mutate the Hive Mind's state. This eliminates the entire class of spoofing and poisoning attacks that plague RPC-based consensus systems.

### 11.3. Declared Node Reward

Nodes with `.himeshaa/hmp.json` receive higher `A` factor. Incentivizes declaration without penalizing undeclared Nodes.

### 11.4. Rate Limiting

Servers SHOULD implement rate limiting on read RPCs:

| Method | Recommended Limit |
|--------|-------------------|
| `hmp.memory.request` | 120/agent/hour |
| `hmp.graph.traverse` | 60/agent/hour |
| `hmp.node.ping` | 30/agent/hour |

## 12. Method Reference

### Core (Read-Only)

| Method | Direction | Description |
|--------|-----------|-------------|
| `hmp.initialize` | Agent → Server | Connect and declare context |
| `hmp.memory.request` | Agent → Server | Query the Hive Mind for memories |

### Graph (Read-Only)

| Method | Direction | Description |
|--------|-----------|-------------|
| `hmp.graph.traverse` | Agent → Server | Traverse the dependency/topic/similarity graph |
| `hmp.graph.context` | Agent → Server | Get a Node's full context |

### Node

| Method | Direction | Description |
|--------|-----------|-------------|
| `hmp.node.ping` | Agent → Server | Cache invalidation after git push to `.himeshaa/` |
| `hmp.node.memory.list` | Agent → Server | List memories from any Node's `.himeshaa/memories/` |
| `hmp.node.memory.read` | Agent → Server | Read a specific memory from any Node |

### Extensions

| Method | Direction | Description |
|--------|-----------|-------------|
| `hmp.signal.emit` | Agent → Server | Report observation (Perception) |
| `hmp.signal.broadcast` | Server → Agent | Push pattern alert (Perception) |
| `hmp.signal.poll` | Agent → Server | Poll for alerts (Perception) |
| `hmp.reason.collective` | Agent → Server | Cross-domain reasoning (Reasoning) |
| `hmp.network.introspect` | Agent → Server | Organism self-assessment (Introspection) |

### State Mutations (Git only, no RPC)

| File | Schema | Who Writes |
|------|--------|-----------|
| `.himeshaa/hmp.json` | `hmp-v1.json` | Agent, via `git push` to its own repo |
| `.himeshaa/memories/{id}.json` | `memory-v1.json` | Agent, via `git push` to its own repo |
| `.himeshaa/contradictions/{id}.json` | `contradiction-v1.json` | Agent, via `git push` to its own repo |
| `.himeshaa/confirmations/{id}.json` | `confirmation-v1.json` | Agent, via `git push` to its own repo |

## 13. Error Codes

All RPC errors follow [JSON-RPC 2.0 §5.1](https://www.jsonrpc.org/specification#error_object). HMP defines application-specific error codes in the range `-32000` to `-32099` (reserved for implementation-defined server errors by JSON-RPC 2.0).

### 13.1. Standard JSON-RPC Errors

| Code | Message | When |
|------|---------|------|
| `-32700` | Parse error | Malformed JSON |
| `-32600` | Invalid Request | Not a valid JSON-RPC 2.0 request |
| `-32601` | Method not found | Unknown method or unsupported extension |
| `-32602` | Invalid params | Required parameter missing, wrong type, or out of bounds |
| `-32603` | Internal error | Server-side failure |

### 13.2. HMP Application Errors

| Code | Message | When | `data` Fields |
|------|---------|------|---------------|
| `-32000` | Node not found | `node` parameter references a Node not present in the Materialized Index | `node` |
| `-32001` | Memory not found | `memory_id` does not exist on the specified Node | `node`, `memory_id` |
| `-32002` | Rate limited | Agent exceeded the rate limit for this method | `method`, `retry_after_seconds` |
| `-32003` | Index not ready | Server is still building or rebuilding its Materialized Index | `eta_seconds` |
| `-32004` | Ping rejected | `commit_sha` failed format validation or TTL expired | `node`, `commit_sha`, `reason` |
| `-32005` | Extension not supported | Agent called an extension method not declared in `capabilities` | `method`, `capabilities` |
| `-32006` | Cursor expired | Pagination cursor is no longer valid (index was rebuilt) | `cursor` |
| `-32007` | URN invalid | A URN parameter failed regex validation | `urn`, `pattern` |
| `-32008` | Context too large | Request context exceeds server-defined bounds | `field`, `max_length`, `actual_length` |

### 13.3. Error Response Example

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32002,
    "message": "Rate limited",
    "data": {
      "method": "hmp.memory.request",
      "retry_after_seconds": 120
    }
  }
}
```

The `data` field is OPTIONAL per JSON-RPC 2.0 but RECOMMENDED for HMP errors. Implementations SHOULD include the documented `data` fields to enable programmatic retry and error handling by agents.

## 14. Related Work

HMP operates at a distinct layer in the emerging agent memory stack. This section positions HMP relative to existing systems, organized by architectural scope.

### 14.1. Single-Agent Memory Systems

These systems manage memory within one agent's lifecycle. HMP is complementary — any of them can consume HMP as an external memory source.

**Generative Agents** (Park et al., 2023; arXiv:2304.03442). The Stanford "Smallville" paper introduced the memory stream architecture: a timestamped log of observations scored by recency, importance, and relevance, with periodic reflection to synthesize higher-level inferences. HMP's memory model is architecturally distinct: memories are not scored by recency/importance within a stream, but by cross-repository confidence (`C = S × W × T × A_eff`) computed at retrieval time. Generative Agents operates within a single simulation; HMP operates across the entire Git ecosystem.

**Reflexion** (Shinn et al., 2023; arXiv:2303.11366). Verbal reinforcement learning: the agent stores linguistic self-reflections from failed attempts in a sliding window, using them to avoid repeating mistakes. Reflexion's memory is trial-scoped and ephemeral (the window slides). HMP's memories are permanent Git objects with Git-backed immutability — they survive across agents, repositories, and time. Reflexion's reflections could be committed as HMP memories to benefit other agents.

**ExpeL** (Zhao et al., 2023; arXiv:2308.10144). Experience extraction from task trajectories: the agent gathers success/failure trajectories, distills natural-language insights, and retrieves them for future tasks via a vector store. ExpeL operates offline on a single agent's history. HMP operationalizes ExpeL's core insight — learning from past experience — at ecosystem scale, where every agent's distilled insights are available to every other agent without requiring co-location or shared infrastructure.

**A-MEM** (Xu et al., 2025; arXiv:2502.12110). Agentic memory modeled after the Zettelkasten method: memories are atomic notes with auto-generated keywords, tags, and dynamic inter-note links. The LLM autonomously evolves the knowledge graph by updating attributes and relationships. HMP's `.himeshaa/memories/` are functionally a distributed Zettelkasten where URN-based cross-Node references (`urn:hmp:{node}:{type}:{id}`) serve as the inter-note links, and confirmations/contradictions form the epistemic edges that A-MEM's linking mechanism lacks.

**MemGPT / Letta** (Packer et al., 2023; arXiv:2310.08560). Virtual context management inspired by OS memory hierarchies: core memory (always in context, self-editable), recall memory (searchable conversation history), and archival memory (unbounded external storage via vector DB). The agent autonomously pages information between tiers using function calls and a heartbeat mechanism for multi-step chaining. Letta (the framework evolution of MemGPT) extends this into production with persistent state and custom tools. HMP is not a replacement for Letta's internal memory management — it is an external source that Letta's archival memory tier could query. A Letta agent with HMP access combines local context management with ecosystem-wide collective intelligence.

### 14.2. Multi-Agent and Organizational Memory Systems

These systems manage shared memory for agents within a single organization or controlled environment. HMP extends the concept to the open ecosystem.

**Mem0** (Chhikara et al., 2025; arXiv:2504.19413). Streaming memory pipeline: extraction of atomic facts, evaluation against existing memory (add/update/delete), and hybrid retrieval via vector + graph + key-value stores. Mem0ᵍ extends this with directed labeled graphs for relational reasoning. Reports 91% lower latency and 90% lower token costs vs. full-context approaches. Mem0 manages one application's user memories. HMP manages cross-repository engineering knowledge — different data, different trust model, different scale. An application could use Mem0 for user personalization and HMP for technical intelligence simultaneously.

**Zep / Graphiti** (Zep AI, 2024; arXiv preprint). Three-layer knowledge graph: episodic subgraph (raw interactions), semantic entity subgraph (extracted entities/relationships), and community subgraph (clustered summaries). Bi-temporal modeling tracks both event time and ingestion time, enabling historical reasoning. Hybrid retrieval combines embedding similarity, BM25, and graph traversal. Zep is a hosted service for a single organization's agents — it owns the data and the index. HMP is a decentralized protocol where data lives in Git (owned by the repository), no central service exists, and trust is emergent from ecosystem gravity rather than organizational policy.

**Cognee** (Cognee AI, 2024). Open-source memory engine using an Extract–Cognify–Load (ECL) pipeline with ontology-based validation (RDF/OWL). Separates session memory (working context) from permanent memory (cross-connected knowledge artifacts). Adaptive `memify` process prunes stale nodes and reweights edges. Cognee structures memory for one organization's knowledge corpus. HMP structures memory for the global repository ecosystem — different ownership model, different trust requirements.

**LangMem / LangGraph** (LangChain, 2025). Storage-agnostic SDK for semantic, episodic, and procedural memory, integrated with LangGraph's persistent stores and checkpointers. Provides namespaced isolation per user/application. LangMem is a library for building agent memory within one deployment. HMP is a protocol specification for sharing memory across deployments, organizations, and time.

### 14.3. Multi-Agent Reasoning Architectures

These systems orchestrate multiple agents for specific reasoning tasks. HMP provides the persistent knowledge layer that makes their outputs reusable.

**MA-RAG** (2024; arXiv:2501.15022). Modular multi-agent RAG: Planner decomposes queries, Step Definer generates subqueries, Extractor filters evidence, QA Agent synthesizes answers via Chain-of-Thought. Training-free, significantly outperforms standalone RAG on multi-hop QA. MA-RAG produces answers for one query session. HMP makes the answer persistent — once an MA-RAG pipeline solves a complex architectural question, that knowledge becomes a memory available to every future agent.

**SOHM** (2024; arXiv:2412.03065). The "Society of HiveMind" framework: swarms of heterogeneous foundation models optimized via hybrid gradient-based and evolutionary methods. Inspired by Minsky's Society of Mind and natural swarm intelligence. Demonstrates significant gains on intensive logical reasoning tasks, negligible benefit on simple factual recall. SOHM is a training-time optimization framework for model swarms. HMP is a runtime protocol for knowledge sharing. They operate at different layers: SOHM optimizes how agents think together; HMP defines how agents share what they've learned.

### 14.4. Protocol and Infrastructure Layer

**MCP** (Anthropic, 2024). Model Context Protocol: standardized interface between agents and tools. Gives agents "hands" — the ability to interact with external services via a unified tool-calling protocol. HMP gives agents a "shared brain" — persistent collective memory. They are complementary and composable: an HMP Server can be exposed as an MCP tool, allowing any MCP-compatible agent to query the Hive Mind.

**A2A** (Google, 2025). Agent-to-Agent protocol: standardized real-time communication between agents. Enables synchronous peer coordination. HMP enables asynchronous knowledge transfer across time — an agent that ran 6 months ago benefits an agent running today without the two ever communicating directly. A2A is voice; HMP is institutional memory.

### 14.5. Why a New Protocol?

| Category | Systems | Memory Scope | Persistence | Trust Model |
|----------|---------|-------------|-------------|-------------|
| Single-agent | MemGPT, ExpeL, Reflexion, A-MEM | One agent | Session or single-agent DB | Implicit (self-trust) |
| Organizational | Mem0, Zep, Cognee, LangMem | One org's agents | Centralized service/DB | Organizational policy |
| Multi-agent reasoning | MA-RAG, SOHM | Task pipeline | None (ephemeral) | Pipeline-scoped |
| Protocol/infra | MCP, A2A | Tool/peer access | N/A | Auth tokens |
| **HMP** | — | **Global ecosystem** | **Git (immutable)** | **Emergent from ecosystem gravity** |

No existing system provides an open, decentralized, vendor-neutral standard for shared agent intelligence across organizational and temporal boundaries. HMP is not a replacement for any of these systems. It is the missing infrastructure layer beneath them — the protocol that lets a MemGPT agent in one organization benefit from the experience of an ExpeL agent in another, mediated by Git-backed trust and computed confidence.

## 15. Roadmap

### 15.1. Maturity Progression

The protocol follows a three-stage maturity model as defined in [CONTRIBUTING.md](CONTRIBUTING.md):

| Stage | Status | Gate |
|-------|--------|------|
| **Working Draft** | **← Current (v0.1.0-draft)** | Active development. Breaking changes expected. |
| **Candidate** | — | Feature-complete. At least one reference implementation passes all verification vectors. |
| **Standard (v1.0.0)** | — | Two or more independent implementations demonstrate interoperability on all Core and Node RPCs. |

### 15.2. Path to Candidate (v0.2.0)

| Milestone | Description | Status |
|-----------|-------------|--------|
| Reference implementation | First HMP Server implementing Core, Graph, and Node protocols | In progress |
| Schema validation suite | Automated test harness validating `.himeshaa/` files against JSON Schemas | Planned |
| Confidence model tests | End-to-end verification of W, T, A, S, A_eff, and C formulas with test vectors | Planned |
| Extension stabilization | Perception, Reasoning, and Introspection promoted from OPTIONAL to stable extension specs | Planned |
| Interop test suite | Machine-readable conformance tests for any implementation to validate against | Planned |

### 15.3. Path to Standard (v1.0.0)

| Milestone | Description | Status |
|-----------|-------------|--------|
| Second independent implementation | A non-reference HMP Server passes the interop test suite | Not started |
| Pulse federation validation | Two Servers demonstrate federated index synchronization | Not started |
| Security audit | Independent review of the trust model and Sybil resistance properties | Not started |
| Community RFC cycle | At least 3 community-contributed RFCs processed through the RFC pipeline | Not started |

### 15.4. Post-v1.0 Directions

The following areas are under consideration for future versions but are explicitly out of scope for v1.0:

- **Private Hive Minds** — Enterprise-scoped networks with access control beyond public forges.
- **Binary memory attachments** — Git LFS integration for non-textual knowledge (e.g., model weights, embeddings).
- **Agent identity** — Cryptographic agent identity beyond Git commit signatures.
- **Incentive mechanisms** — Economic models for rewarding high-authority Node declarations.
