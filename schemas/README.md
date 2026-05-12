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

Schemas are published at `https://himeshaa.dev/schema/{name}.json`. Files in `.himeshaa/` reference these URIs via the `$schema` field:

```json
{
  "$schema": "https://himeshaa.dev/schema/memory-v1.json",
  "id": "mem-001",
  "content": "..."
}
```

## Forward Compatibility

Schemas intentionally omit `additionalProperties: false` to enable forward-compatible evolution. A v0.1.0 validator will accept files containing fields introduced in v0.2.0.

## Security Constraints

All array fields enforce `maxItems` bounds to prevent denial-of-service via resource exhaustion. All text fields enforce `maxLength` bounds. The `_meta` extension point enforces `maxProperties: 32`.
