<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string. In the 'Mask OpenRouter API key' step, the line `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"` embeds the raw value of `inputs.openrouter_api_key` directly into the shell command via YAML template substitution before the shell ever parses it. A crafted API key value containing shell metacharacters (e.g. `$(...)`, backticks, newlines) could cause command injection. The fix is to pass the value via an env: variable and reference it as `$ENV_VAR` in the shell: `env:\n  API_KEY: ${{ inputs.openrouter_api_key }}\nrun: echo "::add-mask::$API_KEY"`.

Locations:

- `action.yml:88`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:91`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed the script injection vulnerability in the 'Mask OpenRouter API key' step of hardened/action/action.yml. Moved `${{ inputs.openrouter_api_key }}` from the `run:` shell command into an `env:` block as `API_KEY`, and updated the run command to reference `$API_KEY` instead. This prevents shell metacharacters in the API key value from being interpreted as shell commands.

