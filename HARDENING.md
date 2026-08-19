<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.6** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is interpolated directly inside a `run:` shell command string. The 'Mask OpenRouter API key' step in action.yml contains: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`

The value of `inputs.openrouter_api_key` is controlled by the calling workflow and is substituted into the shell command string before the shell ever sees it. An attacker-supplied value containing shell metacharacters (e.g. newlines, semicolons, command substitution) could break out of the `echo` command and execute arbitrary shell code. The fix is to pass the value via an `env:` variable and reference it as a quoted shell variable: `run: echo "::add-mask::$OPENROUTER_API_KEY"` with `env: OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}`.

Locations:

- `action.yml:82`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v4` — a mutable tag reference rather than a pinned full-length commit SHA. A tag can be moved by the upstream repository owner (or a compromised account) to point to a different, potentially malicious commit, enabling a supply-chain attack. Replace with a full 40-character SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/ci.yml:16`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:69`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

1. Fixed script-injection/static-inline-injection in action.yml: The 'Mask OpenRouter API key' step previously used `echo "::add-mask::${{ inputs.openrouter_api_key }}"` which interpolated the expression directly into the shell command. Fixed by adding an `env:` block with `OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}` and changing the run command to `echo "::add-mask::$OPENROUTER_API_KEY"`. 2. Fixed unpinned-uses in .github/workflows/ci.yml: Replaced `actions/checkout@v4` with the pinned SHA `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`.

