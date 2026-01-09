# Distribution Methods for XDS

*Exploration — January 2026*

## Context

Research into how to distribute various XDS artifacts: AI assistant skills/rules, code snippets, component recipes, and starter templates. Goal: make XDS easy to adopt across different tooling ecosystems.

---

## 1. AI Assistant Skills/Rules

How to distribute Cursor rules, Claude Code commands, and similar AI configurations.

| Method | How It Works | Pros | Cons |
|--------|--------------|------|------|
| **PRPM** | Package manager for Cursor rules via npm. `prpm install xds-rules` | Cross-platform (Cursor, Claude, Windsurf), versioned, CLI install | External dependency, community-run |
| **npm package** | Ship rules in npm package, postinstall copies to `.cursor/rules/` | Bundled with library, versioned with components | Manual setup, may conflict |
| **Repo distribution** | Include `.cursor/rules/` and `.claude/commands/` in XDS repo | Zero setup for monorepo users | Doesn't work for npm consumers |
| **Claude Code skills** | Ship skills in `~/.claude/skills/` structure | Auto-activating, trigger-based | Manual install, no package manager yet |

### Claude Code Specifics

- Skills are stored in `~/.claude/skills/`
- Commands in `~/.claude/commands/`
- Skills auto-activate based on triggers (file saves, commits, etc.)
- Distribution: commit to repo or copy to user's `~/.claude/` directory

### Cursor Specifics

- Rules stored in `.cursor/rules/` in project
- PRPM provides cross-editor package management
- Can convert between Cursor, Claude, Windsurf formats automatically

**Recommendation**: Ship rules/skills in npm package + provide PRPM package for standalone install.

Source: [PRPM Package Manager](https://forum.cursor.com/t/prpm-a-package-manager-for-cursor-rules-1800-cursor-rules-installable-via-the-cli/139557)

---

## 2. Code Snippets/Templates

VS Code snippets and similar editor integrations.

| Method | How It Works | Pros | Cons |
|--------|--------------|------|------|
| **VS Code extension** | Publish `.code-snippets` as extension | Auto-updates, marketplace discovery | Maintenance overhead, VS Code only |
| **Repo snippets** | Include `.vscode/xds.code-snippets` in XDS repo | Zero config for repo users | Doesn't travel with npm package |
| **npm postinstall** | Copy snippets to project `.vscode/` on install | Bundled with library | May overwrite user config |

**Recommendation**: Include snippets in repo, document manual copy for npm users.

---

## 3. Component Recipes (Composed Patterns)

Pre-built compositions like "form with validation" or "data table with pagination".

| Method | How It Works | Pros | Cons |
|--------|--------------|------|------|
| **shadcn-style CLI** | `npx xds add form-with-validation` copies source to project | Full ownership, customizable, AI-friendly | Maintenance of CLI, versioning complexity |
| **npm subpackages** | `@xds/recipes` package with pre-composed patterns | Standard npm workflow | Less customizable, bundle size |
| **Storybook recipes** | Document recipes in Storybook with copy button | Visual docs, no CLI needed | Manual copy-paste |
| **Documentation site** | Dedicated recipes section with code examples | Searchable, contextual | Not automated |

### Why shadcn-style Works

From shadcn/ui docs: "A flat-file schema and command-line tool make it easy to distribute components."

Key benefits:
- **Transparency**: Developers see exactly how components function
- **Customization**: Direct code modification replaces style-override workarounds
- **AI Integration**: LLMs can read, understand, and improve component code
- **Open Code**: Top layer remains editable, not locked in library

This aligns with XDS zero-styling architecture — users get the code, can customize, AI can read/modify.

**Recommendation**: shadcn-style CLI for recipes.

Source: [shadcn/ui Documentation](https://ui.shadcn.com/docs)

---

## 4. Starter Templates

Full project scaffolds for new apps.

| Method | How It Works | Pros | Cons |
|--------|--------------|------|------|
| **`create-xds` CLI** | `npx create-xds my-app` scaffolds full project | Familiar pattern (CRA, Vite), batteries-included | Maintenance of templates |
| **GitHub template repos** | "Use this template" button on GitHub | Zero tooling, GitHub-native | No customization prompts |
| **degit** | `npx degit xds/starter my-app` | Lightweight, no git history | Less discoverable |
| **Stackblitz/CodeSandbox** | One-click cloud templates | Instant try-before-install | Not local-first |

**Recommendation**: `create-xds` CLI with prompts for variants (Next.js, Vite, etc.) + GitHub template repos as fallback.

---

## Recommended Architecture

```
xds/
├── packages/
│   ├── core/                    # Components (npm)
│   ├── cli/                     # `npx xds add <recipe>` (shadcn-style)
│   └── create-xds/              # `npx create-xds` scaffolder
├── templates/
│   ├── nextjs-starter/          # GitHub template repo
│   └── vite-starter/
├── skills/
│   ├── .cursor/rules/           # Cursor rules (also on PRPM)
│   └── .claude/commands/        # Claude Code commands
└── .vscode/
    └── xds.code-snippets        # VS Code snippets
```

### Distribution Channels

| Artifact | Primary Channel | Secondary Channel |
|----------|-----------------|-------------------|
| Components | npm (`@xds/core`) | — |
| Recipes | CLI (`npx xds add`) | Docs site |
| Starters | CLI (`npx create-xds`) | GitHub templates |
| AI rules | npm + PRPM | Repo inclusion |
| Snippets | Repo `.vscode/` | Docs (manual copy) |

---

## Open Questions

- Should PRPM be a dependency or optional enhancement?
- How do we version AI rules separately from components?
- Can we auto-detect IDE and install appropriate snippets/rules?

---

## Sources

- [PRPM: Package Manager for Cursor Rules](https://forum.cursor.com/t/prpm-a-package-manager-for-cursor-rules-1800-cursor-rules-installable-via-the-cli/139557)
- [shadcn/ui CLI Documentation](https://ui.shadcn.com/docs)
- [Claude Code Skills Guide](https://gist.github.com/alirezarezvani/a0f6e0a984d4a4adc4842bbe124c5935)
