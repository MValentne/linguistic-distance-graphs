# Introducción (borrador)

**Trabajo:** Análisis computacional de la distancia lingüística mediante grafos
**Institución:** Universidad Nacional de Misiones — Facultad de Ciencias Exactas, Químicas y Naturales
**Autores:** Mende Valentino, Saucedo Beatriz Priscilla
**Estado:** borrador en revisión (2026-10-12)

> Este archivo conserva el borrador de la Introducción tal como lo redactaron los autores. Al final se listan las correcciones de redacción pendientes. El cuerpo principal corresponde a la versión más reciente del boceto.

---

## Resumen (abstract)

*(Pendiente de redacción: todavía no es el momento.)*

---

## Introducción

### Contexto

Determinar qué tan próximas son dos lenguas y cómo se agrupan son cuestiones de interés en la lingüística. Las lenguas emparentadas o en contacto prolongado presentan similitudes o vocabulario compartido en contraste con aquellas en las que no se presentan estos factores. Esta noción de proximidad podría cuantificarse y para los fines de este estudio, visualizable, por lo que este trabajo aborda esta pregunta mediante el uso de grafos ponderados, representando las lenguas estudiadas como nodos, la distancia entre una lengua y otra como una arista con un valor asociado, y aplicando sobre esa red algoritmos de análisis de grafos.

### Justificación

El volumen de datos a procesar da al problema un carácter computacional, por lo que procesos informáticos resultan ser necesarios a la hora de trabajar con dicho volumen de comparaciones.

Si bien este tema ha sido ampliamente estudiado en el campo de la lexicoestadística computacional, no hay suficiente desarrollo en abstracciones de alto nivel, por lo que es una ruta poco transitada (Chavan et al., 2025. 4 palabras, solo caminos mínimos limitados a árboles).

### Objetivo

Se busca en este trabajo el análisis de relaciones lingüísticas de alcance global, mediante operaciones algorítmicas sobre una estructura de datos abstracta, un grafo. El objetivo radica en analizar esta red global de distancias léxicas, evaluando la estructura resultante mediante algoritmos de grafos y su concordancia con las clasificaciones lingüísticas existentes.

### Enfoque y contribución

Si bien es un trabajo fundamentado en conceptos de lexicoestadística computacional, este se inclina por un enfoque informático; la lingüística aporta un dominio y un caso de estudio mientras que la aportación recae en el análisis computacional. La intención es cubrir una brecha metodológica; no se ha profundizado en la caja de herramientas de grafos (detección de comunidades, árbol de expansión mínima, centralidad y caminos mínimos) aplicada a recursos como la base de datos ASJP. Se contrasta y se valida la estructura resultante con las clasificaciones lingüísticas existentes.

---

## Revisión pendiente (correcciones de redacción)

> Notas de revisión, no forman parte del texto. Pendientes de aplicar sobre el borrador de arriba.

### Estructura

- **Brecha duplicada.** La misma idea aparece en *Justificación* ("no hay suficiente desarrollo en abstracciones de alto nivel") y en *Enfoque y contribución* ("no se ha profundizado en la caja de herramientas de grafos…"). Dejar la brecha **solo en Justificación**; que *Enfoque* arranque directo con el posicionamiento y la contribución.
- **Objetivo repetido.** *Objetivo* enuncia la meta dos veces ("Se busca… el análisis…" / "El objetivo radica en analizar…"). Fusionar en una sola oración.

### Frase por frase

- **Contexto:** "podría cuantificarse" → preferible "puede cuantificarse" (la cuantificabilidad ya no se pone en duda). Menor: "mediante el uso de grafos" → "mediante grafos".
- **Justificación, frase 1:** "da al problema un carácter computacional" ✅ (corregido), pero el párrafo queda circular; falta el dato concreto. Sugerencia: *"El volumen de datos da al problema un carácter necesariamente computacional: comparar $N$ lenguas exige evaluar $\binom{N}{2}$ pares, cada uno con 40 comparaciones de distancia, lo que con las más de 11.000 listas de ASJP supera los 66 millones de pares."*
- **Justificación, cita:** "(Chavan et al., 2025**.** 4 palabras, solo caminos mínimos limitados a árboles)" → sacar el punto tras el año y llevar la acotación a prosa: *"El antecedente más cercano (Chavan et al., 2025) emplea solo cuatro palabras y se limita a caminos mínimos sobre un árbol."*
- **Objetivo:** mezcla de formas verbales ("evaluando… y su concordancia…") y "evaluar/analizar" repetidos. Reescritura sugerida: *"El objetivo de este trabajo es analizar la red global de distancias léxicas, determinar qué estructura revelan sobre ella los algoritmos de grafos y evaluar en qué medida esa estructura concuerda con las clasificaciones lingüísticas existentes."* Conviene además explicitar la **pregunta de investigación** de `propuesta-de-investigacion.md`.
- **Enfoque:** falta coma antes de "mientras que" ("un caso de estudio**,** mientras que…").

### Front matter

- Orden de autores unificado como "apellido nombre" ✅.
- Resumen: placeholder ("Todavía no es el momento."). Redactar en impersonal cuando llegue el momento.

---

*Relacionado con: `propuesta-de-investigacion.md`, `bitacora.md`, `lecturas/estado-del-arte.md`*
