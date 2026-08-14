<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.1** was hardened automatically. 3 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Mask OpenRouter API key' step directly interpolates the expression `${{ inputs.openrouter_api_key }}` inside the `run:` shell command string: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`.

Before the shell ever sees the command, GitHub Actions performs YAML template substitution, embedding the raw value of `inputs.openrouter_api_key` into the shell string. A caller supplying a value containing shell metacharacters (e.g. `$(...)`, backticks, `;`, newlines) could achieve command injection. The fix is to pass the value via an `env:` variable and reference it as `$ENV_VAR` in the shell command, e.g.:

```yaml
env:
  API_KEY: ${{ inputs.openrouter_api_key }}
run: echo "::add-mask::$API_KEY"
```

Locations:

- `action.yml:56`

### unpinned-uses (severity: high)

The composite action step `uses: oven-sh/setup-bun@v2` references a mutable tag (`@v2`) rather than a pinned 40-character commit SHA. If the `oven-sh/setup-bun` repository is compromised or the tag is moved, the action will silently execute different code. Pin to a full SHA, e.g. `uses: oven-sh/setup-bun@<40-char-sha> # v2`.

Locations:

- `action.yml:59`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

1. Fixed script injection in 'Mask OpenRouter API key' step: moved `${{ inputs.openrouter_api_key }}` from the run: shell string into an env: block as `API_KEY`, and updated the run command to use `$API_KEY`. 2. Pinned `oven-sh/setup-bun@v2` to full commit SHA `0c5077e51419868618aeaa5fe8019c62421857d6` with `# v2` comment for readability.

### Iteration 2

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `actions/checkout@v4` to its full commit SHA `11d5960a326750d5838078e36cf38b85af677262` in `.github/workflows/enkii-review.yml` (line 26), preserving the `# v4` comment for readability. No other findings were present; the workflow already had a minimal `permissions:` block.

