## 3.1 Programación básica aplicada a la solución de problemas

En las unidades anteriores, construimos una base sólida en lógica y sintaxis de Python. Sin embargo, en el mundo real, los datos no se ingresan manualmente uno por uno a través de la consola; provienen de bases de datos, sistemas de información o, muy frecuentemente, de múltiples archivos de Excel dispersos.

En esta sección, daremos el salto hacia la automatización administrativa utilizando **pandas**, la biblioteca fundamental de Python para la manipulación y análisis de datos.

---

### 3.1.1 Introducción a Pandas y DataFrames

Para trabajar con datos estructurados en Python, `pandas` nos ofrece un objeto llamado **DataFrame**. 



Puedes pensar en un DataFrame como la versión en código de una hoja de cálculo. Tiene filas y columnas, pero con la ventaja de que podemos aplicarle transformaciones a millones de registros en fracciones de segundo sin que el computador se congele.

Para comenzar, siempre debemos importar la librería:
```python
import pandas as pd
```

---

### 3.1.2 Lectura y Exploración Inicial de Datos

El primer paso en cualquier flujo de trabajo es cargar la información y entender su estructura.

```python
# Cargar un archivo de Excel a un DataFrame
df = pd.read_excel("registro_ventas.xlsx")

# Mostrar las primeras 5 filas para verificar que cargó correctamente
print(df.head())

# Obtener un resumen técnico: cantidad de filas, columnas y tipos de datos
print(df.info())

# Obtener estadísticas básicas (promedio, valores máximos y mínimos) de columnas numéricas
print(df.describe())
```

---

### 3.1.3 Limpieza y Transformación de Datos

Rara vez los datos empresariales vienen perfectos. Es común encontrar celdas vacías, registros duplicados o formatos incorrectos. Python nos permite automatizar la limpieza de estos errores.

**1. Manejo de valores nulos y duplicados:**
```python
# Eliminar cualquier fila que tenga al menos una celda vacía
df_limpio = df.dropna()

# Eliminar registros que sean exactamente iguales (duplicados)
df_limpio = df_limpio.drop_duplicates()
```

**2. Filtrado de información:**
Podemos extraer subconjuntos de datos aplicando condiciones lógicas (las mismas que vimos en la Unidad 2).
```python
# Crear un nuevo DataFrame solo con las ventas de la sucursal "Norte"
ventas_norte = df_limpio[df_limpio['Sucursal'] == 'Norte']

# Filtrar por múltiples condiciones (Ventas en el Norte mayores a $500)
ventas_destacadas = df_limpio[(df_limpio['Sucursal'] == 'Norte') & (df_limpio['Total'] > 500)]
```

---

### 3.1.4 Caso de Estudio: Consolidación y Transformación de Múltiples Archivos

Un problema clásico en la gestión de datos es tener la información fragmentada. Supongamos que recibimos la información operativa dividida en tres archivos de Excel separados y necesitamos fusionarlos, limpiarlos y transformarlos en un formato estructurado único. 

Hacer esto manualmente mes a mes es ineficiente. Con el siguiente script, estructuramos el proceso:



```python
import pandas as pd

# 1. Cargar los tres archivos de Excel
# Supongamos que son reportes de tres zonas diferentes
df_zona1 = pd.read_excel("datos_zona1.xlsx")
df_zona2 = pd.read_excel("datos_zona2.xlsx")
df_zona3 = pd.read_excel("datos_zona3.xlsx")

# 2. Consolidar (Unir verticalmente)
# pd.concat agrupa los DataFrames uno debajo del otro
df_maestro = pd.concat([df_zona1, df_zona2, df_zona3], ignore_index=True)

# 3. Limpieza del archivo consolidado
df_maestro = df_maestro.drop_duplicates()
df_maestro = df_maestro.dropna(subset=['ID_Transaccion', 'Monto']) # Solo elimina si faltan datos clave

# 4. Transformación (Pivot/Agrupación)
# Queremos saber el total vendido por cada categoría de producto
resumen_ventas = df_maestro.groupby('Categoria')['Monto'].sum().reset_index()

# 5. Exportar los resultados
# Guardamos tanto el maestro limpio como el resumen en nuevos archivos
df_maestro.to_excel("Base_Datos_Consolidada.xlsx", index=False)
resumen_ventas.to_csv("Resumen_Categorias.csv", index=False)

print("Proceso de consolidación y transformación ejecutado con éxito.")
```

Este tipo de scripts son la base de la automatización de reportes. Una vez programado, no importa si los archivos originales tienen 100 o 100,000 filas; el código los procesará en segundos.

---