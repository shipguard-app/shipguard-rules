# Ship Guard Community Rules

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![GitHub Stars](https://img.shields.io/github/stars/shipguard/community-rules?style=social)](https://github.com/shipguard-app/shipguard-rules)

> A comprehensive, community-curated collection of battle-tested Ship Guard rules for automated code review.

## 🚀 About Ship Guard

[Ship Guard](https://shipguard.dev) is an AI-powered code review assistant that integrates directly with your GitHub pull requests. By defining rules in a `.github/ship-guard.yml` configuration file, you can automate code quality checks, security reviews, and best practice enforcement across your entire development workflow.

This repository serves as the central hub for sharing and discovering reusable Ship Guard rules created by the community.

## 📋 Table of Contents

- [Quick Start](#quick-start)
- [Available Rules](#available-rules)
- [Rule Categories](#rule-categories)
- [Installation](#installation)
- [Usage Examples](#usage-examples)
- [Contributing](#contributing)
- [Rule Schema Reference](#rule-schema-reference)
- [Best Practices](#best-practices)
- [Community & Support](#community--support)
- [License](#license)

## ⚡ Quick Start

### 1. Find a Rule

Browse the [`rules/`](rules/) directory to find rules organized by category.

### 2. Copy to Your Project

Create or edit `.github/ship-guard.yml` in your repository:
````yaml
version: 1
rules:
  # Paste rule content here
````

### 3. Customize (Optional)

Adjust paths, severity levels, or prompts to match your project's needs.

### 4. Commit and Push

Ship Guard will automatically start reviewing your pull requests based on these rules!

## 📚 Available Rules

### Code Quality (5 rules)

| Rule | Description | Severity | Type |
|------|-------------|----------|------|
| [No Console Logs](rules/code-quality/no-console-log.yml) | Prevents debug console statements in production code | Warning | banned-terms |
| [No TODO Comments](rules/code-quality/no-todos.yml) | Flags unresolved TODO comments before merging | Warning | banned-terms |
| [Enforce Code Documentation](rules/code-quality/require-docstrings.yml) | Ensures public functions have documentation | Error | ai-review |
| [Prevent Magic Numbers](rules/code-quality/no-magic-numbers.yml) | Detects unexplained numeric literals | Warning | ai-review |
| [Check Code Complexity](rules/code-quality/complexity-check.yml) | Reviews functions for excessive complexity | Warning | ai-review |

### Security (8 rules)

| Rule | Description | Severity | Type |
|------|-------------|----------|------|
| [Block Hardcoded Secrets](rules/security/block-hardcoded-secrets.yml) | Detects API keys, passwords, and credentials | Error | ai-review |
| [SQL Injection Prevention](rules/security/sql-injection.yml) | Identifies potential SQL injection vulnerabilities | Error | ai-review |
| [XSS Prevention](rules/security/xss-prevention.yml) | Checks for cross-site scripting risks | Error | ai-review |
| [Insecure Dependencies](rules/security/dependency-check.yml) | Reviews package additions for known vulnerabilities | Error | ai-review |
| [Require HTTPS](rules/security/enforce-https.yml) | Ensures HTTP URLs are not used | Warning | banned-terms |
| [Dangerous Functions](rules/security/dangerous-functions.yml) | Flags use of eval(), exec(), and similar | Error | banned-terms |
| [Path Traversal Check](rules/security/path-traversal.yml) | Detects potential path traversal vulnerabilities | Error | ai-review |
| [Crypto Best Practices](rules/security/crypto-review.yml) | Reviews cryptographic implementations | Error | ai-review |

### Testing (4 rules)

| Rule | Description | Severity | Type |
|------|-------------|----------|------|
| [Require Tests](rules/testing/require-tests.yml) | Ensures new features include tests | Error | ai-review |
| [No Skipped Tests](rules/testing/no-skip-tests.yml) | Flags `.skip()` or `.only()` in test files | Error | banned-terms |
| [Test Coverage Check](rules/testing/coverage-review.yml) | Reviews test coverage for new code | Warning | ai-review |
| [Mock Best Practices](rules/testing/mock-review.yml) | Ensures proper test isolation and mocking | Info | ai-review |

### Performance (3 rules)

| Rule | Description | Severity | Type |
|------|-------------|----------|------|
| [N+1 Query Detection](rules/performance/n-plus-one.yml) | Identifies potential database N+1 query issues | Warning | ai-review |
| [Large File Warning](rules/performance/large-files.yml) | Flags files exceeding size thresholds | Warning | file-size |
| [Memory Leak Patterns](rules/performance/memory-leaks.yml) | Detects common memory leak patterns | Warning | ai-review |

### Documentation (3 rules)

| Rule | Description | Severity | Type |
|------|-------------|----------|------|
| [README Updates](rules/documentation/readme-sync.yml) | Ensures README stays current with code changes | Info | ai-review |
| [API Documentation](rules/documentation/api-docs.yml) | Reviews API endpoint documentation | Warning | ai-review |
| [Breaking Changes Doc](rules/documentation/breaking-changes.yml) | Requires documentation for breaking changes | Error | ai-review |

### Git & Workflow (2 rules)

| Rule | Description | Severity | Type |
|------|-------------|----------|------|
| [Commit Message Quality](rules/workflow/commit-messages.yml) | Reviews commit message clarity | Info | ai-review |
| [PR Size Check](rules/workflow/pr-size.yml) | Warns about overly large pull requests | Warning | ai-review |

## 🎯 Rule Categories

Rules are organized into the following categories:

- **`code-quality/`** - Code style, maintainability, and best practices
- **`security/`** - Vulnerability detection and security reviews
- **`testing/`** - Test coverage and quality enforcement
- **`performance/`** - Performance optimization checks
- **`documentation/`** - Documentation requirements and quality
- **`workflow/`** - Git workflow and PR management
- **`frameworks/`** - Framework-specific rules (React, Vue, Django, etc.)
- **`languages/`** - Language-specific rules (Python, JavaScript, Go, etc.)

## 📥 Installation

### Option 1: Copy Individual Rules

Navigate to a rule file, copy its contents, and paste into your `.github/ship-guard.yml`.

### Option 2: Use Multiple Rules

Combine multiple rules in your configuration:
````yaml
version: 1
rules:
  - name: "Block Hardcoded Secrets"
    type: "ai-review"
    severity: "error"
    paths:
      include: ["**/*"]
      exclude: ["*.lock", "*.min.js"]
    prompt: |
      Scan for hardcoded API keys, tokens, and credentials...

  - name: "No Console Logs"
    type: "banned-terms"
    severity: "warning"
    paths:
      include: ["src/**/*.{js,ts,jsx,tsx}"]
      exclude: ["**/*.test.*"]
    terms: ["console.log", "console.warn"]
    message: "Use a proper logging library instead."

  - name: "Require Tests"
    type: "ai-review"
    severity: "error"
    paths:
      include: ["src/**/*.{js,ts}"]
      exclude: ["**/*.test.*", "**/*.spec.*"]
    prompt: |
      Check if this code change includes corresponding tests...
````

### Option 3: Import Base Configuration (Coming Soon)
````yaml
version: 1
extends:
  - "shipguard/community-rules/presets/javascript-strict"
  - "shipguard/community-rules/presets/security-essential"
````

## 💡 Usage Examples

### Example 1: Security-First Setup

Perfect for applications handling sensitive data:
````yaml
version: 1
rules:
  # Include all security rules
  - !include rules/security/block-hardcoded-secrets.yml
  - !include rules/security/sql-injection.yml
  - !include rules/security/xss-prevention.yml
  - !include rules/security/enforce-https.yml
````

### Example 2: Code Quality Focus

Ideal for maintaining high code standards:
````yaml
version: 1
rules:
  - !include rules/code-quality/no-console-log.yml
  - !include rules/code-quality/require-docstrings.yml
  - !include rules/code-quality/complexity-check.yml
  - !include rules/testing/require-tests.yml
````

### Example 3: Custom Severity Levels

Adjust rule severity for your team's workflow:
````yaml
version: 1
rules:
  - name: "No Console Logs"
    type: "banned-terms"
    severity: "error"  # Upgraded from warning to error
    paths:
      include: ["src/**/*.ts"]
    terms: ["console.log"]
````

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

### Quick Contribution Checklist

- [ ] Rule follows the [schema reference](#rule-schema-reference)
- [ ] File is placed in the correct category folder
- [ ] Rule name is descriptive and follows naming conventions
- [ ] Includes comments explaining purpose and use cases
- [ ] Paths are properly scoped with include/exclude
- [ ] README is updated with the new rule entry
- [ ] Tested in a real repository (if possible)

## 📖 Rule Schema Reference

### Rule Types

Ship Guard supports three types of rules:

#### 1. `banned-terms`
Searches for specific strings or patterns in code.
````yaml
name: "Rule Name"
type: "banned-terms"
severity: "error" | "warning" | "info"
paths:
  include: ["**/*.js"]
  exclude: ["**/*.test.js"]
terms:
  - "console.log"
  - "debugger"
message: "Custom message to display"
````

#### 2. `ai-review`
Uses AI to perform contextual code analysis.
````yaml
name: "Rule Name"
type: "ai-review"
severity: "error" | "warning" | "info"
paths:
  include: ["src/**/*"]
  exclude: ["*.lock"]
prompt: |
  You are an expert reviewer. Analyze the code for...
  - Specific thing 1
  - Specific thing 2
  Provide actionable feedback.
````

#### 3. `file-size`
Checks file sizes against thresholds.
````yaml
name: "Rule Name"
type: "file-size"
severity: "warning"
paths:
  include: ["**/*"]
max_size: "100KB"
message: "File exceeds size limit"
````

### Path Patterns

Use glob patterns for file matching:

- `**/*` - All files recursively
- `src/**/*.js` - All JS files in src directory
- `**/*.{js,ts}` - All JS and TS files
- `!**/*.test.js` - Exclude test files (alternative to exclude array)

### Severity Levels

- **`error`** - Blocks PR merge, requires resolution
- **`warning`** - Flags issue but allows merge
- **`info`** - Informational only, no blocking

## 🎓 Best Practices

### Writing Effective AI Prompts

1. **Be Specific**: Clearly state what to look for
2. **Provide Context**: Explain why the rule matters
3. **Give Examples**: Show patterns to detect
4. **Request Actionable Feedback**: Ask for specific improvement suggestions
5. **Set Boundaries**: Specify what NOT to flag

### Scoping Rules Properly
````yaml
# ✅ Good: Specific and targeted
paths:
  include: ["src/**/*.{ts,tsx}"]
  exclude: 
    - "**/*.test.*"
    - "**/*.spec.*"
    - "**/fixtures/**"
    - "**/__mocks__/**"

# ❌ Bad: Too broad, will slow reviews
paths:
  include: ["**/*"]
````

### Choosing Severity Levels

- Use `error` for security issues, critical bugs, or strict requirements
- Use `warning` for code quality, best practices, or potential issues
- Use `info` for suggestions, educational content, or optional improvements

## 🌐 Community & Support

- **GitHub Issues**: [Report bugs or request features](https://github.com/shipguard/community-rules/issues)
- **Discussions**: [Ask questions and share ideas](https://github.com/shipguard/community-rules/discussions)
- **Ship Guard Docs**: [Official documentation](https://docs.shipguard.dev)
- **Discord**: [Join our community](https://discord.gg/shipguard)

## 🗺️ Roadmap

- [ ] Framework-specific rule packs (React, Vue, Angular, Django)
- [ ] Language-specific rule collections (Python, Go, Rust, Java)
- [ ] Preset configurations for common use cases
- [ ] Rule validation and testing framework
- [ ] Interactive rule builder tool
- [ ] VS Code extension for rule development

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

Built with ❤️ by the Ship Guard community. Special thanks to all [contributors](https://github.com/shipguard/community-rules/graphs/contributors) who have helped make this project better.

---

**Ready to improve your code review process?** [Get started with Ship Guard](https://shipguard.dev) today!
