# Informe Técnico: Métrica de Distancia Léxica para el Grafo de Idiomas

**Proyecto:** Análisis computacional de la distancia lingüística mediante grafos  
**Fecha:** 2026-10-12  
**Sesión:** 7 — Definición formal de la métrica  
**Estado:** Versión 1.0 (baseline para implementación)

---

## 1. Resumen Ejecutivo

Este documento define la **métrica exacta** que se utilizará para construir el grafo ponderado de distancias lingüísticas. La decisión es:

> **Métrica base (v1): LDND (Levenshtein Distance Normalized, Corrected)** — la variante de doble normalización usada y validada por el proyecto ASJP (Holman et al. 2008; Wichmann et al. 2025).

> **Métrica extendida (v2+):** PLD (Phonological Levenshtein Distance) con matriz de rasgos fonológicos, como dirección de investigación explícita para comparar robustez de clusters.

---

## 2. Definición Formal de la Métrica Base: LDND

### 2.1 Distancia de Levenshtein Estándar (LD)

Para dos cadenas $s$ y $t$ sobre un alfabeto $\Sigma$:

$$
LD(s, t) = \min_{\text{secuencia de ops}} \sum_{op} \text{costo}(op)
$$

Operaciones permitidas (costo unitario = 1):
- **Inserción:** $s \to s \cdot a$
- **Eliminación:** $s \cdot a \to s$
- **Sustitución:** $s \cdot a \to s \cdot b$ ($a \neq b$)

**Algoritmo:** Programación dinámica, matriz $(|s|+1) \times (|t|+1)$, complejidad $O(|s| \cdot |t|)$.

**Referencia fundacional:**
> Levenshtein, V. I. (1966). Binary codes capable of correcting deletions, insertions, and reversals. *Soviet Physics Doklady*, 10(8), 707–710.

**Implementación estándar (Wagner-Fischer):**
> Wagner, R. A., & Fischer, M. J. (1974). The string-to-string correction problem. *Journal of the ACM*, 21(1), 168–173.

---

### 2.2 Normalización por Longitud: LDN

La LD cruda penaliza palabras largas. Normalización estándar en dialectometría y ASJP:

$$
LDN(s, t) = \frac{LD(s, t)}{\max(|s|, |t|)} \in [0, 1]
$$

**Justificación:** Hace la distancia independiente de la longitud de las palabras comparadas.

**Referencia dialéctica:**
> Heeringa, W. J. (2004). *Measuring Dialect Pronunciation Differences Using Levenshtein Distance*. PhD thesis, University of Groningen. https://pure.rug.nl/ws/portalfiles/portal/9800656/thesis.pdf

---

### 2.3 Corrección por Similitud Aleatoria: LDND (Métrica ASJP)

Dos palabras pueden parecerse por azar (fonotáctica compartida, inventario fonológico similar). ASJP corrige esto:

$$
LDND(s_i, t_i) = \frac{LDN(s_i, t_i)}{\frac{1}{|C|-1} \sum_{j \neq i} LDN(s_i, t_j)}
$$

Donde:
- $s_i, t_i$ = palabras para el **mismo concepto** $i$ en idiomas $s$ y $t$
- El denominador es el **promedio de LDN entre $s_i$ y todas las palabras de $t$ con significado distinto** (pares aleatorios)

**Interpretación:** LDND > 1 significa que las palabras son **más diferentes** de lo esperado por azar; LDND < 1 significa **más similares** (posibles cognados).

**Referencia clave:**
> Holman, E. W., Wichmann, S., Brown, C. H., Velupillai, V., Müller, A., & Bakker, D. (2008). Explorations in automated language classification. *Folia Linguistica*, 42(3–4), 331–354. https://doi.org/10.1515/FLIN.2008.331

> Wichmann, S., Brown, C. H., Holman, E. W., Dryer, M. S., & Ran, Q. (eds.). (2025). *The ASJP Database* (version 21). https://asjp.clld.org/

---

### 2.4 Distancia Final entre Dos Idiomas (Peso de Arista)

