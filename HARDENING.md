<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.4.1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the 'Mask OpenRouter API key' step puts an input expression directly into a run: script: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`. Fix: pass the value through an env: variable and write it double-quoted, e.g. `echo "::add-mask::$OPENROUTER_API_KEY"`.

Locations:

- `action.yml:99`

### script-injection (severity: high)

Rule (a): the 'Mask Anthropic API key' step puts an input expression directly into a run: script: `run: echo "::add-mask::${{ inputs.anthropic_api_key }}"`. Fix: pass the value through an env: variable and write it double-quoted.

Locations:

- `action.yml:104`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:100`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.anthropic_api_key }}" appears directly in run: block of step "Mask Anthropic API key"; move to env: map

Locations:

- `action.yml:105`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

In action.yml the two mask steps now take inputs.openrouter_api_key and inputs.anthropic_api_key through env: as OPENROUTER_API_KEY and ANTHROPIC_API_KEY. The scripts reference them double-quoted: echo "::add-mask::$VAR". A follow-up search found no other ${{ }} expressions on any run: line.

