# cc-marketplaces-plugins

Shows the state of Marketplaces and Plugins across all scopes.

## What's Inside

| Command | Description |
| :------ | :---------- |
| `/cc-marketplaces-plugins:list` | Display all marketplaces and plugins with their status |

## Usage

```
/cc-marketplaces-plugins:list
```

## Sample Output

### Marketplaces (3 active)

| Active | Marketplace | Source |
| :----- | :---------- | :----- |
| ✅ | anthropic-agent-skills | anthropics/skills |
| ✅ | claude-plugins-official | anthropics/claude-plugins-official |

### Installed Plugins (7)

| Marketplace | Plugin | Project | Scope | Installed | Enabled |
| :---------- | :----- | :------ | :---- | :-------- | :------ |
| anthropic-agent-skills | example-skills | docs-for-ai | local | ✅ | ✅ |
| claude-plugins-official | code-review | devflow | project | ✅ | ✅ |
| claude-plugins-official | feature-dev | devflow | project | ✅ | ✅ |
| claude-plugins-official | pr-review-toolkit | devflow | project | ✅ | ❌ |

---

*Output will vary based on your installed marketplaces and plugins.*
