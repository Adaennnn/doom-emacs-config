---
allowed-tools: Bash(emacs:*), Read, Write, TodoWrite
description: Profile and analyze Doom Emacs startup performance
---

# ⚡ PROFILE STARTUP: Performance Analysis & Optimization

Comprehensive startup performance profiling for Doom Emacs with detailed analysis and optimization recommendations.

## Profiling Overview:

### 1. **Basic Startup Timing**
```bash
# Multiple startup measurements for accuracy
echo "⏱️  Measuring basic startup times (10 iterations)..."

TOTAL_TIME=0
for i in {1..10}; do
    TIME=$((/usr/bin/time -f "%e" emacs --eval "(kill-emacs)" 2>&1) | tail -1)
    echo "Run $i: ${TIME}s"
    TOTAL_TIME=$(echo "$TOTAL_TIME + $TIME" | bc)
done

AVERAGE=$(echo "scale=2; $TOTAL_TIME / 10" | bc)
echo "Average startup time: ${AVERAGE}s"
```

### 2. **Detailed Profiling with esup**
```bash
# Use esup (Emacs Start Up Profiler) if available
echo "🔍 Running detailed startup profiling..."

emacs --batch --eval "
(condition-case nil
  (progn
    (require 'package)
    (package-initialize)
    (unless (package-installed-p 'esup)
      (message \"esup not installed, using manual profiling\"))
    (when (package-installed-p 'esup)
      (require 'esup)
      (esup)))
  (error (message \"Using alternative profiling method\")))
" 2>&1 || echo "Running manual profiling..."
```

### 3. **Manual Configuration Profiling**
```bash
# Profile major configuration components
echo "⚙️  Profiling configuration components..."

emacs --batch --eval "
(defun time-require (feature)
  (let ((start-time (current-time)))
    (condition-case err
      (progn 
        (require feature)
        (let ((elapsed (float-time (time-subtract (current-time) start-time))))
          (message \"✅ %s: %.3fs\" feature elapsed)
          elapsed))
      (error 
        (message \"❌ %s: FAILED - %s\" feature err)
        999))))

(let ((total-time 0))
  (message \"🔍 Profiling core components:\")
  (setq total-time (+ total-time (time-require 'evil)))
  (setq total-time (+ total-time (time-require 'org)))
  (setq total-time (+ total-time (time-require 'magit)))
  (setq total-time (+ total-time (time-require 'company)))
  (setq total-time (+ total-time (time-require 'projectile)))
  (setq total-time (+ total-time (time-require 'vertico)))
  (message \"Total core component time: %.3fs\" total-time))
" 2>&1
```

### 4. **Package Loading Analysis**
```bash
# Analyze package loading times
echo "📦 Analyzing package loading performance..."

emacs --batch --eval "
(setq doom-debug-p t)
(load-file \"init.el\")

(defun profile-packages ()
  (let ((package-times '()))
    (dolist (pkg package-activated-list)
      (let* ((start (current-time))
             (result (condition-case nil 
                       (require pkg)
                       (error 'failed)))
             (end (current-time))
             (elapsed (float-time (time-subtract end start))))
        (when (> elapsed 0.001) ; Only report packages taking >1ms
          (push (list pkg elapsed result) package-times))))
    (setq package-times (sort package-times (lambda (a b) (> (cadr a) (cadr b)))))
    (message \"🐌 Slowest packages:\")
    (dolist (entry (take 10 package-times))
      (message \"   %s: %.3fs\" (car entry) (cadr entry)))))

(profile-packages)
" 2>&1
```

