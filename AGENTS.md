# Indie Cross Android Port - AGENTS.md

## Proyecto: Indie Cross Android Port

Puerto Android no oficial del mod **Indie Cross** para Friday Night Funkin' (crossover con Cuphead, Bendy and the Ink Machine y Sans Undertale), construido reutilizando la infraestructura Android ya desarrollada para el port de VS Impostor: Legacy.

**Repo de este port:** https://github.com/jereidk/Indie-Cross-Public-Android
**Repo de referencia del mod (fork, historial truncado al 22 ago 2022):** https://github.com/jereidk/Indie-Cross-Public
**Template Android base:** https://github.com/jereidk/NightmareVision-Android-Support (rama `impostor-legacy-android`)

---

## 📋 Estado del Proyecto

- **Versión actual:** 0.1.0 (recién inicializado, sin `source/`/`assets/` portados todavía)
- **Paquete:** com.brightfyregit.indiecross
- **Rama activa:** `impostor-legacy-android` (nombre heredado del template -- considerar renombrarla más adelante si genera confusión)

---

## ⚠️ Diferencias clave vs. el template (Impostor Legacy)

Este repo arrancó como una copia 1:1 de la infraestructura Android de Impostor Legacy -- pero Indie Cross tiene una arquitectura de motor bastante distinta en su Project.xml original. Antes de portar código a ciegas, tener en cuenta:

- **Scripting:** Indie Cross usa Lua (`linc_luajit`) para mods/scripts. Impostor Legacy (y por lo tanto todo `funkin.scripts.*` de este template) usa `hscript-iris`. Habrá que decidir si se porta el sistema de scripting de Indie Cross tal cual, o si se adapta al hscript-iris ya existente en el template.
- **Discord:** Indie Cross usa `discord_rpc` (no `hxdiscord_rpc`, el que ya trae este template).
- **UI:** Indie Cross NO usa `haxeui-core`/`haxeui-flixel` (el template sí, para los editores tipo ChartEditorState).
- **Librerías de assets:** Indie Cross organiza sus assets en libraries separadas (`songs`, `shared`, `bendy`, `sans`, `cup`, `customSkins`, `achievements`, `hiddenContent`, `notes`) en vez del único `assets/legacy` que usa este template. La estructura de `assets/` va a necesitar un rediseño, no un simple volcado de archivos.
- **`content/NMV-Base-Game` (submódulo):** heredado del template, apunta a `jereidk/base-game-for-android`. Pendiente confirmar si aplica a Indie Cross o hay que reemplazarlo/quitarlo.

---

## 📁 Estructura Clave (heredada del template, sujeta a cambios)

| Archivo/Directorio | Descripción |
|---|---|
| `Project.xml` | Configuración del proyecto Lime/OpenFL (ya actualizado con identidad de Indie Cross) |
| `dlc-registry.json` | Registro de contenido descargable (vacío, sin DLCs propios todavía) |
| `.github/workflows/` | Build automation para Android (heredado del template, revisar nombres/triggers) |
| `content/NMV-Base-Game/` | Submódulo heredado -- ver nota arriba |
| `tools/convert_astc.py`, `tools/astc-config.json` | Pipeline de compresión ASTC para texturas (reutilizable tal cual) |
| `source/` | Código heredado del template (Impostor Legacy) -- pendiente de reemplazo/adaptación con el código real de Indie Cross |
| `assets/` | Assets heredados del template (Impostor Legacy) -- pendiente de reemplazo con los assets reales de Indie Cross |

---

## 🔧 Sistema de Compilación

### Compilación Local

```bash
# 1. Instalar dependencias
haxelib git hxpkg https://github.com/ADA-Funni/hxpkg add-hmm-compatibility
haxelib run hxpkg install

# 2. Configurar Android SDK
haxelib run lime setup android

# 3. Compilar
haxelib run lime build android -release
```

---

## 📝 Notas de Desarrollo

### Referencia del mod original
El clon de referencia (`jereidk/Indie-Cross-Public`, truncado al commit `3c5d63f` -- "Update Prompt.hx", Sirox0, 22 ago 2022) sirve solo como fuente de archivos/assets a portar. El trabajo real de desarrollo se hace sobre este repo (`Indie-Cross-Public-Android`), no sobre esa referencia.

### Keystore
Se reutiliza el mismo `key.keystore` (alias `nvport`) que usaba el template -- decisión explícita del mantenedor, no requiere uno nuevo por proyecto.

---

## 🚀 Próximos Pasos

1. [ ] Decidir estrategia de scripting (adaptar Lua de Indie Cross vs. reusar hscript-iris del template)
2. [ ] Rediseñar estructura de `assets/` según las libraries reales de Indie Cross
3. [ ] Portar `source/` reemplazando el contenido específico de Impostor Legacy
4. [ ] Resolver el submódulo `content/NMV-Base-Game`
5. [x] Reemplazar íconos (`projFiles/icon/*`) con los de Indie Cross (hecho, tomados de `assets/compileData/` del repo de referencia)

---

## 🔗 Links Útiles

- [Repo de este port](https://github.com/jereidk/Indie-Cross-Public-Android)
- [Repo de referencia del mod](https://github.com/jereidk/Indie-Cross-Public)
- [Template base (Impostor Legacy Android)](https://github.com/jereidk/NightmareVision-Android-Support)
