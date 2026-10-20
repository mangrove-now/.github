# Board de proyectos freelance

Todos los tickets de los proyectos freelance de Mangrove viven en un solo board:
**[mangrove-freelance](https://github.com/orgs/mangrove-now/projects/1)**.

Cada ticket es un issue del repo del cliente. El campo **Repository** dice de qué cliente es.

## Campos

| Campo | Valores | Para qué |
|---|---|---|
| **Type** (del issue) | Bug · Feature · Task | Qué clase de trabajo es |
| **Status** | Backlog → Todo → In Progress → In Review → Deployed → Validated | Dónde está |
| **Priority** | Urgent · High · Medium · Low | Qué va primero |
| **Size** | XS (< 1 h) · S (medio día) · M (1–2 días) · L (1 semana) · XL | Cuánto pesa. Un XL se parte |
| **Target** | fecha | Fecha comprometida; alimenta el Roadmap |

### Status
- **Backlog**: ideas y pedidos sin priorizar.
- **Todo**: priorizado, con body completo y Size estimado.
- **In Progress**: alguien lo está haciendo.
- **In Review**: PR abierto, esperando revisión.
- **Deployed**: en producción. Un issue cerrado pasa solo a Deployed. En apps mobile, mergear no es
  estar en producción: el ticket pasa a Deployed cuando se publica la versión.
- **Validated**: el cliente lo vio y lo aprobó. Se mueve a mano.

Lo cerrado se archiva solo a los 7 días, así la columna Deployed muestra lo que salió en la semana.

## Labels

Las labels son de cada repo y marcan el área del producto. Cada cliente tiene las suyas
(por ejemplo, en fl-eliteqa: `web`, `app`, `reportes`, `mails`, `backend`, `infra`).
Para ver las de un repo: `gh label list -R mangrove-now/<repo>`.

## Cómo escribir un ticket

- **Título** en español, con un verbo adelante y sin prefijos: *Agregar…*, *Corregir…*, *Mostrar…*, *Permitir…*.
- **Body** con tres secciones:
  - **Contexto**: qué pasa o qué se necesita, y para quién.
  - **Qué hay que hacer**: checklist (`- [ ]`) con cada cosa a entregar.
  - **Referencias**: capturas, ejemplos, tickets relacionados.
- **Nunca** pegues contraseñas, tokens ni datos de acceso de clientes. El historial de ediciones los guarda.
- Cerralo desde el PR con `Closes #123`.

## Vistas

| Vista | Qué muestra |
|---|---|
| Board | Kanban de lo abierto y lo que salió en la última semana |
| Backlog | Tabla de lo abierto, agrupada por Priority |
| Roadmap | Lo abierto con Target, en el tiempo |
| Bugs | Bugs abiertos |
| Mi trabajo | Lo asignado a quien mira |
| Shipped | Lo deployado y validado: el historial de entregas |

## Sumar un proyecto nuevo

1. Crear el repo en `mangrove-now`. Hereda los templates de issue de este repo.
2. Crear sus labels de área.
3. En el board: **Settings → Workflows → Auto-add to project**, agregar el repo.
