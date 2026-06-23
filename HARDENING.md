<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Timmyy3000--enkii/v0.2.0-beta.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation of `${{ inputs.openrouter_api_key }}` inside a `run:` shell command string. The line `run: echo "::add-mask::${{ inputs.openrouter_api_key }}"` embeds the inputs expression directly into the shell command before the shell ever sees it, allowing an attacker-controlled value to inject shell metacharacters. The value should be passed via an `env:` variable and then referenced as `"$ENV_VAR"` in the shell.

Locations:

- `action.yml:55`

### unpinned-uses (severity: high)

The composite action step `uses: oven-sh/setup-bun@v2` references a mutable tag (`v2`) instead of a full 40-character commit SHA. If the tag is moved or the repository is compromised, the action will silently execute different code. Pin to a specific commit SHA, e.g. `oven-sh/setup-bun@<40-char-sha> # v2`.

Locations:

- `action.yml:58`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.openrouter_api_key }}" appears directly in run: block of step "Mask OpenRouter API key"; move to env: map

Locations:

- `action.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed three findings in action.yml: (1) Moved `${{ inputs.openrouter_api_key }}` from the `run:` shell string of the 'Mask OpenRouter API key' step into an `env:` block as `OPENROUTER_API_KEY`, referencing it as `$OPENROUTER_API_KEY` in the shell to prevent script injection. (2) Pinned `oven-sh/setup-bun@v2` to its full commit SHA `0c5077e51419868618aeaa5fe8019c62421857d6` with a `# v2` comment.

