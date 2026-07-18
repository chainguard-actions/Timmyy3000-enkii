<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.4** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ inputs.openrouter_api_key }} expression is directly interpolated inside a run: shell command string. The line `echo "::add-mask::${{ inputs.openrouter_api_key }}"` passes the raw input value through YAML template substitution before the shell ever sees it, allowing an attacker-controlled value to inject shell metacharacters. The value should be passed via an env: variable and then referenced as a quoted shell variable (e.g., `echo "::add-mask::$INPUT_KEY"`).

Locations:

- `action.yml:73`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v4` which is pinned to a mutable tag rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit. It should be replaced with a full SHA pin, e.g. `actions/checkout@<40-char-sha> # v4`.

Locations:

- `.github/workflows/ci.yml:14`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:69`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

1. Fixed script-injection/static-inline-injection in action.yml: moved `${{ inputs.openrouter_api_key }}` out of the `run:` shell string in the 'Mask OpenRouter API key' step into an `env:` block (`OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}`), then referenced it as `$OPENROUTER_API_KEY` in the shell command. 2. Fixed unpinned-uses in .github/workflows/ci.yml: replaced `actions/checkout@v4` with `actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4` using the resolved full commit SHA.

