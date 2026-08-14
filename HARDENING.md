<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.2** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Mask OpenRouter API key' step directly interpolates `${{ inputs.openrouter_api_key }}` inside a `run:` shell command string: `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"`  This allows an attacker-controlled input value to be injected into the shell command before the shell ever parses it. The value should be passed via an `env:` variable instead, e.g. `run: echo "::add-mask::$OPENROUTER_API_KEY"` with `env: OPENROUTER_API_KEY: ${{ inputs.openrouter_api_key }}`.

Locations:

- `action.yml:55`

### unpinned-uses (severity: high)

Two `uses:` references are pinned to mutable tags rather than immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the referenced tag is moved or overwritten:
- `oven-sh/setup-bun@v2` (action.yml, line 59) — should be pinned to a full SHA, e.g. `oven-sh/setup-bun@<40-char-sha> # v2`
- `actions/checkout@v4` (.github/workflows/enkii-review.yml, line 26) — should be pinned to a full SHA, e.g. `actions/checkout@<40-char-sha> # v4`

Locations:

- `action.yml:59`
- `.github/workflows/enkii-review.yml:26`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed three findings in two files: (1) In action.yml line 55-57, moved `${{ inputs.openrouter_api_key }}` out of the run: shell string and into an env: block as OPENROUTER_API_KEY, then referenced it as $OPENROUTER_API_KEY in the shell command to prevent script injection. (2) In action.yml line 59, pinned oven-sh/setup-bun@v2 to the full SHA 0c5077e51419868618aeaa5fe8019c62421857d6 with # v2 comment. (3) In .github/workflows/enkii-review.yml line 26, pinned actions/checkout@v4 to the full SHA 11d5960a326750d5838078e36cf38b85af677262 with # v4 comment.

