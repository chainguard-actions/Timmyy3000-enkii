<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Timmyy3000--enkii/v0.2.0-beta.4** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Mask OpenRouter API key' step directly interpolates a ${{ inputs.openrouter_api_key }} expression inside a run: shell command string: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`  Any attacker-controlled value in inputs.openrouter_api_key is substituted into the shell command before the shell parses it, enabling script injection.

Locations:

- `action.yml:68`

### unpinned-uses (severity: high)

The workflow uses actions/checkout@v4 — a mutable tag reference rather than a pinned 40-character commit SHA. This is vulnerable to supply-chain attacks if the tag is moved. Use a full SHA pin such as actions/checkout@<40-hex-sha> # v4 instead.

Locations:

- `.github/workflows/ci.yml:17`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:69`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed two issues: (1) In action.yml line 68-69, moved `${{ inputs.openrouter_api_key }}` out of the `run:` shell string and into an `env:` block as `OPENROUTER_API_KEY`, referencing it as `$OPENROUTER_API_KEY` in the shell command to prevent script injection. (2) In .github/workflows/ci.yml line 17, pinned `actions/checkout@v4` to the full commit SHA `actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4` to prevent supply-chain attacks from mutable tag references.