Para un conjunto de conceptos compartidos $C$ (lista ASJP = 40 ítems):

$$
D(A, B) = \frac{1}{|C|} \sum_{c \in C} LDND(w_{A,c}, w_{B,c})
$$

**Propiedades de la métrica resultante:**
- Rango típico: $[0.5, 2.5]$ (no acotado estrictamente a $[0,1]$ por la corrección)
- Simétrica: $D(A,B) = D(B,A)$
- Cumple desigualdad triangular (heredada de LD)
- **Espacio métrico** → válido para MST, Dijkstra, clustering basado en distancia

---

## 3. Variantes Ponderadas (Dirección Futura v2+)

### 3.1 PLD: Phonological Levenshtein Distance

Reemplaza costo unitario de sustitución por **distancia fonológica** entre fonemas:

$$
\text{cost}_{\text{PLD}}(x \to y) = \text{número de rasgos fonológicos que difieren entre } x \text{ e } y
$$

**Matriz de rasgos típica (SPE / Hayes 2009):** ~20 rasgos binarios/multivaluados (sonorante, silábico, consonántico, anterior, coronal, alto, bajo, posterior, redondeado, voz, continuante, nasal, etc.)

**Ejemplo:**
| Par | Rasgos diferentes | Costo PLD |
|-----|-------------------|-----------|
| /p/ → /b/ | [±voz] | 1 |
| /p/ → /m/ | [±voz], [±nasal] | 2 |
| /p/ → /a/ | [±cons], [±son], [±syll], ... | 8–12 |

**Referencias:**
> Kondrak, G. (2002). Algorithms for language reconstruction. PhD thesis, University of Toronto.  
> Wieling, M., Prokić, J., & Nerbonne, J. (2009). Evaluating the pairwise alignment of phonetic transcriptions. *Proceedings of EACL*.  
> Mikula, K., & Sarkociová Remešíková, M. (2025). A Graph Diffusion Algorithm for Lexical Similarity Evaluation. *arXiv:2504.06816*.

---

### 3.2 Mikula & Sarkociová (2025): Damerau-Levenshtein con Pesos Articulatorios

Extiende PLD con:
1. **Operación de transposición** (swap) — captura metátesis
2. **Pesos por clase:** $w_{\text{con}} = 1.0$, $w_{\text{vow}} = 0.7$
3. **Pesos asimétricos** por rasgos articulatorios (lugar, modo, sonoridad)

**Ventaja demostrada:** Distingue mejor "red" [rEd] vs "rød" [KœD] (danés, distancia fonética baja) de "red" vs "coch" (galés, distancia alta), donde Levenshtein estándar da 3 en ambos casos.

---

### 3.3 Evaluación Sistemática LREC 2026

> Evaluating Phonetically Weighted and Unweighted Distance Measures in Dialectometry. (2026). *Proceedings of LREC 2026*. DOI: 10.63317/38ndhg759wui

**Hallazgo:** Pesos fonéticos **mejoran** correlación con juicios perceptuales y calidad de MDS/clustering, pero **la magnitud depende del análisis** (correlacional > clustering > visualización).

---

## 4. Decisión para el Proyecto: Qué Implementar Ahora

| Métrica | Implementación | Datos requeridos | Comparabilidad con ASJP | Prioridad |
|---------|----------------|------------------|-------------------------|-----------|
| **LDND (ASJP estándar)** | ~50 líneas Python/C++ | ASJPcode (ya en DB) | **Total** (mismo pipeline) | **v1 — OBLIGATORIA** |
| LDN simple (sin corrección) | Trivial | ASJPcode | Parcial (sin corrección azar) | Solo para ablation |
| PLD (rasgos fonológicos) | Media (~200 líneas + matriz rasgos) | ASJPcode → IPA → rasgos | Requiere validación propia | **v2 — EXPERIMENTO** |
| Mikula 2025 (Damerau + pesos) | Alta | Tabla pesos por par fonemas | Nueva, sin baseline público | v3 — Exploratorio |

### Justificación de quedarse en LDND para v1

