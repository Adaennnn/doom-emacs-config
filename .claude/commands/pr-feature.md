---
allowed-tools: Bash(git:*), Bash(gh:*), Read, WebFetch
argument-hint: [optional: base-branch (default: main/master)]
description: Create a comprehensive pull request with full context and documentation
---

# 🚀 CREATE FEATURE PR: Professional Pull Request

Generate a comprehensive, review-ready pull request that provides complete context for reviewers.

## Pre-PR Checklist:

### 1. **Analyze Branch Changes**
Run comprehensive analysis in parallel:
```bash
git status
git diff ${1:-main}...HEAD
git log ${1:-main}..HEAD --oneline
git diff ${1:-main}...HEAD --stat
```

### 2. **Quality Verification**
Ensure all checks pass:
```bash
doom doctor
doom sync
emacs --batch --eval "(require 'doom-start)"
# Test configuration loading
emacs --batch --eval "(org-mode)" --eval "(message \"Org-mode loaded successfully\")"
```

### 3. **Push to Remote**
```bash
git push -u origin HEAD
```

## PR Creation:

### Generate PR with Rich Context:

```bash
gh pr create \
  --base "${1:-main}" \
  --title "[PR Title]" \
  --body "$(cat <<'EOF'
## 🎯 Summary

[2-3 sentence overview of what this PR accomplishes]

## 📋 Related Issues

Resolves #[issue-number]
Fixes #[issue-number]
Related to #[issue-number]

## 🔄 Type of Change

- [ ] 🐛 Bug fix (non-breaking change fixing an issue)
- [ ] ✨ New feature (non-breaking change adding functionality)
- [ ] 💥 Breaking change (fix or feature causing existing functionality to change)
- [ ] 📚 Documentation update
- [ ] 🎨 Style update (formatting, renaming)
- [ ] ♻️ Code refactor (no functional changes)
- [ ] ⚡ Performance improvement
- [ ] 🔧 Module configuration update
- [ ] 🎯 GTD workflow enhancement
- [ ] ⌨️ Keybinding update
- [ ] 🎨 Theme/UI change
- [ ] 📝 Org-mode configuration

## 📝 Description

### What Changed?
[Detailed description of the changes made]

### Why These Changes?
[Motivation and context for the changes]

### How Does It Work?
[Technical explanation of the implementation]

## 🖼️ Screenshots/Demo

[If applicable, add screenshots or recordings showing the changes]

<details>
<summary>Before</summary>

[Screenshot/description of previous behavior]

</details>

<details>
<summary>After</summary>

[Screenshot/description of new behavior]

</details>

## 🏗️ Technical Details

### Module Changes
- [List any Doom modules enabled/disabled in init.el]

### New Packages
- [List any new packages added to packages.el and why]

### Configuration Changes
- [List any new settings or customizations in config.el]

### Keybinding Changes
- [List any new keybindings or modifications to existing ones]

## 🧪 Testing

### Configuration Validation
- [ ] `doom doctor` passes without errors
- [ ] `doom sync` completes successfully
- [ ] Configuration loads without warnings
- [ ] All custom functions work as expected

### Manual Testing Steps
1. [Step 1 to test the feature]
2. [Step 2 to test the feature]
3. [Expected results]

### Emacs Testing
- [ ] Fresh Emacs restart loads configuration
- [ ] No conflicts with existing keybindings
- [ ] GTD workflow functions properly
- [ ] Org-mode features work as intended

## 📊 Performance Impact

- Startup time change: [negligible/improved/slight increase]
- Memory usage: [no change/optimized/slight increase]
- Module loading impact: [minimal/noticeable]
- Configuration complexity: [unchanged/simplified/increased]

## 🔒 Security Considerations

- [ ] Input validation implemented
- [ ] Authentication/authorization checked
- [ ] No sensitive data exposed
- [ ] Security best practices followed

## ♿ Usability

- [ ] Keybindings are intuitive and discoverable
- [ ] Which-key descriptions are clear
- [ ] Color scheme provides good contrast
- [ ] Configuration follows Doom conventions

## 📚 Documentation

- [ ] Code comments added/updated
- [ ] README updated (if needed)
- [ ] API documentation updated (if applicable)
- [ ] CLAUDE.md updated (if new patterns introduced)

## ✅ Checklist

### Configuration Quality
- [ ] My configuration follows Doom conventions
- [ ] I have performed a self-review
- [ ] Configuration is well-organized and documented
- [ ] I have commented complex elisp code
- [ ] I have removed debug/test code

### Validation
- [ ] Configuration loads without errors
- [ ] All new functionality works as intended
- [ ] I have tested edge cases
- [ ] I have tested with fresh Doom installation

### Project Standards
- [ ] No configuration errors (`doom doctor`)
- [ ] Sync completes successfully (`doom sync`)
- [ ] Emacs starts without warnings
- [ ] GTD workflow functions properly

## 🚦 Review Focus Areas

Please pay special attention to:
1. [Specific area 1 needing careful review]
2. [Specific area 2 needing careful review]
3. [Any concerns or uncertainties]

## 🔄 Migration Guide

[If breaking changes, provide migration steps]

## 📈 Future Improvements

Potential enhancements identified during development:
- [ ] [Future improvement 1]
- [ ] [Future improvement 2]

## 🙏 Notes for Reviewers

[Any additional context or notes for reviewers]

---
**Ready for Review** ✅
EOF
)" \
  --assignee @me \
  --label "enhancement,needs-review"
```

## Post-PR Actions:

### 1. **Add PR Context**
```bash
# Get PR number
PR_NUMBER=$(gh pr view --json number -q .number)

# Add additional context as comments if needed
gh pr comment $PR_NUMBER --body "## Additional Context
- Development environment tested: [details]
- Performance benchmarks: [if applicable]
- Related documentation: [links]"
```

### 2. **Link to Issues**
```bash
# Link PR to issues
gh issue comment [issue-number] --body "PR created: #$PR_NUMBER"
```

### 3. **Request Reviews**
```bash
# Request specific reviewers
gh pr edit $PR_NUMBER --add-reviewer [username1],[username2]
```

### 4. **Update Project Board**
If using GitHub Projects:
```bash
# Move related issues to "In Review" column
gh project item-edit --id [item-id] --field-id [field-id] --project-id [project-id]
```

### 5. **Create PR Summary Document**
Save PR details locally:
```
docs/prs/PR-$PR_NUMBER-summary.md
```

## PR Templates by Type:

### Feature PR
- Comprehensive description
- User-facing changes highlighted
- Testing instructions detailed
- Screenshots/recordings included

### Bug Fix PR
- Clear problem statement
- Root cause analysis
- Fix explanation
- Regression test details

### Refactor PR
- Motivation explained
- No functional changes verified
- Performance impact analyzed
- Risk assessment included

### Documentation PR
- Changes summarized
- Preview links provided
- Technical accuracy verified

## Review Response Strategy:

Prepare for common review feedback:
1. **Performance concerns**: Have benchmarks ready
2. **Security questions**: Document threat model
3. **Test coverage**: Justify any uncovered code
4. **Architecture decisions**: Explain trade-offs

## Merge Strategy:

Recommend merge approach:
- **Squash and merge**: For feature branches with many commits
- **Rebase and merge**: For clean, logical commit history
- **Create merge commit**: For preserving complete history

## Success Metrics:

PR quality indicators:
- ✅ All CI checks passing
- ✅ Comprehensive description
- ✅ Tests included
- ✅ Documentation updated
- ✅ Quick review turnaround
- ✅ Minimal review iterations