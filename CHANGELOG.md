# Changelog

All notable changes to Asphalt are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.2] - 2026-07-07

- Updated the `rbx-binary` and `rbx-xml` dependencies.

## [2.0.1] - 2026-06-01

- Fixed a race condition that allowed duplicate assets on the `cloud` and `studio` targets.
- Fixed the Studio target using the wrong asset URI scheme, which produced duplicate Studio assets.

## [2.0.0] - 2026-04-30

The sync pipeline was rewritten for concurrency and split into walking and processing phases. Integration tests were added.

### Added

- A `--project` argument to point Asphalt at a project directory other than the current directory.
- Config schema generation into `schema.json` via the hidden `generate-config-schema` command; the schema is now referenced in the README example.
- Integration tests under `tests/` covering the sync flow.

### Changed

- Rewrote the sync pipeline to walk and process files concurrently, passing events over a channel between a walker and a collector.
- Wrapped asset hashes in a dedicated `Hash` newtype.
- Assets are processed (SVG rasterization, alpha-bleed) right before a sync rather than during walking, reducing data cloning.
- The lockfile and codegen are keyed/processed per input; an obsolete config key was removed.
- SVG `currentColor` fills now default to white before conversion.
- `sync` no longer errors partway through on a single failed asset; it reports the failure and stops after the run.

### Deprecated / Removed

- Removed the `ROBLOX_CONTENT_PATH` environment variable.
- Removed the now-unused `thiserror` dependency.

### Fixed

- Brace glob sync walk pruning.

## [1.2.0] - 2025-10-23

### Added

- Support for CurveAnimation (curves).
- `aarch64-unknown-linux-gnu` build target.

### Changed

- Replaced cookie-based authentication with Open Cloud API-key authentication for animation and model uploads.
- Switched path handling to the `relative-path` crate.
- Upgraded dependencies.

### Removed

- All cookie handling and the `ROBLOX_CONTENT_PATH` environment variable.

## [1.1.0] - 2025-09-13

### Added

- A `codegen.content` option to emit a `Content` instance instead of a `string`.

### Changed

- Updated the author name and email in `Cargo.toml` and the license.

## [1.0.0] - 2025-09-02

The 1.0 release. Major rewrite of the sync pipeline and a versioned lockfile format.

### Added

- `asphalt upload`, which uploads a single asset and returns its asset ID (or a link with `--link`).
- Video uploads via a `--expected-price` CLI flag.
- The `migrate-lockfile` command (help text split into a head and body).
- Parallel file walking.
- Snapshot tests for codegen.
- Local Studio sync outputs files named by content hash.

### Changed

- Rewrote the sync pipeline; `sync` now has explicit `cloud` / `studio` / `debug` targets and a `--dry-run` that exits nonzero if any asset would change.
- The lockfile is now keyed by file hash rather than file path, and is read/written in a versioned format that distinguishes V0/V1/V2.
- The project now targets the Rust 2024 edition.
- Asset processing now happens at sync time instead of at walk time.

### Fixed

- Studio sync no longer clears the lockfile.
- The API key is no longer required for non-cloud targets or dry runs.
- Codegen flat paths on Windows, and duplicate assets that didn't generate code.
- A nonexistent lockfile was allowed; trailing backslashes in local sync paths were replaced; skipped-file warnings became debug logs.

### Removed

- The `checksum` key from the lockfile (set to an empty array for compatibility).
- Manual cookie handling for asset delivery.

## [0.9.1] - 2025-02-12

- Bleed the entire image rather than only its alpha edges during processing.

## [0.9.0] - 2025-02-06

- Fixed the alpha-bleeding algorithm.

## [0.8.4] - 2025-01-13

- Added FLAC and WAV audio support.
- Set up Homebrew install and updated the README.

## [0.8.3] - 2024-12-27

- Added a command to migrate a `tarmac-manifest.toml` lockfile.
- Properly handled animation upload errors.
- Set up Homebrew (cargo-dist) install.

## [0.8.2] - 2024-11-18

- Retry uploads when Roblox returns a 429 rate-limit error.
- Switched to `WalkDir` for directory traversal.
- Added a note about Foreman incompatibility.

## [0.8.1] - 2024-08-05

- Reworked code generation onto an AST-based style, with a `style` option (`flat` / `nested`).
- Joined the asset directory with existing asset entries.
- Added README instructions for ignoring generated files in linters/formatters and for building from source.

## [0.8.0] - 2024-06-30

- Added sync targets: `cloud`, `studio`, and `debug`.
- Luau output is now standard and always generated (removed the opt-in Luau option).
- CI now fails on lint/format warnings.

## [0.7.2] - 2024-06-15

- Added an asset exclude glob.
- Updated resvg.

## [0.7.1] - 2024-06-06

- Automatically detect the cookie using `rbx_cookie`.

## [0.7.0] - 2024-06-06

- Added animation (KeyframeSequence) upload support.

## [0.6.3] - 2024-06-04

- Fixed a missing `?` in the file-check error format string.

## [0.6.2] - 2024-06-03

- Added verbose logging (`-v` / `-vv`).
- Added alpha bleeding for TGA images.

## [0.6.1] - 2024-05-28

- Added an `init` command.
- Improved error contexts and removed unused dependencies.

## [0.6.0] - 2024-05-26

- Split the CLI into subcommands.

## [0.5.1] - 2024-05-03

- Alpha-bleed images (`bleed` option).
- Use Rustls for TLS.
- Added the `strip_extensions` codegen option.

## [0.5.0] - 2024-04-25

- Added nested codegen support, controlled by the codegen `style` option (`flat` / `nested`).
- Reorganized codegen into its own `[codegen]` config section with a style argument.
- Documented how to obtain an API key.

## [0.4.2] - 2024-04-14

- Added support for configuring existing assets (the `web` section).
- Added codegen tests.

## [0.4.1] - 2024-04-04

- Improved error context.

## [0.4.0] - 2024-03-29

- Configure Asphalt with an `asphalt.toml` project file instead of CLI flags.
- Use file names instead of file paths for asset names.

## [0.3.5] - 2024-03-29

- Cache the font database across SVG renders and avoid reading files twice.

## [0.3.4] - 2024-03-29

- Added SVG support (converted to PNG).

## [0.3.3] - 2024-03-28

- Error-message polish.

## [0.3.2] - 2024-03-28

- Better error handling with no more recursion.

## [0.3.1] - 2024-03-25

- Support a Luau file extension with `--luau`.
- Fixed file paths on Windows.
- Retry-with-backoff when uploading.
- Configurable output name.

## [0.3.0] - 2024-03-11

- Use `rbxcloud`'s extension-based asset type detection.
- Support nested directories and auto-created output directories.

## [0.2.1] - 2024-03-07

- Support `.tga` images.

## [0.2.0] - 2024-03-07

- Support Roblox groups (creator type and ID).

## [0.1.1] - 2024-03-07

- Hotfix: removed a hardcoded user ID.

## [0.1.0] - 2024-03-07

- Initial release.
