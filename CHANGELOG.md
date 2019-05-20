# Changelog

## Cargo 1.36 (2019-07-04)
[6f3e9c36...HEAD](https://github.com/rust-lang/cargo/compare/6f3e9c36...HEAD)

### Added
- (Nightly only): Added [`-Z install-upgrade`
  feature](https://github.com/rust-lang/cargo/blob/7b13469ee90a24fa5aa06166d447dac3370846ef/src/doc/src/reference/unstable.md#install-upgrade)
  to track details about installed crates and to automatically update them if
  they are out-of-date. [#6798](https://github.com/rust-lang/cargo/pull/6798)
- (Nightly only): Added the [`public-dependency`
  feature](https://github.com/rust-lang/cargo/blob/7b13469ee90a24fa5aa06166d447dac3370846ef/src/doc/src/reference/unstable.md#public-dependency)
  which allows tracking public versus private dependencies.
  [#6772](https://github.com/rust-lang/cargo/pull/6772)

### Changed
- `publish = ["crates-io"]` may be added to the manifest to restrict
  publishing to crates.io only.
  [#6838](https://github.com/rust-lang/cargo/pull/6838)
- macOS: Only include the default paths if `DYLD_FALLBACK_LIBRARY_PATH` is not
  set. Also, remove `/lib` from the default set.
  [#6856](https://github.com/rust-lang/cargo/pull/6856)
- `cargo publish` will now exit early if the login token is not available.
  [#6854](https://github.com/rust-lang/cargo/pull/6854)
- HTTP/2 stream errors are now considered "spurious" and will cause a retry.
  [#6861](https://github.com/rust-lang/cargo/pull/6861)
- (Nightly only): The `publish-lockfile` feature has had some significant
  changes. The default is now `true`, the `Cargo.lock` will always be
  published for binary crates. The `Cargo.lock` is now regenerated during
  publishing. `cargo install` now ignores the `Cargo.lock` file by default,
  and requires `--locked` to use the lock file. Warnings have been added if
  yanked dependencies are detected.
  [#6840](https://github.com/rust-lang/cargo/pull/6840)
- Setting a feature on a dependency where that feature points to a *required*
  dependency is now an error. Previously it was a warning.
  [#6860](https://github.com/rust-lang/cargo/pull/6860)
- Resolver performance improvements for some cases.
  [#6853](https://github.com/rust-lang/cargo/pull/6853)

### Fixed
- More carefully track the on-disk fingerprint information for dependencies.
  This can help in some rare cases where the build is interrupted and
  restarted. [#6832](https://github.com/rust-lang/cargo/pull/6832)
- `cargo run` now correctly passes non-UTF8 arguments to the child process.
  [#6849](https://github.com/rust-lang/cargo/pull/6849)

## Cargo 1.35 (2019-05-23)
[6789d8a0...6f3e9c36](https://github.com/rust-lang/cargo/compare/6789d8a0...6f3e9c36)

### Added
- Added the `rustc-cdylib-link-arg` key for build scripts to specify linker
  arguments for cdylib crates.
  [#6298](https://github.com/rust-lang/cargo/pull/6298)
- (Nightly only): `cargo clippy-preview` is now a built-in cargo command.
  [#6759](https://github.com/rust-lang/cargo/pull/6759)

### Changed
- When passing a test filter, such as `cargo test foo`, don't build examples
  (unless they set `test = true`).
  [#6683](https://github.com/rust-lang/cargo/pull/6683)
- Forward the `--quiet` flag from `cargo test` to the libtest harness so that
  tests are actually quiet.
  [#6358](https://github.com/rust-lang/cargo/pull/6358)
- The verification step in `cargo package` that checks if any files are
  modified is now stricter. It uses a hash of the contents instead of checking
  filesystem mtimes. It also checks *all* files in the package.
  [#6740](https://github.com/rust-lang/cargo/pull/6740)
- Jobserver tokens are now released whenever Cargo blocks on a file lock.
  [#6748](https://github.com/rust-lang/cargo/pull/6748)
- Issue a warning for a previous bug in the TOML parser that allowed multiple
  table headers with the same name.
  [#6761](https://github.com/rust-lang/cargo/pull/6761)
- Removed the `CARGO_PKG_*` environment variables from the metadata hash and
  added them to the fingerprint instead. This means that when these values
  change, stale artifacts are not left behind. Also added the "repository"
  value to the fingerprint.
  [#6785](https://github.com/rust-lang/cargo/pull/6785)
- `cargo metadata` no longer shows a `null` field for a dependency without a
  library in `resolve.nodes.deps`. The dependency is no longer shown.
  [#6534](https://github.com/rust-lang/cargo/pull/6534)
- `cargo new` will no longer include an email address in the `authors` field
  if it is set to the empty string.
  [#6802](https://github.com/rust-lang/cargo/pull/6802)
- `cargo doc --open` now works when documenting multiple packages.
  [#6803](https://github.com/rust-lang/cargo/pull/6803)
- `cargo install --path P` now loads the `.cargo/config` file from the
  directory P. [#6805](https://github.com/rust-lang/cargo/pull/6805)
- Using semver metadata in a version requirement (such as `1.0.0+1234`) now
  issues a warning that it is ignored.
  [#6806](https://github.com/rust-lang/cargo/pull/6806)
- `cargo install` now rejects certain combinations of flags where some flags
  would have been ignored.
  [#6801](https://github.com/rust-lang/cargo/pull/6801)
- (Nightly only): The `build-override` profile setting now includes
  proc-macros and their dependencies.
  [#6811](https://github.com/rust-lang/cargo/pull/6811)
- Resolver performance improvements for some cases.
  [#6776](https://github.com/rust-lang/cargo/pull/6776)
- (Nightly only): Optional and target dependencies now work better with `-Z
  offline`. [#6814](https://github.com/rust-lang/cargo/pull/6814)

### Fixed
- Fixed running separate commands (such as `cargo build` then `cargo test`)
  where the second command could use stale results from a build script.
  [#6720](https://github.com/rust-lang/cargo/pull/6720)
- Fixed `cargo fix` not working properly if a `.gitignore` file that matched
  the root package directory.
  [#6767](https://github.com/rust-lang/cargo/pull/6767)
- Fixed accidentally compiling a lib multiple times if `panic=unwind` was set
  in a profile. [#6781](https://github.com/rust-lang/cargo/pull/6781)
- Paths to JSON files in `build.target` config value are now canonicalized to
  fix building dependencies.
  [#6778](https://github.com/rust-lang/cargo/pull/6778)
- Fixed re-running a build script if its compilation was interrupted (such as
  if it is killed). [#6782](https://github.com/rust-lang/cargo/pull/6782)
- Fixed `cargo new` initializing a fossil repo.
  [#6792](https://github.com/rust-lang/cargo/pull/6792)
- Fixed supporting updating a git repo that has a force push when using the
  `git-fetch-with-cli` feature. `git-fetch-with-cli` also shows more error
  information now when it fails.
  [#6800](https://github.com/rust-lang/cargo/pull/6800)
- `--example` binaries built for the WASM target are fixed to no longer
  include a metadata hash in the filename, and are correctly emitted in the
  `compiler-artifact` JSON message.
  [#6812](https://github.com/rust-lang/cargo/pull/6812)

## Cargo 1.34 (2019-04-11)
[f099fe94...6789d8a0](https://github.com/rust-lang/cargo/compare/f099fe94...6789d8a0)

### Added
- 🔥 Stabilized support for [alternate
  registries](https://doc.rust-lang.org/1.34.0/cargo/reference/registries.html).
  [#6654](https://github.com/rust-lang/cargo/pull/6654)
- (Nightly only): Added `-Z mtime-on-use` flag to cause the mtime to be
  updated on the filesystem when a crate is used. This is intended to be able
  to track stale artifacts in the future for cleaning up unused files.
  [#6477](https://github.com/rust-lang/cargo/pull/6477)
  [#6573](https://github.com/rust-lang/cargo/pull/6573)
- Added documentation on using builds.sr.ht Continuous Integration with Cargo.
  [#6565](https://github.com/rust-lang/cargo/pull/6565)
- `Cargo.lock` now includes a comment at the top that it is `@generated`.
  [#6548](https://github.com/rust-lang/cargo/pull/6548)
- Azure DevOps badges are now supported.
  [#6264](https://github.com/rust-lang/cargo/pull/6264)
- (Nightly only): Added experimental `-Z dual-proc-macros` to build proc
  macros for both the host and the target.
  [#6547](https://github.com/rust-lang/cargo/pull/6547)
- Added a warning if `--exclude` flag specifies an unknown package.
  [#6679](https://github.com/rust-lang/cargo/pull/6679)

### Changed
- `cargo test --doc --no-run` doesn't do anything, so it now displays an error
  to that effect. [#6628](https://github.com/rust-lang/cargo/pull/6628)
- Various updates to bash completion: add missing options and commands,
  support libtest completions, use rustup for `--target` completion, fallback
  to filename completion, fix editing the command line.
  [#6644](https://github.com/rust-lang/cargo/pull/6644)
- Publishing a crate with a `[patch]` section no longer generates an error.
  The `[patch]` section is removed from the manifest before publishing.
  [#6535](https://github.com/rust-lang/cargo/pull/6535)
- `build.incremental = true` config value is now treated the same as
  `CARGO_INCREMENTAL=1`, previously it was ignored.
  [#6688](https://github.com/rust-lang/cargo/pull/6688)
- Errors from a registry are now always displayed regardless of the HTTP
  response code. [#6771](https://github.com/rust-lang/cargo/pull/6771)

### Fixed
- Fixed bash completion for `cargo run --example`.
  [#6578](https://github.com/rust-lang/cargo/pull/6578)
- Fixed a race condition when using a *local* registry and running multiple
  cargo commands at the same time that build the same crate.
  [#6591](https://github.com/rust-lang/cargo/pull/6591)
- Fixed some flickering and excessive updates of the progress bar.
  [#6615](https://github.com/rust-lang/cargo/pull/6615)
- Fixed a hang when using a git credential helper that returns incorrect
  credentials. [#6681](https://github.com/rust-lang/cargo/pull/6681)
- Fixed resolving yanked crates with a local registry.
  [#6750](https://github.com/rust-lang/cargo/pull/6750)

## Cargo 1.33 (2019-02-28)
[8610973a...f099fe94](https://github.com/rust-lang/cargo/compare/8610973a...f099fe94)

### Added
- `compiler-artifact` JSON messages now include an `"executable"` key which
  includes the path to the executable that was built.
  [#6363](https://github.com/rust-lang/cargo/pull/6363)
- The man pages have been rewritten, and are now published with the web
  documentation. [#6405](https://github.com/rust-lang/cargo/pull/6405)
- (Nightly only): Allow using registry *names* in `[patch]` tables instead of
  just URLs. [#6456](https://github.com/rust-lang/cargo/pull/6456)
- `cargo login` now displays a confirmation after saving the token.
  [#6466](https://github.com/rust-lang/cargo/pull/6466)
- A warning is now emitted if a `[patch]` entry does not match any package.
  [#6470](https://github.com/rust-lang/cargo/pull/6470)
- `cargo metadata` now includes the `links` key for a package.
  [#6480](https://github.com/rust-lang/cargo/pull/6480)
- (Nightly only): `cargo metadata` added the `registry` key for dependencies.
  [#6500](https://github.com/rust-lang/cargo/pull/6500)
- "Very verbose" output with `-vv` now displays the environment variables that
  cargo sets when it runs a process.
  [#6492](https://github.com/rust-lang/cargo/pull/6492)
- `--example`, `--bin`, `--bench`, or `--test` without an argument now lists
  the available targets for those options.
  [#6505](https://github.com/rust-lang/cargo/pull/6505)
- Windows: If a process fails with an extended status exit code, a
  human-readable name for the code is now displayed.
  [#6532](https://github.com/rust-lang/cargo/pull/6532)
- Added `--features`, `--no-default-features`, and `--all-features` flags to
  the `cargo package` and `cargo publish` commands to use the given features
  when verifying the package.
  [#6453](https://github.com/rust-lang/cargo/pull/6453)

### Changed
- If `cargo fix` fails to compile the fixed code, the rustc errors are now
  displayed on the console.
  [#6419](https://github.com/rust-lang/cargo/pull/6419)
- (Nightly only): Registry names are now restricted to the same style as
  package names (alphanumeric, `-` and `_` characters).
  [#6469](https://github.com/rust-lang/cargo/pull/6469)
- (Nightly only): `cargo login` now displays the `/me` URL from the registry
  config. [#6466](https://github.com/rust-lang/cargo/pull/6466)
- Hide the `--host` flag from `cargo login`, it is unused.
  [#6466](https://github.com/rust-lang/cargo/pull/6466)
- (Nightly only): `cargo login --registry=NAME` now supports interactive input
  for the token. [#6466](https://github.com/rust-lang/cargo/pull/6466)
- (Nightly only): Registries may now elide the `api` key from `config.json` to
  indicate they do not support API access.
  [#6466](https://github.com/rust-lang/cargo/pull/6466)
- Build script fingerprints now include the rustc version.
  [#6473](https://github.com/rust-lang/cargo/pull/6473)
- macOS: Switched to setting `DYLD_FALLBACK_LIBRARY_PATH` instead of
  `DYLD_LIBRARY_PATH`. [#6355](https://github.com/rust-lang/cargo/pull/6355)
- `RUSTFLAGS` is now included in the metadata hash, meaning that changing
  the flags will not overwrite previously built files.
  [#6503](https://github.com/rust-lang/cargo/pull/6503)
- When updating the crate graph, unrelated yanked crates were erroneously
  removed. They are now kept at their original version if possible. This was
  causing unrelated packages to be downgraded during `cargo update -p
  somecrate`. [#5702](https://github.com/rust-lang/cargo/issues/5702)
- TOML files now support the [0.5 TOML
  syntax](https://github.com/toml-lang/toml/blob/master/CHANGELOG.md#050--2018-07-11).

### Fixed
- `cargo fix` will now ignore suggestions that modify multiple files.
  [#6402](https://github.com/rust-lang/cargo/pull/6402)
- `cargo fix` will now only fix one target at a time, to deal with targets
  which share the same source files.
  [#6434](https://github.com/rust-lang/cargo/pull/6434)
- (Nightly only): Fixed panic when using `--message-format=json` with metabuild.
  [#6432](https://github.com/rust-lang/cargo/pull/6432)
- Fixed bash completion showing the list of cargo commands.
  [#6461](https://github.com/rust-lang/cargo/issues/6461)
- `cargo init` will now avoid creating duplicate entries in `.gitignore`
  files. [#6521](https://github.com/rust-lang/cargo/pull/6521)
- (Nightly only): Fixed detection of publishing to crates.io when using
  alternate registries. [#6525](https://github.com/rust-lang/cargo/pull/6525)
- Builds now attempt to detect if a file is modified in the middle of a
  compilation, allowing you to build again and pick up the new changes. This
  is done by keeping track of when the compilation *starts* not when it
  finishes. Also, [#5919](https://github.com/rust-lang/cargo/pull/5919) was
  reverted, meaning that cargo does *not* treat equal filesystem mtimes as
  requiring a rebuild. [#6484](https://github.com/rust-lang/cargo/pull/6484)

## Cargo 1.32 (2019-01-17)
[339d9f9c...8610973a](https://github.com/rust-lang/cargo/compare/339d9f9c...8610973a)

### Added
- (Nightly only): Allow usernames in registry URLs.
  [#6242](https://github.com/rust-lang/cargo/pull/6242)
- Registries may now display warnings after a successful publish.
  [#6303](https://github.com/rust-lang/cargo/pull/6303)
- Added a [glossary](https://doc.rust-lang.org/cargo/appendix/glossary.html)
  to the documentation. [#6321](https://github.com/rust-lang/cargo/pull/6321)
- Added the alias `c` for `cargo check`.
  [#6218](https://github.com/rust-lang/cargo/pull/6218)
- (Nightly only): Added `"compile_mode"` key to the build-plan JSON structure
  to be able to distinguish running a custom build script versus compiling the
  build script. [#6331](https://github.com/rust-lang/cargo/pull/6331)

### Changed
- 🔥 HTTP/2 multiplexing is now enabled by default. The `http.multiplexing`
  config value may be used to disable it.
  [#6271](https://github.com/rust-lang/cargo/pull/6271)
- Use ANSI escape sequences to clear lines instead of spaces.
  [#6233](https://github.com/rust-lang/cargo/pull/6233)
- Disable git templates when checking out git dependencies, which can cause
  problems. [#6252](https://github.com/rust-lang/cargo/pull/6252)
- Include the `--update-head-ok` git flag when using the
  `net.git-fetch-with-cli` option. This can help prevent failures when
  fetching some repositories.
  [#6250](https://github.com/rust-lang/cargo/pull/6250)
- When extracting a crate during the verification step of `cargo package`, the
  filesystem mtimes are no longer set, which was failing on some rare
  filesystems. [#6257](https://github.com/rust-lang/cargo/pull/6257)
- `crate-type = ["proc-macro"]` is now treated the same as `proc-macro = true`
  in `Cargo.toml`. [#6256](https://github.com/rust-lang/cargo/pull/6256)
- An error is raised if `dependencies`, `features`, `target`, or `badges` is
  set in a virtual workspace. Warnings are displayed if `replace` or `patch`
  is used in a workspace member.
  [#6276](https://github.com/rust-lang/cargo/pull/6276)
- Improved performance of the resolver in some cases.
  [#6283](https://github.com/rust-lang/cargo/pull/6283)
  [#6366](https://github.com/rust-lang/cargo/pull/6366)
- `.rmeta` files are no longer hard-linked into the base target directory
  (`target/debug`). [#6292](https://github.com/rust-lang/cargo/pull/6292)
- A warning is issued if multiple targets are built with the same output
  filenames. [#6308](https://github.com/rust-lang/cargo/pull/6308)
- When using `cargo build` (without `--release`) benchmarks are now built
  using the "test" profile instead of "bench". This makes it easier to debug
  benchmarks, and avoids confusing behavior.
  [#6309](https://github.com/rust-lang/cargo/pull/6309)
- User aliases may now override built-in aliases (`b`, `r`, `t`, and `c`).
  [#6259](https://github.com/rust-lang/cargo/pull/6259)
- Setting `autobins=false` now disables auto-discovery of inferred targets.
  [#6329](https://github.com/rust-lang/cargo/pull/6329)
- `cargo verify-project` will now fail on stable if the project uses unstable
  features. [#6326](https://github.com/rust-lang/cargo/pull/6326)
- Platform targets with an internal `.` within the name are now allowed.
  [#6255](https://github.com/rust-lang/cargo/pull/6255)
- `cargo clean --release` now only deletes the release directory.
  [#6349](https://github.com/rust-lang/cargo/pull/6349)

### Fixed
- Avoid adding extra angle brackets in email address for `cargo new`.
  [#6243](https://github.com/rust-lang/cargo/pull/6243)
- The progress bar is disabled if the CI environment variable is set.
  [#6281](https://github.com/rust-lang/cargo/pull/6281)
- Avoid retaining all rustc output in memory.
  [#6289](https://github.com/rust-lang/cargo/pull/6289)
- If JSON parsing fails, and rustc exits nonzero, don't lose the parse failure
  message. [#6290](https://github.com/rust-lang/cargo/pull/6290)
- (Nightly only): `--out-dir` no longer copies over build scripts.
  [#6300](https://github.com/rust-lang/cargo/pull/6300)
- Fixed renaming a project directory with build scripts.
  [#6328](https://github.com/rust-lang/cargo/pull/6328)
- Fixed `cargo run --example NAME` to work correctly if the example sets
  `crate_type = ["bin"]`.
  [#6330](https://github.com/rust-lang/cargo/pull/6330)
- Fixed issue with `cargo package` git discovery being too aggressive. The
  `--allow-dirty` now completely disables the git repo checks.
  [#6280](https://github.com/rust-lang/cargo/pull/6280)
- Fixed build change tracking for `[patch]` deps which resulted in `cargo
  build` rebuilding when it shouldn't.
  [#6493](https://github.com/rust-lang/cargo/pull/6493)

## Cargo 1.31 (2019-12-06)
[36d96825...339d9f9c](https://github.com/rust-lang/cargo/compare/36d96825...339d9f9c)

### Added
- 🔥 Stabilized support for the 2018 edition.
  [#5984](https://github.com/rust-lang/cargo/pull/5984)
  [#5989](https://github.com/rust-lang/cargo/pull/5989)
- 🔥 Added the ability to [rename
  dependencies](https://doc.rust-lang.org/1.31.0/cargo/reference/specifying-dependencies.html#renaming-dependencies-in-cargotoml)
  in Cargo.toml. [#6319](https://github.com/rust-lang/cargo/pull/6319)
- 🔥 Added support for HTTP/2 pipelining and multiplexing. Set the
  `http.multiplexing` config value to enable.
  [#6005](https://github.com/rust-lang/cargo/pull/6005)
- (Nightly only): Added `--registry` flag to `cargo install`.
  [#6128](https://github.com/rust-lang/cargo/pull/6128)
- (Nightly only): Added `registry.default` configuration value to specify the
  default registry to use if `--registry` flag is not passed.
  [#6135](https://github.com/rust-lang/cargo/pull/6135)
- (Nightly only): Added `--registry` flag to `cargo new` and `cargo init`.
  [#6135](https://github.com/rust-lang/cargo/pull/6135)
- Added `http.debug` configuration value to debug HTTP connections. Use
  `CARGO_HTTP_DEBUG=true RUST_LOG=cargo::ops::registry cargo build` to display
  the debug information. [#6166](https://github.com/rust-lang/cargo/pull/6166)
- `CARGO_PKG_REPOSITORY` environment variable is set with the repository value
  from `Cargo.toml` when building .
  [#6096](https://github.com/rust-lang/cargo/pull/6096)

### Changed
- `cargo test --doc` now rejects other flags instead of ignoring them.
  [#6037](https://github.com/rust-lang/cargo/pull/6037)
- `cargo install` ignores `~/.cargo/config`.
  [#6026](https://github.com/rust-lang/cargo/pull/6026)
- `cargo version --verbose` is now the same as `cargo -vV`.
  [#6076](https://github.com/rust-lang/cargo/pull/6076)
- Comments at the top of `Cargo.lock` are now preserved.
  [#6181](https://github.com/rust-lang/cargo/pull/6181)
- When building in "very verbose" mode (`cargo build -vv`), build script
  output is prefixed with the package name and version, such as `[foo 0.0.1]`.
  [#6164](https://github.com/rust-lang/cargo/pull/6164)
- If `cargo fix --broken-code` fails to compile after fixes have been applied,
  the files are no longer reverted and are left in their broken state.
  [#6316](https://github.com/rust-lang/cargo/pull/6316)

### Fixed
- Windows: Pass Ctrl-C to the process with `cargo run`.
  [#6004](https://github.com/rust-lang/cargo/pull/6004)
- macOS: Fix bash completion.
  [#6038](https://github.com/rust-lang/cargo/pull/6038)
- Support arbitrary toolchain names when completing `+toolchain` in bash
  completion. [#6038](https://github.com/rust-lang/cargo/pull/6038)
- Fixed edge cases in the resolver, when backtracking on failed dependencies.
  [#5988](https://github.com/rust-lang/cargo/pull/5988)
- Fixed `cargo test --all-targets` running lib tests three times.
  [#6039](https://github.com/rust-lang/cargo/pull/6039)
- Fixed publishing renamed dependencies to crates.io.
  [#5993](https://github.com/rust-lang/cargo/pull/5993)
- Fixed `cargo install` on a git repo with multiple binaries.
  [#6060](https://github.com/rust-lang/cargo/pull/6060)
- Fixed deeply nested JSON emitted by rustc being lost.
  [#6081](https://github.com/rust-lang/cargo/pull/6081)
- Windows: Fix locking msys terminals to 60 characters.
  [#6122](https://github.com/rust-lang/cargo/pull/6122)
- Fixed renamed dependencies with dashes.
  [#6140](https://github.com/rust-lang/cargo/pull/6140)
- Fixed linking against the wrong dylib when the dylib existed in both
  `target/debug` and `target/debug/deps`.
  [#6167](https://github.com/rust-lang/cargo/pull/6167)
- Fixed some unnecessary recompiles when `panic=abort` is used.
  [#6170](https://github.com/rust-lang/cargo/pull/6170)

## Cargo 1.30 (2019-10-25)
[524a578d...36d96825](https://github.com/rust-lang/cargo/compare/524a578d...36d96825)

### Added
- 🔥 Added an animated progress bar shows progress during building.
  [#5995](https://github.com/rust-lang/cargo/pull/5995/)
- Added `resolve.nodes.deps` key to `cargo metadata`, which includes more
  information about resolved dependencies, and properly handles renamed
  dependencies. [#5871](https://github.com/rust-lang/cargo/pull/5871)
- When creating a package, provide more detail with `-v` when failing to
  discover if files are dirty in a git repository. Also fix a problem with
  discovery on Windows. [#5858](https://github.com/rust-lang/cargo/pull/5858)
- Filters like `--bin`, `--test`, `--example`, `--bench`, or `--lib` can be
  used in a workspace without selecting a specific package.
  [#5873](https://github.com/rust-lang/cargo/pull/5873)
- `cargo run` can be used in a workspace without selecting a specific package.
  [#5877](https://github.com/rust-lang/cargo/pull/5877)
- `cargo doc --message-format=json` now outputs JSON messages from rustdoc.
  [#5878](https://github.com/rust-lang/cargo/pull/5878)
- Added `--message-format=short` to show one-line messages.
  [#5879](https://github.com/rust-lang/cargo/pull/5879)
- Added `.cargo_vcs_info.json` file to `.crate` packages that captures the
  current git hash. [#5886](https://github.com/rust-lang/cargo/pull/5886)
- Added `net.git-fetch-with-cli` configuration option to use the `git`
  executable to fetch repositories instead of using the built-in libgit2
  library. [#5914](https://github.com/rust-lang/cargo/pull/5914)
- Added `required-features` to `cargo metadata`.
  [#5902](https://github.com/rust-lang/cargo/pull/5902)
- `cargo uninstall` within a package will now uninstall that package.
  [#5927](https://github.com/rust-lang/cargo/pull/5927)
- (Nightly only): Added
  [metabuild](https://doc.rust-lang.org/1.30.0/cargo/reference/unstable.html#metabuild).
  [#5628](https://github.com/rust-lang/cargo/pull/5628)
- Added `--allow-staged` flag to `cargo fix` to allow it to run if files are
  staged in git. [#5943](https://github.com/rust-lang/cargo/pull/5943)
- Added `net.low-speed-limit` config value, and also honor `net.timeout` for
  http operations. [#5957](https://github.com/rust-lang/cargo/pull/5957)
- Added `--edition` flag to `cargo new`.
  [#5984](https://github.com/rust-lang/cargo/pull/5984)
- Temporarily stabilized 2018 edition support for the duration of the beta.
  [#5984](https://github.com/rust-lang/cargo/pull/5984)
  [#5989](https://github.com/rust-lang/cargo/pull/5989)
- Added support for `target.'cfg(…)'.runner` config value to specify the
  run/test/bench runner for config-expressioned targets.
  [#5959](https://github.com/rust-lang/cargo/pull/5959)

### Changed
- Windows: `cargo run` will not kill child processes when the main process
  exits. [#5887](https://github.com/rust-lang/cargo/pull/5887)
- Switched to the `opener` crate to open a web browser with `cargo doc
  --open`. This should more reliably select the system-preferred browser on
  all platforms. [#5888](https://github.com/rust-lang/cargo/pull/5888)
- Equal file mtimes now cause a target to be rebuilt. Previously only if files
  were strictly *newer* than the last build would it cause a rebuild.
  [#5919](https://github.com/rust-lang/cargo/pull/5919)
- Ignore `build.target` config value when running `cargo install`.
  [#5874](https://github.com/rust-lang/cargo/pull/5874)
- Ignore `RUSTC_WRAPPER` for `cargo fix`.
  [#5983](https://github.com/rust-lang/cargo/pull/5983)
- Ignore empty `RUSTC_WRAPPER`.
  [#5985](https://github.com/rust-lang/cargo/pull/5985)

### Fixed
- Fixed error when creating a package with an edition field in `Cargo.toml`.
  [#5908](https://github.com/rust-lang/cargo/pull/5908)
- More consistently use relative paths for path dependencies in a workspace.
  [#5935](https://github.com/rust-lang/cargo/pull/5935)
- `cargo fix` now always runs, even if it was run previously.
  [#5944](https://github.com/rust-lang/cargo/pull/5944)
- Windows: Attempt to more reliably detect terminal width. msys-based
  terminals are forced to 60 characters wide.
  [#6010](https://github.com/rust-lang/cargo/pull/6010)
- Allow multiple target flags with `cargo doc --document-private-items`.
  [6022](https://github.com/rust-lang/cargo/pull/6022)