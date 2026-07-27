# Dominio especializado: SOFA, soft robotics y simulación rígido-blando

Cargar este archivo cuando la conversación mencione SOFA (Simulation Open Framework Architecture), soft robotics, robots blandos/deformables, el brazo Niryo Ned2, o simulación de interacción entre un manipulador rígido y objetos/robots blandos o deformables.

Todo lo aquí escrito viene de documentación oficial y papers encontrados por búsqueda al momento de crear este archivo — el ecosistema de software (versiones, plugins, compatibilidad) cambia con frecuencia. Antes de dar por buena una versión, comando o compatibilidad específica, volver a buscar para confirmar que sigue vigente.

## 1. El framework SOFA

- **Qué es**: biblioteca open-source en C++ (licencia LGPL) para simulación física multi-modelo, orientada originalmente a simulación médica/quirúrgica y extendida ampliamente a soft robotics.
- **Arquitectura**: un grafo de escena donde los componentes (solvers, modelos mecánicos, mapeos, detección de colisiones) se combinan de forma modular — se puede sustituir un solver o modelo sin reescribir el resto de la simulación.
- **Modelos de deformación disponibles**: modelos masa-resorte, y varios modelos de elementos finitos (FEM) — lineal, corrotacional lineal, y Neo-Hookeano — este último más adecuado para grandes deformaciones hiperelásticas.
- **Plugins clave para soft robotics**:
  - `SoftRobots` (y `SoftRobots.Inverse`): modelado mecánico de actuadores blandos con solvers FEM directos/inversos en tiempo real; permite tanto simular el comportamiento del robot como controlarlo.
  - `STLIB`: plantillas y abstracciones de alto nivel para construir escenas más rápido.
  - `SofaPython3` (sucesor de `SofaPython`/SP2): bindings modernos de Python para escribir y controlar escenas; es el que se recomienda para proyectos nuevos.
  - `SPLIB`: utilidades de Python para usar junto con SofaPython3 (históricamente parte de STLIB).
  - `SofaGym`: entorno tipo Gym para entrenar políticas de aprendizaje por refuerzo directamente sobre escenas SOFA.
- **Quién lo mantiene**: el consorcio SOFA y, específicamente para soft robotics, el equipo **DEFROST** (INRIA Lille, liderado por Christian Duriez) — su código está en el GitHub org `SofaDefrost`.
- **Documentación oficial**: `sofa-framework.org`, `project.inria.fr/softrobot`, `sofapython3.readthedocs.io`. Tratar estas como fuentes primarias citables como documentación técnica (no como papers revisados por pares).

## 2. Fundamentos de soft robotics relevantes

- **Tipos de actuación comunes**: neumática (cámaras infladas), por cable/tendón, y magnética (campos externos que deforman materiales blandos con partículas magnéticas embebidas) — cada una tiene su propia forma de modelarse en SOFA (presión, tensión de cable, o fuerza magnética aplicada).
- **Enfoques de modelado de objetos deformables** (tradeoff precisión vs. costo computacional):
  - **Mass-Spring System (MSS)**: rápido, pero menos preciso físicamente.
  - **Position-Based Dynamics (PBD)**: estable y eficiente, popular en gráficos, pero no siempre físicamente exacto.
  - **FEM**: el más preciso físicamente y el que mejor maneja grandes deformaciones, pero computacionalmente más costoso — es el enfoque dominante en SOFA para trabajo serio de soft robotics.
- **Validación**: la práctica estándar en los papers revisados es comparar la simulación contra el comportamiento real del actuador/robot (sim-to-real), no quedarse solo con la simulación.

## 3. Niryo Ned2 — especificaciones y ecosistema

