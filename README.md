# Skill: `investigacion-tesis`

Skill personalizado para Claude que acompaña trabajo académico de punta a punta: desde tener solo una idea vaga hasta entregar una propuesta o tesis lista para comité. No es un skill oficial de Anthropic — es un paquete de conocimiento y flujos de trabajo empaquetado en formato de Claude Skills.

## 1. Qué es y qué problema resuelve

Sin este skill, Claude puede ayudar con tareas de investigación, pero de forma genérica: puede inventar una referencia por error, afirmar "novedad" sin haber buscado, mezclar hechos con especulación, o sonar más complaciente de lo que le conviene a alguien que necesita pasar un comité de tesis.

Este skill le da a Claude:

- Un **proceso concreto** en 9 etapas para cualquier tarea de investigación.
- **Reglas anti-alucinación** explícitas (nunca inventar fuentes, verificar antes de afirmar novedad).
- **Plantillas** listas para usar (matriz de literatura, PICOC, esqueleto de propuesta).
- **Checklists** de calidad y viabilidad.
- Un modo de **revisor crítico ("Reviewer #2")** para estresar el trabajo antes de entregarlo.

## 2. Cómo lo activa Claude (mecanismo automático)

El archivo `SKILL.md` incluye un `description` en su frontmatter YAML. Claude lee esa descripción al inicio de la conversación (bajo costo, ~100 tokens) y decide si el pedido del usuario coincide. Si coincide, recién ahí carga el contenido completo de `SKILL.md`, y desde ahí abre **solo** el módulo puntual que la tarea requiere — no los 15 archivos de golpe.

Se activa con menciones de: papers/artículos científicos, revisión de literatura, tema o idea de tesis, propuesta de investigación, marco teórico, metodología, o novedad científica — **incluso si el usuario no pide explícitamente "ayuda con mi tesis"**.

No hace falta invocarlo por nombre ni con un comando. Ejemplos que lo disparan solos:

- "Ayúdame a resumir estos 3 papers y encontrar qué patrón se repite"
- "Tengo un tema pero no sé si es viable"
- "¿Cómo convierto este vacío de literatura en una pregunta de investigación?"
- "Revisa mi propuesta como si fueras un revisor duro antes de mandarla a mi comité"

## 3. Estructura de archivos

```
investigacion-tesis/
├── SKILL.md                          # Índice/router — se lee siempre primero
├── modules/                          # El "cómo hacer" de cada etapa
│   ├── temas-e-ideas.md              # Acotar un área amplia a un tema investigable
│   ├── analisis-papers.md            # Leer/resumir/criticar uno o varios papers
│   ├── revision-literatura.md        # Organizar papers en una revisión coherente
│   ├── vacios-investigacion.md       # Detectar research gaps y evaluarlos
│   ├── preguntas-hipotesis.md        # Convertir un vacío en pregunta + hipótesis
│   ├── propuesta-tesis.md            # Armar la propuesta completa y la metodología
│   └── revisor-2.md                  # Modo de revisión crítica adversarial
├── checklists/                       # Verificación puntual
│   ├── viabilidad-tema.md            # Criterio FINER
│   ├── calidad-revision-literatura.md
│   └── antes-de-entregar.md
├── templates/
│   └── plantillas.md                 # Matriz de literatura, PICOC, esqueleto de propuesta
├── knowledge/
│   └── marcos-y-normas.md            # Estilos de cita (APA/MLA/Chicago/Vancouver/IEEE) y diseños de investigación
└── examples/
    └── pipeline-ejemplo.md           # Caso ilustrativo de punta a punta
```

**No es necesario abrir todo.** Cada módulo indica en su primera línea ("Cuándo usar") si aplica a la tarea en curso.

## 4. El pipeline de 9 etapas

| # | Etapa | Pregunta que responde | Módulo |
|---|---|---|---|
| 1 | Pregunta | ¿Qué se quiere saber, exactamente? | `modules/preguntas-hipotesis.md` |
| 2 | Búsqueda | ¿Qué dice la literatura ya publicada? | `modules/analisis-papers.md` |
| 3 | Taxonomía | ¿Cómo se agrupa lo encontrado? | `modules/revision-literatura.md` |
| 4 | Comparación | ¿Dónde coinciden o se contradicen los estudios? | `modules/revision-literatura.md` |
| 5 | Vacíos | ¿Qué falta o está desactualizado? | `modules/vacios-investigacion.md` |
| 6 | Novedad | ¿Ese vacío es una oportunidad viable? | `checklists/viabilidad-tema.md` |
| 7 | Metodología | ¿Cómo se respondería con evidencia propia? | `modules/propuesta-tesis.md` |
| 8 | Revisor #2 | ¿Resistiría una revisión crítica externa? | `modules/revisor-2.md` |
| 9 | Recomendación | ¿Qué hacer a continuación, concretamente? | Cierre de cualquier tarea sustantiva |

