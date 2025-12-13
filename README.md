# RevSpell Converter (TFS RevScript)

RevSpell Converter reads `spells.xml` from a TFS-based server and generates modern RevScript Lua files. It preserves original combat logic when available and wraps it into a clean `Spell("instant"|"rune")` interface.

Desktop tool developed with Tauri + Rust for converting and managing TFS files.

## Features

- XML parsing for `instant` and `rune` entries
- Duplicate words handling by merging vocations
- ID rules
  - Runes use XML `itemID`
  - Normal and conjuring spells use sequential IDs starting from a base (default `100`)
- Output structure
  - `converter/runes/{group}/...`
  - `converter/spells/{group}/...`
  - `converter/conjuring/...`
- Lua generation
  - Keeps original Lua combat on top and calls `combat:execute(creature, var)`
  - Adds required spell configs (group, id, name, words, level, mana, vocations)
  - Applies defaults when missing (`cooldown`, `groupCooldown`)
  - Reads XML flags like `needWeapon`, `casterTargetOrDirection`, `blockwalls`, `range`
- UI
  - Dark theme
  - Language toggle `PT/EN`
  - Full table with Words, Premium, Range, Cooldown, Script
  - Preview shows the original callback when available

## 📋 Prerequisites

Before starting, ensure you have the following installed on your machine:

1. **Rust** (Programming Language)
   - Download and install: https://rustup.rs/
   - During installation, select the default option.
   - Verify:
     ```
     rustc --version
     cargo --version
     ```
2. **Visual Studio Build Tools** (For compiling C++ on Windows)
   - Required for compiling Rust dependencies.
   - Download “Visual Studio Build Tools” and install the “Desktop development with C++” workload.
3. **WebView2** (Already installed on updated Windows 10/11)
   - Tauri uses Edge’s WebView2 to render the interface.
4. **Tauri CLI** (Command Line Tool)
   - After installing Rust, run in your terminal:
     ```
     cargo install tauri-cli
     ```

## How to Run

### Using the build script (recommended)

- Portuguese: run `build_pt.bat`
- English: run `build_eng.bat`

The script provides options like:
- Install and verify prerequisites (Rust / Tauri CLI)
- Run in DEV
- Build .exe (fast, with cache)
- Build .exe (full, clean cache)
- Test the built .exe

### Build outputs (Windows)

After a successful build, you will find:
- Portable executable:
  - `src-tauri\target\release\spell-converter.exe`
  - Run directly without installing
- Installer (MSI):
  - `src-tauri\target\release\bundle\msi\Spell Converter_1.0.0_x64_en-US.msi`
  - Installs the app on your PC with Start Menu entry and uninstall support
- Installer (NSIS setup):
  - `src-tauri\target\release\bundle\nsis\Spell Converter_1.0.0_x64-setup.exe`
  - Classic setup wizard installer

Pick one:
- Quick use → double-click `spell-converter.exe`
- Install on your PC → run the MSI or the NSIS setup

### Manual

```bash
cd src-tauri
cargo tauri dev
# or
cargo tauri build
```

## Usage

1. Open the app
2. Click “Escolher Pasta” / “Choose Folder” and select the folder containing `spells.xml`
3. Set Base ID if needed (default `100`)
4. Click “Converter RevScripts”
5. Generated files are placed under `converter/` following the folder rules above

## Deployment (Install on your OT server)

After clicking “Converter RevScripts”, open the output folder:
- Default output root: `converter/`
- Optionally rename `converter/` to `spells/` if your server expects that name

Copy the generated scripts to your server:
- Spells (instant): `converter/spells/{group}/...` → place under your server `data/scripts/spells/`
- Runes: `converter/runes/{group}/...` → place under `data/scripts/runes/` (or your runes script folder)
- Conjuring: `converter/conjuring/...` → place under `data/scripts/spells/` (they are `Spell("instant")` with words)

Notes:
- Keep folder structure by group (attack, support, healing) or flatten if your server expects a flat layout
- RevScript files already call `spell:register()`, so no XML registration is required
- If your distribution uses a different RevScript root, adjust paths accordingly (e.g., `data/scripts/`)

### Instalação (PT-BR)

Depois de clicar em “Converter RevScripts”, abra a pasta de saída:
- Raiz padrão: `converter/` (você pode renomear para `spells/` se seu OT exigir)

Copie os scripts para seu servidor:
- Spells normais: `converter/spells/{group}/...` → `data/scripts/spells/`
- Runes: `converter/runes/{group}/...` → `data/scripts/runes/`
- Conjuração: `converter/conjuring/...` → `data/scripts/spells/`

Observações:
- Mantenha a estrutura por grupos ou ajuste conforme o seu OT
- Os RevScripts já fazem `spell:register()`, não precisa cadastrar no XML

## Contributing

Pull requests are welcome. Just make sure you are using English language.

### How to Contribute

- Fork the project
- Create a feature branch: `git checkout -b feature/new-feature`
- Commit your changes: `git commit -am 'Add new feature'`
- Push the branch: `git push origin feature/new-feature`
- Open a Pull Request

### Como Contribuir

- Fork o projeto
- Crie uma branch para sua feature: `git checkout -b feature/nova-feature`
- Commit suas mudanças: `git commit -am 'Adiciona nova feature'`
- Push para a branch: `git push origin feature/nova-feature`
- Abra um Pull Request

Suggested areas to improve:
- Support for additional XML attributes and edge cases
- Better handling of special groups (custom, house, party)
- More validations for duplicate words and conflicting IDs
- Additional language strings (EN/PT) for footer and badges
- Optional export formats or script naming strategies

Workflow:
- Fork the repo
- Create a topic branch
- Add tests or reproducible examples when possible
- Open a PR describing the change and reasoning in English

## Notes

- Secrets or keys should never be committed
- Generated Lua scripts follow RevScript conventions
- The tool aims to minimize manual adjustments while staying compatible with legacy combat code
