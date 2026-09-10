<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: The 'Mask OpenRouter API key' step directly interpolates a ${{ inputs.* }} expression inside a run: shell command string. The line `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"` causes the GitHub Actions template engine to substitute the input value into the shell command before the shell executes it. A crafted value for `openrouter_api_key` containing shell metacharacters (e.g. `", `, $(...), newlines) could break out of the echo argument and execute arbitrary commands. The safe pattern is to pass the value via an env: variable and reference it as a quoted shell variable: `env: { KEY: "${{ inputs.openrouter_api_key }}" }` then `run: echo "::add-mask::$KEY"`.

Locations:

- `action.yml:76`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:73`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed the script injection vulnerability in the 'Mask OpenRouter API key' step of hardened/action/action.yml. Moved the `${{ inputs.openrouter_api_key }}` expression from the `run:` shell command string into an `env:` block (as `OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}`), and updated the shell command to reference it as the safe shell variable `$OPENROUTER_API_KEY`. This prevents crafted input values containing shell metacharacters from executing arbitrary commands.

