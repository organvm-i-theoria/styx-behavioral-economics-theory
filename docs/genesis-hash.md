# Genesis Hash

This repo now exposes a verifiable Genesis Hash. The purpose of the hash is simple: give the theory layer a stable, reproducible proof of origin that downstream systems can cite without depending on mutable prose.

## Canonical Payload

The canonical payload is stored at:

`docs/genesis/genesis-payload.json`

The Genesis Hash is defined as the SHA-256 digest of the exact bytes of that file.

## Recorded Digest

`e8ac8499586390d069afbbefa01c9881b30702db9caa7c114cab4373414ddcfd`

## Verification

Run:

```bash
shasum -a 256 docs/genesis/genesis-payload.json
```

The command output should match the recorded digest above.

## What The Payload Anchors

The payload ties the hash to:

- repo identity
- organ identity
- seed-tier identity
- the initial commit SHA
- the initial commit tree hash
- the initial commit timestamp

This means the Genesis Hash is not a decorative constant. It is a reproducible checksum over a canonical statement of repository origin.
