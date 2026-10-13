# Resumen: Quantifying Language Disparities (Chavan et al. 2025)

## Referencia bibliográfica

**Autores:** Akshata Chavan, Nicole Lee, Rezza Moieni, Mary Legrand

**Afilación:** Cultural Infusion Pty Ltd., Melbourne, Australia

**Título:** Quantifying Language Disparities: A Distance-Based Predictive Model Using Linguistic Tree Structures

**Revista:** *Open Journal of Social Sciences*, 13(12), 554–570

**Año:** 2025 (recibido 13/10/2025, aceptado 28/12/2025, publicado 31/12/2025)

**DOI:** [10.4236/jss.2025.1312040](https://doi.org/10.4236/jss.2025.1312040)

**Acceso:** Abierto (CC BY 4.0). PDF descargado en `~/Desktop/Chavan2025-Quantifying-Language-Disparities.pdf`

**Cita textual:** "the world's first scalable model to measure language disparities between any two languages" (los propios autores)

---

## Qué propone

Un modelo computacional que asigna a **cualquier par de lenguas** una puntuación numérica de *disparidad* (distancia lingüística), combinando dos fuentes:

1. **Distancia ancestral** — recorrido en un árbol de familias lingüísticas, vía algoritmo de caminos más cortos.
2. **Similitud léxica** — distancia de Levenshtein sobre un puñado de palabras.

El objetivo declarado **no es lingüístico sino organizacional**: medir "barreras idiomáticas" en entornos laborales multilingües para diseñar estrategias de comunicación inclusiva. La aplicación práctica es una herramienta web (bajo el dominio de Diversity Atlas) que recibe dos lenguas y devuelve su score, ancestro común, camino en el árbol y similitud léxica.

---

## Metodología (4 componentes)

### 1. Construcción del árbol de familias
Grafo-árbol estático provisto por **Diversity Atlas** (dataset propio de la empresa de los autores). Lenguas = nodos, relaciones genealógicas/clasificatorias = aristas. Se inspira en *The Linguasphere Register* (Dalby, 2000), que distingue *phylosectors* (parentesco histórico/genético) y *geosectors* (proximidad geográfica).

### 2. Distancia ancestral: caminos más cortos
- Entre lenguas de ramas distintas: **Shortest Path** estándar. La distancia `d` = número de aristas en el camino que pasa por el ancestro común más bajo.
- Entre lenguas muy cercanas (misma familia, dialectos): usan **DFS** en lugar de shortest path, para recorrer nodos hermanos e intermedios.
- Ejemplo del paper: inglés–zulú = 6 pasos; inglés–yidis = 5 pasos.

### 3. Similitud léxica: Levenshtein
- Usa **solo 4 palabras**: *water, house, sun, hello*.
- Fórmulas: `D = LD(w1, w2)`; `L = max(len(w1), len(w2))`; `distancia normalizada = D/L`; `similitud = 1 − D/L`. Luego promedio sobre los 4 pares.
- **Las traducciones las generó Claude AI** (LLM), normalizadas a minúsculas. Ejemplo del paper: Afrikaans–Aachenisch ≈ 0.7925; Afrikaans–Aboriginal English ≈ 0.5175.

### 4. Score de relación: decaimiento exponencial
$$\text{Similarity Score} = e^{-d}$$
donde `d` es la distancia en el árbol. Umbrales:
- **Estricto:** $e^{-1} \approx 0.368$ → solo pares a `d = 1` son "estrechamente relacionados".
- **Relajado:** $e^{-2} \approx 0.135$ → pares hasta `d ≤ 2`.

Interpretación: score > 0.368 ⇒ mutua inteligibilidad apreciable; score < 0.135 ⇒ distancia grande (requiere traducción/formación).

---

## Hallazgos y afirmaciones

- Presentan el modelo como el **primer marco escalable** para cuantificar disparidad entre cualquier par de lenguas.
- Justifican el decaimiento exponencial (frente a lineal, logarítmico o polinómico) aludiendo a analogías ecológicas (Nekola & White 1999; Dias et al. 2021), a la regularización de verbos irregulares (Lieberman et al. 2007), a la dialectometría de Levenshtein (Heeringa 2004) y a la diversidad fonémica en islas (Bromham et al. 2024).
- El antecedente Lévenshtein validado que citan es Gooskens & Heeringa (2004).

---

## Limitaciones (declaradas por los autores)

- Traducciones generadas por LLM → posibles inexactitudes, sobre todo en lenguas raras o dialectos.
- No incluye lenguas de señas.
- El árbol es **estático** (provisto por Diversity Atlas).

---

## Limitaciones no declaradas (relevantes para nuestro proyecto)

1. **El nombre de "modelo de grafos" es engañoso.** El grafo es fijo y acíclico (un árbol), así que el único algoritmo aplicable es caminos más cortos. No hay detección de comunidades, ni centralidad, ni MST: precisamente lo que un árbol no permite.
2. **La LD y la distancia de árbol nunca se combinan en una fórmula.** El score final $e^{-d}$ usa **solo** la distancia de árbol; la LD sirve para "ordenar" lenguas dentro de la familia. La "integración" que anuncian los autores no aparece en la matemática.
3. **Léxico mínimamente representado:** 4 palabras, generadas por IA, sin control de calidad nativo. Contrasta con las 40 (o 100) palabras curadas de ASJP.
4. **Sin validación contra clasificaciones establecidas.** No comparan sus scores con Glottolog, Ethnologue ni con juicios de hablantes.
5. **Contexto comercial y de revista ajena al área.** La publicación es de ciencias sociales, no de lingüística computacional; los autores son la propia empresa dueña del dataset (Diversity Atlas / Cultural Infusion). El código y datos están en GitHub (`CulturalInfusion/language-distance`) con **acceso solo bajo pedido** → reproducibilidad limitada.

---

## Relevancia para nuestro proyecto

Es el **antecedente más cercano** citado en `lecturas/estado-del-arte.md` §6.1, y ayuda a delimitar el *gap*.

### Diferencias clave frente a nuestro enfoque

| Aspecto | Chavan et al. 2025 | Nuestro trabajo propuesto |
|---|---|---|
| Léxico | 4 palabras generadas por IA (Claude) | 40 conceptos curados de ASJP (ASJPcode) |
| Estructura del grafo | Árbol estático (dado) | Grafo completo ponderado (derivado de datos) |
| Distancia | `d` = pasos en el árbol | `D(A,B)` = LDND media sobre 40 conceptos |
| Algoritmos | Solo caminos más cortos (+ DFS) | Comunidades, MST, centralidad, caminos mínimos |
| Validación | Ninguna contra clasificaciones | Comparación con Glottolog / Ethnologue |
| Reproducibilidad | Datos/código privados, acceso a pedido | ASJP abierto (CC BY 4.0) |

### Argumentos que aporta

- **Justifica el uso de Levenshtein normalizada** como medida de similitud léxica (coincide con Gooskens & Heeringa 2004, ya citado).
- **Refuerza el gap:** aun el trabajo más reciente y "de grafos" se queda en caminos más cortos sobre un árbol curado a mano; nadie explota la estructura completa del grafo de distancias léxicas.
- **Sirve de contraste metodológico:** su score descarta la información léxica en la fórmula final; el nuestro calcula la distancia directamente del léxico, sin árbol predefinido.
- **Aporta la analogía del decaimiento exponencial** (distancia decay), útil para discutir cómo la similitud decae con la distancia — aunque no la adoptemos como métrica.

> **Uso con cautela:** por sus limitaciones (LLM, 4 palabras, sin validación, contexto comercial), conviene citarlo como *punto de comparación y delimitación del gap*, no como respaldo metodológico. Para respaldo, usar Holman et al. (2008) y la validación dialectométrica (Heeringa 2004; Gooskens & Heeringa 2004).

---

## Referencias clave citadas por el paper

- Chiswick, B. R., & Miller, P. W. (2005). *Journal of Multilingual and Multicultural Development*, 26, 1–11.
- Dalby, D. (2000). *The Linguasphere Register of the World's Languages*.
- Gooskens, C., & Heeringa, W. (2004). *Language Variation and Change*, 16, 189–207.
- Heeringa, W. J. (2004). *Measuring Dialect Pronunciation Differences Using Levenshtein Distance* (tesis).
- Nekola, J. C., & White, P. S. (1999). *Journal of Biogeography*, 26, 867–878.
- Lieberman, E. et al. (2007). *Nature*, 449, 713–716.

---

## Estado de la nota

- [x] PDF obtenido (acceso abierto, CC BY 4.0)
- [x] Lectura completa
- [x] Extracción de limitaciones no declaradas
- [ ] Contrastar con implementación futura en C++

---

*Creado: 2026-10-12*
*Relacionado con: `propuesta-de-investigacion.md`, `lecturas/estado-del-arte.md`, `lecturas/chavan2025-referencia.md`, `bitacora.md`*
