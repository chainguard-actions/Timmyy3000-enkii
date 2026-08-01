<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.5** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Mask OpenRouter API key' step in action.yml directly interpolates `${{ inputs.openrouter_api_key }}` inside a `run:` shell command string: `echo "::add-mask::${{ inputs.openrouter_api_key }}"`.

GitHub Actions performs YAML template substitution before the shell ever sees the string, so a crafted value for `inputs.openrouter_api_key` (e.g. containing newlines, semicolons, or command substitution syntax) could inject arbitrary shell commands. The value should be passed via an `env:` variable and referenced as `$ENV_VAR` in the shell instead:

```yaml
env:
  OR_KEY: ${{ inputs.openrouter_api_key }}
run: echo "::add-mask::$OR_KEY"
```

Locations:

- `action.yml:76`

### unpinned-uses (severity: high)

The workflow file ci.yml references `actions/checkout@v4`, which uses a mutable tag rather than a full 40-character commit SHA. A mutable tag can be silently moved to point to a different (potentially malicious) commit, enabling a supply-chain attack. Replace with a pinned SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/ci.yml:16`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:69`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

1. Fixed script-injection and static-inline-injection in hardened/action/action.yml: moved `${{ inputs.openrouter_api_key }}` out of the `run:` shell string in the 'Mask OpenRouter API key' step into an `env:` block as `OR_KEY`, then referenced it as `$OR_KEY` in the shell command. 2. Fixed unpinned-uses in hardened/action/.github/workflows/ci.yml: replaced `actions/checkout@v4` with the pinned SHA `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`.

