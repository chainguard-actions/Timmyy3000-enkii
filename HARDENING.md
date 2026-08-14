<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Timmyy3000--enkii/v0.2.0-beta.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a `run:` shell command string. The step 'Mask OpenRouter API key' uses `echo "::add-mask::${{ inputs.openrouter_api_key }}"` — the `inputs.openrouter_api_key` value is expanded by the GitHub Actions template engine before the shell ever sees it, allowing an attacker-controlled value to inject shell metacharacters. The value should be passed via an `env:` variable and referenced as `$ENV_VAR` inside the script instead.

Locations:

- `action.yml:60`

### unpinned-uses (severity: high)

One or more `uses:` references are pinned to mutable tags rather than immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the tag is moved or overwritten.

- `action.yml`: `uses: oven-sh/setup-bun@v2` — `v2` is a mutable tag.
- `.github/workflows/enkii-review.yml`: `uses: actions/checkout@v4` — `v4` is a mutable tag.

Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `action.yml:64`
- `.github/workflows/enkii-review.yml:22`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed three findings in hardened/action/: (1) Script injection in 'Mask OpenRouter API key' step — moved `${{ inputs.openrouter_api_key }}` out of the run: shell string into an env: variable and referenced it as `$OPENROUTER_API_KEY`; (2) Pinned `oven-sh/setup-bun@v2` to SHA `0c5077e51419868618aeaa5fe8019c62421857d6` in action.yml; (3) Pinned `actions/checkout@v4` to SHA `34e114876b0b11c390a56381ad16ebd13914f8d5` in .github/workflows/enkii-review.yml.

