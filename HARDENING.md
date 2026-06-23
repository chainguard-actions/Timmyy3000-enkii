<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Timmyy3000--enkii/v0.2.0-beta.2** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is directly interpolated inside a `run:` shell command string. In the 'Mask OpenRouter API key' step, `${{ inputs.openrouter_api_key }}` is embedded directly in the shell command: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`. This causes the expression to be substituted into the shell command before the shell executes it, allowing an attacker-controlled value to inject shell metacharacters.

Locations:

- `action.yml:56`

### unpinned-uses (severity: high)

The action uses `oven-sh/setup-bun@v2`, which references a mutable tag (`v2`) rather than a pinned 40-character commit SHA. A compromised or updated tag could silently change the code executed by this action, enabling a supply-chain attack. It should be pinned to a full SHA, e.g. `oven-sh/setup-bun@<40-char-sha> # v2`.

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

Fixed two categories of issues in action.yml: (1) Script injection in 'Mask OpenRouter API key' step — moved `${{ inputs.openrouter_api_key }}` from the run: shell string into an env: block as OPENROUTER_API_KEY, then referenced it as $OPENROUTER_API_KEY in the shell command. (2) Unpinned action — pinned `oven-sh/setup-bun@v2` to its full commit SHA `0c5077e51419868618aeaa5fe8019c62421857d6` with a `# v2` comment.

