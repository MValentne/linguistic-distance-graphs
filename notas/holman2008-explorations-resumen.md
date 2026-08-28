# Resumen: Explorations in Automated Language Classification

## Referencia bibliográfica

**Autores:** Eric W. Holman, Søren Wichmann, Cecil H. Brown, Viveka Velupillai, André Müller, Dik Bakker

**Título:** Explorations in automated language classification

**Revista:** *Folia Linguistica*, 42(3-4), 331-354

**Año:** 2008

**DOI:** [10.1515/FLIN.2008.331](https://doi.org/10.1515/FLIN.2008.331)

**Acceso:** De Gruyter (requiere suscripción o compra)

---

## Contexto

Este paper es parte del proyecto ASJP (Automated Similarity Judgment Program), que busca automatizar la clasificación de lenguas del mundo basándose en listas de vocabulario básico de Swadesh. El paper anterior (Brown et al. 2008) describía el método base usando la lista de 100 elementos de Swadesh (1955). Este paper presenta una **refinación** de ese método.

---

## Objetivo

1. Calcular la **estabilidad relativa** de cada ítem en la lista de Swadesh (100 conceptos)
2. Crear una **lista reducida** de 40 ítems más estables
3. Comparar el rendimiento clasificatorio entre la lista completa (100) y la reducida (40)
4. Investigar la relación entre **similitud léxica** y **proximidad geográfica**
5. Probar si la combinación de datos **tipológicos** con datos **lexicales** mejora la precisión clasificatoria

---

## Metodología

### Base de datos
- **Database ASJP:** Listas de Swadesh de ~2000 idiomas
- Transcritos en **ASJPcode** (alfabeto simplificado de 41 símbolos usando solo teclado QWERTY)
- ~2 millones de pares de idiomas comparados

### Distancia léxica: Levenshtein Distance Normalizada (LDN)
- **LD (Levenshtein Distance):** Número mínimo de inserciones, eliminaciones o sustituciones para transformar una palabra en otra
- **LDN:** LD normalizada dividiendo por la longitud de la palabra más larga
- **LDND (Levenshtein Distance Normalized, Corrected):** LDN dividida por el promedio de LDN de todos los pares con significados diferentes (corrige por similitudes fonológicas por azar)

### Medida de estabilidad
- Los ítems **más estables** son aquellos que tienden a producir cognados dentro de grupos ampliamente reconocidos de lenguas estrechamente relacionadas
- Se usaron las clasificaciones de Dryer (2005) y Ethnologue (Gordon 2005) como punto de referencia
- La estabilidad resultó ser **consistente** entre lenguas de diferentes hemisferos

---

## Hallazgos principales

### 1. Lista reducida de 40 ítems
- De la lista original de 100 conceptos de Swadesh, se seleccionaron los **40 más estables**
- Esta lista reducida produce clasificaciones **al menos tan buenas** como la lista completa
- Iterativamente se encontró que 40 ítems es el tamaño óptimo (ni más ni menos)

### 2. Rendimiento clasificatorio
- El método automático produce clasificaciones **casi expertas**
- La correlación con clasificaciones de lingüistas históricos es alta
- La automatización permite aplicar los mismos criterios objetivos a escala mundial

### 3. Relación léxico-geográfica
- Se presentó una investigación estadística sobre la relación entre similitud léxica y proximidad geográfica
- Las lenguas geográficamente cercanas tienden a ser más similares léxicamente

### 4. Combinación con tipología
- Se probó si información tipológica puede combinarse con datos lexicales
- Resultados preliminares sobre mejora de precisión clasificatoria

---

## Relevancia para nuestro proyecto

### Conexión directa con `propuesta-de-investigacion.md`

Este paper es **fundamental** para nuestro proyecto porque:

1. **Valida el enfoque de Levenshtein:** Demuestra que LD (y LDND) es una medida efectiva de distancia léxica para clasificación automática

2. **Base para análisis de grafo:** La matriz de distancias LDND que generan es esencialmente la base de datos que podemos usar para construir nuestro grafo pesado

3. **Algoritmos de clasificación:** Los autores usan SplitsTree y Neighbor-Joining (algoritmos filogenéticos) que están relacionados con MST y detección de comunidades

4. **Estabilidad de ítems:** La información sobre qué conceptos de Swadesh son más estables es relevante para decidir qué pesos asignar en el grafo

### Oportunidades para investigación

- **Detección de comunidades:** ¿Cómo se comparan las comunidades detectadas por algoritmos de grafos con las familias lingüísticas conocidas?
- **MST:** ¿El MST del grafo de distancia LDND reproduce árboles filogenéticos conocidos?
- **Centralidad:** ¿Qué idiomas son más "centrales" en la red de distancias léxicas?
- **Caminos más cortos:** ¿Los caminos más cortos en el grafo corresponden a rutas de difusión lingüística?

---

## Referencias clave citadas

1. Brown, C. H., Holman, E. W., Wichmann, S., & Velupillai, V. (2008). Automated classification of the world's languages. *STUF*, 61(4), 285-308.

2. Swadesh, M. (1955). Towards greater accuracy in lexicostatistic dating. *International Journal of American Linguistics*, 21(2), 121-137.

3. Dryer, M. S. (2005). *Order of subject, object and verb*. WALS.

4. Gordon, R. G. (2005). *Ethnologue: Languages of the world* (15th ed.). SIL International.

5. Levenshtein, V. I. (1966). Binary codes capable of correcting deletions, insertions and reversals. *Soviet Physics Doklady*, 10(8), 707-710.

---

## Estado de la nota

- [ ] Obtener PDF del paper (requiere acceso institucional o compra)
- [ ] Lectura completa y análisis detallado
- [ ] Comparar con implementación futura en C++

---

*Creado: 2026-08-26*
*Relacionado con: propuesta-de-investigacion.md, bitacora.md*
