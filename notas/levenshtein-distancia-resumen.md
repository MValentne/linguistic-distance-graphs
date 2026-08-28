# La Distancia de Levenshtein: Historia, Algoritmo y Aplicaciones

## Referencia principal

**Levenshtein, V. I. (1966).** Binary codes capable of correcting deletions, insertions, and reversals. *Soviet Physics Doklady*, 10(8), 707-710.

---

## 1. Historia y origen

### 1.1 Vladimir Levenshtein

Vladimir Levenshtein (1935-2017) fue un matemático ruso que trabajó en el Instituto de Matemáticas Steklov de Moscú. Su paper de 1966 es uno de los más citados en ciencias de la computación, con más de **10,000 citas**.

### 1.2 Contexto original

El trabajo surgió de la **teoría de códigos y corrección de errores**. Levenshtein buscaba extender los resultados de Hamming (que solo manejaba sustituciones) para incluir:
- Inserciones
- Eliminaciones
- Inversiones (intercambio de símbolos adyacentes)

### 1.3 Publicación original

- **Idioma original:** Ruso (1965)
- **Título:** "Двоичные коды с исправлением выпадений, вставок и замещений символов"
- **Revista:** Доклады Академии Наук СССR
- **Traducción al inglés:** 1966, *Soviet Physics Doklady*

---

## 2. Definición formal

### 2.1 Definición intuitiva

La distancia de Levenshtein entre dos palabras es el **número mínimo de operaciones de edición** (inserción, eliminación o sustitución de un carácter) necesarias para transformar una palabra en otra.

### 2.2 Definición matemática

Para dos cadenas $s$ y $t$ de longitudes $|s|$ y $|t|$:

$$\text{lev}(s, t) = \begin{cases} |t| & \text{si } |s| = 0 \\ |s| & \text{si } |t| = 0 \\ \text{lev}(\text{tail}(s), \text{tail}(t)) & \text{si } s[0] = t[0] \\ 1 + \min \begin{cases} \text{lev}(\text{tail}(s), t) \\ \text{lev}(s, \text{tail}(t)) \\ \text{lev}(\text{tail}(s), \text{tail}(t)) \end{cases} & \text{de lo contrario} \end{cases}$$

### 2.3 Las tres operaciones

| Operación | Costo | Descripción |
|-----------|-------|-------------|
| Inserción | 1 | Agregar un carácter |
| Eliminación | 1 | Remover un carácter |
| Sustitución | 1 | Reemplazar un carácter |

### 2.4 Ejemplo básico

```
kitten → sitting

k → s (sustitución)
e → i (sustitución)
_ → g (inserción)

Levenshtein distance = 3
```

---

## 3. El algoritmo

### 3.1 Programación dinámica

La distancia de Levenshtein se calcula usando **programación dinámica** (bottom-up). Se construye una matriz $D$ de tamaño $(|s|+1) \times (|t|+1)$.

### 3.2 Ecuación de recurrencia

$$D[i, j] = \min \begin{cases} D[i-1, j] + 1 & \text{(eliminación)} \\ D[i, j-1] + 1 & \text{(inserción)} \\ D[i-1, j-1] + \delta(s_i, t_j) & \text{(sustitución/match)} \end{cases}$$

Donde $\delta(a, b) = 0$ si $a = b$, y $1$ en caso contrario.

### 3.3 Pseudocódigo

```
function LevenshteinDistance(s, t):
    m = length(s)
    n = length(t)
    
    // Crear matriz de distancias
    D = matrix[0..m, 0..n]
    
    // Casos base
    for i from 0 to m:
        D[i, 0] = i
    for j from 0 to n:
        D[0, j] = j
    
    // Llenar la matriz
    for j from 1 to n:
        for i from 1 to m:
            if s[i] == t[j]:
                cost = 0
            else:
                cost = 1
            
            D[i, j] = min(
                D[i-1, j] + 1,      // eliminación
                D[i, j-1] + 1,      // inserción
                D[i-1, j-1] + cost  // sustitución
            )
    
    return D[m, n]
```

### 3.4 Ejemplo visual

Para "kitten" y "sitting":

```
    ""  s  i  t  t  i  n  g
""   0  1  2  3  4  5  6  7
k    1  1  2  3  4  5  6  7
i    2  2  1  2  3  4  5  6
t    3  3  2  1  2  3  4  5
t    4  4  3  2  1  2  3  4
e    5  5  4  3  2  2  3  4
n    6  6  5  4  3  3  2  3
```

La distancia final es **3** (celda inferior derecha).

---

## 4. Complejidad computacional

### 4.1 Tiempo

- **Algoritmo estándar:** $O(|s| \times |t|)$
- **Con optimización de prefijos/sufijos:** $O(n^2)$ en el peor caso, pero mucho mejor en la práctica para cadenas similares

### 4.2 Espacio

