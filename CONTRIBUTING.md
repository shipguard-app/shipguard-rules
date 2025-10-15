# Contributing to Ship Guard Community Rules

First off, thank you for considering contributing to Ship Guard Community Rules! It's people like you that make this community-driven project thrive.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Getting Started](#getting-started)
- [Contribution Workflow](#contribution-workflow)
- [Rule Development Guidelines](#rule-development-guidelines)
- [Style Guide](#style-guide)
- [Testing Your Rules](#testing-your-rules)
- [Pull Request Process](#pull-request-process)
- [Community](#community)

## Code of Conduct

This project and everyone participating in it is governed by our Code of Conduct. By participating, you are expected to uphold this code. Please report unacceptable behavior to conduct@shipguard.dev.

### Our Standards

- **Be respectful**: Treat everyone with respect and kindness
- **Be inclusive**: Welcome diverse perspectives and experiences
- **Be collaborative**: Work together and help each other
- **Be constructive**: Provide helpful feedback and suggestions
- **Be patient**: Remember that everyone has different skill levels

## How Can I Contribute?

### 1. Submit New Rules

Create new rules that solve common code review problems. See [Rule Development Guidelines](#rule-development-guidelines).

### 2. Improve Existing Rules

- Enhance rule prompts for better accuracy
- Add more comprehensive path patterns
- Improve documentation and examples
- Fix bugs or edge cases

### 3. Report Issues

- Bug reports for broken rules
- Feature requests for new rule types
- Documentation improvements
- Performance issues

### 4. Review Pull Requests

Help review and test rules submitted by other contributors.

### 5. Improve Documentation

- Add usage examples
- Create tutorials or guides
- Translate documentation
- Fix typos and clarify explanations

## Getting Started

### Prerequisites

- Git installed on your local machine
- GitHub account
- Basic understanding of YAML syntax
- (Optional) Ship Guard account for testing

### Development Setup

1. **Fork the repository**
   
   Click the "Fork" button at the top right of the repository page.

2. **Clone your fork**
````bash
   git clone https://github.com/YOUR_USERNAME/community-rules.git
   cd community-rules
````

3. **Add upstream remote**
````bash
   git remote add upstream https://github.com/shipguard/community-rules.git
````

4. **Create a new branch**
````bash
   git checkout -b feature/add-react-hooks-rule
````

## Contribution Workflow

### Step-by-Step Process

#### 1. Check Existing Issues

Before starting work, check if an issue exists for what you want to do. If not, create one to discuss your idea.

#### 2. Create a New Branch

Use descriptive branch names:
````bash
# For new rules
git checkout -b add-rule/no-default-exports

# For improvements
git checkout -b improve/security-secrets-detection

# For bug fixes
git checkout -b fix/console-log-false-positives

# For documentation
git checkout -b docs/add-react-examples
````

#### 3. Make Your Changes

Follow the [Rule Development Guidelines](#rule-development-guidelines) below.

#### 4. Test Your Rule

See [Testing Your Rules](#testing-your-rules) section.

#### 5. Commit Your Changes

Write clear, descriptive commit messages:
````bash
git add rules/security/new-rule.yml
git commit -m "Add rule: Detect insecure random number generation

- Checks for Math.random() in security contexts
- Suggests crypto.randomBytes() alternative
- Applies to JS/TS files only
- Excludes test files"
````

#### 6. Push to Your Fork
````bash
git push origin add-rule/no-default-exports
````

#### 7. Create a Pull Request

Go to the original repository and click "New Pull Request". Fill out the PR template completely.

## Rule Development Guidelines

### Directory Structure
````
rules/
├── code-quality/          # Code style and maintainability
├── security/              # Security vulnerability detection
├── testing/               # Test coverage and quality
├── performance/           # Performance optimization
├── documentation/         # Documentation requirements
├── workflow/              # Git and PR management
├── frameworks/            # Framework-specific rules
│   ├── react/
│   ├── vue/
│   ├── django/
│   └── spring/
└── languages/             # Language-specific rules
    ├── python/
    ├── javascript/
    ├── go/
    └── rust/
````

### File Naming Conventions

- Use lowercase with hyphens: `no-console-log.yml`
- Be descriptive but concise: `detect-sql-injection.yml`
- Avoid abbreviations unless universally understood
- Include category if ambiguous: `react-hooks-rules.yml`

### Rule Template

Use this template as a starting point:
````yaml
# [One-line description of what this rule does]
#
# Use Case:
# [Explain when and why someone would use this rule]
#
# Example violation:
# [Show code that would trigger this rule]
#
# Example fix:
# [Show how to fix the violation]

name: "[Descriptive Rule Name]"
type: "ai-review" | "banned-terms" | "file-size"
severity: "error" | "warning" | "info"
paths:
  include:
    - "**/*.js"
    - "**/*.ts"
  exclude:
    - "**/*.test.js"
    - "**/*.spec.ts"
    - "**/node_modules/**"
    - "**/dist/**"
    - "**/build/**"

# For banned-terms rules:
terms:
  - "console.log"
  - "debugger"
message: "[Clear explanation of why this is flagged]"

# For ai-review rules:
prompt: |
  You are a [role, e.g., "senior JavaScript developer" or "security expert"] reviewing code for [specific concern].
  
  Analyze the code changes for:
  1. [Specific thing to check]
  2. [Another specific thing]
  3. [Another specific thing]
  
  Look for patterns such as:
  - [Example pattern 1]
  - [Example pattern 2]
  - [Example pattern 3]
  
  If you find issues:
  - Explain what the problem is
  - Why it matters (security/performance/maintainability)
  - Provide specific code suggestions to fix it
  
  If the code looks good, briefly confirm it follows best practices.
````

### Writing Effective AI Prompts

#### ✅ Good Prompt Example
````yaml
prompt: |
  You are a React expert reviewing component code for accessibility issues.
  
  Check for these specific accessibility problems:
  1. Images without alt attributes
  2. Buttons that are actually divs with onClick handlers
  3. Form inputs without associated labels
  4. Missing ARIA roles on interactive elements
  5. Insufficient color contrast (if visible in code)
  
  For each issue found:
  - Quote the problematic line
  - Explain the accessibility impact
  - Provide a corrected code example
  
  If the code is accessible, confirm it follows WCAG guidelines.
````

#### ❌ Bad Prompt Example
````yaml
prompt: |
  Check for accessibility issues.
````

**Why it's bad**: Too vague, no specific guidance, no examples, no instructions for output format.

### Path Configuration Best Practices

#### ✅ Good Path Configuration
````yaml
paths:
  include:
    # Specific source directories
    - "src/**/*.{js,jsx,ts,tsx}"
    - "lib/**/*.{js,ts}"
  exclude:
    # Test files
    - "**/*.test.{js,jsx,ts,tsx}"
    - "**/*.spec.{js,ts}"
    - "**/__tests__/**"
    
    # Generated files
    - "**/*.min.js"
    - "**/dist/**"
    - "**/build/**"
    - "**/coverage/**"
    
    # Dependencies
    - "**/node_modules/**"
    - "**/vendor/**"
    
    # Configuration
    - "**/*.config.js"
    - "**/.*rc.js"
````

#### ❌ Bad Path Configuration
````yaml
paths:
  include:
    - "**/*"  # Too broad, will check everything including dependencies
````

### Severity Guidelines

| Severity | When to Use | Examples |
|----------|-------------|----------|
| `error` | Security vulnerabilities, data loss risks, breaking changes | Hardcoded secrets, SQL injection, missing authentication |
| `warning` | Code quality issues, performance concerns, deprecated APIs | Console logs, unused variables, N+1 queries |
| `info` | Suggestions, educational content, optional improvements | Documentation reminders, formatting suggestions |

## Style Guide

### YAML Formatting

- Use 2 spaces for indentation (never tabs)
- Use double quotes for strings with special characters
- Use single quotes or no quotes for simple strings
- Add blank lines between major sections
- Align multi-line strings with `|` or `>` indicators
````yaml
# Good
name: "No Console Logs"
type: "banned-terms"
severity: "warning"

paths:
  include:
    - "src/**/*.js"
  exclude:
    - "**/*.test.js"

terms:
  - "console.log"
  - "console.warn"

message: "Avoid using console.log() in production code."
````

### Comment Style
````yaml
# Main rule description (required)
# Additional context about when to use this rule (recommended)
#
# Example violation (helpful):
# const password = "hardcoded123";
#
# Example fix (helpful):
# const password = process.env.DB_PASSWORD;

name: "Rule Name"
# ... rest of rule
````

### Documentation Style

- Use clear, concise language
- Write for developers of all experience levels
- Include examples and use cases
- Use active voice: "Check for" not "Checks for"
- Be specific about what the rule does

## Testing Your Rules

### Manual Testing

1. **Create a test repository**

   Set up a small test repository with code that should trigger your rule.

2. **Add your rule**

   Create `.github/ship-guard.yml` and add your rule.

3. **Create a test PR**

   Make changes that should trigger the rule and create a pull request.

4. **Verify the results**

   Check that Ship Guard correctly identifies the issues.

### Test Checklist

- [ ] Rule triggers on code that should violate it
- [ ] Rule doesn't trigger on code that shouldn't violate it
- [ ] Paths are correctly scoped (includes and excludes work)
- [ ] Severity level is appropriate
- [ ] Error messages/prompts are clear and actionable
- [ ] Rule doesn't have excessive false positives
- [ ] Performance is acceptable (especially for AI rules with broad paths)

### Example Test Cases

Document test cases in your PR description:
````markdown
## Test Cases

### Should Trigger (True Positives)

✅ File: `src/utils/auth.js`
```javascript
const API_KEY = "sk-1234567890abcdef";  // Should flag
```

✅ File: `src/components/Form.tsx`
```typescript
console.log("Debug info");  // Should flag
```

### Should Not Trigger (True Negatives)

✅ File: `src/utils/auth.test.js`
```javascript
console.log("Test output");  // Should be excluded
```

✅ File: `src/config.js`
```javascript
const API_KEY = process.env.API_KEY;  // Correct pattern
```
````

## Pull Request Process

### PR Title Format

Use conventional commit format:
````
feat: Add React Hooks dependency rule
fix: Correct false positives in SQL injection rule
docs: Add usage examples for security rules
refactor: Improve path patterns in no-console-log rule
test: Add test cases for XSS prevention rule
````

### PR Description Template
````markdown
## Description
[Clear description of what this PR does]

## Motivation
[Why is this change needed? What problem does it solve?]

## Rule Details
- **Name**: [Rule name]
- **Type**: [banned-terms | ai-review | file-size]
- **Severity**: [error | warning | info]
- **Category**: [code-quality | security | testing | etc.]

## Testing
[Describe how you tested this rule]

### Test Cases
[Include example code that should/shouldn't trigger the rule]

## Checklist
- [ ] Rule follows naming conventions
- [ ] File is in the correct category directory
- [ ] Includes descriptive comments
- [ ] Paths are properly scoped
- [ ] Tested in a real repository
- [ ] README.md updated with new rule entry
- [ ] No merge conflicts with main branch
- [ ] Follows YAML style guide

## Screenshots (if applicable)
[Add screenshots of the rule in action]

## Additional Notes
[Any other context or considerations]
````

### Review Process

1. **Automated Checks**: CI will run linting and validation
2. **Community Review**: At least one maintainer will review
3. **Testing**: Reviewers may test the rule in their own projects
4. **Feedback**: Address any requested changes
5. **Approval**: Once approved, your PR will be merged!

### After Your PR is Merged

- Your contribution will be added to the contributors list
- The rule will be available in the next release
- You can share your rule with the community on social media!

## Community

### Getting Help

- **GitHub Discussions**: Ask questions and share ideas
- **Discord**: Join our community chat
- **Issues**: Report bugs or request features
- **Email**: contribute@shipguard.dev

### Recognition

We value all contributions! Contributors are recognized in:
- The README contributors section
- Release notes
- GitHub contributor graph
- Special shout-outs on social media

### Maintainer Guidelines

If you're interested in becoming a maintainer, please reach out! We look for contributors who:
- Have submitted multiple high-quality rules
- Actively participate in reviews
- Help others in discussions
- Follow our code of conduct

## Questions?

Don't hesitate to ask! We're here to help:

- Email us at contribute@shipguard.dev

---

Thank you for contributing to Ship Guard Community Rules! 🚀

Your contributions help thousands of developers write better code.