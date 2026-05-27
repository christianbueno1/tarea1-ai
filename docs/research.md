Grupo # 11: 

Topico a explorar:
Aplicaciones de IA o AA en sistemas de recomendación personalizados para plataformas de entretenimiento digital, basados en algoritmos híbridos de filtrado colaborativo y contenido.


## Algoritmo Hibrido de filtrado colaborativo y basado en contenido

Son motores de recomendación que combinan el filtrado colaborativo (que analiza el comportamiento y preferencias de usuarios similares) con el filtrado basado en contenido (que analiza las características de los productos o películas). Su objetivo es superar los fallos que tienen estos métodos por separado, ofreciendo sugerencias mucho más precisas.

## Componentes principales del sistema híbrido 

- Filtrado colaborativo (CF) (Collaborative Filtering (CF)): Analiza los hábitos del usuario. Encuentra patrones en los historiales de visualización, calificaciones o clics. Agrupa a usuarios similares. Si el usuario A y el usuario B comparten gustos, el sistema recomienda los programas favoritos del usuario A al usuario B. /cite{DataMListic-collaborative-filtering}

- Filtrado basado en contenido (CB) (Content-Based Filtering (CB)): Analiza las características de los elementos. Evalúa el texto, los géneros, los directores, los actores o las frecuencias de audio. Si ves una película de ciencia ficción protagonizada por un actor específico, el sistema sugiere otras películas de ciencia ficción con ese mismo actor. /cite{DataMListic-content-based-recommendation}

## Collaborative Filtering (CF) 
Metodos matemáticos para calcular la similaridad entre usuarios o items.
- Coeficiente de similitud del coseno (Cosine Similarity): Mide el ángulo entre los vectores de calificación de dos usuarios.
- Correlación de Pearson (Pearson Correlation): Mide la relación lineal, ajustada para corregir el sesgo de calificación.
- Distancia euclidiana (Euclidean Distance): Distancia geométrica en el espacio de calificaciones.

## Cosine Similarity in Collaborative Filtering

**Cosine Similarity** measures how similar two users (or two items) are by comparing the **angle between their rating vectors**.

Instead of focusing on rating averages like Pearson Correlation, cosine similarity focuses on the **direction of preferences**.

---

# Core Idea

Imagine each user's ratings as a vector in multidimensional space.

Example:

* User A → ( [5,4,1,2] )
* User B → ( [4,3,2,1] )

If the vectors point in similar directions, the users have similar tastes.

---

# Formula

\cos(\theta)=\frac{A\cdot B}{|A||B|}

Where:

* (A \cdot B) = dot product of the vectors
* (|A|) = magnitude (length) of vector A
* (|B|) = magnitude of vector B

---

# Expanded Formula

\cos(\theta)=\frac{\sum_{i=1}^{n}A_iB_i}{\sqrt{\sum_{i=1}^{n}A_i^2}\sqrt{\sum_{i=1}^{n}B_i^2}}

---

# Output Range

| Value | Meaning            |
| ----- | ------------------ |
| 1     | Perfectly similar  |
| 0     | No similarity      |
| -1    | Opposite direction |

In recommendation systems with positive ratings only, values are usually between:

[
0 \rightarrow 1
]

---

# Example

Suppose:

| Movie    | User A | User B |
| -------- | ------ | ------ |
| Avengers | 5      | 4      |
| Batman   | 4      | 3      |
| Titanic  | 1      | 2      |
| Joker    | 2      | 1      |

Vectors:

[
A=[5,4,1,2]
]

[
B=[4,3,2,1]
]

---

# Step 1 — Dot Product

Multiply corresponding values and sum them:

A\cdot B=(5\times4)+(4\times3)+(1\times2)+(2\times1)=36

---

# Step 2 — Compute Magnitudes

Magnitude of A:

|A|=\sqrt{5^2+4^2+1^2+2^2}=\sqrt{46}

Magnitude of B:

|B|=\sqrt{4^2+3^2+2^2+1^2}=\sqrt{30}

---

# Step 3 — Final Similarity

\cos(\theta)=\frac{36}{\sqrt{46}\sqrt{30}}\approx0.97

