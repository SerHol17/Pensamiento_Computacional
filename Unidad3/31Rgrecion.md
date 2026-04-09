# Unidad 3.3 — Modelos de Regresión en Machine Learning

La regresión es una de las herramientas fundamentales del aprendizaje supervisado. Su objetivo es modelar la relación entre una o más variables independientes (predictores) y una variable dependiente continua (respuesta), para entender esa relación y hacer predicciones sobre datos nuevos.

Antes de construir cualquier modelo, es imprescindible entender la correlación entre las variables. Esta unidad parte de ese fundamento y avanza progresivamente por todos los tipos de regresión, desde los más simples hasta los más avanzados.

---

## 1. Correlación: el paso previo obligatorio

### 1.1 Definición

La correlación mide la **fuerza y dirección** de la relación entre dos variables. Su valor va de -1 a +1:

| Valor | Significado |
|-------|-------------|
| +1 | Correlación positiva perfecta. Cuando una variable sube, la otra sube proporcionalmente. |
| 0 | No hay relación lineal. Las variables no se mueven juntas de forma predecible. |
| -1 | Correlación negativa perfecta. Cuando una sube, la otra baja proporcionalmente. |

Es importante recalcar desde el inicio: **correlación no implica causalidad**. Que dos variables se muevan juntas no significa que una cause la otra. Puede existir una tercera variable (confusora) que las afecte a ambas, o puede ser simplemente coincidencia.

### 1.2 Tipos de coeficientes de correlación

**Coeficiente de Pearson (r)**

Es el más utilizado. Mide la fuerza de la relación **lineal** entre dos variables continuas.

$$
r = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2 \cdot \sum (y_i - \bar{y})^2}}
$$

Supuestos que exige: ambas variables deben ser continuas, la relación debe ser lineal, distribución aproximadamente normal y ausencia de outliers extremos.

**Coeficiente de Spearman ($\rho$)**

Trabaja con los **rangos** de los datos en lugar de los valores originales. Mide relaciones monótonas, no necesariamente lineales.

$$
\rho = 1 - \frac{6 \sum d_i^2}{n(n^2 - 1)}
$$

donde $d_i$ es la diferencia entre los rangos de cada par de observaciones. Es la opción adecuada cuando hay outliers que distorsionarían Pearson, cuando las variables son ordinales (por ejemplo, encuestas con escala Likert) o cuando la relación es curvilínea pero monótona.

**Tau de Kendall ($\tau$)**

Más robusto que Spearman con muestras pequeñas. Se basa en contar pares concordantes y discordantes:

$$
\tau = \frac{\text{pares concordantes} - \text{pares discordantes}}{n(n-1)/2}
$$

Es preferible cuando la muestra es reducida o hay muchos empates en los rangos.

### 1.3 Guía de interpretación

| Rango del coeficiente | Fuerza | Implicación práctica |
|----------------------|--------|---------------------|
| 0.00 -- 0.10 | Despreciable | No vale la pena modelar esta relación |
| 0.10 -- 0.30 | Débil | La relación existe pero el modelo tendrá bajo poder predictivo |
| 0.30 -- 0.50 | Moderada | Puede ser útil combinada con otras variables |
| 0.50 -- 0.70 | Fuerte | Buena candidata para incluir en un modelo de regresión |
| 0.70 -- 1.00 | Muy fuerte | Excelente para regresión. Si es entre predictores, cuidado con multicolinealidad |

### 1.4 Por qué la correlación es necesaria antes de la regresión

No es un paso opcional. Cumple cinco funciones concretas:

**Validación de relación.** Si no hay correlación significativa entre X e Y, un modelo de regresión lineal será inútil. Estarías ajustando ruido, no una relación real.

**Detección de multicolinealidad.** Si dos variables predictoras tienen correlación superior a 0.80 entre sí, incluir ambas en un modelo múltiple genera inestabilidad en los coeficientes. La matriz de correlación te alerta de esto antes de construir el modelo.

**Selección de variables.** La correlación te ayuda a priorizar qué variables incluir. Variables con correlación cercana a 0 con la variable objetivo probablemente no aportarán nada al modelo.

**Dirección de la relación.** Te indica si la relación es directa (positiva) o inversa (negativa), lo cual debe coincidir con la lógica del negocio o del dominio. Si no coincide, puede haber un error en los datos o una variable confusora oculta.

