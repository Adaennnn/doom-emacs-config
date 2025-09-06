---
allowed-tools: Task
argument-hint: <configuration-name> [optional: based on exploration file]
description: Generate a comprehensive implementation plan for a new Doom configuration
---

# 📋 CREATE CONFIGURATION PLAN: $ARGUMENTS

Use the Task tool with the **doom-planner** agent to generate a comprehensive implementation plan document following the structure and detail level of `docs/plans/gtd-system-PLAN.md`.

## Pre-Planning Analysis:

1. If an exploration file exists at `docs/explorations/*-exploration.md`, read and analyze it first
2. Review the current Doom configuration structure (init.el, config.el, packages.el)
3. Analyze existing module configurations for patterns and conventions
4. Check current keybinding schemes and workflow patterns

## Plan Document Structure:

Create a detailed plan document at `docs/plans/$1-PLAN.md` with the following sections:

### 1. **Executive Summary**
- Configuration overview and productivity value
- Integration with existing Doom setup
- Key benefits and workflow improvements

### 2. **System Overview**
- Purpose and objectives
- User workflows and use cases
- Success criteria and measurable outcomes

### 3. **Technical Architecture**
- Module structure (init.el, config.el, packages.el organization)
- Package dependencies and integration
- Configuration flow and loading patterns
- Custom function definitions

### 4. **Package Strategy**
- Core packages required
- Package configuration approach
- Lazy loading and defer strategies
- Conflict resolution with existing packages

### 5. **Keybinding Design**
- Leader key organization
- Mode-specific bindings
- Custom keybinding definitions
- Conflict resolution strategy

### 6. **Workflow Integration**
- Current workflow analysis
- Proposed workflow optimizations
- External tool integration
- Automation opportunities

### 7. **Performance Considerations**
- Startup time impact analysis
- Memory usage considerations
- Lazy loading strategies
- Optimization techniques

### 8. **Configuration Management**
- Environment-specific settings
- Feature toggles and customization
- Backup and recovery strategy
- Version control considerations

### 9. **Testing Strategy**
- Configuration validation approach
- Fallback and recovery testing
- Performance benchmarking
- User acceptance testing

### 10. **Security Considerations**
- Package source verification
- Sensitive data handling
- System access implications
- Configuration safety patterns

### 11. **Implementation Phases**

#### Phase 1: Core Setup (1-2 days)
- [ ] Package installation and basic configuration
- [ ] Core keybindings setup
- [ ] Basic functionality validation
- [ ] Performance baseline measurement

#### Phase 2: Workflow Integration (3-4 days)  
- [ ] Advanced feature configuration
- [ ] Custom function implementation
- [ ] Workflow optimization
- [ ] External tool integration

#### Phase 3: Polish and Documentation (1-2 days)
- [ ] Configuration refinement
- [ ] Documentation updates
- [ ] User guide creation
- [ ] Performance optimization

### 12. **Risk Assessment & Mitigation**
- Configuration conflicts and solutions
- Package compatibility risks
- Performance impact mitigation
- Rollback strategies

### 13. **Success Metrics**
- Startup time improvements
- Workflow efficiency gains
- Configuration stability measures
- User satisfaction indicators

### 14. **Documentation Requirements**
- Configuration documentation
- Keybinding reference
- Workflow guides
- Troubleshooting documentation

### 15. **Task Breakdown** (ATOMIC)
Create a numbered list of atomic, implementable tasks:
1. Create backup of current configuration
2. Install and configure core packages
3. Set up basic keybinding structure
4. Implement core functionality
5. Configure lazy loading patterns
6. [Continue with 15-25 atomic tasks...]

## Post-Planning Actions:

After creating the plan:
1. Update the TodoWrite tool with the high-level phases
2. Save the plan to the specified location
3. Create a summary comment with:
   - Total estimated effort
   - Number of atomic tasks
   - Key dependencies
   - Recommended starting point

## Notes:
- Each task in the breakdown should be small enough to complete in 1-4 hours
- Tasks should be independent when possible
- Include task dependencies where necessary
- Consider parallel implementation opportunities
- Account for testing and validation time
