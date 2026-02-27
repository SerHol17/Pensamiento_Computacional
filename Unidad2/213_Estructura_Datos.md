### 2.1.3 Estructuras de Datos

Hasta ahora hemos trabajado con variables simples que almacenan un solo dato a la vez. Sin embargo, en la gestión administrativa y el análisis de datos, es necesario manejar grandes volúmenes de información de manera organizada. Las estructuras de datos nos permiten agrupar, almacenar y manipular colecciones de datos bajo un solo nombre.

---

#### A. Listas (Lists)
Una lista es una colección ordenada de elementos. En Python, las listas se definen utilizando corchetes `[]` y los elementos se separan por comas. Son ideales para manejar inventarios, listas de empleados o registros de ventas.

* **Índice:** Cada posición en la lista tiene un número que comienza desde el **0**. El primer elemento es el 0, el segundo el 1, y así sucesivamente.
* **Versatilidad:** Pueden contener diferentes tipos de datos (números, texto, o incluso otras listas) y su tamaño puede cambiar dinámicamente.
* **Operaciones comunes:** Se pueden agregar elementos (`append`), eliminar elementos (`remove`) o modificar valores existentes accediendo a su índice.

```python
# Ejemplo de una lista de sucursales
sucursales = ["Norte", "Sur", "Este", "Oeste"]

# Acceder al primer elemento (índice 0)
print(sucursales[0])  # Salida: Norte

# Agregar una nueva sucursal al final de la lista
sucursales.append("Centro")
```
<img src="_static/images/2-python-listas.png" width="600" />

---

#### B. Diccionarios (Dictionaries)
Un diccionario es una estructura de datos que almacena información en pares de **Clave: Valor**. Se definen utilizando llaves `{}`. Es la estructura que mejor representa un "registro" o una ficha técnica, similar a una fila en una base de datos.

* **Clave (Key):** Es el nombre del campo o etiqueta (ej. "nombre", "precio"). Debe ser única.
* **Valor (Value):** Es el dato asociado a esa etiqueta (ej. "Laptop", 2500).
* **Uso:** Es fundamental para representar objetos complejos donde cada dato tiene un significado específico, facilitando la búsqueda de información sin depender de una posición numérica.

```python
# Ejemplo de un registro de producto
producto = {
    "id": 101,
    "nombre": "Teclado Mecánico",
    "precio": 45.99,
    "disponible": True
}

# Acceder a un valor mediante su clave específica
print(producto["nombre"])  # Salida: Teclado Mecánico
```
<img src="_static/images/2-python-diccionarios.png" width="600" />

---

#### C. Aplicación Práctica: Listas de Diccionarios
La verdadera potencia de estas estructuras aparece cuando las combinamos. Una **Lista de Diccionarios** nos permite representar una tabla completa de datos (como un archivo de Excel), donde cada diccionario es una fila (un registro) y la lista es el conjunto total de datos.

```python
# Simulación de una base de datos de empleados
empleados = [
    {"nombre": "Ana", "cargo": "Gerente", "salario": 5000},
    {"nombre": "Luis", "cargo": "Analista", "salario": 3200},
    {"nombre": "Marta", "cargo": "Contadora", "salario": 4100}
]

# Recorrer la estructura con un ciclo para generar un reporte automatizado
for emp in empleados:
    print(f"Empleado: {emp['nombre']} | Cargo: {emp['cargo']}")
```
<img src="_static/images/2-python-estructuras-complejas.png" width="600" />

---