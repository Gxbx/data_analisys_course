# 🐍 Clase 2.3 — Integración de Power BI con Python (Guía paso a paso)

> **Nivel:** iniciación en Python + integración práctica con Power BI  
> **Objetivo:** que el estudiante sin conocimientos previos use Python para explorar datos en Google Colab y luego integre scripts y visuales de Python en Power BI.

---

## 🧭 Ruta de la sesión
1) **Introducción a Python en Google Colab** (conceptos básicos).  
2) **Python para análisis de datos** (pandas + matplotlib).  
3) **Integración con Power BI**:  
   - Visual de Python en el lienzo.  
   - Script de Python en Power Query (ETL).  
4) **Mini-proyecto**: gráfico con Python dentro de Power BI.

---

## 🧰 Requisitos
- Cuenta de Google (para usar **Google Colab**).  
- **Power BI Desktop** instalado.  
- Dataset: **Netflix Movies and TV Shows** (`netflix_titles.csv`) — Kaggle.

> Puedes reemplazar por cualquier CSV simple para las prácticas.

---

# 1) Python desde cero en Google Colab

### 1.1 ¿Qué es Google Colab?
Entorno gratuito en la nube para ejecutar Python en el navegador. No requiere instalación.

**Pasos:**
1. Ve a **https://colab.research.google.com**.  
2. **Nuevo cuaderno** (Notebook).  
3. Cambia el nombre a: `intro_python.ipynb`.

---

## 1.2 Conceptos básicos de Python

### Hola mundo
```python
print("Hola, Power BI + Python 👋")
```
### Variables y tipos de datos
```python
entero = 42
real = 3.1416
texto = "Netflix"
logico = True
print(type(entero), type(real), type(texto), type(logico))
n = int("2024")
pi_texto = str(3.14)
flag = bool(1)
```
### Operadores y expresiones
```python
a, b = 10, 3
print(a + b, a - b, a * b, a / b, a // b, a % b, a ** b)
print(a > b, a == b, a != b)
print((a > 5) and (b < 5), (a < 5) or (b < 5), not(a > b))
```
### Condicionales
```python
puntos = 87
if puntos >= 90:
    nivel = "Excelente"
elif puntos >= 70:
    nivel = "Bueno"
else:
    nivel = "En progreso"
print("Nivel:", nivel)
```

### Ciclos
```python
for i in range(1, 6):
    print("Iteración", i)

contador = 3
while contador > 0:
    print("Cuenta:", contador)
    contador -= 1
```

### Estructuras de datos
```python
lista = ["Movie", "TV Show", "Documentary"]
lista.append("Short")
tupla = ("US", "MX", "ES")
dic = {"pais": "US", "titulos": 1200}
dic["anio"] = 2024
```
### Funciones
```python
def es_reciente(anio, umbral=2020):
    return int(anio) >= umbral

print(es_reciente(2021), es_reciente(2018))
```
## 2) Integración con Power BI

### 3.1 Configurar Python

1. Instala Python 3.10+ o Anaconda.

2. Power BI → Archivo → Opciones → Scripts de Python.

3. Selecciona tu ejecutable python.exe.

4. Reinicia Power BI.

5. Instala pandas y matplotlib:
```
pip install pandas matplotlib
```

### 3.2 Visual de Python

1. Inserta Visual de Python.

2. Arrastra País principal y Total Títulos.

3. Agrega el script:
```python
import matplotlib.pyplot as plt
dataset = dataset.sort_values(by=dataset.columns[-1], ascending=False).head(10)
dataset.plot(kind="bar", x=dataset.columns[0], y=dataset.columns[-1])
plt.title("Top 10 países por títulos (Power BI + Python)")
plt.tight_layout()
plt.show()
```

### 3.3 Script en Power Query
```python
import pandas as pd
dataset["release_year"] = pd.to_numeric(dataset["release_year"], errors="coerce")
dataset["País principal"] = dataset["country"].fillna("").str.split(",").str[0].str.strip()
dataset = dataset.drop_duplicates()
```

## 3) Repaso y recursos

* Google Colab → https://colab.research.google.com

* pandas → https://pandas.pydata.org

* Matplotlib → https://matplotlib.org

* Documentación Power BI (Python scripting)