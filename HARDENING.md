<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v1.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v1.5.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a), direct expression interpolation. The step 'Mask OpenRouter API key' puts an input expression straight into the shell script: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`. The value is pasted into the script before bash runs, so shell metacharacters in it would execute. Fix: pass it through env (e.g. `OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}`) and use `echo "::add-mask::$OPENROUTER_API_KEY"`.

Locations:

- `action.yml:107`

### script-injection (severity: high)

Rule (a), direct expression interpolation. The step 'Mask Anthropic API key' puts an input expression straight into the shell script: `run: echo "::add-mask::${{ inputs.anthropic_api_key }}"`. Fix: pass it through env and quote the variable, e.g. `"$ANTHROPIC_API_KEY"`.

Locations:

- `action.yml:112`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:108`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.anthropic_api_key }}" appears directly in run: block of step "Mask Anthropic API key"; move to env: map

Locations:

- `action.yml:113`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved inputs.openrouter_api_key and inputs.anthropic_api_key from the run: scripts of the two mask steps into step env: vars (OPENROUTER_API_KEY, ANTHROPIC_API_KEY), referenced as "$VAR" in the bash echo ::add-mask:: commands.

