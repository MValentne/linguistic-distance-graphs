# Estado del Arte: Análisis Computacional de Distancia Lingüística mediante Grafos

**Compilado:** 2026-08-26  
**Propósito:** Revisión bibliográfica para el proyecto de investigación  
**Formato:** Documento de lectura (exportable a Kindle)

---

## 1. Introducción

Este documento resume el estado del arte en la intersección entre **lingüística computacional**, **medición de distancia lingüística** y **análisis de grafos**. El objetivo es identificar los antecedentes directos, las metodías existentes y las oportunidades para una investigación que aplique algoritmos de grafos a un grafo ponderado de distancias entre idiomas.

---

## 2. Distancia Lingüística: Fundamentos

### 2.1 Listas de Swadesh

Las listas de Swadesh son conjuntos estandarizados de conceptos universales (relaciones familiares, partes del cuerpo, números, verbos básicos, elementos naturales) que existen en prácticamente todas las lenguas humanas. Morris Swadesh las desarrolló en la década de 1950 con el propósito de medir la relación genealógica entre idiomas.

Variantes principales:
- **Swadesh 207**: lista original completa, la más utilizada actualmente
- **Swadesh 100**: sublista reducida, ampliamente usada en lexicoestadística
- **Swadesh-Yakhontov (35 palabras)**: sublista considerada especialmente estable por el lingüista ruso Sergei Yakhontov
- **Lista ASJP (40 palabras)**: determinada estadísticamente como la más estable por Holman et al. (2008)

**Dato clave:** Holman et al. (2008) demostró que la lista ASJP de 40 palabras es tan precisa como la lista Swadesh de 100 para identificar relaciones entre idiomas. La estabilidad relativa de las palabras fue calculada comparando retenciones entre lenguas de familias establecidas.

**Referencia:** Holman, E. W., Wichmann, S., Brown, C. H., Velupillai, V., Müller, A., & Bakker, D. (2008). Explorations in automated lexicostatistics. *Folia Linguistica*, 42(2), 331-354.

### 2.2 Distancia de Levenshtein

La distancia de Levenshtein (o distancia de edición) mide el número mínimo de operaciones (inserción, eliminación, sustitución) necesarias para transformar una cadena de texto en otra.

**Propiedades métricas:**
- Satisface la desigualdad triangular
- Es simétrica
- La distancia normalizada por longitud最长 palabra da un valor en [0, 1]

**Aplicación en lingüística:**
- Kessler (1995) fue pionero en aplicar la distancia de Levenshtein a dialectos del irlandés
- Nerbonne et al. (1996) la extendieron a dialectos del neerlandés
- Heeringa (2004) realizó la tesis doctoral más completa sobre el tema, aplicándola a dialectos del neerlandés, noruego y alemán

