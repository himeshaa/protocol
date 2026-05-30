# HMP File Schemas

Machine-readable JSON Schema definitions for all `.himeshaa/` file formats defined by the Himeshaa Protocol.

## Schema Files

| Schema | Validates | File Path |
|--------|-----------|-----------|
| [`common-v1.json`](common-v1.json) | Shared type definitions | Referenced by other schemas |
| [`hmp-v1.json`](hmp-v1.json) | Node Declaration | `.himeshaa/hmp.json` |
| [`memory-v1.json`](memory-v1.json) | Memory | `.himeshaa/memories/{id}.json` |
| [`contradiction-v1.json`](contradiction-v1.json) | Contradiction | `.himeshaa/contradictions/{id}.json` |
| [`confirmation-v1.json`](confirmation-v1.json) | Confirmation | `.himeshaa/confirmations/{id}.json` |

## JSON Schema Version

All schemas use [JSON Schema draft-07](http://json-schema.org/draft-07/schema#) for maximum tooling compatibility across programming languages.

## Canonical URIs

Schemas are published at `https://himeshaa.com/schema/{name}.json`. Files in `.himeshaa/` reference these URIs via the `$schema` field:

```json
{
  "$schema": "https://himeshaa.com/schema/memory-v1.json",
  "id": "mem-001",
  "content": "..."
}
```

## Forward Compatibility

All schemas enforce `additionalProperties: false` to prevent denial-of-service via arbitrary field injection. This means schema versions are **strict** — a v0.1.0 validator will reject files containing fields introduced in v0.2.0. New fields (even optional ones) require a schema version increment as defined in §8.6 of the protocol specification. This trade-off prioritizes security and implementation determinism over forward compatibility.

## Security Constraints

All array fields enforce `maxItems` bounds to prevent denial-of-service via resource exhaustion. All text fields enforce `maxLength` bounds. The `_meta` extension point enforces `maxProperties: 32`.
