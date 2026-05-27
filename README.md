# bump-golangci-lint

A composite action that fetches the latest `golangci-lint` release and updates a target file with the new version.

## Usage

Update a YAML value located by path:

```yaml
- uses: craigsloggett/bump-golangci-lint@v1
  with:
    file: action.yml
    path: .inputs.golangci-lint-version.default
```

Update a line by regex:

```yaml
- uses: craigsloggett/bump-golangci-lint@v1
  with:
    file: Makefile
    match: '^GOLANGCI_LINT_VERSION'
    replace: 'GOLANGCI_LINT_VERSION := {version}'
```

Open a pull request with the bump:

```yaml
- name: Bump golangci-lint
  id: bump-golangci-lint
  uses: craigsloggett/bump-golangci-lint@v1
  with:
    file: Makefile
    match: '^GOLANGCI_LINT_VERSION'
    replace: 'GOLANGCI_LINT_VERSION := {version}'

- name: Open Pull Request
  uses: craigsloggett/create-github-pull-request@v1
  with:
    title: 'chore(build): Update golangci-lint to ${{ steps.bump-golangci-lint.outputs.version }}'
    branch: update-golangci-lint-to-${{ steps.bump-golangci-lint.outputs.version }}
```

## Inputs

| Input     | Required | Default | Description                                                                                  |
| --------- | -------- | ------- | -------------------------------------------------------------------------------------------- |
| `file`    | Yes      |         | Path to the file to update.                                                                  |
| `path`    | No       |         | yq expression locating the line to update (e.g. `.inputs.golangci-lint-version.default`).    |
| `match`   | No       |         | Regex matching the line to rewrite. Pair with `replace`.                                     |
| `replace` | No       |         | Replacement line. Use `{version}` as the placeholder for the new version. Pair with `match`. |

Provide either `path` (YAML mode) or `match`+`replace` (line mode), not both.

For YAML replacements:

- The path must begin with `.` and resolve to an existing line.
- The first `vX.Y.Z` substring on the resolved line is rewritten; the rest of the line is preserved.

For line-based replacements:

- The match pattern must match exactly one line in the file.
- The action errors out if zero or more than one lines match.
- `{version}` is the only placeholder recognized in `replace`.

## Outputs

| Output    | Description                                                                                   |
| --------- | --------------------------------------------------------------------------------------------- |
| `version` | The latest `golangci-lint` version, as reported by the Go module proxy.                       |
| `changed` | `true` if the file was modified by this run, `false` if it was already at the latest version. |
