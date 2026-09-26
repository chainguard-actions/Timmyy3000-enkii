<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: The 'Mask OpenRouter API key' step directly interpolates a ${{ inputs.* }} expression inside a run: shell command string. The offending line is: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`

GitHub Actions substitutes the expression value into the shell command string before the shell processes it. If the API key value contains shell metacharacters (`;`, `|`, `&`, `$(...)`, backticks, etc.), they will be interpreted by the shell. The safe pattern is to pass the value via an env: variable and reference it as `$ENV_VAR` inside the run: block, e.g.:

```yaml
env:
  OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}
run: echo "::add-mask::$OPENROUTER_API_KEY"
```

Locations:

- `action.yml:72`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:69`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed the 'Mask OpenRouter API key' step in action.yml by moving the ${{ inputs.openrouter_api_key }} expression from the run: shell command into an env: block (OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}), then referencing it as $OPENROUTER_API_KEY in the run: command. This prevents shell metacharacters in the API key from being interpreted by the shell.

