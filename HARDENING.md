<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.3.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.3.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Mask OpenRouter API key' step directly interpolates `${{ inputs.openrouter_api_key }}` inside a `run:` shell command string: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`.

Before the shell ever sees the command, GitHub Actions performs YAML template substitution, expanding the expression inline. If the input value contains shell metacharacters (e.g. `$(...)`, backticks, `;`, `&`), they will be interpreted by bash. The value should instead be passed via an `env:` variable and referenced as `"$OPENROUTER_API_KEY"` inside the run block.

Locations:

- `action.yml:82`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:91`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed the 'Mask OpenRouter API key' step in action.yml (line 82/91): moved `${{ inputs.openrouter_api_key }}` out of the `run:` shell string and into an `env:` block as `OPENROUTER_API_KEY`. The shell command now references it as `$OPENROUTER_API_KEY`, preventing shell metacharacters in the API key value from being interpreted by bash.

