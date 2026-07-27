# Guía: entrega de resultados como HTML interactivo

Esta skill entrega sus resultados sustantivos como artifacts HTML autocontenidos, no como texto plano en el chat. Este archivo explica cuándo, cómo, y con qué componente.

## Cuándo generar HTML en vez de responder en texto

Generar un HTML interactivo cuando la respuesta es un **entregable** que el usuario va a revisar, guardar o volver a consultar: una matriz de literatura, un análisis de vacíos, una checklist FINER o de calidad, una propuesta de tesis estructurada, o una revisión tipo Revisor #2.

Responder en texto plano del chat (sin generar archivo) cuando la interacción es conversacional: una pregunta puntual, una aclaración, una respuesta corta de una o dos frases, o cuando se está construyendo el entregable junto con el usuario paso a paso y todavía no está listo para "cerrarse".

Ante la duda, preguntarse: "¿esto es algo que el usuario querría abrir de nuevo mañana, o es parte de la conversación de ahora mismo?". Lo primero va a HTML; lo segundo se queda en el chat.

## Cómo generarlo (pasos técnicos)

1. Abrir `assets/base-template.html` como punto de partida — copiar el bloque `<style>` completo (el sistema de diseño) y el componente(s) que correspondan de la sección `<body>`, o construir uno nuevo consistente con las variables CSS ahí definidas si ninguno encaja.
2. Reemplazar el contenido de ejemplo por el contenido real de la tarea del usuario. Nunca dejar datos de ejemplo (nombres de autores, hallazgos ficticios) en el entregable final.
3. Guardar como un único archivo `.html` autocontenido (CSS y JS inline, sin dependencias externas) con `create_file`, en `/mnt/user-data/outputs/`.
4. Compartirlo con `present_files`.
5. En el mensaje de chat, un resumen breve de 2-4 líneas de lo más importante — el HTML es el entregable, el chat no debe repetir todo su contenido.

## Qué componente usar según el tipo de resultado

| Resultado | Componente de `assets/base-template.html` |
|---|---|
| Matriz de literatura, matriz de vacíos | Tabla interactiva (ordenable + filtro de texto) |
| Checklist FINER, checklist de calidad, checklist antes de entregar | Checklist con barra de progreso |
| Estructura de propuesta de tesis, revisión de literatura por tema | Acordeón (una sección expandible por bloque temático) |
| Revisor #2 | Acordeón para fortalezas/debilidades/preguntas + badge de veredicto (azul=verificado/listo, verde=viable/completo, óxido=alerta/vacío/no verificado) |
| Progreso dentro del pipeline de 9 etapas | El rastreador (`stepper`) del encabezado, marcando la etapa activa |
| PICOC, formularios de una sola respuesta | Adaptar el patrón de panel simple; no hace falta forzar tabla o acordeón si el contenido es corto |

El encabezado tipo "registro de laboratorio" (`letterhead`) va en todo artifact, sin excepción — es lo que da identidad reconocible a la skill entre un entregable y otro.

## Reglas del sistema de diseño (no improvisar un estilo nuevo cada vez)

- Usar siempre las variables CSS ya definidas en `assets/base-template.html` (`--paper`, `--ink`, `--signal-blue`, `--signal-rust`, `--signal-green`, `--mono`, `--serif`) — no introducir colores o tipografías nuevas sueltas.
- `--signal-blue` = verificado / activo / acción principal. `--signal-rust` = alerta, vacío sin resolver, cita sin verificar, revisión mayor. `--signal-green` = completo, viable, verificado.
- Nunca usar `localStorage` ni `sessionStorage` — estos artifacts no los soportan. Mantener el estado interactivo en variables de JavaScript en memoria, como ya hace la plantilla.
- Mantener cada HTML en un solo archivo autocontenido — no separar CSS o JS en archivos aparte.
- Los marcadores numerados del `stepper` (1–9) son legítimos porque reflejan el pipeline real de la skill — no agregar numeración decorativa en otros lugares donde el contenido no sea realmente una secuencia.
- Si el resultado no encaja en ninguno de los componentes existentes, construir uno nuevo respetando las mismas variables y el mismo lenguaje visual, en vez de forzarlo en un componente que no corresponde.
