---
allowed-tools: Task
argument-hint: <doom-module or configuration-area>
description: Deep exploration of a Doom module or configuration area
---

# 🔍 EXPLORE DOOM: $ARGUMENTS

Use the Task tool with the **doom-explorer** agent to perform a comprehensive exploration of the **$ARGUMENTS** Doom module/configuration area in this Emacs setup.

## Exploration Requirements:

Please provide a detailed analysis covering:

### 1. **Configuration Architecture**
- Module structure and file organization (init.el, config.el, packages.el)
- Key components and their relationships
- Configuration flow and loading patterns
- Integration points with other modules

### 2. **Package Stack**
- Packages and libraries used
- Package configuration and customization
- Use-package patterns and defer strategies
- Package dependencies and conflicts

### 3. **Functionality Overview**
- Core functionality and features provided
- User workflows and key interactions  
- Configuration variables and options
- Hook usage and customization patterns

### 4. **Keybinding Structure**
- Leader key hierarchies and organization
- Custom keybinding definitions
- Mode-specific bindings
- Keybinding conflicts and overlaps

### 5. **Dependencies**
- External package dependencies (MELPA, ELPA, etc.)
- Internal dependencies (other Doom modules)
- System tool integrations
- External service connections

### 6. **Performance Profile**
- Startup time impact
- Memory usage patterns
- Lazy loading and deferral strategies
- Optimization opportunities

### 7. **Configuration Management**
- Configuration variables and customization
- Environment-specific settings
- Feature toggles and flags
- Module activation patterns

### 8. **Documentation Quality**
- Inline documentation and comments
- Configuration examples
- Usage patterns and workflows
- Troubleshooting information

### 9. **Workflow Integration**
- User workflow optimization
- Automation and custom functions
- External tool integration
- Productivity enhancements

### 10. **Security & Safety**
- Package source verification
- Configuration safety patterns
- Sensitive data handling
- System access implications

## Output Format:

Structure the exploration as a comprehensive markdown document with:
- Clear section headers
- Configuration examples where relevant
- File path references (file_path:line_number format)
- Identified patterns and conventions
- Potential improvements or optimizations
- Integration opportunities with other modules

Save the exploration results to: `docs/explorations/$ARGUMENTS-exploration.md`

This exploration will serve as the foundation for planning new configurations or modifications to this Doom module.