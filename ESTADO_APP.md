# RZ Pro Examen — Estado al 2026-11-16

## Qué es
App web + APK Android para aplicar los exámenes de conocimiento del diplomado (Etapa 1 · Refrigeración, Etapa 2 · Aire Acondicionado) a varios grupos.

- **Web (alumnos):** `https://orioszz201.github.io/examen-irca/`
- **Panel (instructor):** `https://orioszz201.github.io/examen-irca/#panel`
- **APK:** `github.com/orioszz201/examen-irca/releases` → `RZProExamen-v2.7-debug.apk`
- **Código:** `/home/oscar/workspace/Utiles/examen_app/` (repo git `orioszz201/examen-irca`)

## Stack
HTML/CSS/JS vanilla (sin framework) · Supabase (Postgres + RPC) · GitHub Pages (hosting) · Capacitor 8 (APK) · `@capacitor/share` (compartir nativo).

## Supabase
- Project URL: `https://ahcblxfjshrvukhfrteq.supabase.co`
- Publishable key: `sb_publishable_mNCgIg37tfX7hBbudsWdKw_xQ87xkGG`
- Tablas: `claves` (clave de respuestas, **sin lectura pública**), `resultados`, `grupos`
- Funciones: `calificar(examen, respuestas)` (solo calcula, no guarda) · `guardar_resultado(nombre, examen, respuestas, abierta, grupo)` (recalcula y guarda)

## Flujo del alumno (link)
1. Elige **grupo** + escribe **nombre** → "Empezar examen".
2. Contesta → **"Calificar"** → ve la nota (no se guarda todavía).
3. Satisfecho → **"✅ Guardar nota"**. No → **"🔄 Reintentar"** (máx 3 intentos).
4. Preguntas y opciones se **barajan** en cada intento.

## Flujo del instructor (app / #panel)
- "📤 Compartir link" (hoja nativa Android) · "👁️ Ver examen" · "➕ Agregar grupo" · "➖ Quitar grupo" · filtro por grupo · "Exportar JSON" · "Imprimir/PDF" · "🗑️ Vaciar resultados" (con confirmación).

## Seguridad
- Clave de respuestas **solo en el servidor** (no se ve inspeccionando el código).
- Bloqueo por **nombre + etapa + grupo** (servidor).
- **Candado por dispositivo** (localStorage `rz_done_<etapa>_<grupo>`): el mismo celular no repite aunque cambie el nombre.
- ⚠️ Límite honesto: el candado es de dispositivo, no de identidad. Bypass real = borrar datos del navegador / otro equipo. Blindaje 100% = link único por alumno (contradice "un mismo link para el grupo").

## Reglas de publicación (de cada cambio)
1. Editar `index.html` → `cp index.html www/index.html` → `npx cap sync android`.
2. `git add/commit/push` (token: `/home/oscar/.config/opencode/.gh-token`, usar `GH_TOKEN`+`GITHUB_TOKEN`).
3. `./gradlew assembleDebug` en `android/` → copiar a `RZProExamen-v<n>-debug.apk`.
4. `gh release create v<n> RZProExamen-v<n>-debug.apk` (Release = único canal de prueba).

## Pendientes
- [ ] **Ícono propio** de la app (hoy usa el default de Capacitor).
- [ ] **Confirmar clave de respuestas Etapa 2** (la de Etapa 1 ya está en `claves`).
- [ ] **Supabase gratis pausa el proyecto tras ~1 semana de inactividad** → reanudar desde el dashboard (los datos se conservan).
- [ ] Opcional: link único por alumno (token) para blindaje total.
- [ ] Opcional: nota de corte (70%) configurable; hoy es fijo en el front.

## Exámenes (fuera de la app)
- V1 (original, 29 preg.) y V2 (22→15) en `/home/oscar/Escritorio/Irca/`
- `Examen_Etapa_1_Refrigeracion_V2.pdf/.md` (15 preg) · `Examen_Etapa_2_Aire_Acondicionado.pdf/.md` (15 preg)
- `Intencion_clase_S13-S24.md/.pdf` (intención por sesión, solo para nosotros)
- Generador: `/home/oscar/.tmp/opencode/gen_examenes_v2.py` (Etapa 1 V2) y `gen_examenes.py` (V1 + Etapa 2)
- SQL de Supabase en `sql_para_pegar.txt` (solo texto SQL, sin ``` para pegar directo)
