# dependency-preflight hosted check

Runs [dependency-preflight](https://github.com/lemind/dependency-preflight) on a GitHub-hosted runner against three
public Solana repositories, each pinned to a fixed commit:

| Target | Commit | Path assessed |
| --- | --- | --- |
| `anza-xyz/wallet-adapter` | `3663416` | repository root |
| `solana-foundation/anchor` | `53cd88f` | `ts/` |
| `drift-labs/protocol-v2` | `13e8e9b` | `sdk/` |

Each run's report is in the job summary of the [Actions runs](../../actions).

## What it checks

- Every npm, pnpm and Yarn 1 lockfile under the path, per workspace member.
- Each locked version against public OSV advisory data, including malicious-package records.
- Direct dependencies against the latest release on the npm registry.
- A verdict (blocked, review, ready) and, separately, how complete the assessment was (complete, partial, failed).

## What it does not check

It is an npm dependency assessment, not a security audit. It doesn't install, build, test or run the target's code,
inspect source code, or assess Rust (`Cargo.lock`) or other ecosystems; those files are listed as not assessed.

## Notes

- No secrets or wallet: free mode only, nothing is bought.
- The Action step may fail on purpose when a target is **blocked**; the job then checks that a verdict was produced.
  A missing verdict fails the job.
