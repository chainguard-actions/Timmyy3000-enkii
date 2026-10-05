<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.3.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is directly interpolated inside a `run:` shell command string. In the 'Mask OpenRouter API key' step, `${{ inputs.openrouter_api_key }}` is embedded directly in the shell command: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`. Before the shell ever sees the command, GitHub Actions substitutes the expression value verbatim into the shell string. If the API key value contains shell metacharacters or newlines, this can lead to command injection. The safe pattern is to pass the value via an `env:` variable and reference it as `$ENV_VAR` in the shell script.

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

Fixed the 'Mask OpenRouter API key' step in action.yml: moved `${{ inputs.openrouter_api_key }}` from the inline `run:` shell string into an `env:` block as `OPENROUTER_API_KEY`, and updated the shell command to reference `$OPENROUTER_API_KEY` instead. This prevents shell metacharacters in the API key from being interpreted as shell commands.

