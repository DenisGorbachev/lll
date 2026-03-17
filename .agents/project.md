# LLM Linter concepts

## `lll` package

A Rust package that implements a CLI for running LLM-based lints.

- Must have dependencies:
  - `openai_dive`

## AskOneCommand

- Must have fields:
  - `shell: PathBuf`
    - Default: "/bin/sh"
  - `question: String`
  - `dir: PathBuf`
  - `silent: bool`
- Must have a `filter` field
  - Must be a [path filter field](#path-filter-field)
- Must have methods:
  - `run`
    - Must get context from dir
    - Must call an LLM
      - Must use openai_dive
      - Must use openai_auth_ext
      - Must use completions API
    - Must exit with a [boolish exit code](#boolish-exit-code)

Notes:

- This command works similarly to calling `codex` with a boolean schema, but with a custom exit status

## AskManyCommand

## AskMultiCommand

- Must have fields
  - `queries: ReaderKind`
- Must have methods
  - `run`
    - Must read the path, properties pairs

## Path filter field

- Must have a type `Option<String>`
- Must have a doc comment: "A shell script that is executed in dir cwd, should accept the relative path as the first argument, should return 0 to include the path, should return 7 to exclude the path. Any other status code is treated as error."

## ReaderKind

A Rust enum.

- Must have variants
  - `Stdin`
  - `File(PathBuf)`
- Must have methods:
  - `to_reader`
    - Must return `impl Read`

## Boolish exit code

An `u8` whose value has the following meanings:

- 0 - success, true
- 1 - failure
- 7 - success, false
