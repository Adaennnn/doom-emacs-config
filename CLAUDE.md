# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview
This is a Doom Emacs configuration repository.

## Key Commands

### Doom Emacs Management
```bash
# Sync configuration after modifying init.el, packages.el, or config.el
doom sync

# Check for configuration issues
doom doctor

# Upgrade Doom Emacs framework
doom upgrade

# Clean build cache and orphaned packages
doom gc
```

### Development Commands
```bash
# Reload configuration without restarting Emacs
# Inside Emacs: M-x doom/reload

# Test configuration changes
emacs --batch --eval "(require 'doom-start)"

# Debug startup issues
emacs --debug-init
```

## Architecture & Structure

### Configuration Files
- **init.el**: Module declarations - controls which Doom modules are loaded
- **config.el**: Personal configuration - all customizations go here
- **packages.el**: Package declarations - additional packages beyond Doom defaults

### Module System
Doom uses a modular architecture. Enabled modules in init.el:
- **Completion**: corfu + orderless, vertico
- **UI**: doom, doom-dashboard, modeline, workspaces
- **Editor**: evil (vim bindings), file-templates, fold, snippets
- **Tools**: magit (git), eval, lookup
- **Lang**: emacs-lisp, org, markdown, sh

### Project Agents
The repository includes custom Claude agents for Doom Emacs development:
- **doom-explorer**: Deep configuration exploration and analysis
- **doom-planner**: Comprehensive implementation planning
- **security-auditor**: Security review for configurations
- **issue-architect**: GitHub issue creation from plans
- **commit-craftsman**: Semantic commit message creation

## Important Considerations

### Doom Keybindings
Doom has extensive keybindings under `SPC` prefix. When adding custom bindings:
- Check existing bindings with `SPC h b b` (describe-bindings)
- Prefer unused prefixes like `SPC g` for GTD
- Use `map!` macro for keybinding definitions

### Configuration Loading Order
1. early-init.el (Doom internal)
2. init.el (module declarations)
3. packages.el (package declarations)
4. config.el (user configuration)

### After Modifications
Always run `doom sync` after changing:
- init.el (module changes)
- packages.el (package additions/removals)

### Org Directory
Currently set to `~/org/` in config.el. GTD system will use subdirectories within this location.