1. **Reproducibilidad total:** Cualquier resultado es directamente comparable con árboles ASJP publicados (Müller et al. 2010, ASJP v21 2025).
2. **Validación empírica:** 15+ años de uso en dialectometría (Heeringa 2004; Gooskens & Heeringa 2004; Nerbonne & Heeringa 2010) y clasificación automática (Holman et al. 2008; Brown et al. 2008).
3. **Datos listos:** ASJP entrega las 40 palabras ya alineadas por concepto en ASJPcode — **cero preprocesamiento lingüístico**.
4. **Complejidad controlada:** $O(N^2 \cdot M \cdot L^2)$ donde $N$=idiomas, $M$=40 conceptos, $L$≈5–8 chars/palabra. Manejable en C++ para $N \sim 1000$.
5. **Puerta abierta a v2:** El grafo se construye con *cualquier* matriz de distancias. Cambiar la métrica = recalcular pesos, mismos algoritmos de grafo.

---

## 5. Especificación de Implementación (para C++)

### 5.1 Función Core: `ldnd_distance(word_a, word_b, all_words_b)`

```cpp
// Entrada: word_a (string ASJPcode), word_b (string ASJPcode), 
//          vector<string> all_words_b (todas las palabras del idioma B, 40 conceptos)
// Salida: double LDND

double ld_distance(const string& a, const string& b);  // Wagner-Fischer standard
double ldn_distance(const string& a, const string& b); // ld / max(len)

double ldnd_distance(const string& word_a, const string& word_b, 
                     const vector<string>& all_words_b) {
    double ldn = ldn_distance(word_a, word_b);
    
    // Promedio LDN contra palabras de distinto concepto (índice != concepto actual)
    double sum = 0.0;
    int count = 0;
    for (size_t j = 0; j < all_words_b.size(); ++j) {
        if (j != current_concept_index) {  // requiere saber el índice del concepto
            sum += ldn_distance(word_a, all_words_b[j]);
            count++;
        }
    }
    double mean_random = sum / count;
    
    return ldn / mean_random;
}
```

**Nota:** El índice del concepto es necesario porque ASJP alinea por posición (concepto 1 = "yo", concepto 2 = "tú", ... concepto 40 = "noche").

### 5.2 Matriz Completa $N \times N$

```cpp
vector<vector<double>> compute_distance_matrix(
    const vector<array<string, 40>>& languages  // N idiomas × 40 conceptos
) {
    size_t N = languages.size();
    vector<vector<double>> D(N, vector<double>(N, 0.0));
    
    for (size_t i = 0; i < N; ++i) {
        for (size_t j = i+1; j < N; ++j) {
            double sum = 0.0;
            for (int c = 0; c < 40; ++c) {
                sum += ldnd_distance(languages[i][c], languages[j][c], 
                                     vector<string>(languages[j].begin(), languages[j].end()));
            }
            D[i][j] = D[j][i] = sum / 40.0;
        }
    }
    return D;
}
```

**Complejidad:** $O(N^2 \cdot 40 \cdot L^2)$ ≈ 0.5 seg para $N=500$ en C++ optimizado (single thread).

---

## 6. Validación Empírica Planificada (Checklist v1)

| Test | Método | Criterio de éxito |
|------|--------|-------------------|
| **Reproducción ASJP** | Calcular matriz para 100 idiomas ASJP, comparar distancias con matriz publicada | Correlación $r > 0.99$ |
| **Familias conocidas** | Leiden clustering en matriz 200 idiomas, comparar clusters con Ethnologue/Glottolog | ARI > 0.7 vs familias macro |
| **MST vs Neighbor-Joining** | Comparar topología MST vs árbol NJ de ASJP | Coincidencia en ramas principales |
| **Ablation LDND vs LDN** | Repetir clustering con LDN simple (sin corrección azar) | LDND produce clusters más limpios |

---

## 7. Referencias Bibliográficas Completas