| Aspecto | Dato |
|---|---|
| Grados de libertad | 6 ejes |
| Carga útil (payload) | ~300 g |
| Alcance | ~440–490 mm según la fuente/generación de datasheet |
| Precisión y repetibilidad | 0.5 mm |
| Cómputo a bordo | Raspberry Pi 4 (ARM v8 64-bit), 4 GB RAM |
| Sistema operativo / ROS | Documentado con Ubuntu 18.04 + ROS Melodic en manuales más antiguos, y Ubuntu 20.04 + ROS Noetic en documentación más reciente; Niryo también menciona compatibilidad con ROS2 en material de producto — **confirmar la versión exacta contra el firmware real del robot del usuario**, porque cambia el API y los paquetes de simulación disponibles. |
| Control / API | PyNiryo (API en Python), NiryoStudio (Blockly y Python/C++), integraciones con MATLAB (ROS Toolbox) y RoboDK |
| Simulación nativa | RViz (visualización de geometría/cinemática, sin física, bajo costo de CPU) y Gazebo (con física) vía `niryo_robot_bringup`; también RoboDK y Niryo Studio 2 con visualización 3D en Unity |
| Descripción del robot | El robot cuenta con modelo URDF (formato estándar de ROS), reutilizable en otras herramientas de simulación/visualización |

## 4. Acoplamiento rígido-blando: métodos y tradeoffs

Relevante para simular la interacción del brazo (rígido) con un actuador o objeto blando/deformable:

| Método | Idea central | Tradeoff |
|---|---|---|
| Acoplamiento por restricciones/compliant (Lagrange multipliers) | Trata el contacto/acoplamiento como restricciones resueltas junto con la dinámica | Encaja naturalmente con simuladores de cuerpo rígido; usado en SOFA vía formulaciones de Duriez y extensiones posteriores con rigidez geométrica |
| FEM corrotacional en tiempo real | Corrige la rigidez lineal con la rotación local de cada elemento | Buen balance precisión/velocidad para control en tiempo real de actuadores blandos (enfoque característico de SOFA/SoftRobots) |
| Métodos de Newton no suaves | Resuelven contacto y elasticidad en una sola fase, en vez de por etapas | Mejor estabilidad en sistemas rígido-deformable acoplados, mayor complejidad de implementación |
| Incremental Potential Contact (IPC) | Garantiza simulación libre de intersecciones con contacto friccional | Alta robustez geométrica; computacionalmente más pesado, usado en trabajos recientes de manipulación robot-objeto deformable |
| Material Point Method (MPM) | Discretiza el objeto en partículas sobre una malla de fondo | Maneja bien grandes deformaciones y cambios topológicos; el manejo de fricción y acoplamiento rígido-blando de dos vías todavía es un reto activo en la literatura |
| Reducción de orden del modelo (model order reduction) | Reduce la dimensión del modelo FEM manteniendo precisión aceptable | Clave para lograr control/simulación en tiempo real de modelos FEM complejos (línea de trabajo de Goury y Duriez) |

Ninguno de estos métodos es "el correcto" en abstracto — la elección depende de si la tesis prioriza precisión física, velocidad en tiempo real, o robustez geométrica frente a intersecciones.

## 5. El puente entre ROS/Gazebo y SOFA (posible desafío metodológico)

Al momento de esta búsqueda no se encontró un bridge oficial y ampliamente documentado que conecte específicamente el stack ROS de Niryo (Gazebo/RViz) con una escena SOFA. Los caminos que sí tienen precedente en la literatura/documentación general de robótica:

1. **Importar la geometría y cinemática del brazo vía su URDF** dentro de SOFA, modelándolo como un mecanismo articulado rígido controlado por los mismos ángulos de articulación que reporta ROS.
2. **Sincronización por estados**: correr el control del brazo en ROS/Gazebo y reproducir/enviar las trayectorias o posiciones articulares resultantes hacia la escena SOFA (o viceversa), en vez de una co-simulación físicamente acoplada en tiempo real.
3. **Co-simulación con paso de mensajes** entre el proceso de ROS y el proceso de SOFA (p. ej. vía sockets o un nodo puente), si el objetivo es un acoplamiento bidireccional en tiempo real.

