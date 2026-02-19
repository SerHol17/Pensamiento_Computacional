## 2.1.1 Lógica de Programación

La lógica de programación es la base sobre la cual se construye cualquier algoritmo. Permite organizar las ideas de manera que una computadora pueda ejecutarlas paso a paso para resolver un problema específico.

---

### Variables y Asignación
Una variable es un espacio de memoria con un nombre asociado que almacena un valor que puede cambiar durante la ejecución del programa. En administración, esto se puede comparar con una celda de Excel que contiene un dato específico.

<img src="_static/images/2-1-variables.png" width="600" />

### Procesos Matemáticos (Operadores)
Los algoritmos permiten realizar cálculos aritméticos fundamentales para el procesamiento de datos financieros. Los operadores principales son:

* **Suma (+):** Sumar valores o saldos.
* **Resta (-):** Calcular descuentos o egresos.
* **Multiplicación (*):** Cálculo de impuestos o totales.
* **División (/):** Prorrateo o cálculo de promedios.
* **Potencia (**):** Aplicado comúnmente en fórmulas de interés compuesto.
* **Módulo (%):** Obtiene el residuo de una división; útil para determinar ciclos o agrupaciones.

<img src="_static/images/2-1-operadores.png" width="600" />

---

### Estructuras Condicionales

Permiten que el programa tome decisiones basadas en una condición lógica (Verdadero o Falso). Es el núcleo de la automatización de procesos bajo reglas de negocio complejas que requieren evaluar múltiples escenarios de manera jerárquica.

* **Si (If):** Es la instrucción principal. Si la condición evaluada es verdadera, el sistema ejecuta el bloque de código correspondiente.
* **Sino Si (Elif):** Abreviatura de "Else If". Se utiliza para evaluar condiciones adicionales únicamente si las anteriores fueron falsas. Permite gestionar múltiples categorías sin necesidad de anidar demasiadas estructuras, lo cual es vital para procesos de clasificación (por ejemplo, rangos de impuestos o niveles de riesgo).
* **Sino (Else):** Define el camino a seguir cuando ninguna de las condiciones previas se ha cumplido. Actúa como el caso por defecto o de cierre.

**Ejemplo aplicado:** Clasificación de riesgo para aprobación de créditos:
1. **If** (Puntaje > 800): Aprobación inmediata con tasa preferencial.
2. **Elif** (Puntaje > 600): Aprobación sujeta a verificación de ingresos.
3. **Else**: Crédito rechazado por perfil de riesgo.

<img src="_static/images/2-1-condicionales.png" width="600" />

---

### Ciclos (Bucles)

Los ciclos o bucles son estructuras de control que permiten ejecutar un bloque de instrucciones de manera repetitiva. En el contexto de la administración y la ciencia de datos, son herramientas esenciales para procesar grandes volúmenes de información (como registros contables o bases de datos de clientes) sin necesidad de escribir manualmente la misma instrucción para cada dato.

* **Ciclo Mientras (While):** Esta estructura repite el bloque de código **mientras** una condición específica se mantenga como verdadera. Es ideal cuando no se conoce de antemano cuántas veces será necesario repetir el proceso. Por ejemplo, se puede usar para seguir procesando transacciones mientras el saldo de una cuenta sea positivo. 
    * **Nota de seguridad:** Es fundamental asegurar que la condición eventualmente se vuelva falsa para evitar un "bucle infinito", el cual puede bloquear los recursos del sistema.

* **Ciclo Para (For):** Se utiliza cuando se conoce con exactitud el número de iteraciones o cuando se desea recorrer todos los elementos de una colección (como una lista de facturas o un arreglo de salarios). En lenguajes modernos, este ciclo es la forma más eficiente y legible de aplicar una operación a cada elemento de un conjunto de datos de tamaño $n$.

**Aplicación práctica en Gestión:**
Imagina que necesitas aplicar un incremento del 5% a los salarios de una lista de 500 empleados. En lugar de realizar 500 cálculos manuales, un ciclo **Para** recorre la lista, toma cada salario, realiza la operación matemática y guarda el resultado automáticamente en segundos.

<img src="_static/images/2-1-ciclos.png" width="600" />

---
### 2.1.2 Herramientas de Representación de Algoritmos

Para que un algoritmo sea funcional, primero debe ser diseñado y documentado. Estas herramientas permiten visualizar la lógica antes de escribir una sola línea de código profesional, facilitando la detección de errores en la fase de planeación.

#### A. Diagramas de Flujo (Simbología ANSI)
Es la representación gráfica de un proceso. Se basa en una simbología estándar (American National Standards Institute) para que cualquier profesional pueda entender la secuencia lógica sin ambigüedades.

* **Inicio / Fin (Óvalo):** Todo diagrama debe empezar y terminar con este símbolo.
* **Entrada / Salida (Paralelogramo):** Representa la lectura de datos (ej. "Ingresar Salario") o la impresión de resultados.
* **Proceso (Rectángulo):** Indica una acción o cálculo matemático (ej. "Interés = Capital * Tasa").
* **Decisión (Rombo):** Representa un condicional. Tiene una entrada y dos salidas (Sí/No) que bifurcan el flujo.
* **Líneas de Flujo (Flechas):** Conectan los símbolos y determinan el orden de ejecución.


<img src="_static\images\2-4.png" width="600" />

##### Ejemplos de Aplicación Lógica

A continuación, se presentan dos casos prácticos donde se aplican estructuras condicionales para resolver problemas de validación y reglas de negocio:

---