**Identificación de no linealidad.** Si un scatter plot muestra una relación clara pero Pearson arroja un valor bajo, la relación probablemente no es lineal y necesitas considerar un modelo diferente: polinomial, logarítmico u otro.

### 1.5 Correlación no es causalidad

Este es el error más frecuente y peligroso en análisis de datos. Un ejemplo clásico: las ventas de helado y los ahogamientos en piscina están correlacionados positivamente. Esto no significa que el helado cause ahogamientos. Ambas variables aumentan en verano porque la temperatura (variable confusora) las afecta a las dos.

Los tipos de relación que pueden producir correlación son:

- **Causal directa:** X realmente causa Y (fumar causa cáncer de pulmón).
- **Causal inversa:** Y causa X, pero la medimos al revés.
- **Variable confusora:** una tercera variable Z causa tanto X como Y, creando una correlación espuria.
- **Coincidencia pura:** no hay relación real, solo coincidencia estadística en la muestra.

---

## 2. Fundamentos de la regresión

### 2.1 Qué es un modelo de regresión

La regresión es una técnica de aprendizaje supervisado que busca la función que mejor describe la relación entre predictores y respuesta, con el fin de hacer predicciones sobre datos no vistos.

La diferencia fundamental con la clasificación es el tipo de variable objetivo: la clasificación predice categorías (spam o no spam, aprobado o reprobado), mientras que la regresión predice valores continuos (precio, temperatura, ventas).

### 2.2 Terminología esencial

| Término | Sinónimos comunes | Descripción |
|---------|-------------------|-------------|
| Variable dependiente (Y) | respuesta, target, outcome | Lo que queremos predecir |
| Variable independiente (X) | predictor, feature, covariable, regresor | Lo que usamos para predecir |
| Coeficiente ($\beta$) | peso, parámetro | Cuánto cambia Y por cada unidad de cambio en X |
| Intercepto ($\beta_0$) | sesgo, bias | Valor de Y cuando todos los predictores son 0 |
| Residuo ($\varepsilon$) | error | Diferencia entre el valor observado y el predicho: $\varepsilon = Y_{real} - Y_{predicho}$ |
| Función de costo | función de pérdida | Función que el modelo minimiza durante el entrenamiento |

### 2.3 Supuestos de la regresión lineal clásica

Estos supuestos aplican a la regresión lineal estimada por mínimos cuadrados. Modelos como árboles o SVR no los requieren todos.

**Linealidad.** La relación entre X e Y es lineal. Se verifica con un scatter plot de X contra Y y un plot de residuos contra valores predichos. Si se viola, se pueden aplicar transformaciones o usar modelos no lineales.

**Independencia de errores.** Los residuos no están correlacionados entre sí. Se verifica con el test de Durbin-Watson (un valor cercano a 2 indica independencia). Su violación es común en series temporales.

**Homocedasticidad.** La varianza de los residuos es constante en todos los niveles de X. Se verifica graficando residuos contra valores predichos: no debe haber patrón de embudo. Si se viola, se puede aplicar una transformación logarítmica a Y o usar regresión ponderada (WLS).

**Normalidad de residuos.** Los residuos siguen una distribución normal. Se verifica con un Q-Q plot o el test de Shapiro-Wilk. Con muestras grandes (n > 30) es menos crítico gracias al Teorema Central del Límite.

**Ausencia de multicolinealidad.** Las variables independientes no deben estar altamente correlacionadas entre sí. Se mide con el VIF (Factor de Inflación de Varianza): un VIF superior a 10 es problemático. Las soluciones incluyen eliminar una de las variables correlacionadas, usar PCA o aplicar regularización.

---

## 3. Tipos de regresión

### 3.1 Regresión lineal simple

Es el modelo más básico. Modela la relación entre **una** variable independiente y **una** variable dependiente mediante una línea recta.

$$
Y = \beta_0 + \beta_1 X + \varepsilon
$$

El método de ajuste es **Mínimos Cuadrados Ordinarios (OLS)**, que busca minimizar la suma de los cuadrados de los residuos:

$$
\min \sum (y_i - \hat{y}_i)^2
$$

La idea geométrica es encontrar la línea que hace que la distancia vertical total entre los puntos reales y la línea sea lo más pequeña posible.

