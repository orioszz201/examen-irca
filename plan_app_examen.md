# App Examen de Conocimientos — Plan

## Objetivo
App web (y luego APK Android) para aplicar los exámenes de conocimiento a un grupo por WhatsApp:
- **Un solo link público** para todo el grupo.
- El alumno pone su **nombre**, contesta y al **guardar** ve su **nota al instante**.
- Los resultados (nombre + respuestas + nota) llegan al **panel del instructor**.
- El instructor ve el reporte en **PC (web)** y **Android (APK)**.

## Stack
| Capa | Tecnología |
|---|---|
| Frontend (examen + panel) | HTML + CSS + JS vanilla |
| Backend (resultados + clave) | Supabase (gratis) |
| Hosting del link | Vercel / GitHub Pages (gratis) |
| Android | Capacitor → APK |
| Link | Uno solo, público, para el grupo |

## Arquitectura
1. Alumno abre el link → pone nombre → contesta → **guardar**.
2. La calificación se hace en el **servidor** (la clave no viaja al navegador) → devuelve la nota al instante.
3. El resultado se guarda en **Supabase** (nombre, respuestas, nota, fecha).
4. El panel lee Supabase y muestra la tabla (se actualiza en tiempo real), en PC y Android.

## Seguridad
- **Clave de respuestas en el backend**: inspeccionar el código del link no revela las respuestas.
- **Un solo link** (sin token por alumno): cualquiera con el link puede contestar. Control por **nombre** (evitar repetidos si se desea).

## Datos del examen
- **Etapa 1 (Refrigeración):** 23 preguntas de opción múltiple.
- **Etapa 2 (Aire Acondicionado):** 15 preguntas (14 opción múltiple + 1 abierta de instalación).
- La pregunta abierta **no se auto-califica** (se registra y se revisa aparte).

## Fases
### Fase 1 — Web
1. ✅ Estructura y app local (califica solo, sin servidor) — `index.html`.
2. Clave de respuestas validada con Oscar.
3. Conectar Supabase (submit + panel en tiempo real).
4. Hosting público (Vercel/GitHub Pages) → link al grupo.

### Fase 2 — Android (Capacitor)
1. Envolver la web app con Capacitor.
2. Generar APK (flujo GitHub Releases).

### Fase 3 (opcional)
- Exportar/importar JSON y PDF del reporte.

## Pendientes
- [ ] Validar la clave de respuestas (borrador con criterio técnico).
- [ ] Crear proyecto Supabase (cuenta gratis + pegar URL/anon key).
- [ ] Elegir hosting (Vercel o GitHub Pages).

## Clave de respuestas (BORRADOR — validar con Oscar)
Ver `index.html` (const `EXAMENES`, campo `correcta`). Marcado **[por confirmar]**: Etapa 1 · "color de lata R-134a" (el estándar real es azul claro, no está en las opciones).
