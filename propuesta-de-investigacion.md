# Análisis computacional de la distancia lingüística mediante grafos

## Idea general

Representar la proximidad entre idiomas como un grafo ponderado, donde el peso de las aristas se derive de la distancia de Levenshtein normalizada aplicada a vocabulario equivalente (listas de Swadesh). Sobre ese grafo, aplicar algoritmos de análisis de grafos para descubrir estructuras, patrones y propiedades computacionales de la red de distancias lingüísticas.

## Pregunta de investigación

¿Qué estructura computacional tiene la red de distancias léxicas entre idiomas, y qué algoritmos de grafos revelan patrones que concuerdan — o no — con el conocimiento lingüístico existente sobre familias y agrupaciones de lenguas?

## Fundamentación

La lingüística computacional ya ha establecido que la distancia de Levenshtein normalizada sobre listas de Swadesh produce mediciones útiles de similitud léxica entre idiomas. El **Automated Similarity Judgment Program (ASJP)** es el referente principal: utiliza esta métrica para calcular distancias entre miles de variedades lingüísticas y construir árboles filogenéticos.

El punto de partida metodológico está probado. Lo que se propone aquí es tomar esos datos y explorarlos desde la perspectiva del **análisis de grafos**: no solo calcular distancias, sino estudiar la estructura de red que emerge de esas distancias.

## Representación del grafo

### Definición

* **Nodos:** idiomas o dialectos.
* **Aristas:** distancia lingüística entre dos idiomas.
* **Peso de cada arista:** promedio de la distancia de Levenshtein normalizada entre pares de palabras correspondientes a los mismos conceptos.

Formalmente, para un conjunto de conceptos compartidos (C):

$$D(A,B) = \frac{1}{|C|} \sum_{c \in C} LDN(w_{A,c}, w_{B,c})$$

donde:

* $w_{A,c}$ es la palabra correspondiente al concepto $c$ en el idioma $A$,
* $LDN$ es la distancia de Levenshtein normalizada por longitud.

### Propiedades del grafo

- Es **completo** (todos los nodos conectados) si se calcula la distancia entre todos los pares.
- Los pesos están en el rango $[0, 1]$ (distancia normalizada).
- Es **no dirigido** con la métrica base (simétrica).

## Algoritmos y análisis propuesto

El corazón de la investigación está en aplicar y comparar algoritmos sobre este grafo:

### 1. Detección de comunidades / clustering
- Algoritmos: Louvain, Leiden, Label Propagation.
- Pregunta: ¿los clusters naturales del grafo coinciden con las familias lingüísticas conocidas?

### 2. Árbol de expansión mínima (MST)
- Algoritmo: Kruskal o Prim.
- Pregunta: ¿cuál es la "cadena" de idiomas más corta que conecta dos lenguas lejanas?

### 3. Centralidad
- Métricas: betweenness, closeness, eigenvector centrality.
- Pregunta: ¿qué idiomas son más "centrales" en la red global de distancias?

### 4. Caminos mínimos
- Algoritmo: Dijkstra.
- Pregunta: ¿cuántos "saltos" lingüísticos hay entre idiomas de familias distintas?

### 5. Comparación de métricas de distancia
- ¿Qué pasa si se usa una métrica alternativa a Levenshtein?
- ¿Qué tan robustos son los resultados ante cambios en la métrica?

### 6. Visualización
- Representaciones del grafo que revelen su estructura.
- Comparación visual entre clusters computacionales y clasificaciones lingüísticas conocidas.

## Datos

### Listas de Swadesh
Conjuntos estandarizados de vocabulario básico (100–207 conceptos). Constituyen el input para el cálculo de distancias.

### Base de datos ASJP
Contiene listas léxicas para miles de variedades lingüísticas. Es la fuente principal de datos para construir la matriz de distancias.

## Metodología

1. Seleccionar un conjunto de idiomas representativo.
2. Obtener listas de Swadesh correspondientes.
3. Calcular la matriz de distancias (Levenshtein normalizada).
4. Construir el grafo ponderado.
5. Aplicar los algoritmos de análisis enumerados arriba.
6. Comparar resultados con clasificaciones lingüísticas conocidas.
7. Documentar hallazgos y discutir implicancias.

## Hipótesis

La red de distancias léxicas entre idiomas tiene una estructura de grafo que refleja las relaciones filogenéticas conocidas. Diferentes algoritmos de análisis de grafos pueden extraer esa estructura con distintos niveles de precisión, y la comparación entre ellos permite evaluar qué propiedades computacionales son más informativas para describir la organización de las lenguas humanas.

## Posibles direcciones futuras

- Incorporar componentes adicionales (fonológico, gramatical, ortográfico).
- Explorar grafos dirigidos (distancia asimétrica).
- Comparar con métodos jerárquicos (UPGMA, Neighbor Joining).

## Referencias iniciales

* **ASJP (Automated Similarity Judgment Program)**
* Heeringa, W. – *Measuring Dialect Pronunciation Differences using Levenshtein Distance*
* Kondrak, G. – trabajos sobre distancia fonética y cognados en lingüística computacional
* Listas de Swadesh y literatura de lexicoestadística computacional.