Como ejemplo, predecir el precio de una casa basándose únicamente en los metros cuadrados: $Y = 50000 + 1500 \cdot X$ significa que cada metro cuadrado adicional agrega 1500 dólares al precio, y el intercepto de 50000 es un precio base teórico.

Sus ventajas son la facilidad de interpretación, la rapidez de entrenamiento y que sirve como línea base sólida para comparar con modelos más complejos. Sus limitaciones son que solo captura relaciones lineales, es sensible a outliers y solo usa una variable predictora.

### 3.2 Regresión lineal múltiple

Extensión natural de la regresión simple. Usa **dos o más** variables independientes para predecir Y. La mayoría de problemas reales requieren múltiples predictores.

$$
Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \cdots + \beta_n X_n + \varepsilon
$$

Cada coeficiente $\beta_i$ representa el cambio en Y por una unidad de cambio en $X_i$, **manteniendo constantes todas las demás variables**. Esta interpretación "controlando por" es fundamental y es lo que diferencia a la regresión múltiple de ejecutar varias regresiones simples por separado.

El problema principal a vigilar es la **multicolinealidad**: cuando dos o más predictores están altamente correlacionados entre sí. Esto no afecta la calidad de las predicciones globales, pero hace que los coeficientes individuales sean inestables e ininterpretables. Se detecta calculando el VIF para cada variable (VIF mayor a 5 es preocupante, mayor a 10 es grave) y se resuelve eliminando una de las variables correlacionadas, combinándolas mediante PCA, o usando regularización.

Para la selección de variables existen varios métodos: Forward Selection (empezar sin variables, agregar una a una la que más mejore el modelo), Backward Elimination (empezar con todas, eliminar una a una la menos significativa), Stepwise (combinación de ambos) y Regularización (dejar que Lasso elimine automáticamente las irrelevantes).

### 3.3 Regresión polinomial

Cuando la relación entre X e Y no es una línea recta sino una curva, se agregan términos de potencia para capturar la curvatura.

$$
Y = \beta_0 + \beta_1 X + \beta_2 X^2 + \cdots + \beta_n X^n + \varepsilon
$$

Aunque la ecuación parece no lineal, técnicamente sigue siendo lineal **en los parámetros** (los $\beta$), por lo que se resuelve con OLS.

Para elegir el grado: un grado 2 (cuadrática) captura relaciones en forma de U o U invertida, un grado 3 (cúbica) relaciones con un punto de inflexión, y rara vez se necesita un grado superior a 4. A mayor grado, mayor riesgo de overfitting: un polinomio de grado suficientemente alto puede pasar exactamente por todos los puntos de entrenamiento, pero será desastroso prediciendo datos nuevos.

Un ejemplo práctico es la relación entre velocidad de un vehículo y consumo de combustible, que suele ser cuadrática: consumo alto a velocidades bajas (ciudad), bajo a velocidades medias y alto nuevamente a velocidades altas (carretera rápida).

Sus ventajas son la flexibilidad para capturar no linealidad y que se resuelve con OLS. Sus limitaciones son el alto riesgo de overfitting, la extrapolación peligrosa fuera del rango de datos y la dificultad de interpretación con grados altos.

### 3.4 Regresión Ridge (Regularización L2)

Modifica la regresión lineal agregando una penalización proporcional al **cuadrado** de los coeficientes. Esto evita que los coeficientes crezcan demasiado, reduciendo el overfitting.

$$
\min \sum (y_i - \hat{y}_i)^2 + \alpha \sum \beta_j^2
$$

El hiperparámetro $\alpha$ controla la fuerza de la penalización. Con $\alpha = 0$ se obtiene regresión lineal normal. A medida que $\alpha$ crece, todos los coeficientes se acercan a cero, pero **nunca llegan exactamente a cero**. Esto significa que Ridge **no elimina variables**, solo reduce su influencia. El valor óptimo de $\alpha$ se determina mediante validación cruzada.

Se debe usar cuando hay multicolinealidad entre predictores, cuando hay más variables que observaciones, cuando todas las variables son potencialmente relevantes, o como prevención general de overfitting en regresión múltiple.

Sus ventajas son que maneja bien la multicolinealidad, reduce la varianza del modelo y es numéricamente estable. Sus limitaciones son que no hace selección de variables (las mantiene todas), es menos interpretable y requiere estandarizar las variables antes de aplicar.

