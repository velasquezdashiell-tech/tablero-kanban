# AGENTS.md · Reglas para asistentes de IA

Este archivo lo leen los asistentes de programación (Copilot, Claude Code, Cursor, Codex y otros)
antes de trabajar en este repositorio. Cualquier persona también puede leerlo: resume cómo
está organizado el proyecto y qué reglas sigue.

## Contexto

- Proyecto académico de **Lenguaje de Programación** (5.º semestre, Ingeniería de Sistemas).
- La persona que programa es **dashiell gomez**: está aprendiendo. Tu rol es enseñar y revisar, no
  resolverle la materia.
- Producto Sistema de tablero Kanban y gestión operativa para bodegones que permite organizar tareas y flujos de trabajo con columnas configurables y seguimiento, además de controlar reposición de inventario, vencimientos, precios, fiados/consignaciones y alertas

## Reglas obligatorias

1. **No escribas el código de `src/shared/structures/` ni de `src/shared/algorithms/`** (épicas E3 y E4).
   Esas estructuras y algoritmos los escribe el estudiante a mano. Puedes explicar el concepto,
   dar pistas, revisar su código o señalar errores, pero no entregues la implementación.
2. **SQL escrito a mano, sin ORM.** Toda consulta va en un repositorio (`*.repository.ts`) y es
   parametrizada (`$1`, `$2`). Nunca concatenes texto del usuario dentro de una consulta.
3. **Cambios a la base = migración nueva** en `db/migrations/NNN_descripcion.sql`.
   Nunca edites una migración existente.
4. **Valida toda entrada** a la API con el esquema Zod del `*.model.ts` de la funcionalidad y responde con el código HTTP correcto.
5. **Cada cambio lleva pruebas.** Unitarias en `tests/unit`, de API en `tests/integration`.
6. **Explica lo que propones.** Cada bloque de código que sugieras debe venir con una explicación
   breve de qué hace y por qué, para que el estudiante pueda defenderlo en vivo.
7. **No subas secretos.** Nada de contraseñas ni `.env` en el código.
8. **Screaming architecture.** Una carpeta por funcionalidad del negocio en `src/` (`src/sales`, `src/products`…),
   con su `*.model.ts`, `*.repository.ts`, `*.routes.ts`, `*.api.ts` y `*.view.ts`. Nunca crees carpetas
   por capa técnica (`controllers`, `models`, `services`, `routes`, `utils`…). `src/shared` no importa
   funcionalidades; las funcionalidades no importan `src/app`. Ver `docs/arquitectura.md`.

## Convenciones

- Código, nombres, commits y PR en **inglés**; comentarios y documentación pueden ir en español.
- TypeScript en modo `strict`, sin `any`. Solo sintaxis que Node.js puede ejecutar directamente:
  n                  ada de `enum`, `namespace` ni propiedades en parámetros de constructor.
- Los imports locales llevan la extensión `.ts` (ejemplo: `import { x } from './x.ts'`).
- Commits con Conventional Commits: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`, `chore:`.

## Estructura

```
src/<funcionalidad>/   una carpeta por funcionalidad del negocio (screaming architecture):
                       *.model.ts · *.repository.ts · *.routes.ts · *.api.ts · *.view.ts
src/tasks/             funcionalidad de ejemplo de la plantilla
src/shared/            lo técnico y reutilizable: config, db, logger, http-client, operation-log
src/shared/structures/ estructuras de datos propias (las escribe el dashiell)
src/shared/algorithms/ ordenamiento y búsqueda propios (los escribe el dashiell)
src/app/               arranque: server.ts (API y señales), create-app.ts (rutas), web/ (index.html, main.ts)
db/                    migraciones SQL, semillas y ejecutor de migraciones
tests/                 unit · integration · e2e (architecture.test.ts revisa la estructura)
docs/                  PRD, arquitectura, decisiones (ADR), ingeniería inversa, seguridad
```

## Comandos

| Comando | Qué hace |
| --- | --- |
| `docker compose up -d` | Levanta PostgreSQL |
| `npm run migrate` | Aplica migraciones |
| `npm run dev` | API + frontend en modo desarrollo |
| `npm run check` | Lint + tipos + pruebas unitarias (correr antes de cada commit) |
| `npm run test:integration` | Pruebas de API contra PostgreSQL |
| `npm run test:e2e` | Pruebas en navegador con Playwright |
