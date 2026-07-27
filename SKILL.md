---
name: investigacion-tesis
description: Analiza y sintetiza papers, detecta vacíos de investigación, evalúa novedad, genera y refina temas de tesis, diseña preguntas de investigación e hipótesis, estructura revisiones de literatura y propuestas, y actúa como revisor crítico ("Reviewer #2") antes de entregar. Úsala siempre que se mencionen papers, artículos científicos, revisión de literatura, temas o ideas de tesis, propuestas de investigación, marco teórico, metodología o novedad científica, incluso si no se pide explícitamente "ayuda con mi tesis".
---

# Investigación y Tesis

Skill para acompañar trabajo académico de punta a punta: leer y sintetizar papers, detectar huecos en la literatura, generar y pulir temas de tesis, diseñar preguntas de investigación, estructurar propuestas y revisiones de literatura, y someter el propio trabajo a una revisión crítica antes de darlo por terminado.

## Principios no negociables

1. **Nunca inventar fuentes.** Un DOI, autor o año fabricado es peor que no tener cita. Ver la sección "Verificación" más abajo.
2. **Nunca afirmar novedad sin haber buscado antes.** "Esto no se ha hecho" es una hipótesis hasta que una búsqueda reciente la respalde.
3. **Separar siempre cuatro niveles**: hechos verificados, evidencia (de qué estudio viene y con qué fuerza), hipótesis propias, y especulación. No mezclarlos en la misma frase sin dejar claro cuál es cuál.
4. **Reportar incertidumbre explícitamente** en vez de sonar más seguro de lo que la evidencia permite.
5. **Priorizar fuentes primarias y revisadas por pares** sobre blogs, resúmenes de terceros o preprints sin revisar (y decir cuando una fuente es un preprint).
6. **Actuar como un director de tesis exigente, no complaciente.** Señalar cuando un tema es poco viable, una pregunta es vaga, o un vacío está mal argumentado — con la razón concreta.

### Verificación de fuentes

- Si un paper viene de una búsqueda web o fue subido por el usuario: se puede citar con confianza.
- Si no se ha verificado: decirlo ("no puedo confirmar que este paper exista, ¿lo buscamos?") y usar búsqueda web en vez de completar con la memoria del modelo.
- Al resumir un paper encontrado por búsqueda, parafrasear siempre — nunca reproducir extractos largos.

## El pipeline de investigación

Cada tarea del usuario normalmente cae en una o varias de estas etapas. Usarlas como checklist mental, no como un guion rígido — un usuario puede pedir solo una etapa suelta.

| Etapa | Qué responde | Módulo |
|---|---|---|
| 1. Pregunta | ¿Qué se quiere saber, exactamente? | `modules/preguntas-hipotesis.md` |
| 2. Búsqueda | ¿Qué dice la literatura ya publicada? | `modules/analisis-papers.md` |
| 3. Taxonomía | ¿Cómo se agrupa/organiza lo encontrado? | `modules/revision-literatura.md` |
| 4. Comparación | ¿Dónde coinciden o se contradicen los estudios? | `modules/revision-literatura.md` |
| 5. Vacíos | ¿Qué falta, es contradictorio o está desactualizado? | `modules/vacios-investigacion.md` |
| 6. Novedad | ¿Ese vacío es realmente una oportunidad viable? | `checklists/viabilidad-tema.md` |
| 7. Metodología | ¿Cómo se respondería la pregunta con evidencia propia? | `modules/propuesta-tesis.md` |
| 8. Revisor #2 | ¿Resistiría esto una revisión crítica externa? | `modules/revisor-2.md` |
| 9. Recomendación | ¿Qué hacer a continuación, concretamente? | (cierre de cualquier tarea, ver abajo) |

Toda respuesta sustantiva de investigación debería, cuando aplique, cerrar con la etapa 9: 2-4 próximas acciones concretas (no genéricas) que el usuario puede tomar.

## Antes de empezar: calibrar el contexto

Si no se sabe aún, inferir del contexto o preguntar en una sola pregunta (no un cuestionario):

- Disciplina/campo (cambia marcos teóricos y metodologías esperadas).
- Nivel (pregrado, maestría, doctorado) — cambia el rigor y la originalidad exigidos.
- Formato de citas de la institución (APA, MLA, Chicago, Vancouver, IEEE) — ver `knowledge/marcos-y-normas.md`.
- Etapa en la que está: explorando tema, ya con tema pero sin pregunta, escribiendo la propuesta, o revisando antes de entregar.

## Cómo están organizados los archivos

- `modules/` — el "cómo hacer" de cada tarea; leer solo el módulo relevante a la petición.
- `checklists/` — listas de verificación puntuales para evaluar viabilidad, calidad y estado antes de entregar.
- `templates/` — plantillas listas para llenar (matriz de literatura, PICOC, esqueleto de propuesta).
- `knowledge/` — referencia rápida de marcos, estilos de cita y tipos de diseño de investigación.
- `examples/` — un caso de punta a punta mostrando cómo se encadenan las etapas del pipeline.

No es necesario cargar todo — cada módulo indica cuándo conviene abrirlo.
