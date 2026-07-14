<!-- Satellite context file — extends the global hub (~/.claude/CLAUDE.md | ~/.pi/agent/AGENTS.md). Host-neutral; project-specific only. Do not duplicate hub standards here. -->

# sublime-lumos

> Sublime Text package for the LUMOS schema language with syntax highlighting, LSP integration, and snippets.

**Ecosystem context:** See [getlumos/lumos/AGENTS.md](https://github.com/getlumos/lumos/blob/main/AGENTS.md) for the LUMOS ecosystem overview, cross-repo standards, and shared guidelines.

**Status:** v0.1.0 development
**Target:** Sublime Text 4 (and 3 build 3103+)
**Dependencies:** LSP package (optional), lumos-lsp server

## Key Files

| File | Purpose |
|------|---------|
| `LUMOS.sublime-syntax` | Syntax definition (YAML-based, ~130 lines) |
| `LUMOS.sublime-settings` | Package settings (indentation, comments) |
| `LSP-lumos.sublime-settings` | LSP client configuration |
| `snippets/*.sublime-snippet` | 6 snippets for common patterns |
| `README.md` | Installation and usage guide |

## Features

- ✅ Syntax highlighting (keywords, types, attributes, comments, numbers)
- ✅ LSP integration via LSP package and lumos-lsp server
- ✅ 6 snippets (struct, enum variants, account, deprecated)
- ✅ Auto-indentation (2 spaces, smart indent)
- ✅ Comment toggling (line and block comments)
- ✅ Bracket matching and auto-pairing

## Installation

### Package Control (when published)
Submit to [Package Control](https://packagecontrol.io/docs/submitting_a_package) via PR.

### Manual

```bash
cd ~/Library/Application\ Support/Sublime\ Text/Packages  # macOS
# or ~/.config/sublime-text/Packages  # Linux
git clone https://github.com/getlumos/sublime-lumos.git LUMOS
```

**Windows:**
```powershell
cd "%APPDATA%\Sublime Text\Packages"
git clone https://github.com/getlumos/sublime-lumos.git LUMOS
```

## LSP Integration

1. **LSP Package:** Community package providing LSP client infrastructure
2. **lumos-lsp Server:** Language server from core lumos repo (v0.1.1+)
3. **LSP-lumos.sublime-settings:** Tells LSP package how to start lumos-lsp

Default configuration:

```json
{
  "clients": {
    "lumos-lsp": {
      "enabled": true,
      "command": ["lumos-lsp"],
      "selector": "source.lumos"
    }
  }
}
```

**LSP features:** auto-completion, diagnostics, hover documentation, go-to-definition, document symbols.

## Snippets

| File | Trigger | Description |
|------|---------|-------------|
| `struct.sublime-snippet` | `struct` | Basic struct definition |
| `enum-unit.sublime-snippet` | `enum` | Unit variant enum |
| `enum-tuple.sublime-snippet` | `enumtuple` | Tuple variant enum |
| `enum-struct.sublime-snippet` | `enumstruct` | Struct variant enum |
| `solana-account.sublime-snippet` | `account` | Solana account with #[solana] #[account] |
| `deprecated-field.sublime-snippet` | `deprecated` | Field with #[deprecated] attribute |

## Syntax Definition

**Format:** YAML-based (Sublime Text 3.0+)
**Contexts:** main, comments, attributes, keywords, types, strings, numbers, operators, punctuation

**Scope naming convention:**
- Keywords: `keyword.control.lumos`
- Types: `entity.name.type.{primitive|solana|option}.lumos`
- Attributes: `entity.name.function.attribute.lumos`
- Comments: `comment.{line|block}.lumos`
- Strings: `string.quoted.double.lumos`
- Numbers: `constant.numeric.{decimal|hex|binary|octal}.lumos`

## Development Workflow

### Testing syntax changes
1. Edit `LUMOS.sublime-syntax`
2. Save and reload Sublime Text (`View` → `Reload Syntax`)
3. Open a `.lumos` file and check highlighting
4. Debug via `View` → `Show Console`

### Testing LSP integration
1. Install LSP package from Package Control
2. Install lumos-lsp: `cargo install lumos-lsp`
3. Open a `.lumos` file — status bar shows `lumos-lsp` indicator (green ✓ = working)
4. Troubleshoot via `Tools` → `LSP` → `Toggle Log Panel` / `Troubleshoot Server`

### Testing snippets
Snippets load on startup — restart Sublime Text after editing, then type trigger + `Tab`.

## Gotchas

- Use `.sublime-syntax` YAML format, **not** outdated `.tmLanguage` XML
- Use standard Sublime scope names (`keyword.control`, not `keyword.custom`)
- Snippet format must be valid Sublime snippet XML
- File must have `.lumos` extension to be recognized

## Common Issues

**Syntax highlighting not working** — ensure `.lumos` extension, or set via `View` → `Syntax` → `LUMOS`.
**LSP not starting** — install LSP package, verify `which lumos-lsp`, check LSP logs.
**Snippets not expanding** — restart Sublime Text, ensure `.lumos` file type, press `Tab` after trigger.