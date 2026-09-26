<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.5** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string. In the 'Mask OpenRouter API key' step, `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"` substitutes the value of inputs.openrouter_api_key directly into the shell command before the shell processes it. An attacker who controls this input (e.g. via workflow_dispatch or a calling workflow) could inject shell metacharacters. The value should be passed via an env: variable and referenced as a quoted shell variable instead.

Locations:

- `action.yml:68`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:69`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed the script injection vulnerability in the 'Mask OpenRouter API key' step in hardened/action/action.yml. The ${{ inputs.openrouter_api_key }} expression was moved from the run: shell command string into an env: block as OPENROUTER_API_KEY, and the run: command now references it as the shell variable $OPENROUTER_API_KEY. This prevents attacker-controlled input from being interpreted as shell metacharacters.