No todas las conversaciones recorren las 9 etapas — muchas veces el usuario solo necesita una etapa suelta (ver `examples/pipeline-ejemplo.md` para un caso completo).

## 5. Principios no negociables (siempre activos)

1. **Nunca inventar fuentes** — un DOI, autor o año fabricado es peor que no citar nada.
2. **Nunca afirmar novedad sin haber buscado antes** — "esto no se ha hecho" es una hipótesis hasta confirmarse con búsqueda reciente.
3. **Separar 4 niveles**: hechos verificados, evidencia (de qué estudio y con qué fuerza), hipótesis propias, y especulación.
4. **Reportar incertidumbre explícitamente**, en vez de sonar más seguro de lo que la evidencia permite.
5. **Priorizar fuentes primarias revisadas por pares** sobre blogs o preprints sin revisar (y decir cuando algo es preprint).
6. **Actuar como un director de tesis exigente, no complaciente** — señalar cuando un tema es poco viable o una pregunta es vaga, con la razón concreta.

## 6. Cómo instalarlo y activarlo

**En Claude.ai (web, desktop o móvil):**

1. Asegúrate de tener habilitada la ejecución de código: **Settings → Capabilities → Code execution and file creation** (en cuentas Team/Enterprise, un admin debe habilitarlo en Organization settings → Skills).
2. Ve a **Customize → Skills → "+" → "Create skill"**.
3. Sube el `.zip` — **debe contener la carpeta del skill como única entrada en la raíz** (ver advertencia en la sección 8).
4. El skill aparecerá en tu lista de skills con un interruptor on/off; actívalo.
5. En Team/Enterprise, un Owner puede subirlo una vez en **Organization settings → Skills** para que quede disponible para todo el equipo.

**En Claude Code o vía API:** el mismo `.zip` puede subirse como Skill personalizado (beta) — funciona igual, activándose por coincidencia de descripción.

Una vez activo, no requiere ningún paso adicional: se dispara solo cuando el tema de la conversación coincide con su descripción.

## 7. Ejemplos de uso por etapa

| Lo que quieres hacer | Qué decir |
|---|---|
| Encontrar un tema desde un interés amplio | "Me interesa [campo], quiero hacer mi tesis de [pregrado/maestría/doctorado] sobre eso pero no tengo un tema acotado" |
| Analizar papers propios | "Analiza estos 3 papers adjuntos y dime qué patrón se repite entre ellos" |
| Justificar la novedad | "¿Qué vacío de investigación real hay en [tema], según la literatura de los últimos años?" |
| Convertir un vacío en pregunta | "Tengo este vacío identificado, ayúdame a convertirlo en una pregunta de investigación con PICOC" |
| Armar la propuesta | "Ya tengo tema, pregunta y revisión de literatura — ayúdame a estructurar la propuesta completa" |
| Revisión crítica antes de entregar | "Revisa esta propuesta como si fueras un revisor duro antes de que la presente a mi comité" |

## 8. Advertencia técnica importante: estructura del `.zip`

Al comprimir una carpeta en macOS con el compresor nativo (clic derecho → "Comprimir"), se suele generar automáticamente una carpeta oculta `__MACOSX/` con metadatos (archivos `._nombre`). El resultado es un `.zip` con **dos entradas en la raíz** (`investigacion-tesis/` y `__MACOSX/`).

El uploader de skills de Claude.ai/Claude Code/API exige que el `.zip` contenga **una sola carpeta en la raíz** (la del skill). Si el archivo trae `__MACOSX/`, puede que el sistema lo rechace o se comporte de forma inesperada al subirlo.

**Solución:** comprimir excluyendo esos metadatos, por ejemplo desde terminal:
```bash
cd carpeta-que-contiene-investigacion-tesis
zip -r investigacion-tesis.zip investigacion-tesis -x "*.DS_Store" -x "__MACOSX*"
```
(Te dejo también un `.zip` ya limpio junto con este README, listo para subir directamente.)

## 9. Limitaciones a tener en cuenta

- No reemplaza a tu director/a de tesis ni a un comité real — el módulo "Revisor #2" simula una revisión crítica, pero no conoce los requisitos específicos de tu institución a menos que se los indiques.
- No conoce automáticamente el formato de citas, extensión o normas exigidas por tu universidad — hay que decírselo o Claude preguntará antes de asumir uno por defecto.
- El módulo de detección de vacíos exige búsqueda real antes de afirmar novedad — si Claude no tiene acceso a búsqueda web habilitada en la conversación, ese paso queda incompleto y debería decirlo en vez de asumir.
- Es una guía de comportamiento para Claude, no una base de datos de papers — no contiene literatura precargada de ningún campo específico.
