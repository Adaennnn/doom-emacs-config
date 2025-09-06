---
allowed-tools: Bash(emacs:*), Bash(timeout:*), Read, Write, TodoWrite
description: Comprehensive testing of Doom Emacs configuration
---

# 🧪 TEST CONFIG: Configuration Validation & Testing

Run comprehensive tests on your Doom Emacs configuration to ensure stability, performance, and functionality.

## Test Suite Overview:

### 1. **Syntax Validation Tests**
```bash
# Test basic Elisp syntax in configuration files
echo "🔍 Testing Elisp syntax..."

# Check init.el
emacs -Q --batch --eval "(condition-case err (load-file \"init.el\") (error (message \"ERROR in init.el: %s\" err)))" 2>&1

# Check config.el 
emacs -Q --batch --eval "(condition-case err (load-file \"config.el\") (error (message \"ERROR in config.el: %s\" err)))" 2>&1

# Check packages.el
emacs -Q --batch --eval "(condition-case err (load-file \"packages.el\") (error (message \"ERROR in packages.el: %s\" err)))" 2>&1
```

### 2. **Configuration Loading Tests**
```bash
# Test clean configuration loading
echo "🚀 Testing configuration loading..."

# Test minimal startup
timeout 60s emacs --batch --eval "(message \"✅ Basic loading successful\")" 2>&1 || echo "❌ Basic loading failed"

# Test with user configuration
timeout 60s emacs --batch -l init.el --eval "(message \"✅ Full config loading successful\")" 2>&1 || echo "❌ Full config loading failed"
```

### 3. **Package Integrity Tests**  
```bash
# Test package loading and integrity
echo "📦 Testing package integrity..."

# Check for broken packages
~/.emacs.d/bin/doom doctor | grep -i "broken\|missing\|error" || echo "✅ No broken packages detected"

# Test critical package loading
emacs --batch --eval "
(require 'package)
(package-initialize)
(dolist (pkg '(evil org magit))
  (condition-case err 
    (require pkg)
    (message \"✅ Package %s loads successfully\" pkg)
    (error (message \"❌ Package %s failed: %s\" pkg err))))
" 2>&1
```

### 4. **Keybinding Tests**
```bash
# Test critical keybinding functionality
echo "⌨️  Testing keybinding configuration..."

emacs --batch --eval "
(defun test-keybinding (key command)
  (condition-case err
    (if (key-binding (kbd key))
        (message \"✅ Keybinding %s -> %s works\" key (key-binding (kbd key)))
      (message \"⚠️  Keybinding %s not bound\" key))
    (error (message \"❌ Keybinding %s error: %s\" key err))))

(test-keybinding \"SPC f f\" 'find-file)
(test-keybinding \"SPC b b\" 'switch-to-buffer)
(test-keybinding \"SPC p p\" 'projectile-switch-project)
" 2>&1
```

### 5. **Performance Tests**
```bash
# Startup time tests
echo "⚡ Testing startup performance..."

# Measure startup time (3 iterations)
echo "Startup time measurements:"
for i in {1..3}; do
    /usr/bin/time -f "Run $i: %e seconds" emacs --eval "(kill-emacs)" 2>&1 | grep "seconds"
done

# Memory usage test  
echo "Memory usage test:"
emacs --batch --eval "
(message \"Memory usage: %.2f MB\" 
  (/ (car (car (memory-use-counts))) 1048576.0))
" 2>&1
```

### 6. **Module-Specific Tests**
```bash
# Test specific Doom modules
echo "🏗️  Testing Doom modules..."

# Test org-mode setup
emacs --batch --eval "
(condition-case err
  (progn 
    (require 'org)
    (message \"✅ Org-mode loads successfully\")
    (if (fboundp 'org-agenda)
        (message \"✅ Org-agenda available\")
      (message \"⚠️  Org-agenda not available\")))
  (error (message \"❌ Org-mode error: %s\" err)))
" 2>&1

# Test completion framework
emacs --batch --eval "
(condition-case err
  (cond
    ((require 'vertico nil t) (message \"✅ Vertico completion available\"))
    ((require 'ivy nil t) (message \"✅ Ivy completion available\"))
    ((require 'helm nil t) (message \"✅ Helm completion available\"))
    (t (message \"⚠️  No completion framework detected\")))
  (error (message \"❌ Completion framework error: %s\" err)))
" 2>&1
```

### 7. **External Tool Integration Tests**
```bash
# Test external tool integrations
echo "🔧 Testing external integrations..."

# Test git integration
which git > /dev/null && echo "✅ Git available" || echo "❌ Git not found"

# Test ripgrep for searching  
which rg > /dev/null && echo "✅ Ripgrep available" || echo "⚠️  Ripgrep not found (search may be slower)"

# Test language servers if configured
which clangd > /dev/null && echo "✅ Clangd available" || echo "ℹ️  Clangd not found"
which python-language-server > /dev/null && echo "✅ Python LSP available" || echo "ℹ️  Python LSP not found"
```

