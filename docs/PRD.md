# Product Requirements Document (PRD) — tablero-kanban

## 1. Problema
Los encargados de bodegones y pequeños comercios suelen llevar el control diario de forma fragmentada o manual, lo que dificulta detectar a tiempo productos próximos a vencer, planificar la reposición de stock, mantener precios actualizados y hacer seguimiento claro a ventas a crédito (fiados) o mercancía en consignación. A su vez, se requiere un tablero Kanban organizado para gestionar el avance de las tareas y entregas.

## 2. Usuarios objetivo
- Dueño o administrador del bodegón: Supervisa inventario, precios, márgenes, alertas críticas y cuentas por cobrar (fiados / consignaciones).
- Encargado de tienda / cajero: Consulta precios, registra operaciones diarias y atiende alertas de reposición o vencimiento.
- Desarrollador / colaborador: Gestiona historias de usuario, hitos y tareas en el tablero Kanban y construye las funcionalidades del sistema.

## 3. Alcance
- Incluido: Configuración del entorno, repositorio, protección de rama main y tablero en GitHub Projects con las historias del backlog; documentación en AGENTS.md y docs/PRD.md; desarrollo progresivo del tablero Kanban y módulos operativos del bodegón.
- Excluido (en esta fase): Facturación fiscal compleja, integraciones de pago externas avanzadas o aplicaciones móviles nativas fuera del alcance inicial.

## 4. Requisitos
- Funcionales: Visualizar y actualizar el estado de tareas en un tablero Kanban con columnas configurables; gestionar inventario, alertas de reposición y fechas de vencimiento; actualizar precios y controlar fiados o consignaciones con sus alertas.
- No funcionales: Interfaz clara y ágil para la operación diaria; código organizado por funcionalidad de negocio en src/ con pruebas y trazabilidad en GitHub.

## 5. Funcionalidades y carpeta en src/
- Tablero Kanban (src/tablero/): Gestión visual de tarjetas, columnas y estados de seguimiento.
- Inventario y reposición (src/inventario/): Control de existencias, umbrales mínimos y alertas de reposición.
- Vencimientos y alertas (src/vencimientos/): Seguimiento de fechas de caducidad y alertas preventivas.
- Precios y catálogo (src/precios/): Consulta y actualización de precios de los productos.
- Fiados y consignaciones (src/fiados/): Registro y seguimiento de créditos/consignaciones y alertas de cobro o liquidación.
