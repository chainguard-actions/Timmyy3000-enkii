<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.0.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string. The step 'Mask OpenRouter API key' contains: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`  — the inputs.openrouter_api_key value is expanded by the GitHub Actions template engine before the shell ever sees it, allowing an attacker-controlled value to inject shell metacharacters. The value should be passed via an env: variable and referenced as a shell variable instead.

Locations:

- `action.yml:83`

### unpinned-uses (severity: high)

The workflow file ci.yml references `actions/checkout@v4` using a mutable tag instead of a full 40-character commit SHA. This is vulnerable to supply-chain attacks if the tag is moved. It should be pinned to a specific SHA (e.g., `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

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

1. Fixed script injection in action.yml 'Mask OpenRouter API key' step: moved ${{ inputs.openrouter_api_key }} from the run: shell string into an env: variable (OPENROUTER_API_KEY), then referenced it as $OPENROUTER_API_KEY in the echo command. This prevents the GitHub Actions template engine from expanding the value directly into the shell command string. 2. Pinned actions/checkout@v4 in .github/workflows/ci.yml to full SHA 11d5960a326750d5838078e36cf38b85af677262 with # v4 comment for readability.

