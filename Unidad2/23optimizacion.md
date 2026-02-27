## 2.3 Técnicas de Optimización en Algoritmos

Una vez que un algoritmo funciona correctamente y comprendemos su complejidad computacional, el siguiente paso lógico es la optimización. Optimizar significa refactorizar el código para que consuma menos recursos (tiempo de procesamiento y memoria RAM) y sea más fácil de leer y mantener, sin alterar su resultado final.

A continuación, exploramos las técnicas fundamentales para lograr algoritmos más eficientes.

---

### A. Principio DRY (Don't Repeat Yourself)

El principio "No te repitas" establece que ninguna pieza de lógica debería aparecer duplicada en el código. Si te encuentras copiando y pegando el mismo bloque de instrucciones múltiples veces, es momento de optimizar.

* **Solución:** Agrupar la lógica repetitiva dentro de **Ciclos** o **Funciones**.
* **Beneficio:** Si la regla de negocio cambia (por ejemplo, el impuesto pasa del 19% al 15%), solo debes modificar el código en un solo lugar, reduciendo drásticamente los errores de mantenimiento.

<img src="../_static/images/2-3-dry-principle.png" width="600" />

---

### B. Salidas Tempranas (Early Exit / Break)

Cuando realizamos búsquedas o validaciones dentro de un ciclo, no siempre es necesario recorrer todos los datos. Si ya encontramos la respuesta que buscábamos o detectamos un error que invalida el proceso, debemos detener la ejecución inmediatamente.

* **Uso del comando `break`:** Interrumpe un ciclo completo.
* **Uso del comando `return` (en funciones):** Devuelve el resultado y sale del subprograma.

**Ejemplo ineficiente:** Un ciclo que revisa 10,000 facturas para ver si existe la factura #5. Si la encuentra en la posición 2, igual revisa las 9,998 restantes.

**Ejemplo optimizado:**
```python
factura_buscada = 5
for factura in lista_facturas:
    if factura['id'] == factura_buscada:
        print("Factura encontrada")
        break  # Detiene el ciclo inmediatamente
```


<img src="../_static/images/2-3-early-exit.png" width="600" />

---

### C. Uso de Estructuras de Datos Eficientes (Sets)

En la sección anterior vimos la superioridad de los diccionarios sobre las listas para búsquedas por clave. Otra estructura vital para la optimización en Python son los **Conjuntos (Sets)**.

* **Características:** Son colecciones desordenadas de elementos únicos. No permiten duplicados.
* **Ventaja Computacional:** Al igual que los diccionarios, los conjuntos tienen una complejidad de tiempo $O(1)$ para verificar si un elemento existe (membresía).
* **Aplicación:** Si necesitas cruzar dos bases de datos enormes para ver qué clientes de la lista A también están en la lista B, convertir las listas a Sets hará que el proceso pase de tardar minutos a milisegundos.

```python
# Búsqueda extremadamente rápida usando Sets
clientes_vip = {"Juan", "Ana", "Carlos"}

if "Ana" in clientes_vip:  # Esta operación toma tiempo O(1)
    print("Aplicar trato preferencial")
```

<img src="../_static/images/2-3-sets-vs-lists.png" width="600" />

---

### D. Comprensión de Listas (List Comprehensions)

Esta es una técnica específica de Python que permite crear listas nuevas a partir de listas existentes de una manera mucho más concisa y rápida que usando un ciclo `for` tradicional. Python optimiza estas expresiones a nivel interno (en el lenguaje C subyacente), ejecutándolas a mayor velocidad.

**Código Tradicional (Toma 3 líneas):**
```python
precios_con_iva = []
for precio in lista_precios:
    precios_con_iva.append(precio * 1.19)
```

**Código Optimizado (Toma 1 línea):**
```python
precios_con_iva = [precio * 1.19 for precio in lista_precios]
```

<img src="../_static/images/2-3-list-comprehension.png" width="600" />

---