### 3.5 Regresión Lasso (Regularización L1)

El nombre viene de *Least Absolute Shrinkage and Selection Operator*. Es similar a Ridge pero penaliza con el **valor absoluto** de los coeficientes en lugar del cuadrado.

$$
\min \sum (y_i - \hat{y}_i)^2 + \alpha \sum |\beta_j|
$$

La diferencia crucial es que Lasso **puede hacer coeficientes exactamente cero**, eliminando variables del modelo automáticamente. Esto lo convierte en una herramienta de **selección automática de variables**.

Es ideal cuando hay muchas variables y se sospecha que solo algunas son relevantes, cuando se necesita un modelo interpretable y disperso (sparse), o en datasets de alta dimensionalidad.

Sus ventajas son la selección automática de variables, la simplicidad del modelo resultante y que funciona como regularización y selección simultánea. Su principal limitación es que con variables correlacionadas tiende a seleccionar solo una de ellas de forma arbitraria, lo cual puede generar resultados inconsistentes.

### 3.6 Regresión Elastic Net

Combina las penalizaciones de Ridge (L2) y Lasso (L1), tomando lo mejor de ambos mundos.

$$
\min \sum (y_i - \hat{y}_i)^2 + \alpha \left[ \rho \sum |\beta_j| + (1 - \rho) \sum \beta_j^2 \right]
$$

Tiene dos hiperparámetros: $\alpha$ controla la fuerza total de regularización, y $\rho$ (l1_ratio) controla la proporción entre L1 y L2. Con $\rho = 1$ equivale a Lasso, con $\rho = 0$ equivale a Ridge, y con valores intermedios se obtiene una combinación.

Es la mejor opción cuando hay muchas variables con grupos de variables correlacionadas entre sí, cuando Lasso solo es inestable por multicolinealidad, o cuando no se sabe a priori si Ridge o Lasso será mejor (se deja que la validación cruzada lo decida).

Su principal limitación es que tiene dos hiperparámetros para ajustar, lo cual es más costoso computacionalmente.

### 3.7 Regresión logística

A pesar de su nombre, la regresión logística es un modelo de **clasificación**, no de regresión. Se incluye aquí porque comparte nombre y fundamentos matemáticos con los modelos de regresión. Lo que predice es la **probabilidad** de pertenecer a una clase.

$$
\log\left(\frac{p}{1-p}\right) = \beta_0 + \beta_1 X_1 + \cdots + \beta_n X_n
$$

$$
p = \frac{1}{1 + e^{-(\beta_0 + \beta_1 X_1 + \cdots + \beta_n X_n)}}
$$

La función sigmoide transforma cualquier valor real en un número entre 0 y 1, interpretable como probabilidad. Tiene forma de S: valores muy negativos se mapean a 0 y valores muy positivos a 1.

Existen tres variantes: **binaria** (dos clases, como spam o no spam), **multinomial** (tres o más clases sin orden, como tipo de flor) y **ordinal** (tres o más clases con orden, como satisfacción baja, media y alta).

El umbral de decisión por defecto es 0.5: si $p \geq 0.5$ se asigna la clase 1, si $p < 0.5$ la clase 0. Este umbral se puede ajustar según el costo relativo de los diferentes tipos de error.

Las métricas de evaluación relevantes son accuracy, precision, recall, F1-Score, la curva ROC con su AUC y la matriz de confusión.

### 3.8 Regresión con árboles de decisión

En lugar de ajustar una ecuación, el árbol divide el espacio de datos en regiones rectangulares y asigna a cada región el **promedio** de los valores observados. Cada nodo interno representa una pregunta del tipo "es $X_j > c$?".

El proceso de construcción es: evaluar todas las variables y todos los puntos de corte posibles, elegir la combinación que minimiza el MSE en los subgrupos resultantes, repetir recursivamente en cada subgrupo, y detener según criterios de profundidad máxima o mínimo de muestras.

Los hiperparámetros clave son `max_depth` (profundidad máxima del árbol, que controla la complejidad), `min_samples_split` (mínimo de muestras para dividir un nodo) y `min_samples_leaf` (mínimo de muestras en cada hoja).

