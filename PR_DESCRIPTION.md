## Summary

This PR adds four major capabilities to StarForge: a local network simulation environment, cross-chain bridge support, formal verification integration, and an automated deployment verification system.

### #337 — Network Simulation and Testing Environment

- Added `src/utils/network_sim.rs` — deterministic in-memory ledger simulator with seeded execution
- Added `starforge simulate` CLI with subcommands: `run`, `snapshot`, `restore`, `time`, `fail`, `scenario`, `list`
- Supports state snapshot/restore, virtual time and ledger control, failure injection, and built-in test scenarios
- Integration tests in `tests/network_simulation.rs`

### #390 — Cross-Chain Bridge Support

- Added `src/utils/bridge/` module (providers, routes, security, state sync, monitoring)
- Added `starforge bridge` CLI with subcommands: `transfer`, `status`, `routes`, `configure`, `sync`, `verify`, `monitor`, `history`
- Bridge config and transfer history persisted under `~/.starforge/bridge/`
- Integration tests in `tests/bridge_integration.rs`

### #389 — Contract Formal Verification Integration

- Wired existing `starforge verify` command into the CLI (`main.rs`)
- Added `verify visualize` for ASCII chart visualization of verification results
- Added `.github/workflows/verify.yml` for continuous verification in CI
- Existing harness generation, property specs, run/report, and CI snippet generation are now accessible

### #369 — Contract Deployment Verification System

- Added `src/utils/deployment_verify.rs` — automated bytecode, storage layout, and functionality checks
- Extended `starforge deployments verify` with `--report` and `--json` flags
- Added `deployments report` and `deployments ci` subcommands
- Verification reports saved to `~/.starforge/deploy_verify/`
- Added `.github/workflows/deploy-verify.yml`
- Integration tests in `tests/deployment_verification.rs`

## Test plan

- [ ] `starforge simulate list` — lists built-in scenarios
- [ ] `starforge simulate scenario --name basic-deploy-invoke` — runs deterministic scenario
- [ ] `starforge simulate fail --mode timeout` — confirms failure injection
- [ ] `starforge bridge routes` — lists available cross-chain routes
- [ ] `starforge bridge verify --source stellar-testnet --dest ethereum-sepolia --amount 1000000 --sender G... --recipient 0x...` — security checks
- [ ] `starforge verify harness --wasm <path>` — generates verification harness
- [ ] `starforge verify property add/list` — property registry
- [ ] `starforge verify visualize --contract <name>` — ASCII result chart
- [ ] `starforge deployments verify --id <id> --save --report` — full deployment verification
- [ ] `starforge deployments report --id <id>` — shows saved report
- [ ] `cargo test network_simulation bridge_integration deployment_verification`

closes #337
closes #390
closes #389
closes #369
