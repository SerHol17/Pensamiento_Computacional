## 4.1 Ingeniería de Instrucciones (Prompting)

Interactuar con modelos de lenguaje de gran escala (LLMs) como Gemini o GPT no es lo mismo que usar un motor de búsqueda tradicional. La "Ingeniería de Instrucciones" o Prompt Engineering es la habilidad de diseñar textos de entrada estructurados para programar el comportamiento, el tono y la precisión de la inteligencia artificial.

Para evitar respuestas genéricas, incompletas o con "alucinaciones" (datos inventados), utilizaremos un marco de trabajo profesional basado en cuatro pilares fundamentales: **Rol, Contexto, Razonamiento e Instrucciones**.



---

### A. Rol (Role)
Asignar un rol es el primer paso para "calibrar" el modelo. Al definir quién debe ser la IA, delimitamos el vocabulario que utilizará, el nivel de profundidad técnica de su respuesta y la perspectiva desde la cual abordará el problema.

* **Por qué es importante:** Un resumen financiero explicado por un "profesor de primaria" será radicalmente distinto al mismo resumen explicado por un "auditor contable senior".
* **Ejemplo:** *"Actúa como un científico de datos experto en análisis de comportamiento del consumidor..."* o *"Asume el rol de un ingeniero de soporte técnico especializado en bases de datos..."*

---

### B. Contexto (Context)
El modelo de lenguaje tiene conocimiento general del mundo, pero tiene cero conocimiento sobre tu situación específica, tu empresa o tu problema actual. El contexto provee el "terreno de juego" sobre el cual la IA debe trabajar.

* **Qué debe incluir:** Antecedentes del problema, datos históricos relevantes, el público objetivo al que va dirigida la respuesta o las limitaciones del proyecto.
* **Ejemplo:** *"Nuestra empresa de comercio electrónico ha experimentado una caída del 20% en las ventas de la categoría de tecnología durante el último trimestre. Nuestro público principal son jóvenes universitarios y tenemos un presupuesto de marketing muy limitado."*

---

### C. Razonamiento (Reasoning / Chain of Thought)
Este es el pilar que diferencia un prompt básico de uno avanzado. En lugar de pedirle a la IA que salte directamente a la conclusión, le exigimos que desglose su proceso lógico. Esto reduce drásticamente los errores y permite auditar cómo llegó a su respuesta.

* **Técnica clave:** Indicarle explícitamente que "piense paso a paso" o pedirle que analice los pros y los contras antes de emitir un veredicto.
* **Ejemplo:** *"Antes de darme tu recomendación final, analiza paso a paso las posibles causas de la caída en ventas mencionada en el contexto. Evalúa primero factores económicos y luego factores de plataforma."*

---

### D. Instrucciones (Instructions & Task)
Aquí es donde definimos la tarea específica que el modelo debe ejecutar y, fundamentalmente, **el formato exacto de salida**. Una instrucción ambigua genera resultados ambiguos.

* **Claridad y Restricciones:** Debes indicar qué quieres, qué NO quieres, y cómo debe verse visualmente la respuesta (una tabla, una lista, código, un archivo JSON).
* **Ejemplo:** *"Genera un plan de acción de 3 pasos para recuperar las ventas. No incluyas introducciones ni conclusiones generales. Devuelve el resultado en formato de tabla con las columnas: Acción, Responsable y Tiempo Estimado."*

---

### 4.1.2 Ejemplo Integrador: El Contraste

Veamos la diferencia entre un enfoque inexperto y el uso del framework completo para automatizar una tarea de análisis.

**Prompt Pobre (Ineficiente):**
> *"Ayúdame a mejorar las ventas de mi tienda que están bajando."*
*(Resultado: La IA dará una lista genérica de 10 consejos de marketing que aplican a cualquier negocio y no resuelven el problema de fondo).*

**Prompt Avanzado (Framework Completo):**
> **[Rol]:** Actúa como un consultor de estrategia de negocios experto en retención de clientes.
> **[Contexto]:** Administro una plataforma de cursos en línea. Este mes, la tasa de cancelación (churn rate) aumentó del 5% al 12%. Los clientes indican que los cursos son buenos, pero no tienen tiempo para terminarlos.
> **[Razonamiento]:** Primero, analiza paso a paso por qué la falta de tiempo es una barrera en la educación virtual. Luego, piensa en 3 micro-intervenciones que se puedan automatizar desde la plataforma para mantenerlos enganchados.
> **[Instrucciones]:** Redacta un documento estructurado. Utiliza viñetas para tu análisis. Para las 3 micro-intervenciones, entrégame el texto exacto que debería ir en un correo electrónico automático de re-enganche.