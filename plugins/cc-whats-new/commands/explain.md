---
description: What's new in Claude Code (eg 2.1.2 or 2.1 for all 2.1.*)
argument-hint: [version]
allowed-tools: Task, Read, Bash(claude:*), Bash(curl:*), Bash(awk:*), Bash(echo:*), Bash(npm view:*), Bash(grep:*), Bash(tail:*), Bash(tac:*), Bash(sed:*), Bash(tr:*), Bash(paste:*), Bash(column:*)
---

**Version provided:** $ARGUMENTS

**Claude Code User Version:** !`claude --version`

## Step 1: Show Latest Versions

Run these commands for changelog data:

```bash
# Cache both data sources once
CHANGELOG=$(curl -s https://raw.githubusercontent.com/anthropics/claude-code/refs/heads/main/CHANGELOG.md);
CHANGELOG=$(echo "$CHANGELOG" | awk '/^## [01]\./{exit} {print}');  # Exclude pre-2.0.0 versions (wonky data)
NPM_RELEASE_DATES=$(npm view @anthropic-ai/claude-code time);

# Extract versions from changelog (source of truth for this command)
VERSIONS=$(echo "$CHANGELOG" | awk '/^## [0-9]/{count++; if(count<=4) print $2}');
PATTERN=$(echo "$VERSIONS" | paste -sd '|');

# When were these versions released?
echo "<changelog_data>";
echo "=== Release Dates ===";
echo "<release_dates>";
echo "$NPM_RELEASE_DATES" | grep -E "'($PATTERN)'" | tac | sed "s/T.*Z'//" | tr -d "':," | column -t;
echo "</release_dates>";

# What changed?
echo "";
echo "=== Latest Changelog ===";
echo "<latest_changelog>";
echo "$CHANGELOG" | awk '/^## [0-9]/{count++} count<=4';
echo "</latest_changelog>";

# All versions with item counts
echo "";
echo "=== All Versions ===";
echo "<all_versions_with_item_count>";
echo "version|count";
echo "$CHANGELOG" | awk '/^## /{if(v)print v"|"c;v=$2;c=0}/^- /{c++}END{print v"|"c}';
echo "total: $(echo "$CHANGELOG" | awk '/^## [0-9]/{n++}END{print n}') versions";
echo "</all_versions_with_item_count>";
echo "</changelog_data>";
```

Analyse changelog data and present in this format (apply backticks):

<format_changelog_summary>

Changelog covers [total_versions] versions (`[latest]` → `[earliest]`)

LATEST `[N]` Versions:

┌─────────┬───────┬────────────┬────────────────────────────────────────────────────┐
│ Version │ Items │ Released   │ At A Glance                                        │
├─────────┼───────┼────────────┼────────────────────────────────────────────────────┤
│ x.x.xxx │    nn │ YYYY-MM-DD │ most impactful on user experience (40-50 chars)    │
│ ...     │   ... │ ...        │                                                    │
└─────────┴───────┴────────────┴────────────────────────────────────────────────────┘

</format_changelog_summary>

## Step 2: Determine Provided Version

Check if `$ARGUMENTS` is provided and contains a version in the changelog.

**Valid version?** → Proceed to Step 3

**No version or invalid?** → Try to infer a valid version the user might mean:

> 🤔 Which version? Did you perhaps mean `[version]` or ...?

## Step 3: Acknowledge & Proceed

| Input Type | Example | Response |
| ---------- | ------- | -------- |
| Minor series | `2.1` | "🙂 Analysing all `2.1.*` versions..." |
| Exact version | `2.1.2` | "🙂 Analysing version `2.1.2`... (tip: use `2.1` for all 2.1.* versions)" |

Then proceed to Step 4.

## Step 4: Extract Changelog

Extract the changelog section for the determined version:

```bash
VERSION="[VERSION]"  # e.g., "2.1.3" or "2.1"

if [[ "$VERSION" =~ ^[0-9]+\.[0-9]+$ ]]; then
  # Series (e.g., 2.1) - get all matching versions
  SECTION=$(curl -s https://raw.githubusercontent.com/anthropics/claude-code/refs/heads/main/CHANGELOG.md | awk -v ser="$VERSION." '/^## [0-9]/ && index($2, ser) == 1 { p=1 } /^## [0-9]/ && p && index($2, ser) == 0 { exit } p')
else
  # Exact version (e.g., 2.1.3)
  SECTION=$(curl -s https://raw.githubusercontent.com/anthropics/claude-code/refs/heads/main/CHANGELOG.md | awk -v ver="$VERSION" '/^## [0-9]/ { if ($2 == ver) { p=1 } else if (p) { exit } } p')
fi

echo "<changelog_extracted version=\"$VERSION\">"
echo "$SECTION"
echo "</changelog_extracted>"
```

Proceed to Step 5 where this output will be provided to the agent.

### Step 5: Explain Changes Practically

Use the Task tool to spawn `claude-code-guide` agent with the extracted changelog from Step 4 above:

<agent_prompt>

**GOAL:** Help users use Claude Code more effectively by explaining what's new in version [VERSION].

Changelog entries:

[INSERT THE <changelog_extracted version="...">...</changelog_extracted> BLOCK FROM ABOVE]

1. Evaluate which changes highly impact Claude Code user experience.

2. Create a **Summary table:** "Feature | Benefit". Rank by impact on Claude Code users (most impactful first). Width <100 characters. Group fixes separately if many items.

3. Explain **significant features:** using the `<explain_features_template>` below and your Claude Code Guide knowledge for useful examples.

   <explain_features_template>

   ---

   ## ⭐ **[Feature Name]**

   [Explain what the feature means and WHY it matters (2 sentences max)]

   EXAMPLE: [Practical example helping user see relevance and realise value from this feature.]

   ---

   </explain_features_template>

</agent_prompt>

Present the agent's output directly to the user.
