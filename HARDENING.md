<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Mask OpenRouter API key' step directly interpolates `${{ inputs.openrouter_api_key }}` inside a `run:` shell command string: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`.

GitHub Actions performs YAML template substitution before the shell processes the command, so if `inputs.openrouter_api_key` contains shell metacharacters (`;`, `|`, `$(...)`, etc.), they will be interpreted by bash. The value should be passed via an `env:` variable and referenced as `$ENV_VAR` in the shell command instead.

Locations:

- `action.yml:59`

### unpinned-uses (severity: high)

The action uses `oven-sh/setup-bun@v2`, which is pinned to a mutable tag (`@v2`) rather than an immutable 40-character commit SHA. A compromised or updated tag could silently introduce malicious code into the action's execution. It should be pinned to a full SHA, e.g. `uses: oven-sh/setup-bun@<40-char-sha> # v2`.

Locations:

- `action.yml:62`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed two issues in hardened/action/action.yml: (1) Moved `${{ inputs.openrouter_api_key }}` out of the 'Mask OpenRouter API key' run: block into an env: variable (OPENROUTER_API_KEY), referencing it as $OPENROUTER_API_KEY in the shell command — this prevents shell metacharacter injection. (2) Pinned `oven-sh/setup-bun@v2` to its full immutable commit SHA `0c5077e51419868618aeaa5fe8019c62421857d6` with a `# v2` comment.