**1. Control de Acceso (Validación de Edad)**
Usted es el encargado de una discoteca y necesita crear un algoritmo que valide la edad de los clientes. El sistema evalúa el dato ingresado y solo permite el acceso si la persona tiene 18 años o más.


<img src="_static/images/Ejemplo1.png" width="600" />

---

**2. Sistema de Descuentos (Reglas de Negocio)**
Como dueño de un almacén, desea automatizar una promoción: cualquier producto cuyo precio sea mayor a 100 dólares recibirá automáticamente un descuento del 10%. El algoritmo debe evaluar el precio y aplicar la resta correspondiente si se cumple la condición.


<img src="_static/images/Ejemplo2.png" width="600" />

**3. Reporte de Cierre de Caja (Procesamiento de Lotes)**

En el área administrativa, una de las tareas más comunes es la consolidación de datos. Este algoritmo automatiza el proceso de cierre de caja diario: el usuario debe ingresar la cantidad total de facturas emitidas y el sistema solicitará el valor de cada una de ellas de forma secuencial, acumulándolas para generar un reporte final del total de ingresos percibidos.

* **Propósito:** Sustituir el cálculo manual de múltiples transacciones por un proceso automatizado que minimiza errores humanos.
* **Lógica de Acumulación:** Se utiliza una variable "acumuladora" (`total_acumulado`) que inicia en cero y se actualiza en cada repetición del ciclo agregando el valor de la nueva venta.

<img src="_static/images/Ejemplo3.png" width="600" />
---

#### B. Pseudocódigo

Es una herramienta de diseño que permite escribir la lógica de un algoritmo utilizando una mezcla de lenguaje natural y estructuras de control. Se considera un "lenguaje intermedio" porque es más estructurado que el habla cotidiana, pero más sencillo de entender que un lenguaje de programación como Python o Java.

Su objetivo principal es permitir que el diseñador se concentre en la **solución lógica** y el flujo de los datos, dejando de lado las reglas técnicas estrictas (sintaxis).

**Reglas básicas:**
* **Palabras clave:** Uso de términos como `INICIO`, `FIN`, `LEER`, `IMPRIMIR`, `SI/SINO` y `PARA/MIENTRAS`.
* **Indentación (Sangría):** El uso de espacios para indicar que un bloque de instrucciones pertenece a una condición o a un ciclo.
* **Claridad:** Debe ser lo suficientemente claro para que cualquier persona pueda seguir la lógica del proceso.
<img src="_static\images\2pseudocodigo.png"/>
---

##### Ejemplos de Pseudocódigo Aplicado

A continuación, se presentan los algoritmos para los casos de negocio vistos anteriormente:

**1. Validación de Mayoría de Edad**
```text
ALGORITMO ControlAcceso
    INICIO
        LEER edad
        SI edad >= 18 ENTONCES
            IMPRIMIR "Acceso Permitido"
        SINO
            IMPRIMIR "Acceso Denegado"
        FIN SI
    FIN
```


**2. Cálculo de Descuento Automático**
Algoritmo para un sistema de ventas que aplica un 10% de descuento solo si la compra supera los 100 dólares.
```text
ALGORITMO DescuentoAlmacen
    INICIO
        LEER precio_producto
        SI precio_producto > 100 ENTONCES
            descuento = precio_producto * 0.10
            precio_final = precio_producto - descuento
            IMPRIMIR "Precio con descuento: ", precio_final
        SINO
            IMPRIMIR "No aplica descuento. Total: ", precio_producto
        FIN SI
    FIN
```

**3. Procesamiento de Varios Registros (Ciclo)**

Uso de un ciclo para sumar múltiples ventas y obtener un reporte final de caja.

```text
ALGORITMO SumarVentas
    INICIO
        LEER cantidad_ventas
        total_acumulado = 0
        PARA i DESDE 1 HASTA cantidad_ventas HACER
            LEER monto_venta
            total_acumulado = total_acumulado + monto_venta
        FIN PARA
        IMPRIMIR "El total de ventas del día es: ", total_acumulado
    FIN
```

[Vamos a programar!!](https://www.rollapp.com/app/pseint)

1. **Cálculo de Descuento:** Crear un algoritmo que solicite el precio de un producto y aplique un descuento del 15%. Mostrar el precio final al usuario.
2. **Determinación de Paridad:** Pedir un número entero y determinar si es par o impar.
3. **Conversión de Unidades:** Crear un programa que convierta una distancia dada en metros a sus equivalentes en centímetros y milímetros.
4. **Mayor de Tres Números:** Solicitar tres números diferentes y determinar cuál de ellos es el mayor.
5. **Cálculo de Salario:** Pedir las horas trabajadas en el mes y el valor de la hora. Calcular el salario total de un empleado.
6. **Validación de Nota:** Pedir una calificación de 1 a 10. Si la nota es mayor o igual a 6, mostrar "Aprobado"; de lo contrario, mostrar "Reprobado".
7. **Interés Simple:** Calcular el interés ganado por un capital inicial a una tasa fija del 5% anual durante un tiempo determinado (en años).
8. **Contador de Múltiplos:** Mostrar todos los números del 1 al 50 que sean múltiplos de 5 utilizando un ciclo.
9. **Suma de Rango:** Pedir al usuario un número N y calcular la suma de todos los números desde el 1 hasta el N.
10. **Clasificación por Edad:** Solicitar la edad de una persona y clasificarla en una de las siguientes categorías: "Niño" (0-12), "Adolescente" (13-17), "Adulto" (18-64) o "Adulto Mayor" (65+).


[Recurso extra](https://www.kaggle.com/learn/intro-to-programming)


---