Sus ventajas son que no requiere supuestos sobre la distribución, captura relaciones no lineales e interacciones automáticamente, es fácil de interpretar y visualizar, y no requiere escalar las variables. Sus limitaciones son el alto riesgo de overfitting sin poda, la inestabilidad (pequeños cambios en los datos pueden cambiar todo el árbol), las predicciones escalonadas (no suaves) y la mala extrapolación fuera del rango de entrenamiento.

### 3.9 Random Forest para regresión

Es un **ensemble** de múltiples árboles de decisión. Cada árbol se entrena con una muestra diferente de los datos (bootstrap) y con un subconjunto aleatorio de variables en cada división. La predicción final es el **promedio** de las predicciones de todos los árboles.

Dos conceptos clave lo sustentan. El **bagging** (Bootstrap Aggregating) consiste en que cada árbol se entrena con una muestra aleatoria con reemplazo del dataset original. La **aleatoriedad de features** significa que en cada división solo se considera un subconjunto de variables (típicamente $p/3$ para regresión). Al promediar muchos árboles diversos, se reduce la varianza sin aumentar significativamente el sesgo. Los errores individuales tienden a cancelarse.

Los hiperparámetros principales son `n_estimators` (número de árboles, típicamente entre 100 y 500), `max_features` (número de variables por split) y `max_depth` (profundidad de cada árbol).

Sus ventajas son el buen rendimiento con poca configuración, la resistencia al overfitting comparado con un solo árbol, que proporciona importancia de variables y que no requiere escalar datos. Sus limitaciones son la menor interpretabilidad, el costo computacional y que no extrapola fuera del rango de datos de entrenamiento.

### 3.10 Gradient Boosting para regresión

Construye árboles de forma **secuencial**. Cada nuevo árbol se enfoca en corregir los errores (residuos) del árbol anterior. Es consistentemente uno de los algoritmos más poderosos para datos tabulares.

El proceso es: comenzar con una predicción simple (por ejemplo, la media de Y), calcular los residuos, entrenar un árbol pequeño para predecir esos residuos, actualizar las predicciones sumando el nuevo árbol multiplicado por el learning rate, y repetir.

Las tres implementaciones más populares son:

**XGBoost.** Incorpora regularización L1/L2, manejo de valores faltantes y paralelización. Es muy rápido y eficiente en memoria.

**LightGBM.** Usa crecimiento por hojas (leaf-wise) e histogramas para los splits. Es más rápido que XGBoost con datasets grandes.

**CatBoost.** Tiene manejo nativo de variables categóricas y ordered boosting. Produce menos overfitting y funciona bien sin mucha configuración.

Los hiperparámetros clave son `n_estimators` (número de árboles), `learning_rate` (valores bajos como 0.01 a 0.1 requieren más árboles pero generalizan mejor), `max_depth` (usualmente entre 3 y 8, mucho menor que en Random Forest) y `subsample` (fracción de datos para entrenar cada árbol).

Sus ventajas son el rendimiento superior en datos tabulares, la capacidad para relaciones complejas, la importancia de variables y las implementaciones maduras. Sus limitaciones son la mayor propensión al overfitting comparado con Random Forest, la sensibilidad a hiperparámetros, que el entrenamiento es secuencial y la menor interpretabilidad.

### 3.11 Support Vector Regression (SVR)

Adaptación de las máquinas de vectores de soporte para regresión. En lugar de buscar un hiperplano que separe clases, busca un **"tubo"** (epsilon-tube) de ancho $\varepsilon$ que contenga la mayor cantidad de datos posible.

Los puntos que caen fuera o en el borde del tubo se llaman **vectores de soporte** y son los únicos que definen el modelo. Esto hace a SVR robusto frente a outliers: los puntos dentro del tubo no influyen en absoluto.

Los kernels disponibles son: lineal (para relaciones lineales), RBF o Radial Basis Function (para relaciones no lineales, el más usado) y polinomial (para relaciones polinomiales de grado específico).

Los hiperparámetros son: $C$ (trade-off entre ajuste y complejidad; un C alto implica ajuste más estricto), $\varepsilon$ (ancho del tubo, define la tolerancia al error) y $\gamma$ (para kernel RBF, controla el alcance de influencia de cada punto de soporte).

