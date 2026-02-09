# Unidad 2: Diseño y Evaluación de Algoritmos

Esta unidad se centra en la transición de la idea lógica a la construcción técnica de soluciones. Aprenderemos a estructurar datos, medir la eficiencia de nuestros procesos y aplicar técnicas de optimización para garantizar que los algoritmos sean escalables en entornos empresariales.

---

## 2.1 Conceptos de Algoritmos y Estructuras de Datos

Un algoritmo es una secuencia finita de instrucciones precisas para resolver un problema. En el ámbito financiero, estas instrucciones deben ser inequívocas para evitar errores en el procesamiento de transacciones o cálculos de intereses.

###  Herramientas para el Diseño de Algoritmos

Antes de implementar una solución en herramientas de bloques o código, debemos diseñarla usando estándares universales:

### Diagramas de Flujo (Norma ANSI)
Representación visual del flujo lógico mediante símbolos estandarizados:
* **Óvalo:** Inicio/Fin del proceso.
* **Rectángulo:** Proceso o acción (ej. "Calcular IVA").
* **Rombo:** Decisión o condición (ej. "¿El saldo es suficiente?").
* **Paralelogramo:** Entrada o salida de datos.

<img src="_static\images\2-4.png"/>

### Pseudocódigo
Es una forma de escribir el algoritmo utilizando una mezcla de lenguaje natural y estructuras de control (Si, Entonces, Mientras). Es la herramienta preferida para validar la lógica antes de la programación final.


```text
ALGORITMO ValidarCredito
  INICIO
    LEER monto_solicitado
    LEER puntaje_datacredito
    SI puntaje_datacredito > 700 ENTONCES
      IMPRIMIR "Crédito Pre-aprobado"
    SINO
      IMPRIMIR "Requiere estudio manual"
    FIN SI
  FIN
`
```

<img src="_static\images\2pseudocodigo.png"/>

### Estructuras de Datos
Son las formas en que organizamos la información en la memoria de un sistema para que pueda ser utilizada de manera eficiente.
* **Listas y Arreglos:** Conjuntos de datos ordenados (ej. una lista de facturas pendientes).
* **Diccionarios (Mapas):** Estructuras que asocian una clave con un valor (ej. un NIT asociado a un nombre de proveedor).
* **Pilas y Colas:** Formas de procesar datos según el orden de llegada (ej. una cola de solicitudes de crédito).

<img src="_static\images\2-1.png"/>


---

## 2.2 Complejidad Computacional y Eficiencia

No todos los algoritmos son iguales. Algunos pueden resolver un problema en segundos con 10 datos, pero tardar horas si los datos suben a 10,000. La **Complejidad Computacional** nos permite medir este rendimiento.

### Notación Big O
Es el lenguaje que usamos para describir cuánto tiempo o espacio necesita un algoritmo a medida que crece el volumen de datos ($n$).
* **O(1) - Constante:** El tiempo es el mismo sin importar el volumen (ej. consultar el saldo de una cuenta si se tiene el ID directo).
* **O(n) - Lineal:** El tiempo crece proporcionalmente a los datos (ej. revisar una por una todas las facturas del mes para encontrar una específica).
* **O(n²) - Cuadrática:** El tiempo aumenta drásticamente (ej. comparar cada cliente de una lista contra todos los demás clientes para encontrar duplicados).

<img src="_static\images\2-2.png"/>

---

## 2.3 Técnicas de Optimización en Algoritmos

Optimizar consiste en modificar un algoritmo para que consuma menos recursos (tiempo de ejecución o memoria) sin cambiar el resultado final. En finanzas, la optimización es la diferencia entre un reporte que se genera en tiempo real y uno que bloquea el sistema.

* **Búsqueda Binaria:** Una técnica de optimización para encontrar datos en listas ordenadas de forma mucho más rápida que una búsqueda secuencial.
* **Memorización:** Almacenar resultados de cálculos costosos ya realizados para no tener que repetirlos (ideal para cálculos actuariales o de interés compuesto recurrente).
* **Refactorización:** Simplificar la lógica del algoritmo para eliminar pasos innecesarios.

<img src="_static\images\2-3.png"/>

---

