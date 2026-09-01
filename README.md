# Ballroom App CL

Organizador de competencias de baile deportivo. Aplicación web estática (HTML + JS vanilla) con backend en Supabase.

## Estructura

| Archivo | Rol |
|---|---|
| `index.html` | Punto de entrada y layout de la app |
| `app.js` | Toda la lógica: roles, PINs, rondas, escrutinio, publicación |
| `styles.css` | Estilos |
| `xlsx-js-style.min.js` | Exportación a Excel con formato |
| `rc-logo.png` | Logo |
| `netlify.toml` | Headers de deploy (no-cache para `published-data.json`) |

## Despliegue

Netlify, conectado a este repositorio. Cada push a `main` dispara un deploy.
No hay build step: el sitio se publica tal cual desde la raíz.

## Backend

Supabase (proyecto `qbgctxfatrdmxakacnpp`, organización BallroomAppCL) — independiente
del resto de los proyectos. Tablas: `ediciones`, `inscripciones`, `judge_votes`,
`judge_votes_multi`, `kv_store`. La `SUPABASE_ANON_KEY` en `app.js` es pública por diseño.

**Modelo de acceso:** la app no usa Supabase Auth; todo entra con la anon key y las
policies RLS conceden acceso total a `anon`. Cualquiera con la URL del sitio puede leer
y escribir las tablas, incluidas las inscripciones con datos personales. Endurecerlo
requiere mover las escrituras a edge functions con `service_role` o adoptar Supabase Auth.

Pendiente: la edge function `send-confirmation` (mail de confirmación de inscripción)
todavía no está desplegada en este proyecto.
Los PINs definidos en `app.js` son valores por defecto del cliente y se sobrescriben
en runtime con los publicados — no constituyen una barrera de seguridad.