### Fundamentos Levenshtein
1. Levenshtein, V. I. (1966). Binary codes capable of correcting deletions, insertions, and reversals. *Soviet Physics Doklady*, 10(8), 707–710.
2. Wagner, R. A., & Fischer, M. J. (1974). The string-to-string correction problem. *J. ACM*, 21(1), 168–173.
3. Backurs, A., & Indyk, P. (2015). Edit distance cannot be computed in strongly subquadratic time (unless SETH is false). *STOC*.

### Dialectometría y Validación
4. Kessler, B. (1995). Computational dialectology in Irish Gaelic. *Proc. EACL*, 60–67.
5. Nerbonne, J., & Heeringa, W. (1997). Measuring dialect distance phonetically. *Proc. ACL*, 11–18.
6. Heeringa, W. J. (2004). *Measuring Dialect Pronunciation Differences Using Levenshtein Distance*. PhD thesis, Univ. Groningen.
7. Gooskens, C., & Heeringa, W. (2004). Perceptive evaluation of Levenshtein dialect distance measurements using Norwegian dialect data. *Language Variation and Change*, 16(3), 189–207.
8. Nerbonne, J., Prokić, J., Wieling, M., & Gooskens, C. (2010). Some further dialectometrical steps. *ASJU*, 53, 41–56.

### Proyecto ASJP y LDND
9. Brown, C. H., Holman, E. W., Wichmann, S., & Velupillai, V. (2008). Automated classification of the world's languages. *STUF*, 61(4), 285–308.
10. Holman, E. W., Wichmann, S., Brown, C. H., Velupillai, V., Müller, A., & Bakker, D. (2008). Explorations in automated language classification. *Folia Linguistica*, 42(3–4), 331–354.
11. Wichmann, S., et al. (eds.). (2025). *The ASJP Database* (version 21). https://asjp.clld.org/
12. Müller, A., Wichmann, S., et al. (2010). ASJP World Language Tree of Lexical Similarity: Version 3.

### Levenshtein Ponderado / Fonológico
13. Kondrak, G. (2002). Algorithms for language reconstruction. PhD thesis, Univ. Toronto.
14. Wieling, M., Prokić, J., & Nerbonne, J. (2009). Evaluating the pairwise alignment of phonetic transcriptions. *EACL*.
15. Mikula, K., & Sarkociová Remešíková, M. (2025). A Graph Diffusion Algorithm for Lexical Similarity Evaluation. *arXiv:2504.06816*.
16. Evaluating Phonetically Weighted and Unweighted Distance Measures in Dialectometry. (2026). *LREC 2026*. DOI: 10.63317/38ndhg759wui.

### Filogenética y Evaluación
17. Pompei, S., Loreto, V., & Tria, F. (2011). On the Accuracy of Language Trees. *PLoS ONE*, 6(6), e20109.
18. Chavan, A., Lee, N., Moieni, R., & Legrand, M. (2025). Quantifying Language Disparities. *Open J. Social Sciences*, 13(12), 554–570.
19. Ordway, G., & Patrangenaru, V. (2024). Sampling the Swadesh List to Identify Similar Languages with Tree Spaces. *arXiv:2405.06549*.

### Algoritmos de Grafos (para análisis posterior)
20. Blondel, V. D., et al. (2008). Fast unfolding of communities in large networks. *JSTAT*, P10008.
21. Traag, V. A., Waltman, L., & van Eck, N. J. (2019). From Louvain to Leiden: guaranteeing well-connected communities. *Scientific Reports*, 9, 5233.

---

## 8. Próximos Pasos Inmediatos

1. [ ] **Descargar ASJP v21** (Zenodo o CLDF GitHub) y verificar formato
2. [ ] **Prototipo Python** (100 líneas) que lea ASJP, calcule LDND para 50 idiomas, genere matriz CSV
3. [ ] **Validar** contra distancias publicadas en ASJP (spot-check 10 pares)
4. [ ] **Registrar en bitácora** la decisión de métrica y pasar a Sesión 8 (datos + prototipo)

---

*Documento generado en Sesión 7. Versión 1.0 — lista para implementación baseline.*