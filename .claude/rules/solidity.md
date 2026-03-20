---
paths:
  - "**/*.sol"
  - "foundry.toml"
  - "remappings.txt"
---

# Solidity Development Rules

## Code Style

- NatSpec: always use `/** */` multi-line block notation, never `///`
- Function visibility order: external → public → internal → private
- Max line length: 120 characters
- Indentation: 4 spaces
- Run `forge fmt` before committing

## Compiler & EVM

- Solidity 0.8.30+ required (EIP-1153 transient storage support)
- EVM version: Cancun
- Compiler: optimizer enabled, 200 runs, via_ir = true
- CREATE2 factory: always_use_create_2_factory = true
- foundry.toml is authoritative for all configuration

## Security

- Reentrancy protection: EIP-1153 transient storage via OpenZeppelin ReentrancyGuardTransient
- Token interactions: always use SafeERC20
- Factory deployments: CREATE2 deterministic deployment
- Minimal proxies: EIP-1167 via OpenZeppelin Clones

## Testing

- Default to smoke tests: target specific tests with `--match-test` or `--match-contract` for fast feedback
- Only run full suite for core protocol changes or before commits
- Invariant test profiles: quick (64 runs), default (256 runs), ci (512 runs), deep (1024 runs)
- Do not write full test suites unprompted — write the specific test requested

## Workflow

- Build: `forge build`
- Test: `forge test`
- Format: `forge fmt`
- Gas report: `forge test --gas-report`
- Pre-commit: format, build, then run affected tests

## OpenZeppelin Ports — Do Not Review

These uniteum repos are minimal ports of OpenZeppelin contracts.
Do not flag style violations or suggest changes to their code.

- uniteum/clones — proxy/Clones.sol
- uniteum/erc20 — token/ERC20/ERC20.sol
- uniteum/ierc20 — token/ERC20/IERC20.sol, IERC20Metadata.sol
- uniteum/math — utils/math/Math.sol, SafeCast.sol, SignedMath.sol
- uniteum/panic — utils/Panic.sol
- uniteum/reentrancy — utils/ReentrancyGuardTransient.sol
- uniteum/strings — utils/Strings.sol

## Common Mistakes to Avoid

- Do not use `///` for NatSpec — always `/** */`
- Do not run the full test suite when a targeted smoke test suffices
- Do not add unnecessary error handling or validation beyond what the protocol requires
- Do not refactor or "improve" surrounding code when fixing a specific issue
- Do not delete or overwrite files without explicit confirmation
