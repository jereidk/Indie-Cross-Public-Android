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

## ⚠️ Principio de arquitectura: el template ES el motor

**Este template (la infraestructura Android heredada del port de Impostor Legacy) es el motor/base del proyecto. Indie Cross es el CONTENIDO a portar, adaptado a la estructura y convenciones que ya existen acá -- no al revés.** No se reestructura el template para calzar con cómo estaba armado Indie Cross originalmente (Project.xml de la referencia, scripting, libraries de assets, etc.); esas son solo referencia de QUÉ hay que portar, no de CÓMO debe organizarse acá.

Diferencias conocidas del Project.xml original de Indie Cross (relevantes solo como referencia de contenido a adaptar, no como objetivo de arquitectura):

- **Scripting:** Indie Cross usaba Lua (`linc_luajit`). Acá se mantiene `hscript-iris` (el que ya trae el template) -- cualquier script de Indie Cross se adapta a hscript-iris al portarlo, no se agrega un runtime de Lua en paralelo.
- **Discord:** Indie Cross usaba `discord_rpc`. Acá se mantiene `hxdiscord_rpc` (ya en el template).
- **UI:** Indie Cross no usaba `haxeui-core`/`haxeui-flixel`. Acá se mantienen (los editores tipo ChartEditorState del template dependen de esto).
- **Librerías de assets:** Indie Cross organizaba sus assets en libraries separadas (`songs`, `shared`, `bendy`, `sans`, `cup`, `customSkins`, `achievements`, `hiddenContent`, `notes`). Al portar los assets, se adaptan al layout único (`assets/legacy`-style) que ya usa este template, no se replica esa separación en libraries.
- **`content/NMV-Base-Game` (submódulo):** heredado del template, apunta a `jereidk/base-game-for-android`. Se mantiene por ahora salvo que surja una razón concreta para sacarlo -- es parte de la infraestructura base, no de lo que se porta.

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
2. [x] Contenido va directo a `assets/legacy/` (reemplazando lo de Impostor Legacy), NO como mod en `content/` -- `content/<mod>/` es un sistema de mods *opcionales* (requiere habilitarse en `modsList.txt`/menú de mods), no encaja con que este repo ES el juego.
3. [~] Portar `source/`/`assets/` reemplazando el contenido específico de Impostor Legacy -- en progreso, semana 1 (Snake-Eyes) y tabla de logros ya portados, ver abajo.
4. [ ] Resolver el submódulo `content/NMV-Base-Game`
5. [x] Reemplazar íconos (`projFiles/icon/*`) con los de Indie Cross (hecho, tomados de `assets/compileData/` del repo de referencia)

### Estado del porteo de contenido (assets/legacy/)

- [x] Personajes `bfswag` (BoyFriend_Cuphead) y `cuphead` (Cuphead_Remastered) -- JSON nativo + atlas Sparrow copiados tal cual.
- [x] Escenario `field` -- reconstruido de `PlayState.hx` (imperativo) a `stageObjects` declarativo.
- [x] Canción `snake-eyes` (easy/normal/hard) -- chart copiado casi verbatim, el auto-corrector de charts legacy del motor ya entiende el formato de Indie Cross sin transformación manual.
- [x] `week1.json` -- solo Snake-Eyes por ahora. Faltan Technicolor-Tussle y Knockout (Knockout necesita `angrycuphead`/`rainbf`, otro atlas distinto).
- [x] `awards.json` -- las 17 logros reales de Indie Cross (de `Achievements.hx`), íconos copiados a `images/awards/`. **Solo la tabla de datos está portada** -- los que dependen de completar semana/canción se auto-desbloquean vía `GameFlags.unlockAwardsFromJson()` en cuanto esas semanas/canciones existan con ese `id` exacto en sus JSON; los que dependen de contadores/eventos (bone notes, ink notes, muertes en Despair, dodge-less, FC en Hard, derrotar a los jefes Nightmare) necesitan un `GameFlags.giveAchievement(id)` manual desde hscript, una vez se porten esas mecánicas -- no están wireados todavía.
- [ ] Semanas 2-12 y freeplay siguen con contenido de Impostor Legacy sin portar (roto intencionalmente por ahora, parte de la transición).

---

## 🔗 Links Útiles

- [Repo de este port](https://github.com/jereidk/Indie-Cross-Public-Android)
- [Repo de referencia del mod](https://github.com/jereidk/Indie-Cross-Public)
- [Template base (Impostor Legacy Android)](https://github.com/jereidk/NightmareVision-Android-Support)
