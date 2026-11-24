# tablero-kanban

> **dashiell:** tablero-kanban organizador.
> Proyecto n.º __ del catálogo · Sitio de referencia: __ · [PRD](docs/PRD.md) · URL publicada: __

Plantilla del proyecto de **Lenguaje de Programación · 2026**.
La guía completa de la actividad está en el repositorio del curso:
**https://github.com/Jeisonpernia/lenguaje-programacion**

---

## Arranque en 11 pasos

Antes de empezar necesitas Git, Node.js 24, Docker Desktop, VS Code y GitHub CLI instalados.
Si te falta alguno, sigue el tutorial `tutoriales/01-instalacion-entorno.md` del repositorio del curso.

1. **Crea tu repositorio desde esta plantilla.** En GitHub, botón verde **Use this template** →
   **Create a new repository**. Nómbralo con el nombre de tu producto, en minúsculas y con guiones
   (por ejemplo `bodega-pos`). Visibilidad: **Public**.
2. **Clónalo en tu computador:**
   ```bash
   gh repo clone TU_USUARIO/bodega-pos
   cd bodega-pos
   ```
3. **tablero-kanban** en dos lugares (así tus contenedores no se mezclan con los
   de un compañero): la línea `name: mi-producto` de `docker-compose.yml` y el campo `"name"` de
   `package.json`. Usa minúsculas y guiones, por ejemplo `bodega-pos`.
4. **Instala las dependencias:**
   ```bash
   npm install
   ```
5. **Crea tu archivo de variables de entorno:**
   ```bash
   cp .env.example .env
   ```
   En Windows (PowerShell): `Copy-Item .env.example .env`
6. **Levanta PostgreSQL con Docker** (Docker Desktop debe estar abierto):
   ```bash
   docker compose up -d --wait
   ```
   `--wait` espera a que la base esté lista antes de devolverte la terminal.
7. **Crea las tablas y carga datos de prueba:**
   ```bash
   npm run migrate
   npm run seed
   ```
8. **Arranca el proyecto:**
   ```bash
   npm run dev
   ```
   Abre **http://localhost:5173**. Deberías ver la lista de tareas de ejemplo.
9. **Comprueba que todo está sano:**
   ```bash
   npm run check
   npm run test:integration
   ```
10. **Carga tu backlog en GitHub** (crea los 45 Issues, los sprints y tu GitHub Project):
   ```bash
   gh auth refresh -s project
   npm run backlog
   ```
11. **Protege tu rama `main`** e invita al profesor y a tu compañero revisor
    (tutorial `tutoriales/02-git-y-github.md`, sección 9).