Sus ventajas son la efectividad en espacios de alta dimensión, la robustez a outliers, el buen funcionamiento con pocas muestras y la flexibilidad de los kernels. Sus limitaciones son la lentitud con datasets grandes ($O(n^2)$ a $O(n^3)$), la necesidad de escalar las variables, la dificultad de interpretación y la sensibilidad a la elección de kernel e hiperparámetros.

### 3.12 K-Nearest Neighbors para regresión (KNN)

Modelo no paramétrico que predice el valor de un nuevo punto como el **promedio** de los K vecinos más cercanos en el espacio de características. No "aprende" una función explícita: memoriza todos los datos y busca en el momento de predicción.

El proceso es: calcular la distancia del nuevo punto a todos los puntos de entrenamiento, seleccionar los K más cercanos, y promediar sus valores de Y (o usar un promedio ponderado por distancia).

Las métricas de distancia más comunes son: euclidiana ($\sqrt{\sum(x_i - y_i)^2}$, la más usada para variables continuas), Manhattan ($\sum|x_i - y_i|$, más robusta a outliers) y Minkowski (generalización paramétrica de las dos anteriores).

El hiperparámetro principal es $K$: un K pequeño produce un modelo flexible pero ruidoso, mientras que un K grande produce un modelo suave pero puede perder patrones locales. Se elige por validación cruzada, probando valores impares para evitar empates. Los pesos pueden ser uniformes (todos los vecinos pesan igual) o por distancia (vecinos más cercanos pesan más).

Sus ventajas son la simplicidad, la ausencia de supuestos distribucionales y la adaptación natural a relaciones no lineales. Sus limitaciones son la lentitud en predicción con datasets grandes, la alta sensibilidad a la escala de las variables (es obligatorio estandarizar), la degradación del rendimiento con muchas variables (maldición de la dimensionalidad) y que no produce un modelo explícito.

### 3.13 Regresión bayesiana

En lugar de estimar un único valor para cada coeficiente, la regresión bayesiana estima una **distribución de probabilidad** para cada uno. Incorpora conocimiento previo (prior) y lo actualiza con los datos observados para obtener la distribución posterior.

$$
P(\beta | \text{datos}) = \frac{P(\text{datos} | \beta) \cdot P(\beta)}{P(\text{datos})}
$$

El **prior** representa lo que creemos sobre los coeficientes antes de ver los datos (puede ser informativo, basado en conocimiento del dominio, o no informativo). El **likelihood** es la probabilidad de observar los datos dado un conjunto de coeficientes. El **posterior** es la distribución actualizada que combina prior y likelihood.

Sus ventajas son que cuantifica la incertidumbre en las predicciones, incorpora conocimiento previo del dominio, funciona bien con pocos datos y tiene una regularización natural a través del prior. Sus limitaciones son el costo computacional (MCMC, inferencia variacional), la necesidad de definir priors (lo cual puede ser subjetivo) y la mayor complejidad de implementación.

### 3.14 Regresión cuantílica

En lugar de predecir la **media** de Y (como hace OLS), predice un **cuantil** específico: la mediana, el percentil 25, el percentil 90, o cualquier otro.

$$
\min \sum \rho_\tau (y_i - \hat{y}_i)
$$

donde $\rho_\tau$ es la función check, que penaliza de forma asimétrica los errores positivos y negativos según el cuantil $\tau$ deseado.

Sus casos de uso principales son: predecir el peor escenario (percentil 95) de tiempos de entrega, estimar intervalos de predicción sin asumir normalidad, análisis de riesgo financiero (Value at Risk), y situaciones donde la dispersión de Y cambia con X (heterocedasticidad).

Sus ventajas son que no asume distribución normal de los errores, es robusta a outliers (especialmente la mediana) y proporciona una vista completa de la distribución condicional. Sus limitaciones son el mayor costo computacional y la necesidad de más datos para estimaciones estables en cuantiles extremos.

### 3.15 Regresión isotónica

Ajusta una función escalonada **monótona** (no decreciente o no creciente) a los datos. No asume ninguna forma paramétrica, solo que la relación tiene una dirección consistente.

$$
\hat{y}_1 \leq \hat{y}_2 \leq \cdots \leq \hat{y}_n \quad \text{(si es no decreciente)}
$$