- **Matriz completa:** $O(|s| \times |t|)$
- **Optimización (dos filas):** $O(\min(|s|, |t|))$
- **Hirschberg (con recuperación de camino):** $O(\min(|s|, |t|))$

### 4.3 Límites inferiores

Se ha demostrado que **no existe un algoritmo subcuadrático** $O(n^{2-\varepsilon})$ para la distancia de Levenshtein, a menos que la hipótesis de tiempo exponencial fuerte (SETH) sea falsa.

### 4.4 Optimizaciones prácticas

1. **Saltar prefijos comunes:** Si $s[0..i] = t[0..i]$, no es necesario calcular esas celdas
2. **Saltar sufijos comunes:** Si $s[i..m] = t[j..n]$, saltar esos caracteres
3. **Intercambiar cadenas:** Si $|s| > |t|$, intercambiar para reducir espacio
4. **Técnica "Four Russians":** Precomputar bloques pequeños para acelerar

---

## 5. Variantes y extensiones

### 5.1 Distancia de Levenshtein ponderada

Permite costos diferentes para cada operación:

$$\text{cost}(a \to b) = \begin{cases} c_{\text{sust}} & \text{si } a \neq b \text{ (sustitución)} \\ c_{\text{ins}} & \text{si } b = \epsilon \text{ (inserción)} \\ c_{\text{del}} & \text{si } a = \epsilon \text{ (eliminación)} \end{cases}$$

### 5.2 Distancia de Damerau-Levenshtein

Agrega una cuarta operación: **transposición** de dos caracteres adyacentes. Más realista para errores tipográficos humanos.

### 5.3 Distancia de Levenshtein con restricciones

- **ED costs:** Costos específicos por par de caracteres
- **Gap costs:** Costos para "huecos" en alineamientos (común en bioinformática)

### 5.4 Distancia de Levenshtein fonológica (PLD)

En lugar de comparar caracteres, compara **fonemas** usando vectores de rasgos fonológicos:

- **Sustitución:** Número de rasgos que difieren entre dos fonemas
- **Ventaja:** Captura similitud fonética (ej: /p/ y /b/ son más similares que /p/ y /m/)

### 5.5 LDND (Levenshtein Distance Normalized, Corrected)

La variante usada en el proyecto ASJP:

1. **LD:** Distancia de Levenshtein estándar
2. **LDN:** LD dividida por la longitud de la palabra más larga
3. **LDND:** LDN dividida por el promedio de LDN de todos los pares con significados diferentes

$$\text{LDND} = \frac{\text{LDN}(s_i, t_i)}{\text{promedio}(\text{LDN}(s_i, t_j) \text{ para } i \neq j)}$$

---

## 6. Aplicaciones en lingüística

### 6.1 Dialectometría

- **Kessler (1995):** Primera aplicación a dialectos del irlandés
- **Heeringa (2004):** Aplicación a dialectos neerlandeses
- **Nerbonne & Heeringa (1997):** Establecimiento del método

### 6.2 Clasificación automática de lenguas

- **Brown et al. (2008):** Proyecto ASJP con 2000+ lenguas
- **Holman et al. (2008):** Selección de 40 ítems estables de Swadesh
- **Serva & Petroni (2008):** Primeros en usar LD para fechado glotocronológico

### 6.3 Inteligibilidad mutua

- Correlación significativa entre LD y inteligibilidad funcional (r = 0.82, p < 0.01)
- LD normalizada predice mejor la inteligibilidad que la distancia percibida

### 6.4 Identificación de cognados

- **Babych (2013):** LD fonológica para identificar cognados en corpus comparables
- **Post (2015):** Distancias interlingua para experimentos psicolingüísticos

### 6.5 Reconocimiento de habla

- Métrica para evaluar sistemas de reconocimiento de voz
- Cuantifica distancia entre estímulos presentados y secuencias identificadas

---

## 7. Normalización

### 7.1 ¿Por qué normalizar?

La LD no normalizada favorece a palabras más largas (más operaciones posibles). Para comparar pares de palabras, es necesario normalizar.

### 7.2 Métodos de normalización

| Método | Fórmula | Uso típico |
|--------|---------|------------|
| Por palabra más larga | $LD / \max(|s|, |t|)$ | ASJP, dialectometría |
| Por longitud promedio | $LD / ((|s| + |t|)/2)$ | Alternativa común |
| Por alineamiento | $LD / \text{largo del alineamiento}$ | Lingüística computacional |

### 7.3 Normalización en ASJP

El método ASJP usa una **doble normalización**:

1. **LDN:** $LD / \max(|s|, |t|)$
2. **LDND:** $LDN / \text{promedio}(LDN \text{ de pares aleatorios})$

Esto corrige por:
- Diferencias en longitud de palabras
- Similitudes fonológicas por azar
- Preferencias fonotácticas compartidas

---

## 8. Limitaciones

