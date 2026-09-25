<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string. In the 'Mask OpenRouter API key' step, the line `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"` embeds the user-supplied input directly into the shell command before the shell ever sees it. An attacker who controls the value of `inputs.openrouter_api_key` (e.g. via a workflow_dispatch or a calling workflow) could inject arbitrary shell commands. The value should be passed via an env: variable and referenced as `$ENV_VAR` instead.

Locations:

- `action.yml:63`

### unpinned-uses (severity: high)

The action uses `oven-sh/setup-bun@v2`, which is pinned to a mutable tag (`@v2`) rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, enabling a supply-chain attack. It should be replaced with a full SHA pin, e.g. `oven-sh/setup-bun@<40-char-sha> # v2`.

Locations:

- `action.yml:67`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed two issues in hardened/action/action.yml: (1) Moved `${{ inputs.openrouter_api_key }}` out of the 'Mask OpenRouter API key' run: shell string and into an env: block, referencing it as `$OPENROUTER_API_KEY` to prevent script injection. (2) Pinned `oven-sh/setup-bun@v2` to its full immutable commit SHA `oven-sh/setup-bun@0c5077e51419868618aeaa5fe8019c62421857d6 # v2`.