Result:

* Very high similarity
* Their rating patterns point in nearly the same direction

---

# Geometric Interpretation

Cosine similarity measures the angle between vectors:

| Angle | Similarity |
| ----- | ---------- |
| 0°    | 1          |
| 90°   | 0          |
| 180°  | -1         |

Smaller angle → more similar users.

---

# Why Cosine Similarity Is Popular

## 1. Simple and Fast

Efficient for large sparse matrices.

Very common in:

* recommendation systems
* search engines
* NLP
* embeddings
* semantic search

---

## 2. Works Well with Sparse Data

Users usually rate only a few items.

Cosine similarity can compare sparse vectors efficiently.

---

## 3. Magnitude Independence

It focuses more on orientation than absolute size.

Example:

| User A | [5,5,5] |
| ------ | ------- |
| User B | [1,1,1] |

Cosine similarity:

[
=1
]

because the direction is identical.

---

# Difference from Pearson Correlation

| Cosine Similarity         | Pearson Correlation             |
| ------------------------- | ------------------------------- |
| Uses raw ratings          | Centers ratings around averages |
| Measures vector angle     | Measures linear relationship    |
| Does not remove user bias | Removes rating bias             |
| Simpler computation       | More statistically robust       |

---

# Important Limitation

Cosine similarity may incorrectly treat these users as identical:

| User A | Always rates 5 |
| User B | Always rates 1 |

because their vectors point in the same direction.

Pearson correlation fixes this by subtracting the mean.

---

# In Collaborative Filtering

Cosine similarity is used in:

## User-Based Filtering

Find users with similar rating vectors.

---

## Item-Based Filtering

Find items frequently liked together.

Example:

* Users who liked “Interstellar”
* also liked “Inception”

---

# Modern ML Usage

Cosine similarity is extremely important in modern AI systems:

* vector databases
* embeddings
* transformers
* semantic search
* retrieval-augmented generation (RAG)

because embeddings are high-dimensional vectors and cosine similarity efficiently measures semantic closeness.

## Pearson Correlation in Collaborative Filtering

In **Collaborative Filtering**, the goal is to recommend items to a user based on the preferences of other similar users.

The **Pearson Correlation Coefficient** is a statistical measure used to determine **how similarly two users rate items**, while correcting for differences in rating habits.

For example:

* User A tends to give high scores to everything.
* User B tends to give lower scores overall.

Even if their raw ratings are different, Pearson checks whether they like and dislike the **same kinds of items** relative to their own averages.

---

# Intuition

Pearson answers this question:

> “When User A rates something above their normal average, does User B also rate it above their average?”

If yes → strong positive similarity.

---

# Formula

r=\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i-\bar{x})^2}\sqrt{\sum_{i=1}^{n}(y_i-\bar{y})^2}}

Where:

* (x_i) = ratings from User X
* (y_i) = ratings from User Y
* (\bar{x}) = average rating of User X
* (\bar{y}) = average rating of User Y

---

# Output Range

Pearson correlation returns a value between:

| Value | Meaning                     |
| ----- | --------------------------- |
| +1    | Perfect positive similarity |
| 0     | No relationship             |
| -1    | Opposite preferences        |

---

# Example

Suppose two users rated the same movies:

| Movie    | User A | User B |
| -------- | ------ | ------ |
| Avengers | 5      | 4      |
| Batman   | 4      | 3      |
| Titanic  | 1      | 2      |
| Joker    | 2      | 1      |

---

## Step 1 — Compute Average Ratings

User A average:

\bar{x}=\frac{5+4+1+2}{4}=3

User B average:

\bar{y}=\frac{4+3+2+1}{4}=2.5

---

## Step 2 — Center Ratings Around Their Mean

| Movie    | A | A−Avg | B | B−Avg |
| -------- | - | ----- | - | ----- |
| Avengers | 5 | 2     | 4 | 1.5   |
| Batman   | 4 | 1     | 3 | 0.5   |
| Titanic  | 1 | -2    | 2 | -0.5  |
| Joker    | 2 | -1    | 1 | -1.5  |

Notice:

* Both users rate superhero movies above average.
* Both rate darker/dramatic movies below average.

This indicates similar taste.

---

## Step 3 — Compute Correlation

The numerator measures whether deviations move together:

[
(2)(1.5)+(1)(0.5)+(-2)(-0.5)+(-1)(-1.5)
]

Positive products indicate aligned preferences.

The final result is close to:

r\approx0.83

This means:

* Strong positive similarity
* The users likely enjoy similar content

---

# Why Pearson Is Useful

## 1. Handles Rating Bias

Some users always rate high:

| User  | Typical Ratings |
| ----- | --------------- |
| Alice | 4–5             |
| Bob   | 2–3             |

Pearson removes this bias by centering ratings around each user’s average.

---

## 2. Focuses on Preference Patterns

It does not care only about absolute values.

Instead, it checks:

* what users like more than usual
* what users dislike more than usual

---

# In Recommendation Systems

If User A is similar to User B:

* and User B liked a movie that User A has not seen,
* recommend that movie to User A.

This is called:

* **User-Based Collaborative Filtering**

---

# Comparison with Cosine Similarity

| Pearson                            | Cosine                      |
| ---------------------------------- | --------------------------- |
| Removes user bias                  | Uses raw ratings            |
| Focuses on rating trends           | Focuses on vector direction |
| Better when users rate differently | Simpler and faster          |

Example:

* Generous user vs strict user
* Pearson usually works better.

---

# Weaknesses

## Sparse Data Problem

If two users only rated 1 or 2 common items, similarity may be unreliable.

---

## Cold Start Problem

New users have few ratings, so correlation cannot be computed well.

---

# Real-World Usage

Pearson correlation has historically been used in systems like:

* movie recommendation engines
* music recommendation platforms
* e-commerce suggestions
* streaming services

Modern systems often combine it with:

* matrix factorization
* embeddings
* neural recommenders

because those scale better for millions of users/items.

# Euclidean Distance in Collaborative Filtering

**Euclidean Distance** measures how far apart two users (or items) are in rating space.

It is the ordinary geometric distance you already know from 2D or 3D geometry, generalized to many dimensions.

In recommendation systems:

* smaller distance → more similar users
* larger distance → less similar users

---

# Core Idea

Imagine each user as a point in space.

Example:

| Movie    | User A | User B |
| -------- | ------ | ------ |
| Avengers | 5      | 4      |
| Batman   | 4      | 3      |
| Titanic  | 1      | 2      |
| Joker    | 2      | 1      |

Vectors:

[
A=[5,4,1,2]
]

[
B=[4,3,2,1]
]

Euclidean distance measures the straight-line distance between these two points.

---

# Formula

d(A,B)=\sqrt{\sum_{i=1}^{n}(A_i-B_i)^2}

Where:

* (A_i) = rating from User A
* (B_i) = rating from User B
* (n) = number of common items

---

# Step-by-Step Example

Using:

[
A=[5,4,1,2]
]

[
B=[4,3,2,1]
]

---

## Step 1 — Compute Differences

| Movie    | A | B | A−B |
| -------- | - | - | --- |
| Avengers | 5 | 4 | 1   |
| Batman   | 4 | 3 | 1   |
| Titanic  | 1 | 2 | -1  |
| Joker    | 2 | 1 | 1   |

---

## Step 2 — Square Differences

1^2+1^2+(-1)^2+1^2=4

---

## Step 3 — Square Root

d(A,B)=\sqrt{4}=2

The Euclidean distance is:

[
2
]

---

# Interpretation

| Distance | Meaning               |
| -------- | --------------------- |
| 0        | Identical ratings     |
| Small    | Similar users         |
| Large    | Different preferences |

Unlike cosine similarity or Pearson correlation:

* Euclidean distance is a **distance metric**
* not a similarity score

---

# Converting Distance into Similarity

Recommendation systems often convert distance into similarity:

similarity=\frac{1}{1+d(A,B)}

This produces values between:

[
0 \rightarrow 1
]

Example:

[
\frac{1}{1+2}=0.33
]

---

# Geometric Interpretation

In 2D:

[
distance = \sqrt{(x_2-x_1)^2+(y_2-y_1)^2}
]

Euclidean distance extends this concept into higher dimensions.

If users rate 100 movies:

* each movie becomes one dimension
* users become points in 100-dimensional space

---

# Advantages

## 1. Easy to Understand

Very intuitive mathematically.

---

## 2. Simple Computation

Efficient for small systems.

---

## 3. Captures Absolute Differences

If users rate very differently, the distance increases clearly.

---

# Weaknesses

## 1. Sensitive to Rating Scale

Example:

| Movie      | User A | User B |
| ---------- | ------ | ------ |
| All movies | 5      | 1      |

Even if both users like the same things relatively:

* Euclidean distance becomes large
* because raw values differ

Pearson handles this better.

---

## 2. Sparse Data Problems

Most users rate only a few items.

Missing values make distance calculations difficult.

---

## 3. High-Dimensional Problems

In very large spaces:

* distances become less meaningful
* many points appear similarly far apart

This is called the:

* **Curse of Dimensionality**

---

# Comparison with Pearson and Cosine

| Method              | Measures                     | Handles Rating Bias? |
| ------------------- | ---------------------------- | -------------------- |
| Euclidean Distance  | Absolute geometric distance  | No                   |
| Cosine Similarity   | Angle between vectors        | No                   |
| Pearson Correlation | Relationship after centering | Yes                  |

---

# Practical Recommendation

## Use Euclidean Distance when:

* data is normalized
* rating scales are consistent
* system is small/simple

---

## Use Pearson when:

* users have different rating habits
* bias correction matters

---

## Use Cosine when:

* working with sparse vectors
* embeddings or large-scale vector systems

---

# Real-World Usage

Euclidean distance appears in:

* K-Nearest Neighbors (KNN)
* clustering algorithms
* anomaly detection
* recommendation prototypes
* computer vision
* machine learning feature spaces

However, in modern large-scale recommenders:

* cosine similarity
* matrix factorization
* neural embeddings

are generally preferred because they scale better and work better with sparse high-dimensional data.



## Core Technical Challenges
- The Cold-Start Problem: New users and newly released digital content lack historical interaction data, severely limiting the effectiveness of both collaborative and content-based approaches.
- Data Sparsity: On platforms with vast entertainment catalogs, the ratio of user interactions to total available content is extremely low, making it difficult to find reliable similarities between users or items.
- Scalability and Real-Time Processing: Digital entertainment platforms require the continuous streaming and processing of user telemetry. Calculating recommendations for millions of active users in real-time strains computational resources.



El **problema del inicio en frío** (*cold-start problem*) es uno de los desafíos más críticos en los sistemas de recomendación, ya que ocurre cuando no existe suficiente información histórica para realizar predicciones precisas. \cite{DBLP:journals/corr/abs-2202-08677} \cite{Wang_Xu_Li_Li_2025}

Según las fuentes, este problema se manifiesta de dos formas principales y se aborda mediante estrategias híbridas:

### 1. El impacto en los algoritmos tradicionales
*   **Filtrado Colaborativo (CF):** Estos sistemas dependen casi exclusivamente de las interacciones previas (como calificaciones o historial de compras) \cite{DBLP:journals/corr/abs-2202-08677}. Cuando un usuario es nuevo o un contenido acaba de ser publicado, no hay datos en la "matriz de calificaciones", lo que impide que el algoritmo encuentre usuarios similares o patrones de consumo. \cite{DBLP:journals/corr/abs-2202-08677}
*   **Limitaciones del filtrado basado en contenido:** Aunque estos sistemas se centran en las propiedades de los ítems, pueden ser limitados si no logran extraer relaciones profundas entre las características de los productos.\cite{DBLP:journals/corr/abs-2202-08677}

### 2. Soluciones propuestas en las fuentes
Las fuentes coinciden en que los **enfoques híbridos** son la mejor forma de superar estas limitaciones, combinando las ventajas del filtrado colaborativo y el basado en contenido.\cite{Wang_Xu_Li_Li_2025}

