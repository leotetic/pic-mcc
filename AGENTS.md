# Repository Guidelines

## Project Structure & Module Organization
`Cargo.toml` defines a shared library in `src/` plus five binaries: `astra` (1D), `astra-2d`, `astra-3d`, `astra-nbody`, and `astra-admin`. Core simulation code lives in `astra-1d/src/`, `astra-2d/src/`, `astra-3d/src/`, and `astra-nbody/`. Keep shared utilities in `src/`, especially license and external-circuit logic. Runtime examples live in `astra-*/configs/` and `astra-*/inputs/`. Reference data is stored in `CrossSections/`, and longer design notes belong in `docs/`.

## Build, Test, and Development Commands
Run all commands from the repository root.

- `cargo build --release`: build all binaries.
- `cargo check --bin astra-2d`: fast compile check for one target; swap in `astra`, `astra-3d`, or `astra-nbody` as needed.
- `cargo run --bin astra -- 4000 d --config=astra-1d/configs/turner1.toml`: run a sample 1D case with diagnostics.
- `cargo fmt --all`: apply standard Rust formatting.
- `cargo clippy --all-targets --all-features -- -D warnings`: enforce the same lint level used in CI.
- `cargo test --all --verbose`: run the full test suite across shared and binary code.

## Coding Style & Naming Conventions
Use default Rust formatting with 4-space indentation via `cargo fmt`. Follow standard naming: `PascalCase` for types, `snake_case` for functions/modules/files, and `SCREAMING_SNAKE_CASE` for constants. Keep modules dimension-specific when behavior differs (`boundary.rs`, `boundary_2d.rs`, `boundary_3d.rs`). Clippy is configured in `clippy.toml`; do not add warnings to CI.

## Testing Guidelines
Tests are inline Rust unit tests placed near the code they exercise with `#[cfg(test)]` and descriptive names such as `test_parse_2d_grid_config`. Add regression tests for solver changes, boundary handling, collision logic, and config parsing. Before opening a PR, run `cargo test --all`, plus a focused command such as `cargo test test_current_boundary_field_signs` when iterating on a single subsystem.

## Commit & Pull Request Guidelines
Recent history uses Conventional Commit style with optional scopes, for example `feat (3d): add ...`, `feat (astra-1d): add ...`, and `release: version 2026.1.2`. Prefer lowercase types like `feat`, `fix`, `docs`, `refactor`, `test`, and `chore`. PRs should summarize the change, note affected binaries, include validation commands run locally, and mention any config/doc updates. Update `CHANGELOG.md` for release-facing changes.
