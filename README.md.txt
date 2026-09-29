# Ecosistema de Automatización IA Autónomo para Negocios

Bot de consulta de stock y generación de presupuestos por Telegram, con aprobación humana (HITL) y descuento automático de inventario.

**Autor:** Eluney Salvaro — eluneyjsalvaro@gmail.com

## Qué hace

Un cliente escribe por Telegram para consultar stock/precios o pedir un presupuesto de compra. Un agente de IA (**Claude Haiku 4.5**, orquestado en **n8n**) interpreta el mensaje, consulta el catálogo real en **Airtable** (nunca inventa precios, stock ni IDs) y responde en lenguaje natural.

Cuando el pedido implica un presupuesto, el flujo (no el modelo) crea el registro en Airtable y dispara un punto de **Human-in-the-Loop (HITL)**: un correo por Gmail con botones Aceptar/Rechazar que se envía al responsable del negocio antes de tocar el stock. Solo si el humano aprueba, el flujo descuenta las cantidades del inventario real y confirma la compra al cliente; si rechaza, el presupuesto se marca como rechazado sin modificar nada. Cada escritura crítica en Airtable tiene manejo de errores con reintento, registro en una tabla de log y aviso al cliente y al equipo interno (Slack).

## Stack técnico

| Categoría | Tecnología |
| --- | --- |
| Agente de IA / LLM | Claude Haiku 4.5 (Anthropic), salida estructurada por JSON Schema |
| Orquestación / automatización | n8n (51 nodos) |
| Base de datos | Airtable — base "Gestión de Stock y Presupuestos" (6 tablas) |
| Canal conversacional | Telegram |
| Intervención humana (HITL) | Gmail — "Send and Wait for Response" |
| Observabilidad interna | Slack (#equipo-urgencias) |
| Memoria de conversación | PostgreSQL (20 mensajes por chat) |

## Enlaces

- **Dashboard (Airtable, solo lectura)** — KPIs de presupuestos y tasa de error: [ver dashboard](https://airtable.com/app798P8GZdbmtQla/pag5UScPS194f2AjH)
- **Documento completo** (diagramas, estructuras de datos, esquemas JSON, matriz de costos, seguridad/resiliencia): [`docs/Entrega_Final_Ecosistema_IA.pdf`](docs/Entrega_Final_Ecosistema_IA.pdf)
- **Flujo de n8n** (export): [`flow/workflow.json`](flow/workflow.json)

## Contenido del documento (`docs/`)

1. Resumen ejecutivo y categorías técnicas cubiertas
2. Diagramas de arquitectura (visión general + rama de presupuesto/HITL/stock)
3. Estructuras de datos — las 6 tablas de Airtable (Clientes, Productos, Presupuestos, Detalle_Presupuesto, Aprobaciones_HITL, Log_Errores)
4. Esquemas JSON de transferencia entre los puntos del flujo
5. Matriz de optimización de costos y justificación del modelo de IA usado
6. Seguridad y resiliencia: minimización de datos, punto HITL, manejo de errores
7. Anexo: guía para el dashboard, las capturas y este mismo repositorio

