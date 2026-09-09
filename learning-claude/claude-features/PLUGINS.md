[← Back to Main README](../README.md)

# The Problem Plugins Solve

When I build a workflow for myself, I create multiple things:

- **Skills** — Custom instructions for specific tasks
- **Sub-agents** — Specialized agents for specific jobs
- **Custom slash commands** — Workflows I use repeatedly
- **Hooks** — Safety and automation mechanisms
- **MCP tools** — Connections to external services

If I want to share this with a teammate, I have to manually tell them:
- Copy these skill files to `.claude/skills/`
- Copy these agent files to `.claude/agents/`
- Copy these command files to `.claude/commands/`
- Copy these hooks and update `settings.json`
- Copy this MCP configuration
- Make sure everything is set up correctly

This was error-prone. Teammates would miss steps. Things wouldn't work exactly the same way. The experience wasn't consistent.

**The better approach:** Package everything into a single entity. One thing to share. One thing to install. Everything works exactly the same.

**That's exactly what plugins are.**

---

## What Plugins Actually Are

> **A plugin is a folder where you put everything you've created—skills, sub-agents, custom slash commands, hooks, and MCP tools—and package it for distribution.**

When someone installs your plugin, they get an exact replica of your workflow. Everything works exactly the same way it works for you.

### The Plugin Structure

A plugin folder looks like this:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── skill-one.md
│   └── skill-two.md
├── agents/
│   ├── security-reviewer.md
│   └── quality-reviewer.md
├── commands/
│   └── my-command.md
├── hooks/
│   └── my-hook.py
└── mcp.json
```

### The Manifest File (plugin.json)

This file is required for a valid plugin:

```json
{
  "name": "my-workflow-plugin",
  "version": "1.0.0",
  "description": "Complete workflow for my team",
  "author": "Your Name",
  "repository": "https://github.com/you/my-plugin",
  "license": "MIT"
}
```

Without this file, Claude Code doesn't recognize it as a valid plugin.

---

## Marketplaces

Now that I have a plugin, how do others discover and install it?

**A marketplace is a place where multiple plugins are stored.**

Think of it like an app store for Claude Code plugins:

**App Store** → **Apps**  
**Marketplace** → **Plugins**

Just like there are multiple app stores (Google Play, Apple App Store, Xiaomi Store), there are multiple marketplaces for Claude Code plugins.

### How Marketplaces Work Technically

A marketplace is just a GitHub repository that contains a `marketplace.json` file:

```json
{
  "name": "My Company's Plugins",
  "owner": "my-company",
  "plugins": [
    {
      "name": "data-science-workflow",
      "version": "1.0.0",
      "description": "Complete data science workflow"
    },
    {
      "name": "backend-deployment",
      "version": "1.0.0",
      "description": "Deploy backend applications"
    }
  ]
}
```

This is exactly how the official Claude Code marketplace works. It's a GitHub repository with a `marketplace.json` file.

### Types of Marketplaces

**Official Marketplace**
- Pre-installed with Claude Code
- Contains plugins from reputable companies
- Over 172 plugins available

**Third-Party Marketplaces**
- Created by companies or individuals
- Need to be installed first
- Can be private for team use

---

## Installing Plugins

### How to Install from a Marketplace

**Step 1:** Open Claude Code and type `/plugins`

**Step 2:** You'll see three tabs:
- Discover → Browse available plugins
- Installed → See what you've installed
- Marketplaces → Manage marketplaces

**Step 3:** To add a third-party marketplace, click "Add Marketplaces" and paste the GitHub URL.

**Step 4:** The marketplace appears in your Marketplaces tab. You can now browse its plugins in the Discover tab.

**Step 5:** Select the plugin you want and install it.

### Quick Installation with Commands

You can also install plugins directly with commands:

```bash
# Add a marketplace
/plugin marketplace add https://github.com/company/marketplace

# Install a plugin
/plugin install plugin-name
```

---

## My Final Mental Model for Plugins

Today, I think about plugins as a way to share and reuse workflows:

```
                ┌─────────────────────────────────┐
                │      Developer Workflow         │
                │  (Skills, Commands, Hooks)      │
                └─────────────┬───────────────────┘
                              │
                              ▼
                ┌─────────────────────────────────┐
                │         Plugin                  │
                │    (Packaged for sharing)       │
                └─────────────┬───────────────────┘
                              │
                              ▼
                ┌─────────────────────────────────┐
                │       Marketplace               │
                │   (Collection of plugins)       │
                └─────────────┬───────────────────┘
                              │
                              ▼
                ┌─────────────────────────────────┐
                │     Another Developer           │
                │   (Installs & uses instantly)   │
                └─────────────────────────────────┘
```

Each layer makes the knowledge more accessible:
- The developer creates the workflow
- The plugin packages it for sharing
- The marketplace organizes it for discovery
- Another developer installs and benefits instantly

---


[← Back to Main README](../README.md)