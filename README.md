# My Claude Plugins (Collected in a Marketplace)

Personal Claude Code marketplace: plugins and skills curated and packaged for individual install/uninstall control.

## Usage

Add this marketplace to your Claude Code:

```shell
/plugin marketplace add michellepace/my-claude-marketplace
```

Then run the command `/plugin` to enable, disable, update, or uninstall a specific plugin in this marketplace at your preferred scope (see [Appendix: Plugin Scope](#appendix-plugin-scope)).

```text
Use the shadcn-ui skill to build this form with proper validation and accessible components.
```

## Plugins Included

Self-authored plugins and skills repackaged from other sources, collected here for modular control.

| Plugin | Origin | Status |
| :----- | :----- | :----- |
| [shadcn-ui](./plugins/shadcn-ui) | This repo | ✅ Self-authored (Shadcn best practices) |
| [cc-whats-new](./plugins/cc-whats-new) | This repo | ✅ Self-authored (Claude Code whats new) |
| [cc-marketplaces-plugins](./plugins/cc-marketplaces-plugins) | This repo | ✅ Self-authored (Show whats enabled) |
| [skill-creator](./plugins/skill-creator) | [anthropics/skills](https://github.com/anthropics/skills) | ✅ Duplicated (don't want other skills) |
| [tailwindcss](./plugins/tailwindcss) | [einverne/dotfiles](https://github.com/einverne/dotfiles) | Remove (uses TW v3) |

## About Plugins and Marketplaces

*Full layman's explanation in [this article](https://ailearnlog.com/claude-code-skills-plugins-marketplaces/).*

A marketplace is a collection of plugins. Each plugin is a "bag" containing any combination of skills, agents, commands, hooks, and MCP servers. Marketplaces consolidate components across all projects, provide source control, easy sharing and updates, and let you enable/disable entire plugins.

For this marketplace, the packaged plugins live in [plugins/](plugins/).

<div align="center">
  <a href="images/marketplace-plugin-sketch.webp" target="_blank">
    <img src="images/marketplace-plugin-sketch.webp" alt="Hand-drawn sketch showing the plugin-marketplace relationship. A large rounded rectangle labelled I will package it into a marketplace contains three smaller rounded shapes representing individual plugins. The leftmost plugin labelled Plugin Bag shows its contents: Skills, Agents, Commands, MCPs, and Hooks. Two additional plugins are labelled another plugin and another one. Below the diagram are five checkmarks listing marketplace benefits with a bracket labelled Marketplace Benefits: Source Control, One place for all projects, plugins can be switched on or off, easy to share, and easy to get updates." width="550">
  </a>
</div>

A plugin can contain any number of the following components:

```text
[plugin-name]/
├── .claude-plugin/plugin.json # Manifest (REQUIRED)
├── commands/ # slash commands (.md)
├── agents/   # new context window (.md)
├── skills/   # extends capabilities (.md)
├── hooks/    # reacts to events (.json)
├── scripts/  # eg. for hooks to run
└── .mcp.json # external tools to use
```

*Note: hooks and MCP servers can alternatively be defined inline in `plugin.json`.*

## Configuring Plugins at Project Level

To share plugin configuration with your team via source control, add this to `.claude/settings.json`:

- **extraKnownMarketplaces** — prompts team members to add the marketplace when they open the project
- **enabledPlugins** — controls which installed plugins are active (`true`) or disabled (`false`)

```json
{
  "extraKnownMarketplaces": {
    "my-claude-marketplace": {
      "source": {
        "source": "github",
        "repo": "michellepace/my-claude-marketplace"
      }
    }
  },

  "enabledPlugins": {
    "shadcn-ui@my-claude-marketplace": true,
    "skill-creator@my-claude-marketplace": true,
    "tailwindcss@my-claude-marketplace": false,
    "cc-whats-new@my-claude-marketplace": true,
    "cc-marketplaces-plugins@my-claude-marketplace": true
  }
}
```

This registers the marketplace and enables 4 plugins (tailwindcss disabled).

## Appendix: Plugin Scope

Plugins can be enabled at four scope levels. Higher scopes override lower ones (managed > local > project > user).

| Scope | Settings File | Who it affects | Shared with team? |
| :---- | :------------ | :------------- | :---------------- |
| **user** | `~/.claude/settings.json` | You, across all projects | No |
| **project** | `.claude/settings.json` | All collaborators on the repo | Yes (committed to git) |
| **local** | `.claude/settings.local.json` | You, in this repo only | No (gitignored) |
| **managed** | `managed-settings.json` (system) | All users on the machine | Yes (deployed by IT) |
