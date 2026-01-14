---
description: Show the state of Marketplaces and Plugins across all scopes
allowed-tools: Read(*)
context: fork
---

# Claude Code Marketplaces and Plugins

Show the user the state of Marketplaces and Plugins across all scopes.

## Step 1: Read these files

Use the Read tool on each file:

1. `~/.claude/plugins/known_marketplaces.json` — added marketplaces
2. `~/.claude/plugins/installed_plugins.json` — installed plugins (scope + project)
3. `~/.claude/settings.json` — user-scope enabledPlugins
4. `.claude/settings.json` — project-scope enabledPlugins
5. `.claude/settings.local.json` — local-scope enabledPlugins

## Step 2: Interpret the data

- **Marketplaces**: only "added" (no install/enable states)
- **Plugins**: must be both installed AND enabled
- If plugin not in `enabledPlugins`, it defaults to **enabled**
- If `enabledPlugins` has `"plugin@marketplace": false`, it's **disabled**

**Source of truth:**

- `installed_plugins.json` — definitive list of installed plugins
- `enabledPlugins` in settings — only controls enabled/disabled toggle
- Orphaned entries may linger in `enabledPlugins` after uninstalling plugins (safe to remove)

**Scope precedence** (highest → lowest):

1. `.claude/settings.local.json` (local) - highest
2. `.claude/settings.json` (project)
3. `~/.claude/settings.json` (user)

## Step 3: Present summary

Use this format (example data shown):

<format>

### Marketplaces (3 active)

Sort table by: Marketplace

| Active | Marketplace | Source |
| :----- | :---------- | :----- |
| ✅ | anthropic-agent-skills | anthropics/skills |
| ✅ | claude-plugins-official | anthropics/claude-plugins-official |
| ✅ | my-claude-marketplace | michellepace/my-claude-marketplace |

### Installed Plugins (7)

Sort table by: Marketplace → Plugin → Project → Scope

| Marketplace | Plugin | Project | Scope | Installed | Enabled |
| :---------- | :----- | :------ | :---- | :-------- | :------ |
| anthropic-agent-skills | example-skills | docs-for-ai | local | ✅ | ✅ |
| claude-plugins-official | code-review | devflow | project | ✅ | ✅ |
| claude-plugins-official | feature-dev | devflow | project | ✅ | ✅ |
| claude-plugins-official | pr-review-toolkit | devflow | project | ✅ | ✅ |
| my-claude-marketplace | shadcn-ui | devflow | project | ✅ | ✅ |
| my-claude-marketplace | skill-creator | devflow | project | ✅ | ✅ |
| my-claude-marketplace | tailwindcss | devflow | project | ✅ | ❌ |

</format>

Ensure accuracy. If unsure, tell the user.
