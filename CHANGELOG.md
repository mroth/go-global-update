# Changelog

## Unreleased

- Include go 1.19 in CI <https://github.com/Gelio/go-global-update/pull/18>.

## v0.2.2 (2022-04-03)

### Fixed

- Allow combining short options in a single CLI argument
  <https://github.com/Gelio/go-global-update/pull/17>.

  Short flags can be used in a single CLI argument, POSIX-style:

  ```sh
  go-global-update -nv
  ```

  is equivalent to

  ```sh
  go-global-update -n -v
  ```

## v0.2.1 (2022-04-03)

### Added

- Short aliases for `--dry-run` and `--verbose` flags
  <https://github.com/Gelio/go-global-update/pull/16>.

  There are new alases for some of the options that the tools accepts:

  - `-n` is an alias for `--dry-run`
  - `-v` is an alias for `--verbose`

  This should make it easier to use the tool for experienced users.

  **BREAKING CHANGE**: a potential breaking change is that the `-v` flag was
  previously an alias for `--version` and now is an alias for `--verbose`.
  Running `go-global-update -v` will run the update process instead of only
  printing the version. To print the version, use the `-V` flag (or the full
  `--version` flag).

## v0.2.0 (2022-04-02)

### Improved

- Improve output by formatting the binary versions summary in a table and adding
  colors <https://github.com/Gelio/go-global-update/pull/10>.

  This should make the output easier and faster to understand.

  ![go-global-update output with colors](https://user-images.githubusercontent.com/889383/161372879-8cf4bd33-ced2-45ad-a27d-888b15ae0dbc.png)

  Colors are enabled when the output is a TTY (for example if the command is run
  directly in the terminal and the output is shown in the terminal). They will
  be disabled if the output is saved to a file or in some temporary buffer.

  Colors can be force-enabled by passing the `--colors` flag to the CLI. In the
  following example, `less -r` will render the colors even though the command
  did not print to the terminal directly.

  ```sh
  go-global-update --colors > output.txt
  less -r output.txt
  ```

  Colors will not be shown if the `NO_COLOR` environment variable is defined,
  regardless of its value (abides by <https://no-color.org/>).

### Added

- [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) containing a description of common
  problems faced while updating binaries with `go-global-update`.

- Include detecting common binary update problems right into `go-global-update`.

  Additional messages will be printed in the output when updating a binary fails
  due to a known reason.

  ![screenshot](https://user-images.githubusercontent.com/889383/159443820-3c11044b-016d-4df3-8d33-983aa2b251ba.png)

- List alternative tools to `go-global-update` in the README
  <https://github.com/Gelio/go-global-update/pull/11>.

  Describe their advantages and disadvantages.

### Maintenance

- Add `npm` scripts to format Markdown documents using
  [prettier](https://prettier.io/) and generate table of contents using
  [markdown-toc](https://github.com/jonschlinkert/markdown-toc).

  Those scripts are run CI for more static verification.

- Only verify formatting once in CI (for one matrix build). This is because
  [gofumpt@v0.3.1](https://github.com/mvdan/gofumpt/tree/v0.3.1) cannot be
  installed on go 1.16 and it does not make sense to verify formatting multiple
  times. This also slightly speeds up the CI.

- Add more context in test output if an assertion fails.

## v0.1.2 (2022-03-21)

A patch release adding support for go 1.18.

### Fixed

- Support go 1.18 <https://github.com/Gelio/go-global-update/pull/7>

  Go 1.18 changed the format of the `go version -m [binary-name]` command to no
  longer include the `mod` information for binaries built from source using
  `go build main.go`.

  go-global-update now parses such outputs and does not attempt to check for the
  latest version of such binaries.

## v0.1.1 (2022-03-21)

A patch release containing fixes for some of the edge cases found in
<https://github.com/Gelio/go-global-update/issues/3>.

### Fixed

- Skip updating binaries built from source.

  Binaries built from source (either using `go build main.go` or `go install` in
  the cloned repository) likely have been modified prior to being built.
  Updating them would likely throw away these changes and end up being annoying
  for engineers who want to keep their modified versions.

  Moreover, packages built using `go build main.go` have
  `command-line-arguments` set as their `path` in `go version -m binary-name`.
  This makes it impossible to update automatically.

- Filesystem path handling on Windows.

  Use correct separator for filesystem paths on Windows. This allows using this
  tool on Windows.

## v0.1.0 (2022-03-14)

- Complete the basic functionality of upgrading globally installed executables.
- Add `--dry-run`, `--verbose`, `--debug` flags.
- Support upgrading only specified binaries by accepting arguments.
- Add integration tests and configure CI.