Si algo falla, revisa la sección [Problemas frecuentes](#problemas-frecuentes).

---

## Comandos

| Comando | Qué hace | Cuándo |
| --- | --- | --- |
| `npm run dev` | API (puerto 3000) y frontend (puerto 5173) con recarga automática | Mientras programas |
| `npm run check` | Lint + revisión de tipos + pruebas unitarias | **Antes de cada commit** |
| `npm test` | Solo pruebas unitarias | Mientras escribes estructuras y algoritmos |
| `npm run test:watch` | Pruebas unitarias que se repiten al guardar | Mientras escribes estructuras y algoritmos |
| `npm run test:integration` | Pruebas de la API contra PostgreSQL (base `app_test`) | Al tocar la API o el SQL |
| `npm run test:e2e` | Pruebas en navegador real con Playwright | Antes de cerrar un sprint |
| `npm run migrate` | Aplica las migraciones pendientes | Después de crear una migración o de hacer `git pull` |
| `npm run db:reset` | **Borra** la base y aplica todas las migraciones | Cuando tu base local quedó desordenada |
| `npm run seed` | Carga 10.000 registros de prueba | Después de `db:reset` |
| `npm run bench` | Benchmark de ordenamiento (H4.3) | Sprint 4 |
| `npm run build` | Construye el frontend para producción en `dist/` | Lo hace la CI y el despliegue |
| `npm start` | Arranca en modo producción | Para probar el build |
| `npm run backlog` | Carga el backlog en Issues y GitHub Project | Una vez, en el Sprint 0 |

La primera vez que corras `npm run test:e2e` instala el navegador: `npx playwright install chromium`.

---

estrutura de carpeta en src, herramientas de pruebas configuradas y uso de docker

```
.github/
  workflows/ci.yml          CI: lint, tipos, pruebas unitarias, integración, E2E y build
  ISSUE_TEMPLATE/           formularios para historias y errores
  pull_request_template.md  checklist y bitácora de IA de cada PR
clon/                       Sprint 1: tu recreación en HTML, CSS y JS puro
db/
  migrations/               cambios a la base, en SQL, numerados (001, 002…)
  init/                     crea la base de pruebas la primera vez que arranca Docker
  migrate.ts                ejecutor de migraciones
  seed.ts                   datos de prueba
docs/                       PRD, ingeniería inversa, arquitectura, seguridad, reto, decisiones (adr/)
scripts/                    backlog del curso y el script que lo carga en GitHub
src/                        SCREAMING ARCHITECTURE: una carpeta por funcionalidad del negocio
  tasks/                    funcionalidad de ejemplo: modelo, SQL, rutas, cliente y pantalla
  shared/                   lo técnico y reutilizable (config, db, logger, http-client, log)
    structures/             TUS estructuras de datos (E3). Trae una pila resuelta de ejemplo
    algorithms/             TUS algoritmos (E4). Trae burbuja resuelto de ejemplo
  app/                      arranque: server.ts, create-app.ts y web/ (index.html, main.ts)
tests/
  unit/                     pruebas rápidas sin base de datos
  integration/              API + PostgreSQL real
  e2e/                      navegador real con Playwright
AGENTS.md                   reglas para asistentes de IA (léelo)
docker-compose.yml          PostgreSQL (y opcionalmente Adminer y la app completa)
Dockerfile                  imagen de producción de la aplicación
```

### Screaming architecture

La estructura de `src/` debe **gritar de qué trata tu producto**: una carpeta por funcionalidad del
negocio (`src/sales`, `src/products`, `src/appointments`…) con todo lo suyo adentro. Nunca carpetas por
capa técnica (`controllers`, `models`, `services`). Las reglas, un ejemplo completo y cómo agregar una
funcionalidad están en [docs/arquitectura.md](docs/arquitectura.md). `tests/unit/architecture.test.ts`
las comprueba en cada `npm test` y en la CI.

### Lo que ya funciona y lo que te toca

| Ya viene hecho (es el ejemplo) | Te toca a ti |
| --- | --- |
| Funcionalidad de ejemplo `src/tasks` con CRUD, validación y pruebas | Crear las carpetas de las funcionalidades de tu producto (H2.1, H10.1, H10.2, H5.1) |
| Pila (`Stack`) y burbuja (`bubbleSort`) resueltos | Lista enlazada, cola, tabla hash, mergesort, quicksort, búsqueda binaria (E3, E4) |
| Cierre ordenado con SIGINT y SIGTERM | Guardar lo pendiente y cerrar tu log antes de salir (H6.1) |
| Esqueleto del log de operaciones | Implementarlo con descriptores de archivo (H5.2) |
| Migraciones, semillas, Docker, CI | Tus migraciones, tus semillas, mantener la CI en verde |

Las funciones pendientes lanzan el error `Pendiente: H?.?` y sus pruebas están marcadas con `it.todo`.
Al implementarlas, cambia `it.todo` por pruebas reales.

---

## Cómo trabajar cada historia

1. En tu GitHub Project, mueve el Issue a **En curso**.
2. Crea una rama: `git switch -c feat/h3-2-pila-deshacer`
3. Programa, con commits pequeños: `git commit -m "feat: add undo stack to editor"`
4. Corre `npm run check` antes de cada push.
5. Sube la rama y abre el PR: `git push -u origin HEAD` y luego `gh pr create --web`
6. En la descripción del PR escribe `Closes #N` y completa la bitácora de IA.
7. Espera la CI en verde y la aprobación de tu compañero. Fusiona con **Merge pull request**.

El paso a paso detallado está en `tutoriales/05-flujo-de-trabajo-diario.md` del repositorio del curso.

---

## Problemas frecuentes

| Síntoma | Causa probable | Solución |
| --- | --- | --- |
| `Falta la variable de entorno DATABASE_URL` | No existe `.env` | `cp .env.example .env` |
| `ECONNREFUSED 127.0.0.1:5433` | PostgreSQL no está corriendo o todavía arranca | Abre Docker Desktop y ejecuta `docker compose up -d --wait` |
| `Cannot connect to the Docker daemon` | Docker Desktop está cerrado | Ábrelo y espera a que diga *Engine running* |
| `port is already allocated` | Otro programa usa el puerto 5433 | Cambia `5433` por `5434` en `docker-compose.yml` y en `.env` |
| `database "app_test" does not exist` | El volumen se creó antes de existir `db/init` | `docker compose down -v` y luego `docker compose up -d` (borra los datos locales) |
| `node: bad option: --env-file-if-exists` o errores con `.ts` | Node.js viejo | Instala Node 24: `node --version` debe decir v24 o superior |
| `Executable doesn't exist` en Playwright | Falta el navegador | `npx playwright install chromium` |
| La CI falla pero en tu máquina pasa | No corriste `npm run check` o falta un archivo en el commit | Lee el log en la pestaña **Actions** del repositorio |