### 8. **Configuration Stress Tests**
```bash
# Stress test the configuration
echo "🔥 Running stress tests..."

# Large file handling test
emacs --batch --eval "
(condition-case err
  (progn
    (find-file \"/dev/null\")
    (insert (make-string 100000 ?a))
    (message \"✅ Large buffer handling works\"))
  (error (message \"❌ Large buffer test failed: %s\" err)))
" 2>&1

# Multiple buffer test
emacs --batch --eval "
(condition-case err
  (progn
    (dotimes (i 50) (get-buffer-create (format \"test-buffer-%d\" i)))
    (message \"✅ Multiple buffer creation works\"))
  (error (message \"❌ Multiple buffer test failed: %s\" err)))
" 2>&1
```

## Generate Test Report:

Create comprehensive test report:

```markdown
# 🧪 Configuration Test Report

**Date**: $(date)
**Configuration**: $DOOM_CONFIG_DIR
**Emacs Version**: $(emacs --version | head -1)

## Test Summary
- **Total Tests Run**: [X]
- **Passed**: [Y] ✅
- **Failed**: [Z] ❌  
- **Warnings**: [W] ⚠️
- **Overall Status**: [PASS/FAIL]

## Detailed Results

### Syntax Validation
- **init.el**: [✅ Pass | ❌ Fail]
- **config.el**: [✅ Pass | ❌ Fail]  
- **packages.el**: [✅ Pass | ❌ Fail]

### Loading Tests
- **Basic Loading**: [✅ Pass | ❌ Fail]
- **Full Configuration**: [✅ Pass | ❌ Fail]
- **Load Time**: [X.X seconds]

### Package Integrity
- **Critical Packages**: [X/Y working]
- **Broken Packages**: [List if any]
- **Missing Dependencies**: [List if any]

### Performance Metrics
- **Average Startup Time**: [X.X seconds]
- **Memory Usage**: [X.X MB]
- **Performance Grade**: [A/B/C/D/F]

### Keybinding Tests
- **Leader Key Functions**: [X/Y working]
- **Critical Bindings**: [X/Y working]
- **Conflicts Detected**: [List if any]

### Module Tests
- **Org-mode**: [✅ Pass | ❌ Fail]
- **Completion**: [Framework name] [✅ Pass | ❌ Fail]
- **Evil Mode**: [✅ Pass | ❌ Fail]
- **Magit**: [✅ Pass | ❌ Fail]

### External Integrations
- **Git**: [✅ Available | ❌ Missing]
- **Ripgrep**: [✅ Available | ⚠️ Missing]
- **Language Servers**: [List available]

### Stress Tests
- **Large Files**: [✅ Pass | ❌ Fail]
- **Multiple Buffers**: [✅ Pass | ❌ Fail]
- **Memory Stability**: [✅ Pass | ❌ Fail]

## Issues Found
[List any critical issues that need attention]

## Warnings
[List any warnings or potential issues]

## Recommendations
[Suggestions for optimization or fixes]

## Performance Comparison
- **Previous Test**: [Date] - [X.X seconds]
- **Current Test**: [Date] - [Y.Y seconds]
- **Change**: [±Z.Z seconds]

---
*Generated by test-config command*
```

## Save Test Report:

Save to: `docs/tests/config-test-$(date +%Y%m%d-%H%M%S).md`

## Failure Response:

### If Critical Tests Fail:
1. **Backup Current Config**: Ensure backup exists
2. **Provide Debug Info**: Show detailed error messages
3. **Suggest Fixes**: Common solutions for detected issues
4. **Safe Mode**: Instructions for minimal configuration
5. **Recovery Steps**: How to restore working state

### If Performance Degrades:
1. **Profile Startup**: Detailed startup profiling
2. **Package Audit**: Check for resource-heavy packages
3. **Configuration Review**: Suggest optimizations
4. **Baseline Comparison**: Compare with previous benchmarks

## Integration Points:

- **Pre-commit Hook**: Run tests before committing config changes
- **CI/CD Pipeline**: Automated testing of configuration changes  
- **Performance Monitoring**: Track performance over time
- **Issue Tracking**: Create issues for failing tests

## Status Updates:

Update TodoWrite with:
- [ ] Syntax validation passed
- [ ] Configuration loads successfully
- [ ] Package integrity verified
- [ ] Performance benchmarks acceptable
- [ ] All critical functions working

## Summary Output:

```
🧪 Configuration Test Results:
• Status: [PASS/FAIL]
• Tests: [X passed, Y failed, Z warnings]
• Startup: [X.X seconds]
• Memory: [Y.Y MB]
• Issues: [None/List critical issues]
• Next Action: [Continue/Fix issues/Optimize]
```