Esto no es un detalle menor de implementación — decidir y justificar cuál de estos caminos se toma (y por qué) puede ser en sí mismo parte de la contribución metodológica de la tesis, especialmente si no hay una solución estándar ya publicada para esta combinación específica de herramientas.

## 6. Cómo buscar en este dominio (papers, temas inexplorados, documentación)

- **Bases de datos**: IEEE Xplore, ACM Digital Library, arXiv (categoría cs.RO), Semantic Scholar y Google Scholar (para rastrear quién cita a quién), Scopus/Web of Science si la institución da acceso.
- **Venues donde suele publicarse este tema**: IEEE RoboSoft (International Conference on Soft Robotics), ICRA, IROS, IEEE Robotics and Automation Letters (RA-L), IEEE Transactions on Robotics (T-RO), la revista *Soft Robotics* (Mary Ann Liebert), *Frontiers in Robotics and AI*, *Current Robotics Reports* (Springer, buena fuente de artículos de revisión/estado del arte).
- **Grupo a seguir de cerca**: el equipo DEFROST (INRIA Lille) y las publicaciones de Christian Duriez y colaboradores — son quienes desarrollan SOFA/SoftRobots y suelen publicar los métodos de referencia del campo.
- **Combinaciones de palabras clave sugeridas** (ajustar y combinar, no usar una sola búsqueda genérica):
  - "SOFA framework" + "soft robot" + "simulation"
  - "rigid-soft coupling" + "manipulation" / "grasping"
  - "FEM" + "soft robot" + "real-time control"
  - "deformable object manipulation" + "robot arm" + "simulation"
  - "digital twin" + "collaborative robot" + "soft robotics" (para el ángulo de brazo rígido educativo/colaborativo)
  - "Niryo" + "simulation" (para rastrear específicamente qué se ha hecho con este brazo en particular, más allá de SOFA)
- **Documentación técnica citable como tal (no como paper)**: `sofa-framework.org`, `project.inria.fr/softrobot`, `sofapython3.readthedocs.io`, `docs.niryo.com`, y los repositorios GitHub de `SofaDefrost`.
- **Nota de honestidad sobre novedad**: en la búsqueda realizada al construir este archivo no apareció literatura publicada que combine específicamente el Niryo Ned2 con SOFA — lo cual sugiere una posible intersección poco explorada. Esto **no es prueba de que no exista**, solo de que no se encontró en esa búsqueda puntual. Antes de afirmar esto como vacío de investigación en la tesis, repetir la búsqueda con varias combinaciones de palabras clave y confirmar la fecha de la búsqueda — esto conecta directo con el principio "nunca afirmar novedad sin buscar" del `SKILL.md` principal.

## 7. Papers de referencia para partir (verificar cada uno antes de citar)

Estos aparecieron en la búsqueda realizada y son un buen punto de partida para rastrear el estado del arte — pero antes de citar cualquiera en la tesis, confirmar autores, año y venue exactos con una búsqueda propia, no asumir los datos de memoria:

- Faure et al., sobre SOFA como framework multi-modelo para simulación física interactiva (capítulo en *Soft Tissue Biomechanical Modeling for Computer Assisted Surgery*, Springer, ~2012).
- Duriez, sobre control de soft robots basado en FEM en tiempo real (ICRA, ~2013).
- Largillière, Verona, Coevoet, Lopez, Dequidt et al., sobre control en tiempo real de soft robots con modelado FEM asíncrono (ICRA, ~2015).
- Coevoet, Escande y Duriez, sobre locomoción y manipulación de soft robots con simulación FEM y programación cuadrática (RoboSoft, ~2019).
- Goury y Duriez, sobre reducción de orden del modelo para control y simulación de soft robots (IEEE Transactions on Robotics, ~2018).

Usar estos como punto de partida para "bola de nieve" (revisar sus referencias y quién los cita después) en vez de como lista cerrada.
