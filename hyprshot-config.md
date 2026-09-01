# hyprshot Configuration - Dell Latitude 7320 + Hyprland + Arch

## Keybindings

| Tecla | Acción | Comando |
|-------|--------|---------|
| `Print` | Capturar región (selección mouse) | `hyprshot -m region` |
| `Shift + Print` | Capturar ventana activa | `hyprshot -m window` |
| `Ctrl + Print` | Capturar monitor completo | `hyprshot -m output` |
| `Fn + Print` (`XF86LaunchA`) | **Capturar región** (tecla laptop) | `hyprshot -m region` |

## Configuración en `~/.config/hypr/hyprland.lua`

### Variables de entorno
```lua
hl.env("HYPRSHOT_DIR", "~/Pictures/Screenshots")
```

### Permisos screencopy (requieren reinicio Hyprland)
```lua
hl.permission("/usr/bin/grim", "screencopy", "allow")
hl.permission("/usr/bin/hyprshot", "screencopy", "allow")
hl.permission("/usr/lib/xdg-desktop-portal-hyprland", "screencopy", "allow")
hl.permission("/usr/bin/hyprpm", "plugin", "allow")
```

### Keybinds (líneas ~315-320)
```lua
-- hyprshot keybinds
hl.bind("Print", hl.dsp.exec_cmd("hyprshot -m region"), { locked = true })
hl.bind("SHIFT + Print", hl.dsp.exec_cmd("hyprshot -m window"), { locked = true })
hl.bind("CTRL + Print", hl.dsp.exec_cmd("hyprshot -m output"), { locked = true })
hl.bind("XF86LaunchA", hl.dsp.exec_cmd("hyprshot -m region"), { locked = true })
```

## Directorio de screenshots
```
~/Pictures/Screenshots/
```

## Dependencias requeridas
- `hyprshot` (instalado)
- `grim` (backend de captura)
- `slurp` (selector de región)
- `wl-clipboard` (clipboard Wayland)
- Daemon notificaciones: `mako` / `dunst` / `swaync`

## Verificación
```bash
# Ver binds activos
hyprctl binds | grep -E "(Print|LaunchA)"

# Test manual
hyprshot -m region

# Ver permisos (tras reinicio)
hyprctl getoption misc:disable_hyprland_logo
```

## Notas Dell Latitude 7320
- `Fn + Print` = `XF86LaunchA` (confirmado via `hyprctl binds`)
- Tecla `Print` sola funciona sin `Fn`
- Si no funciona `Fn + Print`: verificar BIOS → Function Key Behavior → "Function Key First"

## Troubleshooting
| Problema | Solución |
|----------|----------|
| "invalid geometry" | Reiniciar Hyprland (permisos) |
| No notificación | Instalar/iniciar `mako` o `dunst` |
| Clipboard no funciona | `wl-copy` instalado? `wl-paste` prueba |
| Permisos denegados | `hyprctl reload` no basta → `hyprctl dispatch exit` |

## Enlaces relacionados
- [hyprshot GitHub](https://github.com/Gustash/hyprshot)
- [Hyprland Wiki - Binds](https://wiki.hypr.land/Configuring/Basics/Binds/)
- [Hyprland Wiki - Permissions](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Permissions/)