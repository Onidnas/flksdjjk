---
title: "Configuración de Neovim con lazy.nvim"
date: 2026-08-26
tags:
  - nvim
  - programacion
  - herramientas
  - dotfiles
  - second-brain
  - quickshell
  - lua
aliases:
  - "nvim config"
  - "lazyvim"
  - "editor"
---

# Configuración de Neovim con lazy.nvim

> Guía completa para configurar Neovim desde cero usando **lazy.nvim** como gestor de plugins.
> Configurado para **Neovim 0.12+** con las APIs más recientes.

## ¿Qué es lazy.nvim?

[lazy.nvim](https://lazy.folke.io/) es un gestor de plugins moderno para Neovim creado por [folke](https://github.com/folke). Características principales:

- **Autoclonado**: Se instala automáticamente desde GitHub la primera vez que abres Neovim
- **Lazy-loading**: Carga plugins solo cuando se necesitan (mejora el tiempo de inicio)
- **UI visual**: Interfaz gráfica para gestionar plugins (`:Lazy`)
- **Lockfile**: `lazy-lock.json` bloquea versiones para reproducibilidad
- **Cache**: Compila módulos Lua a bytecode para mayor velocidad

**Repo**: `https://github.com/folke/lazy.nvim`

---

## Estructura de archivos

```
~/.config/nvim/
├── init.lua                      ← Punto de entrada
├── lua/
│   ├── config/
│   │   └── lazy.lua              ← Bootstrap + setup de lazy.nvim
│   └── plugins/
│       ├── telescope.lua         ← Buscador fuzzy
│       ├── treesitter.lua        ← Resaltado de sintaxis (branch main)
│       ├── lsp.lua               ← Language servers + blink.cmp
│       ├── nvimtree.lua          ← Explorador de archivos
│       ├── barbar.lua            ← Pestañas/buffers
│       └── colorscheme.lua       ← Tema catppuccin-nvim
```

---

## Instalación paso a paso

### 1. Punto de entrada: `init.lua`

```lua
require("config.lazy")
```

### 2. Bootstrap de lazy.nvim: `lua/config/lazy.lua`

Este código **clona lazy.nvim desde GitHub** la primera vez. No necesitas instalarlo manualmente.

```lua
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.uv.fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then
    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
		      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)

vim.g.mapleader = " "
vim.g.maplocalleader = "\\"

require("lazy").setup({
  spec = {
    { import = "plugins" },
  },
  install = { colorscheme = { "habamax" } },
  checker = { enabled = true },
})
```

**¿Cómo funciona?**
1. Calcula la ruta donde lazy.nvim debería estar (`stdpath("data")/lazy/lazy.nvim`)
2. Verifica si ya existe con `vim.uv.fs_stat` (API de Neovim 0.12, reemplaza a `vim.loop`)
3. Si no existe, clona el repo con `git clone --branch=stable`
4. Añade lazy.nvim al `runtimepath` de Neovim
5. Llama a `require("lazy").setup()` que carga automáticamente todos los archivos en `lua/plugins/`

---

## Plugins instalados

### Telescope (`nvim-telescope/telescope.nvim`)

Buscador fuzzy para archivos, buffers, grep, y más.

| Atajo        | Acción                       |
| ------------ | ---------------------------- |
| `<leader>ff` | Buscar archivos              |
| `<leader>fg` | Búsqueda en vivo (live grep) |
| `<leader>fb` | Listar buffers               |
| `<leader>fh` | Buscar en help               |
| `<leader>fr` | Archivos recientes           |

### Treesitter (`nvim-treesitter/nvim-treesitter` — branch `main`)

Resaltado de sintaxis mejorado y parsing de código.

> **Nota**: En Neovim 0.12+, el repositorio `master` fue archivado. Usa el branch `main`.
> La API antigua `require("nvim-treesitter.configs").setup()` ya no existe.
> Se usa la nueva API: `require("nvim-treesitter").install()` + `vim.treesitter.start()`.

- Instala parsers para: lua, vim, javascript, typescript, tsx, html, css, json, yaml, markdown, bash, **qmljs**
- El parser `qmljs` es para archivos QML/QuickShell
- Usa un `FileType` autocmd para habilitar highlighting e indentación automáticamente

### LSP + Completado (`neovim/nvim-lspconfig` + `mason.nvim` + `blink.cmp`)

Language Server Protocol para autocompletado, errores en tiempo real, y navegación.

**Stack de LSP:**
- `mason-org/mason.nvim` — Gestor de paquetes para LSP servers
- `mason-org/mason-lspconfig.nvim` — Bridge entre Mason y lspconfig
- `neovim/nvim-lspconfig` — Configuración de language servers
- `saghen/blink.cmp` — Motor de completado moderno (reemplaza nvim-cmp)

**Servers instalados vía Mason:**

| Server   | Para qué sirve                |
| -------- | ----------------------------- |
| `lua_ls` | Lua (configuración de Neovim) |
| `ts_ls`  | JavaScript / TypeScript       |
| `html`   | HTML                          |
| `cssls`  | CSS                           |
| `qmlls`  | QML / QuickShell              |

**Atajos del LSP:**

| Atajo        | Acción                |
| ------------ | --------------------- |
| `gd`         | Ir a definición       |
| `K`          | Hover (documentación) |
| `<leader>ca` | Code action           |
| `<leader>rn` | Rename                |
| `gr`         | Referencias           |
| `[d` / `]d`  | Navegar diagnósticos  |

**Atajos de blink.cmp:**

| Atajo             | Acción                        |
| ----------------- | ----------------------------- |
| `<C-y>`           | Aceptar completado            |
| `<C-n>` / `<C-p>` | Siguiente / Anterior          |
| `<C-space>`       | Abrir menú de completado      |
| `<Tab>`           | Siguiente snippet placeholder |
| `<S-Tab>`         | Anterior snippet placeholder  |

### NvimTree (`nvim-tree/nvim-tree.lua`)

Explorador de archivos estilo VS Code.

- `<leader>e` para abrir/cerrar el explorador

### Barbar (`romgrk/barbar.nvim`)

Barra de pestañas/buffers estilo VS Code.

| Atajo   | Acción                      |
| ------- | --------------------------- |
| `<A-,>` | Buffer anterior             |
| `<A-.>` | Buffer siguiente            |
| `<A-c>` | Cerrar buffer               |
| `<A-<>` | Mover buffer a la izquierda |
| `<A->`  | Mover buffer a la derecha   |

### Catppuccin (`catppuccin/nvim`)

Tema de colores con variantes: latte, frappé, macchiato, **mocha** (la que usamos).

> **Nota**: En v2.0.0+, el nombre del colorscheme cambió a `catppuccin-nvim` (diferente al colorscheme builtin de Vim).

---

## QuickShell LSP

> **Sí, existe soporte LSP para QuickShell en Neovim.**

### Servidor: `qmlls`

El language server se llama `qmlls` (QML Language Server). Viene con Qt 6.

**Instalación:**
```bash
# Arch Linux
pacman -S qt6-declarative

# Fedora
sudo dnf install qt6-declarative-devel

# Ubuntu/Debian (requiere Qt 6 full)
# Se instala con Qt Creator o desde el instalador offline de Qt
```

### Configuración en `lsp.lua`

Ya está incluida en la configuración:

```lua
lspconfig.qmlls.setup({
  capabilities = capabilities,
  on_attach = on_attach,
  cmd = { "qmlls", "-E" },
  filetypes = { "qml", "qmljs" },
  root_markers = { ".qmlls.ini", "shell.qml", ".git" },
})
```

### Plugin adicional: `quickshell-completions.nvim`

[quickshell-completions.nvim](https://github.com/cushycush/quickshell-completions.nvim) agrega:

- **Completions** específicas de QuickShell (PanelWindow, Process, Variants, WlrLayershell, etc.)
- **Snippets** para patrones comunes de QuickShell
- **Auto-configuración** de `qmlls` con los import paths correctos
- **Treesitter** para el parser `qmljs`

Requiere [blink.cmp](https://github.com/saghen/blink.cmp) como framework de completado.

### Limitaciones conocidas del LSP

- `qmlls` no funciona bien si el archivo no está estructurado correctamente (llaves cerradas)
- No maneja los `Singleton` de QuickShell
- No provee documentación para tipos de QuickShell
- Los imports `root:` no se resuelven
- `PanelWindow` no se resuelve correctamente

### Tip: Archivo `.qmlls.ini`

Crear un archivo `.qmlls.ini` vacío en la raíz de tu config de QuickShell. Quickshell lo llena automáticamente con el `buildDir` correcto al iniciarse. Agregarlo a `.gitignore`.

```ini
[General]
no-cmake-calls=true
importPaths="/usr/lib/qt6/qml"
```

---

## Notas de migración (Neovim 0.12)

Algunos cambios importantes al pasar de versiones anteriores a 0.12:

| Antes                                         | Ahora                                         | Por qué                                  |
| --------------------------------------------- | --------------------------------------------- | ---------------------------------------- |
| `vim.loop`                                    | `vim.uv`                                      | `vim.loop` fue deprecado                 |
| `vim.diagnostic.goto_prev/next`               | `vim.diagnostic.jump()`                       | API deprecada en 0.12                    |
| `vim.lsp.protocol.make_client_capabilities()` | `require('blink.cmp').get_lsp_capabilities()` | blink.cmp maneja capabilities            |
| `williamboman/mason.nvim`                     | `mason-org/mason.nvim`                        | Repos migrados a la organización         |
| `nvim-treesitter` branch `master`             | branch `main`                                 | Master archivado, API cambiada           |
| `catppuccin` (colorscheme)                    | `catppuccin-nvim`                             | Conflicto con colorscheme builtin de Vim |

---

## Comandos útiles de lazy.nvim

| Comando | Descripción |
|---------|-------------|
| `:Lazy` | Abrir la UI de lazy.nvim |
| `:Lazy sync` | Sincronizar plugins (instalar/actualizar/limpiar) |
| `:Lazy install` | Solo instalar plugins faltantes |
| `:Lazy update` | Solo actualizar plugins |
| `:Lazy clean` | Eliminar plugins no usados |
| `:Lazy check` | Verificar si hay actualizaciones |
| `:Lazy profile` | Ver tiempos de carga de plugins |

---

## Flujo de trabajo

1. **Primera ejecución**: Abrir `nvim` en la terminal. lazy.nvim se clona e instala todo automáticamente
2. **Usar**: Todos los atajos y funcionalidades disponibles inmediatamente
3. **Actualizar**: Abrir `:Lazy` y presionar `U` para actualizar todo
4. **Agregar plugins**: Crear un archivo nuevo en `lua/plugins/` que retorne una tabla

---

## Fuentes

- [Documentación oficial lazy.nvim](https://lazy.folke.io/)
- [Repo lazy.nvim](https://github.com/folke/lazy.nvim)
- [Instalación lazy.nvim](https://lazy.folke.io/installation)
- [blink.cmp documentación](https://cmp.saghen.dev/)
- [mason-lspconfig.nvim](https://github.com/mason-org/mason-lspconfig.nvim)
- [nvim-treesitter branch main](https://github.com/nvim-treesitter/nvim-treesitter/tree/main)
- [Documentación QuickShell - LSP](https://quickshell.org/docs/guide/install-setup)
- [quickshell-completions.nvim](https://github.com/cushycush/quickshell-completions.nvim)
- [catppuccin v2.0.0 changelog](https://github.com/catppuccin/nvim/releases/tag/v2.0.0)

---

#nvim #programacion #herramientas #dotfiles #second-brain #quickshell #lua #obsidian