*   **Uso de Incrustaciones (Embeddings) de Características:** Una solución innovadora consiste en utilizar **Word2Vec** para procesar las características de los ítems como si fueran palabras.\cite{DBLP:journals/corr/abs-2202-08677} Esto permite calcular la similitud entre productos (método **RELFsim**) basándose en sus atributos inherentes, lo que permite recomendar contenido nuevo incluso si nadie lo ha calificado aún.\cite{DBLP:journals/corr/abs-2202-08677}
*   **Estrategias Híbridas Ponderadas y en Cascada:** Se han desarrollado métodos que integran el filtrado de contenido para extraer vectores de características y el filtrado colaborativo para calcular la similitud entre usuarios.\cite{Wang_Xu_Li_Li_2025} Al combinar ambos mediante una estrategia ponderada, se logra aliviar tanto el inicio en frío del **usuario** como el del **ítem**.\cite{Wang_Xu_Li_Li_2025}
*   **Agrupamiento (Clustering):** El uso de análisis de conglomerados para modelar el comportamiento histórico ayuda a estructurar los datos disponibles y mejorar la precisión cuando la información es escasa.\cite{Wang_Xu_Li_Li_2025}

En conclusión, la integración de técnicas de procesamiento de lenguaje natural (como los *feature embeddings*) dentro de sistemas híbridos permite que el sistema "entienda" el contenido nuevo sin depender de la interacción social previa, resolviendo así el vacío de información del inicio en frío.\cite{Wang_Xu_Li_Li_2025} \cite{DBLP:journals/corr/abs-2202-08677}

---
El método **RELFsim** es un algoritmo de recomendación basado en contenido que utiliza técnicas de procesamiento de lenguaje natural para determinar la similitud entre productos basándose en sus características \cite{DBLP:journals/corr/abs-2005-08148}.

Su funcionamiento se desglosa en los siguientes pasos técnicos:

1.  **Tratamiento de características como palabras:** A diferencia del uso convencional de **Word2Vec** (que analiza palabras en oraciones), este método toma las **características de los ítems** (como género, actores o etiquetas) y las trata como si fueran **palabras dentro de una oración** \cite{DBLP:journals/corr/abs-2005-08148}.
2.  **Generación de incrustaciones (Embeddings):** Al procesar estas "oraciones" de características, el sistema genera **incrustaciones de características neuronales** (*neural feature embeddings*) \cite{DBLP:journals/corr/abs-2005-08148}. Estas incrustaciones son vectores matemáticos que capturan las **relaciones implícitas** entre las distintas propiedades de los productos \cite{DBLP:journals/corr/abs-2005-08148}.
3.  **Cálculo de relación entre características:** Gracias a estos vectores, el sistema puede calcular qué tan relacionadas están las características entre sí, y no solo si son idénticas \cite{DBLP:journals/corr/abs-2005-08148}.
4.  **Representación vectorial de los ítems:** Utilizando estas relaciones de características previamente extraídas, el método determina una **representación vectorial para cada ítem** \cite{DBLP:journals/corr/abs-2005-08148}.
5.  **Cálculo de similitud (RELFsim):** Finalmente, la similitud entre dos ítems se calcula comparando sus representaciones vectoriales resultantes. A este proceso específico de calcular la similitud entre ítems basándose en la relación de sus características se le denomina **RELFsim** \cite{DBLP:journals/corr/abs-2005-08148}.

**Propósito y Beneficios:**
*   **Precisión:** Permite predecir las preferencias de un usuario por un conjunto de ítems con una efectividad comparable al filtrado colaborativo puro \cite{DBLP:journals/corr/abs-2005-08148}.
*   **Solución al Inicio en Frío:** Este algoritmo se integra a menudo dentro de sistemas de filtrado colaborativo basados en ítems para mejorar la precisión y resolver el problema del **inicio en frío** (*cold-start*) \cite{DBLP:journals/corr/abs-2005-08148}. Al utilizar vectores de características, permite recomendar contenidos nuevos de los que no se tienen calificaciones previas, superando las limitaciones de los enfoques que dependen únicamente de la interacción histórica \cite{DBLP:journals/corr/abs-2005-08148, Wang_Xu_Li_Li_2025}.