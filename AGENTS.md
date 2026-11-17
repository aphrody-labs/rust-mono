# AGENTS.md

Contract for agents working in rust-mono.

- `forks.toml` is the source of truth. `.gitmodules` and the gitlinks are generated from it (`bun tools/submodules.ts --write`); never edit them by hand.
- Forks are submodules with relative URLs, one checkout each. Never copy a fork's files in here.
- Never build Rust, LLVM or anything heavy on a workstation. Compilation runs on the build host over ssh; see docs/toolchain-build.md.
- Never write to an upstream (rust-lang, llvm, bytecodealliance...): no pull request, issue or comment. Forks of this organization only.
- Do not register a submodule working tree locally with a full clone of rust or LLVM: use `git update-index --add --cacheinfo 160000,<sha>,<path>` (what the tools do) or `--depth 1` / `--filter=blob:none`.
- `ledger/*.jsonl` is append-only output of `tools/sync.ts`.
- Run `bun test tools` before every push. Work on `main`, commit by path, no branches, stash or reset, no AI attribution trailers.
- Nothing hardcoded: the fork organization comes from `forks.toml` `[meta]`, never from a literal in a script or workflow.
