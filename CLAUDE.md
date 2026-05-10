# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code plugin** for OctoberCMS development. It provides version-specific documentation, community answers (530+ solved forum topics), and auto-triggering development guidance when working with OctoberCMS projects.

**Plugin structure:**
- `octobercms/commands/` — Plugin commands: setup, sync-docs, october-version, update-settings
- `octobercms/skills/octobercms-core/` — Core skill (auto-triggers on OctoberCMS context)
- `octobercms/references/developer-guidelines.md` — Coding standards and naming conventions
- `octobercms/scripts/auto_sync.py` — Documentation sync script

## Common Commands

### Testing the plugin locally
```bash
claude --plugin-dir /path/to/octobercms-claude-plugin/octobercms
```

### Updating the plugin
```bash
claude plugin update octobercms@softappstudio
```

### Within OctoberCMS projects (after plugin setup)
```bash
php artisan october:about        # Detect OctoberCMS version
php artisan create:plugin        # Create new plugin
php artisan create:model         # Create model with migration
php artisan create:controller    # Create backend controller
php artisan create:component     # Create frontend component
php artisan list create          # See all scaffolding commands
```

## Architecture

**Documentation storage** (global, shared across projects):
- `~/.claude/octobercms-docs/` — Version-specific official docs (downloaded on demand)
- `~/.claude/octobercms-community-answers/` — Solved forum topics by category

**Per-project config:** `.claude/octobercms-config.json`
```json
{
  "version": "4.x",
  "auto_sync": true,
  "auto_sync_mode": "auto",
  "community_answers": true
}
```

**Skill trigger:** The `octobercms-core` skill auto-activates when:
- Project has `.claude/octobercms-config.json`
- Working in `plugins/`, `themes/`, or `modules/` directories
- User mentions: October, plugin, component, partial, layout, theme, tailor, blueprint, etc.

## OctoberCMS Development Conventions

Key standards from `octobercms/references/developer-guidelines.md`:

| Element | Convention |
|---------|------------|
| Vendor/namespace | `Acme.Blog` (Upper, no underscores) |
| Plugin repo | `blog-plugin` or `oc-blog-plugin` |
| Database tables | `author_plugin_xxx` (prefixed) |
| Model properties | `snake_case` for DB attributes, `camelCase` for PHP |
| Boolean columns | `is_activated`, `is_visible` |
| Controller names | Plural (`Products`, `Categories`) |
| Model names | Singular (`Product`, `Category`) |
| Component names | `ProductList`, `ProductDetails` (suffix for function) |
| View files | Underscore prefix for partials (`_field.htm`) |

**Build order for plugins:** Plugin → Models → Migrations → Controllers → Components

**PSR exceptions:** Controllers may use snake_case method names; AJAX handlers use `index_onAction` or `onAction` patterns.

## Plugin Commands

| Command | Description |
|---------|-------------|
| `/octobercms:setup` | Interactive setup - detect version, download docs |
| `/octobercms:sync-docs` | Manually sync documentation from GitHub |
| `/octobercms:october-version [ver]` | Switch documentation version |
| `/octobercms:update-settings` | Change auto-sync settings |