Se resuelve con el algoritmo PAVA (Pool Adjacent Violators Algorithm). Sus casos de uso son la calibración de probabilidades en clasificadores, curvas dosis-respuesta en medicina y relaciones donde solo se conoce la dirección pero no la forma funcional.

Sus ventajas son que es no paramétrica y respeta la restricción de monotonía. Sus limitaciones son que solo funciona con una variable predictora, produce predicciones escalonadas y requiere que la relación sea efectivamente monótona.

---

## 4. Métricas de evaluación

Elegir la métrica correcta es tan importante como elegir el modelo. Cada una captura un aspecto diferente del rendimiento.

### R-cuadrado (Coeficiente de Determinación)

$$
R^2 = 1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2}
$$

Indica la proporción de la varianza de Y explicada por el modelo. Un $R^2 = 0.75$ significa que el modelo explica el 75% de la variabilidad. Su rango teórico es de 0 a 1, aunque puede ser negativo si el modelo es peor que simplemente predecir la media. Su limitación es que siempre aumenta al agregar variables, aunque sean irrelevantes.

### R-cuadrado ajustado

$$
R^2_{adj} = 1 - \frac{(1 - R^2)(n - 1)}{n - p - 1}
$$

Penaliza por el número de variables incluidas. Solo aumenta si la nueva variable mejora el modelo más de lo esperado por azar. Es la métrica adecuada para comparar modelos con diferente número de variables.

### MSE (Error Cuadrático Medio)

$$
MSE = \frac{1}{n} \sum (y_i - \hat{y}_i)^2
$$

Promedio de los errores al cuadrado. Penaliza desproporcionadamente los errores grandes. Su unidad es el cuadrado de la unidad de Y, lo cual dificulta la interpretación directa.

### RMSE (Raíz del Error Cuadrático Medio)

$$
RMSE = \sqrt{MSE}
$$

Está en la misma escala que Y, lo que facilita la interpretación. Si RMSE es 5000 y Y son precios en dólares, el modelo se equivoca en promedio unos 5000 dólares. Es la métrica más utilizada en la práctica.

### MAE (Error Absoluto Medio)

$$
MAE = \frac{1}{n} \sum |y_i - \hat{y}_i|
$$

Promedio de los errores en valor absoluto. Es menos sensible a outliers que RMSE. Una comparación útil: si RMSE es mucho mayor que MAE, existen algunos errores muy grandes. Si son similares, los errores son uniformes.

### MAPE (Error Porcentual Absoluto Medio)

$$
MAPE = \frac{100}{n} \sum \frac{|y_i - \hat{y}_i|}{|y_i|}
$$

Expresa el error como porcentaje, lo cual facilita la comunicación con audiencias no técnicas. Su limitación es que no funciona cuando $y_i = 0$ y tiene sesgo hacia valores pequeños de Y.

---

## 5. El trade-off sesgo-varianza

Este es un concepto central en Machine Learning que explica por qué un modelo puede funcionar bien en entrenamiento pero mal en datos nuevos.

### Los componentes del error

**Sesgo** es el error causado por simplificaciones en el modelo. Un modelo con alto sesgo no captura la complejidad real de los datos (underfitting). Ejemplo: ajustar una línea recta a datos que siguen una parábola.

**Varianza** es la sensibilidad del modelo a fluctuaciones en los datos de entrenamiento. Un modelo con alta varianza se ajusta demasiado al ruido (overfitting). Ejemplo: un polinomio de grado 20 que pasa exactamente por todos los puntos de entrenamiento.

**Error irreducible** es el ruido inherente en los datos que ningún modelo puede capturar, porque Y depende de factores que no están en nuestros predictores.

$$
\text{Error total} = \text{Sesgo}^2 + \text{Varianza} + \text{Error irreducible}
$$

### El espectro de modelos

Los modelos de **alto sesgo y baja varianza** son simples y estables pero pueden no capturar la realidad. Ejemplos: regresión lineal, KNN con K grande.

Los modelos de **bajo sesgo y alta varianza** son complejos y se ajustan bien al entrenamiento pero fallan con datos nuevos. Ejemplos: árboles profundos sin poda, KNN con K=1, polinomios de alto grado.

Los modelos con **balance óptimo** buscan el punto donde el error total es mínimo. Ejemplos: Random Forest, Gradient Boosting con tuning adecuado, Ridge y Lasso.

---