### 5. **Memory Usage Analysis**
```bash
# Analyze memory consumption
echo "🧠 Analyzing memory usage..."

emacs --batch --eval "
(defun analyze-memory ()
  (let ((memory-info (memory-use-counts)))
    (message \"Memory Analysis:\")
    (message \"  Conses: %d (%.2f MB)\" 
             (car (nth 0 memory-info))
             (/ (car (nth 0 memory-info)) 131072.0))
    (message \"  Symbols: %d\" (car (nth 1 memory-info)))
    (message \"  Strings: %d\" (car (nth 3 memory-info)))
    (message \"  Vectors: %d\" (car (nth 4 memory-info)))
    (message \"  Buffer objects: %d\" (length (buffer-list)))
    (message \"  Total estimated: %.2f MB\"
             (/ (+ (car (nth 0 memory-info))
                   (* (car (nth 1 memory-info)) 50)
                   (* (car (nth 3 memory-info)) 20)) 131072.0))))

(analyze-memory)
" 2>&1
```

### 6. **Hook Analysis** 
```bash
# Analyze expensive hooks
echo "🪝 Analyzing hook performance..."

emacs --batch --eval "
(defun profile-hooks ()
  (message \"Hook Analysis:\")
  (let ((expensive-hooks '()))
    (dolist (hook '(after-init-hook 
                    window-setup-hook
                    before-init-hook
                    emacs-startup-hook))
      (let ((hook-functions (symbol-value hook)))
        (when hook-functions
          (message \"  %s: %d functions\" hook (length hook-functions))
          (when (> (length hook-functions) 5)
            (push hook expensive-hooks)))))
    (when expensive-hooks
      (message \"⚠️  Heavy hooks detected: %s\" expensive-hooks))))

(profile-hooks)
" 2>&1
```

### 7. **Garbage Collection Analysis**
```bash
# Profile GC performance
echo "🗑️  Analyzing garbage collection..."

emacs --batch --eval "
(let ((gc-start (current-time)))
  (garbage-collect)
  (let ((gc-time (float-time (time-subtract (current-time) gc-start))))
    (message \"GC Analysis:\")
    (message \"  GC time: %.3fs\" gc-time)
    (message \"  GC threshold: %d\" gc-cons-threshold)
    (message \"  GC percentage: %f\" gc-cons-percentage)
    (when (> gc-time 0.1)
      (message \"⚠️  Slow garbage collection detected\"))))
" 2>&1
```

## Performance Benchmarking:

### 8. **Comparative Analysis**
```bash
# Compare with minimal configuration
echo "📊 Running comparative benchmarks..."

# Measure minimal Emacs startup
MINIMAL_TIME=$((/usr/bin/time -f "%e" emacs -Q --eval "(kill-emacs)" 2>&1) | tail -1)
echo "Minimal Emacs startup: ${MINIMAL_TIME}s"

# Calculate overhead
DOOM_TIME=$((/usr/bin/time -f "%e" emacs --eval "(kill-emacs)" 2>&1) | tail -1)
OVERHEAD=$(echo "scale=2; $DOOM_TIME - $MINIMAL_TIME" | bc)
echo "Doom configuration overhead: ${OVERHEAD}s"

# Performance classification
if (( $(echo "$DOOM_TIME < 1.0" | bc -l) )); then
    echo "🚀 Performance Grade: A (Excellent)"
elif (( $(echo "$DOOM_TIME < 2.0" | bc -l) )); then
    echo "⚡ Performance Grade: B (Good)"  
elif (( $(echo "$DOOM_TIME < 3.0" | bc -l) )); then
    echo "⏱️  Performance Grade: C (Average)"
else
    echo "🐌 Performance Grade: D (Needs Optimization)"
fi
```

## Generate Profile Report:

Create comprehensive profiling report:

