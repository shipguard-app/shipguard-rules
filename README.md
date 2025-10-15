# Ship Guard Community Rules

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

A community-curated collection of rule templates to supercharge your automated code reviews with [Ship Guard](https://shipguard.dev).

---
## What is Ship Guard?
Ship Guard is an AI-powered co-pilot for your pull requests. It automatically scans for security vulnerabilities, code quality issues, and bugs, providing instant feedback directly in your GitHub workflow. These rules allow you to customize and extend its capabilities.

## How to Use These Rules

1.  Find a rule you want to use from the `rules/` directory in this repository.
2.  Copy the content of the rule's `.yml` file.
3.  Paste it as a new entry under the `rules:` section in your own repository's `.github/ship-guard.yml` file.

**Example:**
Let's say you want to use the `no-console-log.yml` rule.

You would copy its content and add it to your configuration like this:

```yaml
# In your repository's .github/ship-guard.yml

rules:
  # Your existing rules...
  - name: "Block console.log in JavaScript files"
    condition: "banned-terms"
    severity: "warning"
    terms: ["console.log", "console.warn"]
    paths:
      - "src/**/*.js"
      - "src/**/*.ts"
      - "src/**/*.jsx"
      - "src/**/*.tsx"
    excludePaths:
      - "**/*.spec.ts"
      - "**/*.test.ts"