## 6. Guía de selección de modelo

| Escenario | Modelo recomendado | Razón |
|-----------|-------------------|-------|
| Primer modelo (línea base) | Regresión lineal | Rápido, interpretable, establece un benchmark |
| Relación no lineal | Polinomial o árboles | Capturan curvaturas que la lineal no puede |
| Muchas variables con multicolinealidad | Ridge o Elastic Net | Estabilizan coeficientes y reducen overfitting |
| Muchas variables, pocas relevantes | Lasso o Elastic Net | Selección automática de variables |
| Máximo rendimiento predictivo | Gradient Boosting (XGBoost, LightGBM) | Generalmente el mejor en datos tabulares |
| Robustez sin mucho tuning | Random Forest | Buen rendimiento con configuración por defecto |
| Dataset pequeño con muchas variables | SVR o Regresión bayesiana | SVR funciona con pocas muestras; bayesiana incorpora priors |
| Necesidad de cuantificar incertidumbre | Bayesiana o cuantílica | Dan distribuciones o intervalos, no solo puntos |
| Clasificación binaria | Regresión logística | Simple, interpretable, probabilidades como salida |

### Flujo de trabajo recomendado

1. Análisis exploratorio y correlación.
2. Empezar con regresión lineal como baseline.
3. Probar regularización si hay multicolinealidad o muchas variables.
4. Probar modelos de ensemble si la relación parece no lineal.
5. Comparar modelos con validación cruzada.
6. Ajustar hiperparámetros del mejor candidato.
7. Evaluar en conjunto de prueba final (una sola vez).

---

## 7. Validación y buenas prácticas

### División de datos

El esquema clásico es destinar entre el 70% y el 80% a entrenamiento y el 20% a 30% restante a prueba. Cuando se necesita un conjunto intermedio para ajustar hiperparámetros, se usa un esquema de tres partes: 60-70% entrenamiento, 15-20% validación y 15-20% prueba.

Una regla fundamental: el conjunto de prueba se usa **una sola vez** al final. Si se usa repetidamente para ajustar el modelo, deja de ser prueba y se convierte en validación, lo cual contamina la estimación del rendimiento real.

### Validación cruzada (Cross-Validation)

El método más común es **K-Fold**: dividir los datos en K partes, entrenar con K-1 partes, validar con la restante, rotar K veces y promediar los resultados. Los valores típicos de K son 5 o 10. La variante **Stratified K-Fold** mantiene la distribución de Y en cada fold. **Leave-One-Out** es el caso extremo donde K es igual al número de observaciones: cada dato es validación una vez. Es muy costoso y solo práctico con datasets pequeños.

### Preprocesamiento esencial

**Manejo de valores faltantes.** Las opciones incluyen imputación por media o mediana, KNN Imputer (imputa basándose en los vecinos más cercanos) o eliminación de filas o columnas si la proporción de datos faltantes es baja.

**Escalamiento de variables.** Es obligatorio para SVR, KNN, Ridge y Lasso. No es necesario para modelos basados en árboles. Las opciones son: StandardScaler (centra en media 0 con desviación estándar 1), MinMaxScaler (escala al rango 0 a 1) y RobustScaler (usa mediana y rango intercuartílico, robusto a outliers).

**Encoding de variables categóricas.** One-Hot Encoding crea una columna binaria por cada categoría. Label Encoding asigna un número a cada categoría (solo apropiado para variables ordinales). Target Encoding reemplaza cada categoría por la media de Y en esa categoría.

**Detección de outliers.** El método IQR marca como outlier todo valor fuera del rango $[Q1 - 1.5 \cdot IQR, \; Q3 + 1.5 \cdot IQR]$. El Z-score marca valores con $|z| > 3$. Para detección multivariada se puede usar Isolation Forest.

### Diagnóstico de overfitting y underfitting

**Overfitting** se manifiesta como un error de entrenamiento muy bajo pero un error de prueba alto. Las soluciones son: conseguir más datos, aplicar regularización (Ridge, Lasso), reducir la complejidad del modelo, usar early stopping (en Gradient Boosting) o dropout (en redes neuronales).

**Underfitting** se manifiesta como un error alto tanto en entrenamiento como en prueba. Las soluciones son: usar un modelo más complejo, agregar más variables o hacer feature engineering, o reducir la regularización.