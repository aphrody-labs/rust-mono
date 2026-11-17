# rust-mono

Superproject that pins every Rust toolchain fork of the organization at one commit each, kept in sync with upstream.

[![ci](https://github.com/aphrody-labs/rust-mono/actions/workflows/ci.yml/badge.svg)](https://github.com/aphrody-labs/rust-mono/actions/workflows/ci.yml)
[![sync](https://github.com/aphrody-labs/rust-mono/actions/workflows/sync.yml/badge.svg)](https://github.com/aphrody-labs/rust-mono/actions/workflows/sync.yml)
[![license](https://img.shields.io/github/license/aphrody-labs/rust-mono)](LICENSE)

## Why

Rust is not one repository: the compiler, LLVM, cargo, rustup, rust-analyzer, the standard library crates, the books, the governance repositories and the infrastructure live apart, and our own work (bun, zed, WebAssembly tools) builds on them. This repository is the one place that says which commit of each is in use, so a toolchain build, an agent or a review starts from a single, reproducible checkout. It is a superproject of 51 git submodules (relative URLs to the forks in this organization), not a copy: nothing is vendored, and nothing is cloned until asked for.

It is not a source-level merge like Google's or Linux's single tree: a history-preserving merge of rust-lang/rust (1.0 GB) with the LLVM it pins (3.4 GB) alone is over 4 GB on GitHub, and every upstream relationship (merge, rebase, upstream PR) would be lost. Pinned forks give the same single-commit reproducibility and keep upstream sync mechanical.

## Install

```sh
git clone --filter=blob:none https://github.com/aphrody-labs/rust-mono.git
cd rust-mono
bun test tools
```

The clone holds metadata only (a few hundred KB). Check out only what you need, each submodule once:

```sh
git submodule update --init --depth 1 -- books/nomicon devtools/cargo
```

## Usage

```sh
bun tools/forks.ts                 # every fork: tracked branch, ahead/behind upstream, pin, drift
bun tools/forks.ts --check         # exit 1 when forks.toml, .gitmodules, the pins or GitHub disagree
bun tools/sync.ts                  # dry run of the upstream sync
bun tools/sync.ts --apply --commit # merge upstream into the forks, bump the pins, append to ledger/
bun tools/submodules.ts --write    # regenerate .gitmodules and register missing gitlinks from forks.toml
```

The sync runs by itself every six hours from [.github/workflows/sync.yml](.github/workflows/sync.yml).

## Documentation

- [docs/index.md](docs/index.md): start here, how to find anything
- [docs/architecture.md](docs/architecture.md): layout, groups, one checkout per project, reproducibility
- [docs/sync.md](docs/sync.md): how the upstream sync works, the ledger, the token
- [docs/agents.md](docs/agents.md): how an agent finds a file, an issue or a commit
- [docs/inventory.md](docs/inventory.md): what was forked on 2026-11-17 and why
- [docs/knowledge-feed.md](docs/knowledge-feed.md) and [docs/toolchain-build.md](docs/toolchain-build.md): designs for the next phases

## Development

```sh
bun test tools
```

Scripts are Bun built-ins only (no npm dependencies). Tests replay recorded GitHub API sessions from `tools/fixtures/`; none touches the network, and none builds Rust. Anything that compiles runs on the build host, never on a workstation. Contributions follow the [organization guide](https://github.com/aphrody-labs/.github/blob/main/CONTRIBUTING.md); security reports go through the [security policy](https://github.com/aphrody-labs/.github/blob/main/SECURITY.md).

## License

Apache-2.0, see [LICENSE](LICENSE). The submodules keep their own upstream licenses.