### 8.1 Lingüísticamente "ingenua"

La LD no distingue entre:
- **Cognados vs. no cognados:** Trata por相似 léxica por azar igual que cambio heredado
- **Similitud superficial vs. homología:** No distingue préstamos de cambio genético
- **Cambios fonológicos sistemáticos:** No considera que ciertos cambios son más probables

### 8.2 Problemas específicos

1. **Metátesis:** Intercambio de sonidos (ej: "particle" → "partical")
2. **Reduplicación:** Repetición de sílabas
3. **Afijos fosilizados:** Prefijos/sufijos que se fusionan con la raíz
4. **Variación en tasas de cambio:** Algunas lenguas cambian más rápido que otras

### 8.3 Rendimiento limitado

**Greenhill (2011)** encontró que la LD clasifica correctamente solo:
- **40%** del tiempo para lenguas austronesias
- **65%** con estandarización ortográfica dentro de subgrupos

### 8.4 Alternativas propuestas

1. **Pesos fonológicos:** Asignar costos según rasgos fonológicos
2. **Algoritmos adaptivos:** Aprender pesos de transición de los datos
3. **Métodos bayesianos:** Modelos probabilísticos de cambio lingüístico
4. **Distancia basada en rasgos:** Usar vectores de rasgos en lugar de símbolos

---

## 9. Aplicaciones más allá de la lingüística

### 9.1 Bioinformática

- Comparación de secuencias de ADN/proteínas
- Alineamiento de secuencias (Smith-Waterman, Needleman-Wunsch)
- Búsqueda de similitud en bases de datos (BLAST)

### 9.2 Procesamiento de lenguaje natural

- Corrector ortográfico
- Detección de plagio
- Memoria de traducción

### 9.3 Reconocimiento de patrones

- Comparación de series temporales
- Reconocimiento de escritura a mano
- Análisis de secuencias genómicas

---

## 10. Relevancia para nuestro proyecto

### 10.1 LD como base del grafo

La distancia de Levenshtein (especialmente LDND) es la **métrica fundamental** para construir nuestro grafo de distancias lingüísticas:

- **Nodos:** lenguas/dialectos
- **Aristas:** pares con LDND > 0
- **Pesos:** valores LDND

### 10.2 Propiedades métricas

La LD tiene propiedades que la hacen ideal para grafos:
- **No negatividad:** $d(x, y) \geq 0$
- **Simetría:** $d(x, y) = d(y, x)$
- **Desigualdad triangular:** $d(x, z) \leq d(x, y) + d(y, z)$
- **Identidad de indiscernibles:** $d(x, x) = 0$

Estas propiedades garantizan que el grafo sea un **espacio métrico**, lo que permite usar algoritmos como MST y caminos mínimos.

### 10.3 Consideraciones para implementación en C++

- **Complejidad:** $O(n^2)$ por par de palabras, $O(N^2 \cdot M)$ para $N$ lenguas con listas de $M$ palabras
- **Optimizaciones:** Usar arreglos de una fila, saltar prefijos/sufijos comunes
- **Paralelización:** Los pares de lenguas son independientes, ideal para GPU/multithreading

---

## Referencias

1. Levenshtein, V. I. (1966). Binary codes capable of correcting deletions, insertions, and reversals. *Soviet Physics Doklady*, 10(8), 707-710.

2. Wagner, R. A., & Fischer, M. J. (1974). The string-to-string correction problem. *Journal of the ACM*, 21(1), 168-173.

3. Kessler, B. (1995). Computational dialectology in Irish Gaelic. *Proceedings of EACL*, 60-67.

4. Heeringa, W. (2004). *Measuring dialect pronunciation differences using Levenshtein distance*. Ph.D. dissertation, Rijksuniversiteit Groningen.

5. Nerbonne, J., & Heeringa, W. (1997). Measuring dialect distance phonetically. *Proceedings of ACL*, 11-18.

6. Brown, C. H., Holman, E. W., Wichmann, S., & Velupillai, V. (2008). Automated classification of the world's languages. *STUF*, 61(4), 285-308.

7. Holman, E. W., Wichmann, S., Brown, C. H., Velupillai, V., Müller, A., & Bakker, D. (2008). Explorations in automated language classification. *Folia Linguistica*, 42(3-4), 331-354.

8. Greenhill, S. J. (2011). Levenshtein distances fail to identify language relationships accurately. *Computational Linguistics*, 37(4), 689-698.

9. Backurs, A., & Indyk, P. (2015). Edit distance cannot be computed in strongly subquadratic time (unless SETH is false). *Proceedings of STOC*.

10. Masek, W. J., & Paterson, M. S. (1980). A faster algorithm computing string edit distances. *Journal of Computer and System Sciences*, 20(1), 18-24.

---

*Creado: 2026-08-26*
*Relacionado con: holman2008-explorations-resumen.md, propuesta-de-investigacion.md*
