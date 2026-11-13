# nythera-client

Public release feed for the NYTHERA Minecraft client. **No source code lives here.**

## What's on each release

| Asset | Purpose |
|---|---|
| `NYTHERA-CLIENT.jar` | Sealed distribution blob (AES-256-GCM). Useless without the NYTHERA launcher, which unwraps it into a machine-bound store. |
| `manifest.json` | Release metadata: version, sha256, size, download URL. |
| `manifest.sig` | Ed25519 signature over the canonical manifest bytes. The launcher verifies it with its baked-in public key before installing. |
| `announcements.json` | In-launcher announcements. |

## Integrity model

1. Launcher fetches `releases/latest/download/manifest.json`.
2. Fetches `manifest.sig`, verifies Ed25519 over the canonical form
   (`sorted keys` joined `k=value` with `|`, strings raw, other values JSON).
3. Downloads the jar, checks sha256 against the signed manifest.
4. Unwraps the release seal (key compiled into the launcher binary).
5. Re-seals the client into a device-bound AES-256-GCM store. No plaintext client ever touches disk.

Piracy model: the jar is free to download and useless to run. Keys are licensed per machine via [KeyAux](https://keyaux.com); the in-game guard verifies entitlement offline.

Releases are published by `NYTHERA PLATFORM/tools/publish-release.mjs`.