**Validación empírica:**
- Gooskens & Heeringa (2004): correlación significativa (r = 0.67, p < 0.001) entre distancias Levenshtein y distancias perceptuales para 15 variedades del noruego
- Heeringa et al. (2006): consistencia excelente (Cronbach's α = 0.99) y validez (r ≈ 0.7) comparada con juicios de hablantes nativos

**Referencias:**
- Kessler, M. (1995). Computational dialectology in Irish Gaelic: a preliminary analysis. *Proceedings of the Computational Linguistics Conference*.
- Heeringa, W. J. (2004). *Measuring Dialect Pronunciation Differences Using Levenshtein Distance*. Tesis doctoral, Universidad de Groningen. Disponible en: https://pure.rug.nl/ws/portalfiles/portal/9800656/thesis.pdf
- Gooskens, C., & Heeringa, W. (2004). Perceptive evaluation of Levenshtein dialect distance measurements using Norwegian dialect data. *Language Variation and Change*, 16(3), 189-207.

---

## 3. Base de Datos ASJP

### 3.1 Descripción

El **Automated Similarity Judgment Program (ASJP)** es la base de datos más importante para este tipo de investigación. Contiene listas léxicas de 40 conceptos básicos para miles de variedades lingüísticas del mundo.

**Estadísticas actuales (v21, 2025):**
- 11,540 listas de palabras
- 511,960 sinsets (unidades de significado)
- Cobertura de prácticamente todas las lenguas documentadas

**Formato de datos:**
- Transcripción en código ASJP (no IPA directo, pero normalizado)
- Cada lista contiene exactamente 40 ítems
- Los ítems están alineados por número de concepto
- Licencia: Creative Commons Attribution 4.0

**Cómo obtener datos:**
- Sitio web interactivo: https://asjp.clld.org/
- Descarga completa en Zenodo: https://doi.org/10.5281/zenodo.3835822
- Repositorio GitHub (formato CLDF): https://github.com/lexibank/asjp

**Referencia:** Wichmann, S., Brown, C. H., Holman, E. W., Dryer, M. S., & Ran, Q. (eds.). 2025. *The ASJP Database* (version 21).

### 3.2 Árboles filogenéticos del ASJP

El proyecto ASJP ya genera árboles de similitud léxica mundiales utilizando el algoritmo **Neighbor-Joining** sobre matrices de distancias Levenshtein. Esto constituye un punto de partida directo: el árbol ya existe, lo que se propone es explorar la **estructura de grafo completa** (no solo el árbol) y comparar diferentes algoritmos.

Los árboles ASJP identifican correctamente la mayoría de las familias lingüísticas no controvertidas (austroasiáticas, urálicas, mayas, etc.), pero muestran discordancias en casos de contacto lingüístico intenso o frecuencias léxicas atípicas.

**Referencia:** Müller, A., Wichmann, S., et al. (2010). ASJP World Language Tree of Lexical Similarity: Version 3. Disponible en: https://asjp.clld.org/static/WorldLanguageTree-003.pdf

---

## 4. Métodos Computacionales Existentes

### 4.1 Filogenética computacional de lenguas

El campo más desarrollado es la **filogenética computacional**, que busca reconstruir árboles evolutivos de familias lingüísticas. Los métodos principales son:

**Métodos basados en distancia:**
- **Neighbor-Joining (NJ):** Algoritmo greedy que construye árboles minimizando la distancia total. Es el método utilizado por ASJP. Complejidad O(n³).
- **UPGMA (Unweighted Pair Group Method with Arithmetic Mean):** Asume tasa de evolución constante. Más simple pero menos preciso que NJ.
- **MST (Minimum Spanning Tree):** Árbol de expansión mínima que conecta todos los nodos con peso total mínimo. Útil para identificar la "cadena" más corta entre idiomas.

**Métodos basados en caracteres:**
- **Maximum Likelihood (ML):** Busca el árbol más probable dado un modelo de evolución.
- **Bayesian Inference:** Estima la distribución posterior de árboles. Software: BEAST2 + Babel.

**Evaluación de precisión:**
Pompeii, Loreto & Tria (2011) realizaron el primer análisis extensivo de la precisión de estos métodos comparándolos con la clasificación del Ethnologue. Hallazgos clave:
- La precisión es satisfactoria pero altamente heterogénea entre familias lingüísticas
- La completitud y cobertura de las bases de datos afectan significativamente la precisión
- Las familias mejor reconstruidas son las que tienen mejor cobertura de datos

**Referencia:** Pompei, S., Loreto, V., & Tria, F. (2011). On the Accuracy of Language Trees. *PLoS ONE*, 6(6), e20109. https://doi.org/10.1371/journal.pone.0020109

### 4.2 Dialectometría

La dialectometría es el campo que cuantifica las diferencias dialectales usando métodos computacionales. Los antecedentes principales:

**Pioneros:**
- Jean Séguy (1971): primeros cálculos de tasas de similitud entre dialectos del gascon
- Hans Goebl (1982, 1993): desarrollo metodológico independiente, uso de técnicas de biociencias
- Kessler (1995): introducción de la distancia de Levenshtein en dialectología
- Nerbonne & Heeringa (1996 en adelante): expansión y validación del método

**Variantes de Levenshtein en dialectometría:**

1. **Levenshtein VC-sensible (Heeringa 2004):** No permite alineaciones entre vocales y consonantes. Mejora la calidad de alineación.

2. **Levenshtein con operación de intercambio (swap):** Extiende el algoritmo para permitir transposiciones (metátesis). Útil para dialectos donde las transposiciones son frecuentes (ej: búlgaro).

3. **Distancias de segmentos inducidas (Wieling et al. 2009):** En lugar de usar distancias predefinidas entre fonemas, las calcula empíricamente usando Pointwise Mutual Information (PMI). Proceso iterativo:
   - Alinear todas las palabras con Levenshtein estándar
   - Calcular distancias PMI entre fonemas
   - Re-alinear con las nuevas distancias
   - Repetir hasta convergencia

4. **Levenshtein acústico (Heeringa et al. 2009):** Aplica Levenshtein directamente a representaciones acústicas (formantes) en lugar de transcripciones fonéticas. Elimina la dependencia de transcripciones manuales.

**Referencia:** Nerbonne, J., Prokić, J., Wieling, M., & Gooskens, C. (2010). Some further dialectometrical steps. *ASJU Geh*, 53, 41-56.

### 4.3 Distancia fonética con pesos (Mikula et al. 2025)

Un enfoque reciente y relevante es el de Mikula & Sarkociová Remešíková (2025), que presentan un algoritmo de **difusión en grafos** para evaluar similitud léxica:

**Metodología:**
1. Construir un grafo ponderado dirigido donde cada nodo es un idioma
2. Los pesos de las aristas se basan en la distancia fonética entre traducciones
3. Resolver una ecuación de difusión estacionaria con condición de frontera de Dirichlet
4. La solución asigna a cada idioma coordenadas en ℝ^c (donde c = número de clusters de referencia)
5. Las coordenadas se interpretan como probabilidades de pertenecer a cada cluster

**Innovaciones clave:**
- Distancia Damerau-Levenshtein modificada con pesos fonéticos basados en rasgos articulatorios
- Separación de consonantes y vocales con pesos diferentes (w_con = 1.0, w_vow = 0.7)
- Tratamiento de sinónimos con fórmulas específicas para idiomas de referencia vs. clasificados
- Validación con idiomas europeos (eslavos, romances, germánicos)

**Ventaja sobre Levenshtein estándar:** Captura mejor la similitud fonética real. Por ejemplo, la distancia entre "red" [rEd] y "rød" [KœD] danés es 3 con Levenshtein estándar (iguales que "red" y "coch" galés), pero con pesos fonéticos se obtiene una distancia menor que refleja la similitud real.

**Referencia:** Mikula, K., & Sarkociová Remešíková, M. (2025). A Graph Diffusion Algorithm for Lexical Similarity Evaluation. *arXiv:2504.06816*. https://arxiv.org/abs/2504.06816

---

## 5. Algoritmos de Grafos Aplicados

### 5.1 Detección de comunidades

Los algoritmos de detección de comunidades buscan encontrar subgrupos de nodos densamente conectados entre sí y escasamente conectados con el resto de la red.

**Louvain (Blondel et al. 2008):**
- Optimiza modularidad de forma greedy
- Complejidad: O(n·log(n))
- Limitaciones: puede producir comunidades internamente desconectadas, tiene un "resolution limit" (en grafos grandes fusiona comunidades pequeñas)

**Leiden (Traag et al. 2019):**
- Mejora de Louvain que garantiza comunidades bien conectadas
- Tres fases: local moving, refinement, aggregate
- Velocidad 10-100x mayor que Louvain en algunos casos
- Mejora hasta 3% en modularidad
- **Recomendación actual (2026):** Leiden es el algoritmo por defecto para detección de comunidades

**Parámetro de resolución:** Permite controlar la granularidad. Un valor mayor produce más comunidades pequeñas; un valor menor produce menos comunidades grandes.

**Aplicación al problema:** Los clusters encontrados por Louvain/Leiden en el grafo de distancias lingüísticas se compararían con las familias lingüísticas conocidas (Indoeuropea, Sino-Tibetana, Niger-Congo, etc.).

**Referencias:**
- Blondel, V. D., et al. (2008). Fast unfolding of communities in large networks. *Journal of Statistical Mechanics*, 2008(10), P10008.
- Traag, V. A., Waltman, L., & van Eck, N. J. (2019). From Louvain to Leiden: guaranteeing well-connected communities. *Scientific Reports*, 9, 5233.

### 5.2 Árbol de expansión mínima (MST)

El MST conecta todos los nodos del grafo con el peso total de aristas mínimo.

**Algoritmos:**
- **Kruskal:** Ordena aristas por peso, las agrega si no forman ciclo. O(E·log(E))
- **Prim:** Crece el árbol desde un nodo arbitrario agregando la arista más ligera. O(E·log(V))

**Aplicación al problema:**
- El MST del grafo de distancias lingüísticas muestra la "cadena" de transiciones más eficiente entre todos los idiomas
- Las aristas del MST revelan qué pares de idiomas son los más cercanos en la red global
- La estructura del MST puede compararse con árboles filogenéticos tradicionales

**Nota:** El ASJP ya genera árboles usando Neighbor-Joining, que es diferente a MST. Comparar ambos métodos sería un aporte interesante.

### 5.3 Centralidad

Las métricas de centralidad identifican los nodos más "importantes" en la red.

**Betweenness centrality:** Mide cuántos caminos más cortos pasan por un nodo. Los nodos con alta betweenness son "puentes" entre comunidades.

**Closeness centrality:** Mide la distancia promedio de un nodo a todos los demás. Los nodos con alta closeness están "cerca" del centro de la red.

**Eigenvector centralidad:** Mide la importancia de un nodo en función de la importancia de sus vecinos. Un nodo tiene alta eigenvector centralidad si está conectado a nodos que también son importantes.

**Aplicación al problema:**
- ¿Qué idiomas son más "centrales" en la red global de distancias?
- ¿Coinciden los idiomas centrales con lenguas francas o idiomas historicalmente influyentes?
- ¿Los idiomas aislados tienen baja centralidad?

### 5.4 Caminos mínimos (Dijkstra)

El algoritmo de Dijkstra encuentra el camino más corto entre dos nodos en un grafo ponderado.

**Aplicación al problema:**
- ¿Cuántos "saltos lingüísticos" hay entre dos idiomas de familias distintas?
- ¿Qué secuencia de idiomas conecta dos lenguas lejanas con el menor costo acumulado de distancia?
- Esto es diferente a la distancia directa: puede que el camino indirecto a través de un idioma "puente" sea más corto que la conexión directa

---

## 6. Trabajos Relacionados Directos

### 6.1 Chavan et al. (2025): "Quantifying Language Disparities"

Este trabajo es el más cercano al proyecto propuesto. Desarrolla un modelo computacional que:

1. **Construye un árbol de lenguas** usando datos de Diversity Atlas
2. **Calcula distancia ancestral** usando algoritmo de caminos más cortos
3. **Calcula similitud léxica** usando Levenshtein normalizada (solo 4 palabras: water, house, sun, hello)
4. **Modela la relación** con una función de decaimiento exponencial: Score = e^(-d)

**Limitaciones del trabajo:**
- Solo usa 4 palabras para Levenshtein (muy pocas)
- Genera traducciones con IA (potencialmente inexacto)
- No aplica algoritmos de grafos avanzados (solo caminos más cortos)
- No compara con clasificaciones lingüísticas conocidas

**Oportunidad para nuestro proyecto:** Extender este enfoque usando datos ASJP completos (40 conceptos) y aplicar algoritmos más sofisticados (clustering, centralidad, MST).

**Referencia:** Chavan, A., Lee, N., Moieni, R., & Legrand, M. (2025). Quantifying Language Disparities: A Distance-Based Predictive Model Using Linguistic Tree Structures. *Open Journal of Social Sciences*, 13(12), 554-570. DOI: 10.4236/jss.2025.1312040

### 6.2 Cross-Evaluation en Filogenética Lingüística (2024)

Este trabajo (Springer, 2024) analiza problemas de **cross-evaluation** en filogenética computacional de lenguas sudamericanas usando Levenshtein sobre listas de Swadesh. Señala problemas metodológicos importantes:

- Riesgo de sobreajuste al usar los mismos datos para entrenar y evaluar
- Importancia de la validación cruzada en reconstrucción de árboles
- Perspectivas de implementación de comparación con "grupos externos" (outgroup comparison)

**Relevancia:** Nos recuerda la importancia de la validación metodológica rigurosa.

**Referencia:** The Cross-Evaluation Crux for Computational Phylogenetic Linguistics. (2024). En *Lecture Notes in Computer Science*. Springer. DOI: 10.1007/978-3-031-67762-5_7

### 6.3 Sampling the Swadesh List with Tree Spaces (2024)

Ordway & Patrangenaru (2024) aplican un enfoque novedoso usando **espacios de árboles** (tree spaces) del tipo 3-spider para clustering de idiomas:

**Metodología:**
1. Muestrear palabras de las listas de Swadesh
2. Calcular distancias entre pares de idiomas
3. Representar puntos en un espacio de árboles (3-spider)
4. Calcular la media de Fréchet (barycenter) en el espacio de árboles

**Resultados interesantes:**
- **Media no-sticky** (un idioma es más diferente): cuando un idioma es de una familia distinta (ej: inglés, alemán vs. francés → francés es el más diferente)
- **Media sticky** (todos similares): cuando los tres idiomas son de la misma familia (francés, español, italiano) o de familias completamente distintas

**Innovación:** Uso de topología y geometría del espacio de árboles, no solo grafos planos.

**Referencia:** Ordway, G., & Patrangenaru, V. (2024). Sampling the Swadesh List to Identify Similar Languages with Tree Spaces. *arXiv:2405.06549*. https://arxiv.org/abs/2405.06549

---

## 7. Distancia Fonética Ponderada (LREC 2026)

Un paper muy reciente (LREC 2026) compara medidas de distancia con y sin pesos fonéticos en dialectometría:

**Medidas evaluadas:**
- Levenshtein distance (sin pesos)
- Bigram y trigram overlap
- Cosine distance
- Jaro-Winkler
- Jaccard similarity
- Herrgen-Schmidt measure
- Relative Identity Value

**Conclusiones clave:**
- Las medidas con pesos fonéticos explícitos pueden mejorar la representación de similitud lingüística
- La efectividad depende del tipo de análisis (correlacional, distribucional, estabilización, MDS)

**Referencia:** Evaluating Phonetically Weighted and Unweighted Distance Measures in Dialectometry. (2026). *Proceedings of LREC 2026*. DOI: 10.63317/38ndhg759wui

---

## 8. Herramientas y Software

| Herramienta | Uso | URL |
|---|---|---|
| ASJP Database | Datos de listas léxicas | https://asjp.clld.org/ |
| MEGA | Filogenética (Neighbor-Joining) | http://www.megasoftware.net/ |
| BEAST2 + Babel | Filogenética bayesiana | https://taming-the-beast.org/ |
| NetworkX | Análisis de grafos (Python) | https://networkx.org/ |
| igraph | Análisis de grafos (múltiples lenguajes) | https://igraph.org/ |
| Gephi | Visualización de grafos | https://gephi.org/ |
| NetworKit | Análisis de grafos a escala | https://networkit.footdown.com/ |
| Concepticon | Vinculación de listas de conceptos | https://concepticon.clld.org/ |
| Wiktionary Swadesh | Listas de Swadesh por idioma | https://en.wiktionary.org/wiki/Category:Swadesh_lists_by_language |

---

## 9. Brechas en la Literatura (Research Gaps)

Identificando las oportunidades para nuestra investigación:

1. **Foco en algoritmos de grafos, no solo filogenética:** La mayoría del trabajo existente se centra en reconstruir árboles filogenéticos. Poco trabajo ha explorado la **estructura completa del grafo** de distancias (no solo el árbol resultante).

2. **Comparación sistemática de algoritmos:** No existe un estudio comparativo que evalúe Louvain vs. Leiden vs. Label Propagation específicamente sobre grafos de distancias lingüísticas.

3. **Centralidad en grafos lingüísticos:** Muy poco trabajo ha analizado qué idiomas son "centrales" en la red global de distancias léxicas.

4. **MST vs. Neighbor-Joining:** El ASJP genera árboles con NJ, pero no se ha comparado sistemáticamente con MST para este dominio.

5. **Caminos mínimos como métrica de inteligibilidad:** Explorar si el camino más corto en el grafo (no la distancia directa) predice mejor la inteligibilidad mutua.

6. **Sensibilidad a la métrica de distancia:** ¿Qué tan robustos son los resultados de clustering ante cambios en la métrica (Levenshtein estándar vs. ponderada fonéticamente vs. Damerau-Levenshtein)?

---

## 10. Referencias Bibliográficas Clave

### Lingüística computacional y distancia
1. Swadesh, M. (1952). Lexico-statistic dating of prehistoric ethnic contacts. *Proceedings of the American Philosophical Society*, 96(4), 452-463.
2. Kessler, M. (1995). Computational dialectology in Irish Gaelic. *Proceedings of ACL*.
3. Heeringa, W. J. (2004). *Measuring Dialect Pronunciation Differences Using Levenshtein Distance*. Tesis doctoral, Universidad de Groningen.
4. Nerbonne, J., & Heeringa, W. (2010). Measuring dialect distances phonetically. *ASJU*, 53, 41-56.

### Base de datos ASJP
5. Wichmann, S., et al. (eds.). (2025). *The ASJP Database* (version 21). https://asjp.clld.org/
6. Holman, E. W., et al. (2008). Explorations in automated lexicostatistics. *Folia Linguistica*, 42(2), 331-354.
7. Müller, A., et al. (2010). ASJP World Language Tree of Lexical Similarity: Version 3.

### Filogenética computacional
8. Pompei, S., Loreto, V., & Tria, F. (2011). On the Accuracy of Language Trees. *PLoS ONE*, 6(6), e20109.
9. Gray, R. D., & Atkinson, Q. D. (2003). Language-tree divergence times support the Anatolian theory. *Nature*, 426, 435-439.

### Algoritmos de grafos
10. Blondel, V. D., et al. (2008). Fast unfolding of communities in large networks. *JSTAT*, 2008(10), P10008.
11. Traag, V. A., et al. (2019). From Louvain to Leiden: guaranteeing well-connected communities. *Scientific Reports*, 9, 5233.

### Trabajos recientes directamente relacionados
12. Chavan, A., et al. (2025). Quantifying Language Disparities. *Open J. Social Sciences*, 13(12), 554-570.
13. Mikula, K., & Sarkociová Remešíková, M. (2025). A Graph Diffusion Algorithm for Lexical Similarity Evaluation. *arXiv:2504.06816*.
14. Ordway, G., & Patrangenaru, V. (2024). Sampling the Swadesh List to Identify Similar Languages with Tree Spaces. *arXiv:2405.06549*.
15. Evaluating Phonetically Weighted and Unweighted Distance Measures in Dialectometry. (2026). *LREC 2026 Proceedings*.

---

## 11. Conclusión

El estado del arte muestra que:

1. **Los datos existen y son accesibles:** ASJP proporciona datos para más de 11,000 variedades lingüísticas con licencia abierta.

2. **La metodología de Levenshtein está validada:** Décadas de uso en dialectometría han confirmado su fiabilidad.

3. **Los algoritmos de grafos están maduros:** Louvain/Leiden, MST, Dijkstra y métricas de centralidad son algoritmos bien establecidos con implementaciones en múltiples bibliotecas.

4. **El gap está en la integración:** Pocos trabajos han tomado el grafo completo de distancias lingüísticas y aplicado la caja de herramientas completa del análisis de grafos moderno. La mayoría se queda en árboles filogenéticos.

5. **Nuestra oportunidad:** Aplicar y comparar sistemáticamente algoritmos de grafos (clustering, MST, centralidad, caminos mínimos) sobre datos ASJP, y evaluar qué patrones emergen y cómo se comparan con el conocimiento lingüístico establecido.
