# bump-golangci-lint

A composite action to bump the golangci-lint version.

## Usage

```yaml
name: Bump

on:
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch:

permissions:
  contents: write
  pull-requests: write

jobs:
  bump:
    name: golangci-lint
    runs-on: ubuntu-24.04
    steps:
      - name: Checkout
        uses: actions/checkout@v6

      - name: Bump golangci-lint
        uses: craigsloggett/bump-golangci-lint@v1
```

### Inputs

| Input            | Required? | Default                    | Description                                        |
| ---------------- | --------- | -------------------------- | -------------------------------------------------- |
| `my-input`       | `false`   | `The default description.` | This is my input, there is no other input like it. |

### Outputs

| Output      | Description                                |
| ----------- | ------------------------------------------ |
| `my-output` | The output this composite action produces. |
