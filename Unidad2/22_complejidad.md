## 2.2 Complejidad Computacional y Eficiencia

En el análisis de datos y la administración de sistemas, no basta con que un código "funcione"; también debe ser eficiente. Cuando trabajamos con pocos registros (por ejemplo, 100 facturas), cualquier algoritmo parece rápido. Sin embargo, al escalar a miles o millones de datos (Big Data), un algoritmo ineficiente puede tardar horas en ejecutarse, bloquear el sistema o generar altos costos en servicios de la nube.

La complejidad computacional evalúa qué tantos recursos consume un algoritmo a medida que aumenta la cantidad de datos a procesar.

---

### 2.2.1 Complejidad de Tiempo y Espacio

Para medir la eficiencia de un programa, evaluamos dos dimensiones principales:

* **Complejidad de Tiempo:** Mide cuánto tiempo tarda en ejecutarse un algoritmo en función de la cantidad de datos de entrada. No se mide en segundos (ya que esto depende de la potencia del computador), sino en la cantidad de "operaciones" o pasos que debe realizar.
* **Complejidad de Espacio:** Mide cuánta memoria RAM requiere el algoritmo para ejecutarse. Por ejemplo, crear copias innecesarias de una base de datos grande agotará la memoria rápidamente.


<img src="../_static/images/2-2-complejidad-grafico.png" width="600" />

---

### 2.2.2 Notación Big O (Introducción)

La "Notación Big O" es el estándar matemático que usamos para clasificar los algoritmos según su rendimiento en el peor escenario posible. Los tres niveles más comunes que debes conocer son:

1.  **Tiempo Constante $O(1)$:** El algoritmo tarda exactamente lo mismo sin importar la cantidad de datos. Es el escenario ideal.
    * *Ejemplo:* Buscar un valor en un diccionario de Python usando su clave directa.
2.  **Tiempo Lineal $O(n)$:** El tiempo de ejecución crece de forma proporcional a los datos. Si los datos se duplican, el tiempo de procesamiento se duplica.
    * *Ejemplo:* Usar un ciclo `for` para leer todos los elementos de una lista uno por uno.
3.  **Tiempo Cuadrático $O(n^2)$:** El tiempo crece de forma exponencial. Si los datos se duplican, el tiempo se multiplica por cuatro. Suele ser una señal de alerta de ineficiencia y ocurre cuando anidamos ciclos (un bucle dentro de otro bucle).

---

### 2.2.3 Ejemplo de Negocio: Listas vs. Diccionarios

Imagina que tienes un sistema con 1,000,000 de clientes registrados y necesitas verificar si el cliente con el ID `999999` tiene deudas.

**Enfoque 1: Usando una Lista (Ineficiente - Tiempo Lineal)**
El programa debe revisar registro por registro desde el inicio hasta encontrar al cliente. En el peor de los casos, tendrá que hacer 1,000,000 de verificaciones.
```python
# Búsqueda secuencial en una lista
for cliente in lista_clientes:
    if cliente['id'] == 999999:
        print("Deuda encontrada")
```

**Enfoque 2: Usando un Diccionario (Eficiente - Tiempo Constante)**
Al estructurar la base de datos como un diccionario donde la "Clave" es el ID del cliente, el programa "salta" directamente a ese registro sin leer los demás. Toma 1 sola operación, sin importar si hay mil o un millón de clientes.
```python
# Búsqueda directa en un diccionario
if diccionario_clientes[999999]['deuda'] > 0:
    print("Deuda encontrada")
```


<img src="../_static/images/2-2-listas-vs-diccionarios.png" width="600" />

---