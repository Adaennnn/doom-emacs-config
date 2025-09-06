---
allowed-tools: Task, Bash(git:*)
argument-hint: [optional: issue-numbers to reference]
description: Create well-structured commits following conventional commit standards
---

# 📝 COMMIT FEATURE: Structured Commit Creation

Create professional, semantic commits for changes made during implementation. This command is typically used after `/implement-task` to commit the completed work.

Use the Task tool with the **commit-craftsman** agent to analyze changes and create professional, semantic commits that clearly communicate changes and maintain project history quality.

Note: For simple, straightforward commits, you may proceed without the agent. For complex changes or when commit message quality is critical, invoke the commit-craftsman agent for assistance.

## Pre-Commit Analysis:

### 1. **Analyze Changes**
Run in parallel to understand the scope:
```bash
git status
git diff --staged
git diff
git log --oneline -5
```

### 2. **Categorize Changes**
Determine the commit type based on changes:

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | Adding customer search |
| `fix` | Bug fix | Correcting validation logic |
| `refactor` | Code restructuring | Extracting hook logic |
| `test` | Adding/updating tests | New test coverage |
| `docs` | Documentation only | README updates |
| `style` | Formatting, no logic change | Code formatting |
| `perf` | Performance improvement | Query optimization |
| `chore` | Maintenance tasks | Dependency updates |
| `build` | Build system changes | Webpack config |
| `ci` | CI/CD changes | GitHub Actions |

### 3. **Identify Scope**
Determine the module/component affected:
- Module name (e.g., `decryption`, `customer-intel`)
- Component name (e.g., `Button`, `FileUpload`)
- Feature area (e.g., `auth`, `navigation`)

## Commit Message Structure:

### Format:
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Rules:
1. **Subject Line** (required):
   - Maximum 50 characters
   - Imperative mood ("Add" not "Added")
   - No period at the end
   - Capitalize first letter

2. **Body** (optional but recommended):
   - Wrap at 72 characters
   - Explain what and why, not how
   - Bullet points for multiple changes
   - Reference motivation and context

3. **Footer** (when applicable):
   - Issue references: `Resolves #123`, `Fixes #456`
   - Breaking changes: `BREAKING CHANGE: description`
   - Co-authors: `Co-authored-by: Name <email>`

## Smart Commit Creation:

### 1. **Stage Intelligent Groups**
Group related changes:
```bash
# Stage by feature area
git add src/modules/[module-name]/*
git add src/modules/[module-name]/**/*.test.tsx

# Or stage interactively for precision
git add -p
```

### 2. **Generate Commit Message**

Based on staged changes, create message:

#### Example 1: Feature Implementation
```bash
git commit -m "$(cat <<'EOF'
feat(customer-intel): Add customer search with fuzzy matching

- Implement fuzzy search algorithm for customer names
- Add search debouncing for performance
- Include tier-based filtering options
- Update TypeScript interfaces for search results

The search supports partial matches and handles special
characters gracefully. Results are sorted by relevance
and tier priority.

Resolves #42
EOF
)"
```

#### Example 2: Bug Fix
```bash
git commit -m "$(cat <<'EOF'
fix(decryption): Correct file size validation for edge cases

- Handle files exactly at 10MB limit
- Fix byte calculation for different encodings
- Add proper error messages for size violations

Previously, files at exactly 10MB were incorrectly rejected.
This fix ensures the validation boundary is inclusive.

Fixes #38
EOF
)"
```

#### Example 3: Multiple Related Changes
```bash
git commit -m "$(cat <<'EOF'
refactor(module-registry): Improve type safety and lazy loading

- Add strict TypeScript types for module config
- Implement proper error boundaries for modules
- Optimize bundle splitting with dynamic imports
- Update tests for new module structure

This refactoring improves type safety throughout the module
system and reduces initial bundle size by 15%.

Related to #35, #36
EOF
)"
```

### 3. **Verify Commit Quality**

After committing, verify:
```bash
# Check the commit message
git log -1 --pretty=fuller

# Verify files included
git show --stat

# Ensure no unintended files
git status
```

### 4. **Amend if Necessary**

If adjustments needed:
```bash
# Add forgotten files
git add [missed-file]
git commit --amend --no-edit

# Fix commit message
git commit --amend -m "Updated message"
```

## Multi-Commit Strategy:

For larger features, create logical commit sequence:

```bash
# 1. Infrastructure/setup
git commit -m "chore(customer-intel): Set up module structure and types"

# 2. Core implementation
git commit -m "feat(customer-intel): Implement customer data service"

# 3. UI components
git commit -m "feat(customer-intel): Add customer profile components"

# 4. Tests
git commit -m "test(customer-intel): Add comprehensive test coverage"

# 5. Documentation
git commit -m "docs(customer-intel): Add usage documentation and examples"
```

## Issue Linking:

When referencing issues ($ARGUMENTS):
- Single issue: `Resolves #$ARGUMENTS`
- Multiple issues: `Resolves #$1, Fixes #$2`
- Partial work: `Related to #$ARGUMENTS`
- Multiple in footer:
  ```
  Resolves #123
  Fixes #456
  Related to #789
  ```

## Post-Commit Actions:

1. **Update Issue Status**
If issue numbers provided:
```bash
gh issue comment $ARGUMENTS --body "Committed: [commit message subject]
Commit: $(git rev-parse HEAD)"
```

2. **Prepare for Push**
Review before pushing:
```bash
# Review commit history
git log --oneline -5

# Check branch status
git status

# Verify remote tracking
git branch -vv
```

## Best Practices:

- **Atomic Commits**: Each commit should be one logical change
- **Build-Safe**: Each commit should pass tests and build
- **Clear History**: Commits tell the story of the feature
- **Searchable**: Use consistent types and scopes
- **Reviewable**: Make code review easier with clear commits