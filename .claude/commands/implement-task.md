---
allowed-tools: "*"
argument-hint: <issue-number or issue-url>
description: Implement a specific GitHub issue for Doom Emacs configuration
---

# 💻 IMPLEMENT TASK: Issue #$ARGUMENTS

Systematically implement a Doom Emacs configuration issue following best practices.

## Important Note:
This command implements changes but does NOT commit them. Use `/commit-feature` after implementation to create commits.

## Specialized Agents:
During implementation, the following agents may be invoked:
- **doom-explorer**: For understanding existing Doom configurations and modules
- **security-auditor**: For reviewing configuration security implications
- **doom-planner**: For complex implementation planning (if needed)

## Implementation Workflow:

### 1. **Fetch Issue Details**
```bash
gh issue view $ARGUMENTS --json title,body,labels,milestone
```

Parse and understand:
- Task description
- Acceptance criteria
- Dependencies
- Technical requirements
- Files to modify

### 2. **Create or Switch to Feature Branch**
```bash
# Branch naming: feature/issue-[number]-[brief-description]
git checkout -b feature/issue-$ARGUMENTS-[description]
# Or switch if it already exists
git checkout feature/issue-$ARGUMENTS-[description]
```

### 3. **Update TodoWrite**
Load issue acceptance criteria into TodoWrite:
- [ ] Core implementation
- [ ] Configuration validation
- [ ] doom doctor check
- [ ] doom sync if packages changed
- [ ] Startup performance check
- [ ] Update documentation
- [ ] Manual testing of features

### 4. **Implementation Steps**

#### A. **Explore Context**
- Read config.el, init.el, packages.el
- Understand existing keybindings
- Review active Doom modules
- Check for potential conflicts
- Reference existing patterns

#### B. **Core Implementation**
Following the issue requirements:
1. Modify configuration files as needed
2. Follow Doom Emacs conventions
3. Use appropriate Elisp idioms
4. Add helpful comments for complex configs
5. Handle error cases gracefully

For GTD-specific tasks:
- Configure org-mode settings
- Set up capture templates
- Define agenda views
- Configure keybindings carefully

#### C. **Validation & Testing**
```bash
# Check configuration syntax
doom doctor

# Sync if packages were added/removed
doom sync

# Test startup without errors
emacs --debug-init --eval "(kill-emacs)"

# Check byte-compilation
cd ~/.config/doom
emacs --batch -f batch-byte-compile config.el

# Test specific functionality (e.g., for GTD)
emacs --batch --eval "(require 'org)" \
      --eval "(setq org-agenda-files '(\"~/org/gtd/\"))" \
      --eval "(org-agenda-list)" \
      --eval "(kill-emacs)"
```

#### D. **Performance Check**
```bash
# Profile startup time
emacs --eval "(progn (profiler-start 'cpu) (run-with-timer 1 nil #'profiler-report))"

# Or use the profile-startup command
/profile-startup
```

### 5. **Quality Checklist**

Before marking task complete, verify:
- [ ] Configuration follows Doom conventions
- [ ] All acceptance criteria met
- [ ] doom doctor shows no errors
- [ ] doom sync successful (if needed)
- [ ] No byte-compilation warnings
- [ ] Startup time acceptable (< 2 seconds)
- [ ] Manual testing confirms features work
- [ ] Documentation updated if needed
- [ ] No debug code or commented experiments

### 6. **Update Issue Status**

Add implementation notes to the issue:
```bash
gh issue comment $ARGUMENTS --body "Implementation complete:
- ✅ Configuration implemented
- ✅ doom doctor passes  
- ✅ doom sync successful (if needed)
- ✅ Startup performance: Xs
- ✅ Manual testing complete
- ✅ All acceptance criteria met

Branch: feature/issue-$ARGUMENTS-[description]
Status: Ready to commit

Next steps: 
- Run /test-config for comprehensive validation
- Use /commit-feature $ARGUMENTS to create commit
- Continue with other issues or create PR when ready"
```

### 7. **Stage Changes (Optional)**

If you want to prepare files for commit:
```bash
# Review changes
git status
git diff

# Stage relevant files (but don't commit)
git add config.el
git add init.el
# etc.
```

## Quality Gates:

**Must Pass Before Marking Implementation Complete:**
- ✅ doom doctor clean
- ✅ doom sync successful (if packages changed)
- ✅ No startup errors
- ✅ Configuration loads correctly
- ✅ Features work as expected
- ✅ Performance acceptable
- ✅ No breaking changes to existing config

## Important Reminders:
- This command does NOT create commits
- Use `/commit-feature` to commit your changes
- Test thoroughly before committing
- Keep changes focused on the specific issue
- Don't add unrelated improvements

## Doom-Specific Tips:
- Check `~/.doom.d/init.el` for enabled modules
- Use `SPC h d f` in Doom to describe functions
- Test keybindings with `SPC h k` (describe-key)
- Use `M-x doom/reload` to reload config without restart
- Check `*Messages*` buffer for errors

## Next Steps After Implementation:
1. Run `/test-config` for comprehensive testing
2. Use `/commit-feature $ARGUMENTS` to create semantic commit
3. Continue with related issues
4. When ready, use `/pr-feature` to create pull request