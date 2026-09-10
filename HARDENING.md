<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Mask OpenRouter API key' step directly interpolates `${{ inputs.openrouter_api_key }}` inside a `run:` shell command string: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`.

GitHub Actions performs template substitution before the shell processes the string, so a caller-supplied value containing shell metacharacters (e.g. `"; malicious-command #`) could inject arbitrary shell commands. The value should instead be passed via an `env:` variable and referenced as `$OPENROUTER_API_KEY` inside the script.

Locations:

- `action.yml:84`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:91`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed the 'Mask OpenRouter API key' step in hardened/action/action.yml. Moved `${{ inputs.openrouter_api_key }}` from the `run:` shell command string into an `env:` block as `OPENROUTER_API_KEY`, and updated the run command to reference `$OPENROUTER_API_KEY` as a plain environment variable. This prevents shell injection since GitHub Actions template substitution no longer occurs within the shell command string itself.

