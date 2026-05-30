# CLAUDE.md - sublime-lumos

> **Ecosystem Context:** See [getlumos/lumos/CLAUDE.md](https://github.com/getlumos/lumos/blob/main/CLAUDE.md) for LUMOS ecosystem overview, cross-repo standards, and shared guidelines.

**Repository**: https://github.com/getlumos/sublime-lumos
**Purpose**: Sublime Text package for LUMOS schema language

---

## What is sublime-lumos?

Sublime Text package for editing `.lumos` files with syntax highlighting, LSP integration, and snippets.

**Status**: v0.1.0 development
**Target**: Sublime Text 4 (and 3 build 3103+)
**Dependencies**: LSP package (optional), lumos-lsp server
**Installation**: Package Control (when published) or manual Git clone

---

## Key Files

| File | Purpose |
|------|---------|
| `LUMOS.sublime-syntax` | Syntax definition (YAML-based, ~130 lines) |
| `LUMOS.sublime-settings` | Package settings (indentation, comments) |
| `LSP-lumos.sublime-settings` | LSP client configuration |
| `snippets/*.sublime-snippet` | 6 snippets for common patterns |
| `README.md` | Installation and usage guide |

---

## Features Implemented

- ✅ Syntax highlighting (keywords, types, attributes, comments, numbers)
- ✅ LSP integration via LSP package and lumos-lsp server
- ✅ 6 snippets (struct, enum variants, account, deprecated)
- ✅ Auto-indentation (2 spaces, smart indent)
- ✅ Comment toggling (line and block comments)
- ✅ Bracket matching and auto-pairing

---

## Installation Methods

### 1. Package Control (Preferred - When Published)

Requires submission to [Package Control](https://packagecontrol.io/docs/submitting_a_package).

**Submission Process:**
1. Create repository on GitHub (done)
2. Add `package-control.json` (optional) or rely on default branch
3. Submit to Package Control via PR
4. Wait for review and approval

### 2. Manual Installation

Users clone to Sublime Text's `Packages/` directory:

**macOS/Linux:**
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

### 3. Git-based (via Package Control)

Users add repository URL directly in Package Control.

---

## LSP Integration

### How It Works

1. **LSP Package**: Community package that provides LSP client infrastructure
2. **lumos-lsp Server**: Language server from core lumos repo (v0.1.1+)
3. **LSP-lumos.sublime-settings**: Configuration file that tells LSP package how to start lumos-lsp

**Features Provided by LSP:**
- Auto-completion (types, fields, keywords)
- Diagnostics (error checking, red squiggles)
- Hover documentation (type information on hover)
- Go to definition (Ctrl+Click on symbols)
- Document symbols (outline view)

### Configuration

Default LSP configuration in `LSP-lumos.sublime-settings`:

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

Users can override in their user settings if needed (e.g., specify full path to lumos-lsp).

---

## Snippets

6 snippets for common LUMOS patterns:

| File | Trigger | Description |
|------|---------|-------------|
| `struct.sublime-snippet` | `struct` | Basic struct definition |
| `enum-unit.sublime-snippet` | `enum` | Unit variant enum |
| `enum-tuple.sublime-snippet` | `enumtuple` | Tuple variant enum |
| `enum-struct.sublime-snippet` | `enumstruct` | Struct variant enum |
| `solana-account.sublime-snippet` | `account` | Solana account with #[solana] #[account] |
| `deprecated-field.sublime-snippet` | `deprecated` | Field with #[deprecated] attribute |

**Snippet Format:**

Sublime snippets use XML format with `<![CDATA[...]]>` for content and `${1:placeholder}` for tab stops.

---

## Syntax Definition

### LUMOS.sublime-syntax Structure

**Format**: YAML-based (Sublime Text 3.0+)
**Contexts**: main, comments, attributes, keywords, types, strings, numbers, operators, punctuation
**Scopes**: Use Sublime's standard scope naming (e.g., `keyword.control.lumos`, `entity.name.type.lumos`)

**Key Contexts:**

1. **comments** - Line (`//`) and block (`/* */`)
2. **attributes** - `#[solana]`, `#[account]`, `#[version]`, `#[deprecated]`
3. **keywords** - `struct`, `enum`, `use`, `pub`, `type`, `const`
4. **types** - Primitives, Solana types, Option, Vec, user-defined
5. **strings** - Double-quoted with escape sequences
6. **numbers** - Decimal, hex, binary, octal

**Scope Naming Convention:**

- Keywords: `keyword.control.lumos`
- Types: `entity.name.type.{primitive|solana|option}.lumos`
- Attributes: `entity.name.function.attribute.lumos`
- Comments: `comment.{line|block}.lumos`
- Strings: `string.quoted.double.lumos`
- Numbers: `constant.numeric.{decimal|hex|binary|octal}.lumos`

---

## Development Workflow

### Testing Syntax Changes

1. Edit `LUMOS.sublime-syntax`
2. Save file
3. Reload Sublime Text (or run: `View` → `Reload Syntax`)
4. Open a `.lumos` file and check highlighting

**Debugging:**

- Open Sublime Console: `View` → `Show Console`
- Check for syntax errors in the `.sublime-syntax` file
- Use `Ctrl+Shift+P` → `Set Syntax: LUMOS` to manually apply

### Testing LSP Integration

1. Install LSP package from Package Control
2. Install lumos-lsp: `cargo install lumos-lsp`
3. Open a `.lumos` file
4. Check status bar for `lumos-lsp` indicator (green ✓ = working)
5. Test features:
   - Type `str` → auto-completion suggests `struct`
   - Introduce error → red squiggle appears
   - Hover over type → documentation appears

**Troubleshooting:**

- `Tools` → `LSP` → `Toggle Log Panel` - check for errors
- `Tools` → `LSP` → `Troubleshoot Server` - diagnose issues
- Verify `lumos-lsp` is in PATH: `which lumos-lsp`

### Testing Snippets

1. Edit `.sublime-snippet` file
2. Save file
3. Restart Sublime Text (snippets are loaded on startup)
4. Open `.lumos` file
5. Type trigger (e.g., `struct`) → press `Tab`
6. Verify snippet expands correctly

---

## Publishing Checklist

### Package Control Submission

- [ ] Repository public on GitHub
- [ ] README.md complete with installation instructions
- [ ] All files tested locally
- [ ] LICENSE files present
- [ ] Create release tag (e.g., `v0.1.0`)
- [ ] Submit to Package Control: [Submission Guide](https://packagecontrol.io/docs/submitting_a_package)
- [ ] Wait for review (usually 1-3 days)

**What Package Control Checks:**

- Repository structure (no malicious code)
- Valid JSON/YAML files
- License present
- README exists
- Follows naming conventions

### Pre-submission Testing

- [ ] Test on macOS (if available)
- [ ] Test on Windows (if available)
- [ ] Test on Linux (if available)
- [ ] Test with LSP package installed
- [ ] Test without LSP package (syntax should still work)
- [ ] Test all snippets

---

## Related Repositories

- **lumos** (core): Compiler, CLI, LSP server
- **vscode-lumos**: VS Code extension
- **intellij-lumos**: IntelliJ IDEA / Rust Rover plugin
- **nvim-lumos**: Neovim plugin with Tree-sitter
- **lumos-mode**: Emacs major mode
- **tree-sitter-lumos**: Tree-sitter grammar
- **awesome-lumos**: Examples and templates
- **docs-lumos**: Official documentation site

---

## AI Assistant Guidelines

### ✅ DO

- Test syntax highlighting changes in Sublime Text before committing
- Keep `LUMOS.sublime-syntax` in valid YAML format
- Use standard Sublime scope names (e.g., `keyword.control`, not `keyword.custom`)
- Update README.md when adding new features
- Reference file:line when discussing code

### ❌ DON'T

- Use outdated `.tmLanguage` XML format (use `.sublime-syntax` YAML instead)
- Add features without testing in Sublime Text
- Break LSP integration with invalid settings
- Include AI attribution in commits or code
- Change snippet format (must be valid Sublime snippet XML)

---

## Common Issues

### Issue: Syntax Highlighting Not Working

**Cause**: File not recognized as `.lumos`
**Fix**: Ensure file has `.lumos` extension, or manually set syntax via `View` → `Syntax` → `LUMOS`

### Issue: LSP Not Starting

**Cause**: LSP package not installed or `lumos-lsp` not in PATH
**Fix**:
1. Install LSP package via Package Control
2. Verify `lumos-lsp` installed: `which lumos-lsp`
3. Check LSP logs: `Tools` → `LSP` → `Toggle Log Panel`

### Issue: Snippets Not Expanding

**Cause**: Sublime Text didn't reload snippets, or wrong file type
**Fix**: Restart Sublime Text, ensure file is `.lumos`, press `Tab` after trigger

---

## Future Enhancements

Potential improvements (not committed):

- [ ] Build system integration (run `lumos generate` from Sublime)
- [ ] Custom color schemes optimized for LUMOS
- [ ] Auto-import suggestions (via LSP or custom plugin)
- [ ] Inline schema validation (beyond LSP)
- [ ] Quick documentation panel

---

**Last Updated**: 2025-11-24
**Version**: 0.1.0
**Status**: Development