```markdown
# ⚡ Startup Performance Profile

**Date**: $(date)
**Configuration**: $DOOM_CONFIG_DIR
**Emacs Version**: $(emacs --version | head -1)

## Performance Summary
- **Average Startup Time**: [X.X seconds]
- **Minimal Emacs Time**: [Y.Y seconds]
- **Configuration Overhead**: [Z.Z seconds]
- **Performance Grade**: [A/B/C/D]
- **Memory Usage**: [X.X MB]

## Detailed Timing
- **Core Components**: [X.X seconds]
- **Package Loading**: [Y.Y seconds] 
- **Hook Execution**: [Z.Z seconds]
- **Garbage Collection**: [W.W seconds]

## Component Analysis

### Slowest Packages (Top 10)
| Package | Load Time | Status |
|---------|-----------|--------|
| [package1] | [X.Xs] | ✅ |
| [package2] | [Y.Ys] | ✅ |
| [package3] | [Z.Zs] | ❌ |

### Hook Performance
- **after-init-hook**: [X functions, Y.Ys]
- **emacs-startup-hook**: [X functions, Y.Ys]
- **window-setup-hook**: [X functions, Y.Ys]

### Memory Breakdown
- **Conses**: [X.X MB]
- **Symbols**: [Y,YYY objects]
- **Strings**: [Z,ZZZ objects]
- **Vectors**: [W,WWW objects]
- **Buffer Objects**: [B buffers]

## Performance Issues Detected
[List any performance bottlenecks found]

## Optimization Recommendations

### High Priority
- [ ] [Critical optimization suggestion]
- [ ] [Important performance fix]

### Medium Priority  
- [ ] [Moderate optimization]
- [ ] [Configuration improvement]

### Low Priority
- [ ] [Minor optimization]
- [ ] [Nice-to-have improvement]

## Specific Optimizations

### Lazy Loading Opportunities
[Packages that could benefit from deferred loading]

### GC Optimization
- **Current GC threshold**: [X]
- **Recommended threshold**: [Y]
- **Potential time savings**: [Z.Zs]

### Hook Optimization
[Heavy hooks that could be optimized]

## Performance Tracking

### Historical Comparison
| Date | Startup Time | Grade | Notes |
|------|-------------|--------|-------|
| [Previous] | [X.Xs] | [A] | [Notes] |
| [Current] | [Y.Ys] | [B] | [Notes] |
| **Change** | **±Z.Zs** | **±1** | **[Impact]** |

### Regression Analysis
[Any performance regressions detected]

## Action Items

### Immediate Actions
1. [High-impact optimization to implement]
2. [Critical issue to fix]

### Future Optimizations  
1. [Medium-term improvement]
2. [Long-term optimization goal]

## Configuration Recommendations

```elisp
;; Suggested optimizations for config.el
(setq gc-cons-threshold [recommended-value])
(setq gc-cons-percentage [recommended-value])

;; Lazy loading improvements
(use-package [package-name]
  :defer t
  :commands ([command-list]))
```

---
*Generated by profile-startup command*
```

## Save Profile Report:

Save to: `docs/performance/startup-profile-$(date +%Y%m%d-%H%M%S).md`

## Optimization Actions:

### If Performance is Poor (>3s):
1. **Immediate Review**: Identify critical bottlenecks
2. **Package Audit**: Remove or defer heavy packages  
3. **Configuration Review**: Optimize configuration patterns
4. **GC Tuning**: Adjust garbage collection parameters

### If Performance is Average (1-3s):
1. **Lazy Loading**: Implement more deferred loading
2. **Hook Optimization**: Reduce hook overhead
3. **Package Review**: Consider lighter alternatives

### If Performance is Good (<1s):
1. **Maintenance**: Monitor for regressions
2. **Fine-tuning**: Minor optimizations
3. **Documentation**: Record optimization strategies

## Integration Points:

- **Configuration Changes**: Profile after major changes
- **Package Updates**: Monitor performance impact
- **Continuous Monitoring**: Track performance trends
- **Optimization Tracking**: Document improvement efforts

## Summary Output:

```
⚡ Startup Profile Results:
• Time: [X.X seconds] (Grade: [A/B/C/D])
• Overhead: [Y.Y seconds] vs minimal Emacs
• Memory: [Z.Z MB]
• Bottlenecks: [X packages, Y hooks]
• Recommendations: [High/Medium/Low priority items]
• Next Action: [Specific optimization to implement]
```