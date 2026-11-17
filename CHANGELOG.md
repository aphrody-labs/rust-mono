# Changelog

The upstream sync history is machine-readable in `ledger/`. Releases of this repository are not tagged; `main` is the product.

## 2026-11-17

- Forks created for 41 projects, inventory in docs/inventory.md.
- Superproject with 51 pinned forks, `tools/forks.ts`, `tools/sync.ts`, scheduled sync workflow.
- First real sync: 6 forks fast-forwarded, zed merged, bun reported diverged (ledger/2026-11.jsonl).
- Scheduled workflow blocked on an invalid `APHRODY_APP_KEY` org secret (docs/sync.md).
