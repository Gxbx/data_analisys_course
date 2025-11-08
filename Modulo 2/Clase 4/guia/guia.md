# 🧹 Clase 2.4 — Limpieza y Transformación Avanzada de Datos con Python (ETL)

> **Nivel:** intermedio  
> **Duración estimada:** 5 horas (teoría + práctica)  
> **Objetivo:** desarrollar habilidades para depurar, transformar y preparar datos financieros reales usando Python (pandas) y luego integrarlos en Power BI.

---

## 🧭 Contenido de la sesión

1. Introducción al proceso **ETL (Extract, Transform, Load)**.  
2. Exploración del dataset financiero.  
3. Limpieza de valores faltantes y duplicados.  
4. Transformaciones de tipo y formato.  
5. Creación de nuevas columnas (derivadas y categorizadas).  
6. Integración con Power BI mediante scripts de Python.  
7. Exportación de datos limpios.

---

## 📦 Dataset utilizado

**Fuente sugerida:** [Bank Marketing Dataset (Kaggle)](https://www.kaggle.com/datasets/henriqueyamahata/bank-marketing)  
O crea un archivo propio `transacciones.csv` con las siguientes columnas:

| cliente_id | edad | genero | ingreso_mensual | monto_prestamo | fecha_transaccion | tipo_transaccion | sucursal | estado |
|-------------|------|---------|------------------|----------------|-------------------|------------------|-----------|---------|
| 1001 | 34 | F | 2800 | 15000 | 2024-04-05 | Débito | Cali | Aprobado |
| 1002 | 42 | M | 3500 | 22000 | 2024-04-07 | Crédito | Bogotá | Pendiente |
| 1003 | 29 | F | 1800 | 12000 | 2024-04-09 | Débito | Medellín | Aprobado |
| 1004 | 38 | M | 5000 | 27000 | 2024-04-10 | Débito | Cali | Rechazado |

> 💡 Ideal para trabajar limpieza, transformación y segmentación financiera.

---

## 🧮 1. Introducción al ETL

**ETL (Extract, Transform, Load)** es el proceso fundamental para preparar datos:

| Etapa | Descripción | Herramientas |
|--------|-------------|---------------|
| **Extract** | Obtención de datos (CSV, Excel, API, BD) | `pandas`, `requests`, `sqlalchemy` |
| **Transform** | Limpieza, normalización, cálculo de indicadores | `pandas`, `numpy` |
| **Load** | Carga del resultado (Power BI, SQLite, Excel, etc.) | `pandas.to_csv()`, `to_sql()` |

---

## 🧠 2. Carga y exploración inicial del dataset

```python
import pandas as pd

# Cargar dataset
df = pd.read_csv("transacciones.csv")

# Vista general
print(df.shape)
print(df.dtypes)
df.head()

# Resumen estadístico
df.describe(include='all')

# Conteo de valores nulos
df.isna().sum()
```

## 🧹 3. Limpieza de datos
3.1 Eliminar duplicados
df = df.drop_duplicates(subset=["cliente_id", "fecha_transaccion"])

### 3.2 Tratar valores faltantes
```python
# Reemplazar vacíos en texto
df["sucursal"] = df["sucursal"].fillna("Sin información")

# Imputar promedio para ingresos y préstamos
df["ingreso_mensual"] = df["ingreso_mensual"].fillna(df["ingreso_mensual"].median())
df["monto_prestamo"] = df["monto_prestamo"].fillna(df["monto_prestamo"].mean())

# Eliminar registros sin cliente_id
df = df.dropna(subset=["cliente_id"])

3.3 Normalizar formatos
# Mayúsculas/minúsculas
df["genero"] = df["genero"].str.upper().str.strip()

# Convertir fechas
df["fecha_transaccion"] = pd.to_datetime(df["fecha_transaccion"], errors="coerce")

# Validar rango de fechas
df = df[df["fecha_transaccion"].dt.year >= 2020]
```
## 🔄 4. Transformaciones avanzadas
### 4.1 Crear nuevas columnas derivadas
```python
# Mes y año
df["mes"] = df["fecha_transaccion"].dt.month_name()
df["anio"] = df["fecha_transaccion"].dt.year

# Ratio préstamo/ingreso
df["ratio_prestamo_ingreso"] = (df["monto_prestamo"] / df["ingreso_mensual"]).round(2)

# Categoría de ingresos
df["categoria_ingreso"] = pd.cut(
    df["ingreso_mensual"],
    bins=[0, 2000, 4000, 6000, 10000],
    labels=["Bajo", "Medio", "Medio-Alto", "Alto"]
)
```
### 4.2 Limpieza semántica de texto
```python
# Uniformar valores de estado
df["estado"] = df["estado"].str.title()
df["tipo_transaccion"] = df["tipo_transaccion"].replace(
    {"debito": "Débito", "credito": "Crédito"}
)
```
### 4.3 Agregaciones
```python
# Total por sucursal y tipo
resumen = df.groupby(["sucursal", "tipo_transaccion"]).agg({
    "monto_prestamo": "sum",
    "cliente_id": "count"
}).reset_index()

resumen.rename(columns={"cliente_id": "n_transacciones"}, inplace=True)
resumen.head()
```
### 📈 5. Validaciones y calidad de datos
```python
# Rango de edad válido
df = df[(df["edad"] > 18) & (df["edad"] < 100)]

# Validar ratios
df = df[df["ratio_prestamo_ingreso"] < 15]

# Detectar outliers por IQR
Q1 = df["monto_prestamo"].quantile(0.25)
Q3 = df["monto_prestamo"].quantile(0.75)
IQR = Q3 - Q1
df = df[(df["monto_prestamo"] >= Q1 - 1.5 * IQR) & (df["monto_prestamo"] <= Q3 + 1.5 * IQR)]
```

### 💾 6. Exportar datos limpios
```python
df.to_csv("transacciones_limpias.csv", index=False)
resumen.to_excel("resumen_sucursales.xlsx", index=False)
```

🧠 Los archivos generados pueden ser cargados directamente a Power BI.

## 🔗 7. Integración con Power BI (Script de Python)

En Power Query → Ejecutar script de Python, pega:
```python
import pandas as pd

dataset["fecha_transaccion"] = pd.to_datetime(dataset["fecha_transaccion"], errors="coerce")
dataset["mes"] = dataset["fecha_transaccion"].dt.month_name()
dataset["anio"] = dataset["fecha_transaccion"].dt.year
dataset["ratio_prestamo_ingreso"] = (dataset["monto_prestamo"] / dataset["ingreso_mensual"]).round(2)
dataset["categoria_ingreso"] = pd.cut(
    dataset["ingreso_mensual"],
    bins=[0, 2000, 4000, 6000, 10000],
    labels=["Bajo", "Medio", "Medio-Alto", "Alto"]
)
dataset = dataset.dropna(subset=["cliente_id"])
```
### 🧠 8. Actividad práctica
Ejercicio guiado:

1. Limpia el archivo transacciones.csv aplicando todos los pasos de la guía.

2. Crea un nuevo campo saldo_estimado = ingreso_mensual - (monto_prestamo / 12).

3. Calcula el promedio de ratio préstamo/ingreso por sucursal.

4. Exporta los resultados a un archivo resumen_financiero.csv.

5. Carga los datos en Power BI y crea un dashboard con:

    - KPI: Total préstamos.

    - Mapa: Préstamos por sucursal.

    - Treemap: Categoría de ingresos.

    - Línea temporal: Evolución mensual.
# 🧹 Clase 2.4 — Limpieza y Transformación Avanzada de Datos con Python (ETL)

> **Nivel:** intermedio  
> **Duración estimada:** 5 horas (teoría + práctica)  
> **Objetivo:** desarrollar habilidades para depurar, transformar y preparar datos financieros reales usando Python (pandas) y luego integrarlos en Power BI.

---

## 🧭 Contenido de la sesión

1. Introducción al proceso **ETL (Extract, Transform, Load)**.  
2. Exploración del dataset financiero.  
3. Limpieza de valores faltantes y duplicados.  
4. Transformaciones de tipo y formato.  
5. Creación de nuevas columnas (derivadas y categorizadas).  
6. Integración con Power BI mediante scripts de Python.  
7. Exportación de datos limpios.

---

## 📦 Dataset utilizado

**Fuente sugerida:** [Bank Marketing Dataset (Kaggle)](https://www.kaggle.com/datasets/henriqueyamahata/bank-marketing)  
O crea un archivo propio `transacciones.csv` con las siguientes columnas:

| cliente_id | edad | genero | ingreso_mensual | monto_prestamo | fecha_transaccion | tipo_transaccion | sucursal | estado |
|-------------|------|---------|------------------|----------------|-------------------|------------------|-----------|---------|
| 1001 | 34 | F | 2800 | 15000 | 2024-04-05 | Débito | Cali | Aprobado |
| 1002 | 42 | M | 3500 | 22000 | 2024-04-07 | Crédito | Bogotá | Pendiente |
| 1003 | 29 | F | 1800 | 12000 | 2024-04-09 | Débito | Medellín | Aprobado |
| 1004 | 38 | M | 5000 | 27000 | 2024-04-10 | Débito | Cali | Rechazado |

> 💡 Ideal para trabajar limpieza, transformación y segmentación financiera.

---

## 🧮 1. Introducción al ETL

**ETL (Extract, Transform, Load)** es el proceso fundamental para preparar datos:

| Etapa | Descripción | Herramientas |
|--------|-------------|---------------|
| **Extract** | Obtención de datos (CSV, Excel, API, BD) | `pandas`, `requests`, `sqlalchemy` |
| **Transform** | Limpieza, normalización, cálculo de indicadores | `pandas`, `numpy` |
| **Load** | Carga del resultado (Power BI, SQLite, Excel, etc.) | `pandas.to_csv()`, `to_sql()` |

---

## 🧠 2. Carga y exploración inicial del dataset

```python
import pandas as pd

# Cargar dataset
df = pd.read_csv("transacciones.csv")

# Vista general
print(df.shape)
print(df.dtypes)
df.head()

# Resumen estadístico
df.describe(include='all')

# Conteo de valores nulos
df.isna().sum()
```

## 🧹 3. Limpieza de datos
3.1 Eliminar duplicados
df = df.drop_duplicates(subset=["cliente_id", "fecha_transaccion"])

### 3.2 Tratar valores faltantes
```python
# Reemplazar vacíos en texto
df["sucursal"] = df["sucursal"].fillna("Sin información")

# Imputar promedio para ingresos y préstamos
df["ingreso_mensual"] = df["ingreso_mensual"].fillna(df["ingreso_mensual"].median())
df["monto_prestamo"] = df["monto_prestamo"].fillna(df["monto_prestamo"].mean())

# Eliminar registros sin cliente_id
df = df.dropna(subset=["cliente_id"])

3.3 Normalizar formatos
# Mayúsculas/minúsculas
df["genero"] = df["genero"].str.upper().str.strip()

# Convertir fechas
df["fecha_transaccion"] = pd.to_datetime(df["fecha_transaccion"], errors="coerce")

# Validar rango de fechas
df = df[df["fecha_transaccion"].dt.year >= 2020]
```
## 🔄 4. Transformaciones avanzadas
### 4.1 Crear nuevas columnas derivadas
```python
# Mes y año
df["mes"] = df["fecha_transaccion"].dt.month_name()
df["anio"] = df["fecha_transaccion"].dt.year

# Ratio préstamo/ingreso
df["ratio_prestamo_ingreso"] = (df["monto_prestamo"] / df["ingreso_mensual"]).round(2)

# Categoría de ingresos
df["categoria_ingreso"] = pd.cut(
    df["ingreso_mensual"],
    bins=[0, 2000, 4000, 6000, 10000],
    labels=["Bajo", "Medio", "Medio-Alto", "Alto"]
)
```
### 4.2 Limpieza semántica de texto
```python
# Uniformar valores de estado
df["estado"] = df["estado"].str.title()
df["tipo_transaccion"] = df["tipo_transaccion"].replace(
    {"debito": "Débito", "credito": "Crédito"}
)
```
### 4.3 Agregaciones
```python
# Total por sucursal y tipo
resumen = df.groupby(["sucursal", "tipo_transaccion"]).agg({
    "monto_prestamo": "sum",
    "cliente_id": "count"
}).reset_index()

resumen.rename(columns={"cliente_id": "n_transacciones"}, inplace=True)
resumen.head()
```
### 📈 5. Validaciones y calidad de datos
```python
# Rango de edad válido
df = df[(df["edad"] > 18) & (df["edad"] < 100)]

# Validar ratios
df = df[df["ratio_prestamo_ingreso"] < 15]

# Detectar outliers por IQR
Q1 = df["monto_prestamo"].quantile(0.25)
Q3 = df["monto_prestamo"].quantile(0.75)
IQR = Q3 - Q1
df = df[(df["monto_prestamo"] >= Q1 - 1.5 * IQR) & (df["monto_prestamo"] <= Q3 + 1.5 * IQR)]
```

### 💾 6. Exportar datos limpios
```python
df.to_csv("transacciones_limpias.csv", index=False)
resumen.to_excel("resumen_sucursales.xlsx", index=False)
```

🧠 Los archivos generados pueden ser cargados directamente a Power BI.

## 🔗 7. Integración con Power BI (Script de Python)

En Power Query → Ejecutar script de Python, pega:
```python
import pandas as pd

dataset["fecha_transaccion"] = pd.to_datetime(dataset["fecha_transaccion"], errors="coerce")
dataset["mes"] = dataset["fecha_transaccion"].dt.month_name()
dataset["anio"] = dataset["fecha_transaccion"].dt.year
dataset["ratio_prestamo_ingreso"] = (dataset["monto_prestamo"] / dataset["ingreso_mensual"]).round(2)
dataset["categoria_ingreso"] = pd.cut(
    dataset["ingreso_mensual"],
    bins=[0, 2000, 4000, 6000, 10000],
    labels=["Bajo", "Medio", "Medio-Alto", "Alto"]
)
dataset = dataset.dropna(subset=["cliente_id"])
```
### 🧠 8. Actividad práctica
Ejercicio guiado:

1. Limpia el archivo transacciones.csv aplicando todos los pasos de la guía.

2. Crea un nuevo campo saldo_estimado = ingreso_mensual - (monto_prestamo / 12).

3. Calcula el promedio de ratio préstamo/ingreso por sucursal.

4. Exporta los resultados a un archivo resumen_financiero.csv.

5. Carga los datos en Power BI y crea un dashboard con:

    - KPI: Total préstamos.

    - Mapa: Préstamos por sucursal.

    - Treemap: Categoría de ingresos.

    - Línea temporal: Evolución mensual.
