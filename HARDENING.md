<!-- markdownlint-disable -->

# Hardening Report: Timmyy3000--enkii/v0.2.0-beta.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **Timmyy3000--enkii/v0.2.0-beta.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Mask OpenRouter API key' step directly interpolates ${{ inputs.openrouter_api_key }} inside a run: shell command string. Before the shell executes the command, GitHub Actions performs template substitution, so a malicious value in the input could inject arbitrary shell commands. The offending line is: run: echo "::add-mask::${{ inputs.openrouter_api_key }}"

Locations:

- `action.yml:55`

### unpinned-uses (severity: high)

The composite action uses oven-sh/setup-bun@v2, which references a mutable tag (@v2) rather than a pinned 40-character commit SHA. This means the action could be silently updated to a malicious version without the consuming workflow's knowledge, enabling supply-chain attacks.

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

Fixed all three findings in action.yml: (1) Moved ${{ inputs.openrouter_api_key }} from the 'Mask OpenRouter API key' run: shell string into an env: block (OPENROUTER_API_KEY), referencing it as $OPENROUTER_API_KEY in the shell command to prevent script injection. (2) Pinned oven-sh/setup-bun@v2 to its full commit SHA (0c5077e51419868618aeaa5fe8019c62421857d6) with a # v2 comment for readability.

