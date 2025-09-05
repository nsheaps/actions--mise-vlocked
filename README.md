# actions--mise-vlocked

A passthrough to [jdx/mise-action](https://github.com/jdx/mise-action) with all the same inputs except the version defaults to a locked version of mise, managed by renovate (automerged) and published under the same versions with supplements (aka v2/v2.1/v2.1.3/v2.1.3-mise-3.23.5).

## Usage

This action works exactly like `jdx/mise-action@v2`, but with a pre-configured locked version of mise that is automatically updated by Renovate.

```yaml
name: test
on:
  pull_request:
    branches:
      - main
  push:
    branches:
      - main
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: nsheaps/actions--mise-vlocked@v2
        with:
          # version is optional and defaults to the locked version
          # version: 2025.9.0
          install: true # [default: true] run `mise install`
          install_args: "bun" # [default: ""] additional arguments to `mise install`
          cache: true # [default: true] cache mise using GitHub's cache
          experimental: true # [default: false] enable experimental features
          log_level: debug # [default: info] log level
          # automatically write this .tool-versions file
          tool_versions: |
            shellcheck 0.9.0
          # or, if you prefer .mise.toml format:
          mise_toml: |
            [tools]
            shellcheck = "0.9.0"
          working_directory: app # [default: .] directory to run mise in
          reshim: false # [default: false] run `mise reshim -f`
          github_token: ${{ secrets.GITHUB_TOKEN }} # [default: ${{ github.token }}] GitHub token for API authentication
      - run: shellcheck scripts/*.sh
```

## Benefits

- **Automatic Updates**: Renovate automatically updates the mise version and auto-merges the PR
- **Stability**: Uses a locked version by default instead of "latest"
- **Drop-in Replacement**: All the same inputs and outputs as the original action
- **Consistent Versions**: Teams get the same mise version across environments

## Current Locked Version

The current locked version is managed by Renovate in the `action.yml` file. Check the default value for the `version` input to see the current locked version.

## All Features

This action supports all the same features as [jdx/mise-action](https://github.com/jdx/mise-action), including:

- Cache configuration with custom keys and templates
- GitHub API rate limit handling
- Tool installation and configuration
- Environment variable management
- Multiple configuration file formats (.tool-versions, .mise.toml)

For detailed documentation on all features, see the [jdx/mise-action README](https://github.com/jdx/mise-action#readme).
