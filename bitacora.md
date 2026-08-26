# Bitácora de Investigación

Registro de sesiones de trabajo y avances en el proyecto.

---

## Sesión 1 — 2026-08-26

### Contexto del proyecto

El proyecto propone construir un **modelo cuantitativo de distancia lingüística** representado como un grafo ponderado, donde:

- **Nodos:** idiomas/dialectos
- **Aristas:** distancia lingüística entre pares de idiomas
- **Pesos:** distancia de Levenshtein normalizada sobre listas de Swadesh

El objetivo final es estimar si la distancia léxica puede predecir la **dificultad de aprendizaje** de una lengua para hablantes de otra, eventualmente extendiendo el modelo con componentes fonológicos, gramaticales y ortográficos.

### Pilares del modelo (según `base.md`)

1. **Datos:** Listas de Swadesh (100–207 conceptos) + base de datos ASJP
2. **Métrica:** Levenshtein normalizada por longitud, promediada por concepto
3. **Estructuras derivadas:** grafo de similitud, árbol filogenético (UPGMA/NJ), MST
4. **Extensión:** métrica compuesta $D_{apr} = \alpha D_{lex} + \beta D_{fon} + \gamma D_{gram} + \delta D_{esc}$

### ¿Qué se hizo hoy

- Se estableció `base.md` como documento fundacional del proyecto.
- Se creó esta bitácora para registrar avances por sesión.
- Se leyó y analizó la propuesta completa.

### Próximos pasos pendientes

- [ ] Investigar y acceder a la base de datos ASJP (formato, descarga, estructura)
- [ ] Explorar opciones de obtención de listas de Swadesh para múltiples idiomas
- [ ] Decidir stack tecnológico (Python + NetworkX parece natural para este caso)
- [ ] Implementar el cálculo básico de distancia de Levenshtein normalizada
- [ ] Construir primera matriz de distancias con un conjunto pequeño de idiomas

### Notas

- El proyecto está en fase muy temprana: no hay código, solo la propuesta conceptual.
- La hipótesis central es que $D_{lex}$ (solo léxico) ya tiene poder predictivo parcial, y que la métrica compuesta lo mejora.
- Los datos del ASJP son públicos y ampliamente utilizados en lingüística computacional, lo cual es una buena base.

---

## Sesión 2 — 2026-08-26

### ¿Qué se hizo hoy

- **Estructuración del repo:** se creó la estructura de carpetas estándar para un proyecto de investigación:
  - `data/raw/` y `data/processed/` para datos
  - `src/` para código fuente
  - `notebooks/` para exploración
  - `results/` para figuras y matrices generadas
  - `docs/` para documentación adicional
- Se creó `README.md` con descripción del proyecto, estructura y stack tecnológico.
- Se creó `requirements.txt` con las dependencias iniciales de Python.
- Se creó `.gitignore` apropiado para Python + Jupyter.
- Se hizo el primer commit con toda la estructura base.

### Próximos pasos pendientes

- [ ] Investigar formato y descarga de la base de datos ASJP
- [ ] Obtener primeras listas de Swadesh para un grupo pequeño de idiomas
- [ ] Implementar cálculo de distancia de Levenshtein normalizada
- [ ] Construir primera matriz de distancias
- [ ] Crear primer notebook de exploración

---

## Sesión 3 — 2026-08-26

### Cambio de enfoque del proyecto

Se redefinió radicalmente el alcance del proyecto:

**Enfoque anterior (descartado):** modelo de dificultad de aprendizaje de segundas lenguas con métrica compuesta (léxico + fonología + gramática + escritura). Resultaba demasiado ambicioso y ajeno al perfil de informática.

**Nuevo enfoque:** investigación en **análisis computacional de grafos** aplicado a datos lingüísticos. El grafo de distancias entre idiomas es el dominio de entrada, y la contribución está en los algoritmos que se aplican sobre él.

La pregunta central pasó a ser: *¿Qué estructura computacional tiene la red de distancias léxicas entre idiomas, y qué algoritmos de grafos revelan patrones?*

### Cambios realizados

- **`base.md`** fue reescrito completamente con el nuevo enfoque:
  - Se eliminó todo lo referido a dificultad de aprendizaje y métrica compuesta.
  - Se agregó una pregunta de investigación clara.
  - Se detallaron los algoritmos a aplicar: clustering (Louvain, Leiden), MST, centralidad (betweenness, closeness, eigenvector), caminos mínimos (Dijkstra), comparación de métricas.
  - Se mantuvo la fundamentación y los datos de entrada.

- **Reestructuración del repo:**
  - Se eliminaron `src/`, `notebooks/`, `results/`, `requirements.txt`.
  - Se crearon carpetas orientadas a investigación: `lecturas/`, `notas/`, `borradores/`.
  - Se mantuvo `datos/` con subcarpetas crudos/procesados.
  - Se creó `implementacion/` para el código futuro (C++).

- **`README.md`** fue reescrito para reflejar que el repo es un espacio de investigación colaborativo, no un proyecto de código.

### Próximos pasos pendientes

- [x] Buscar papers clave sobre algoritmos de grafos aplicados a datos lingüísticos
- [ ] Investigar y acceder a la base de datos ASJP (formato, descarga)
- [ ] Explorar qué algoritmos de detección de comunidades funcionan mejor en grafos completos ponderados
- [ ] Definir qué grupo de idiomas usar como primer caso de estudio
- [ ] Discutir si el enfoque de investigación está bien acotado

---

## Sesión 4 — 2026-08-26

### ¿Qué se hizo hoy

- **Revisión bibliográfica completa:** se buscó y compiló el estado del arte del proyecto en `lecturas/estado-del-arte.md`. El documento cubre:
  - Fundamentos de distancia lingüística (Swadesh, Levenshtein)
  - Base de datos ASJP (estructura, descarga, árboles existentes)
  - Métodos computacionales existentes (filogenética, dialectometría)
  - Trabajos recientes directamente relacionados (Chavan et al. 2025, Mikula et al. 2025, Ordway & Patrangenaru 2024)
  - Algoritmos de grafos aplicables (Louvain, Leiden, MST, Dijkstra, centralidad)
  - Brechas identificadas en la literatura
  - Referencias bibliográficas clave

### Hallazgos principales de la revisión

1. **ASJP tiene 11,540 listas de palabras** para miles de variedades lingüísticas. Datos abiertos y accesibles.
2. **Levenshtein está validado** con décadas de uso en dialectometría (r ≈ 0.7 con juicios perceptuales).
3. **Leiden es el algoritmo actual (2026)** para detección de comunidades, superando a Louvain.
4. **El gap principal** está en que pocos trabajos han aplicado la caja de herramientas completa de análisis de grafos al grafo completo de distancias lingüísticas (la mayoría se queda en árboles filogenéticos).
5. **Chavan et al. (2025)** es el trabajo más cercano pero usa solo 4 palabras para Levenshtein y no aplica algoritmos de grafos avanzados.

### Próximos pasos pendientes

- [ ] Descargar y explorar datos ASJP
- [ ] Seleccionar conjunto de idiomas para primer caso de estudio
- [ ] Prototipar el grafo de distancias con un grupo pequeño
- [ ] Comparar Louvain vs. Leiden en el grafo lingüístico
- [ ] Analizar centralidad de idiomas en la red global

---
