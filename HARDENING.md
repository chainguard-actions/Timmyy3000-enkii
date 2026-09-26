<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.6** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Mask OpenRouter API key' step directly interpolates a GitHub Actions expression inside a `run:` shell command string. The line `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"` embeds `${{ inputs.openrouter_api_key }}` directly into the shell command before the shell ever sees it. Although the intent is to mask the value, the expression is still substituted into the shell command string by the Actions runner template engine before execution, making it a script-injection risk. The value should instead be passed via an `env:` variable and referenced as `$OPENROUTER_API_KEY` in the shell command.

Locations:

- `action.yml:71`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:69`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed the script injection vulnerability in the 'Mask OpenRouter API key' step in action.yml. Moved `${{ inputs.openrouter_api_key }}` from the `run:` shell command string to an `env:` block as `OPENROUTER_API_KEY`, and updated the shell command to reference `$OPENROUTER_API_KEY` instead. This prevents the GitHub Actions runner template engine from substituting the API key value directly into the shell command string before execution.

