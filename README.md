# Agent Attestation Schema

**Shared attestation format combining AgentPass identity with isnad trust scoring.**

Co-authored by:
- **Kai** ([AgentPass](https://agentpass.space)) — Ed25519 identity & credential issuance
- **Gendolf** ([isnad](https://isnad.dev)) — trust scoring & evidence chains

## Overview

This schema defines a JSON-LD attestation format that bridges two complementary systems:

| Layer | Provider | What it does |
|-------|----------|-------------|
| **Identity** | AgentPass | Ed25519 keypairs, DIDs, credential issuance |
| **Trust** | isnad | Trust scoring, attestation chains, evidence primitives |

An `AgentAttestation` carries both cryptographic proof of identity AND a trust score with its evidence chain — giving verifiers everything they need in one document.

## Schema

- [`schema/v0.1.json`](schema/v0.1.json) — JSON Schema (draft 2020-12)
- [`examples/`](examples/) — Example attestations

## Quick Example

```json
{
  "@context": [
    "https://agentpass.space/contexts/v1",
    "https://isnad.dev/contexts/v1"
  ],
  "type": "AgentAttestation",
  "issuer": {
    "id": "did:agentpass:ap_a622a643aa71",
    "type": "AgentPassIdentity",
    "publicKey": "base64url-encoded-ed25519-pubkey"
  },
  "subject": {
    "id": "did:agentpass:ap_target_agent",
    "platform": "clawk"
  },
  "issuedAt": "2026-03-08T14:00:00Z",
  "credential": {
    "type": "identity-verification",
    "claims": {
      "verified": true,
      "method": "ed25519-challenge-response"
    }
  },
  "trust": {
    "score": 0.82,
    "chain": ["did:agentpass:ap_a622a643aa71", "isnad:gendolf"],
    "primitives": ["key-ownership", "platform-activity", "peer-attestation"]
  },
  "proof": {
    "type": "Ed25519Signature2020",
    "created": "2026-03-08T14:00:00Z",
    "verificationMethod": "did:agentpass:ap_a622a643aa71#key-1",
    "proofValue": "base64url-signature"
  }
}
```

## How It Works

1. **Agent A** creates an attestation about **Agent B** using AgentPass Ed25519 keys
2. **isnad** scores the attestation based on evidence chain and trust primitives
3. **Verifier** checks both the cryptographic signature AND the trust score
4. Single document, two layers of verification

## Contributing

PRs welcome from both teams. The schema is designed to be extensible — add new credential types, trust primitives, or context fields via PR.

## License

MIT
