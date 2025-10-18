# 📊 Módulo 2.2 — Uso de Gráficos Avanzados y KPIs en Power BI

## 🎯 Objetivo
Dominar la creación de visualizaciones avanzadas y la implementación de KPIs estratégicos en Power BI usando medidas DAX, visuales personalizados y diseño profesional.

---

## 📦 Dataset utilizado
**Fuente:** [Netflix Movies and TV Shows – Kaggle](https://www.kaggle.com/datasets/shivamb/netflix-shows)  
**Archivo:** `netflix_titles.csv`

**Columnas principales:**
- `show_id`
- `type` (Movie / TV Show)
- `title`
- `country`
- `release_year`
- `rating`
- `duration`
- `listed_in` (géneros)

---

## 🧩 Parte 1. Preparación de los datos

1. Abre Power BI Desktop.  
2. **Obtener datos → Texto/CSV →** selecciona `netflix_titles.csv`.  
3. Da clic en **Transformar datos** para abrir Power Query.  
4. Cambia los tipos de datos:  
   - `release_year` → número entero.  
   - `date_added` → fecha.  
5. Elimina filas duplicadas o nulas (si aplica).  
6. Renombra la tabla como `tbl_netflix`.  

> 💡 *Consejo:* Mantén tus nombres consistentes para las medidas DAX.

---

## 🧮 Parte 2. Creación de medidas base (DAX)

Crea un grupo de medidas nuevas en **Modelado → Nueva medida**:

```DAX
Total Títulos = COUNT(tbl_netflix[show_id])

Total Películas =
CALCULATE(
    COUNT(tbl_netflix[show_id]),
    tbl_netflix[type] = "Movie"
)

Total Series =
CALCULATE(
    COUNT(tbl_netflix[show_id]),
    tbl_netflix[type] = "TV Show"
)

Porcentaje Series =
DIVIDE([Total Series], [Total Títulos], 0)
```
## 🎯 Parte 3. KPIs estratégicos
### KPI 1: % de Series

1. Inserta una Tarjeta (Card).

2. Campo: [Porcentaje Series].

3. Da formato de porcentaje con 1 decimal.

4. Cambia color del texto a:

    - Verde si > 50%

    - Amarillo si entre 40–50%

    - Rojo si < 40%

Usa Formato condicional → Color de datos → Reglas.

### KPI 2: Títulos recientes (2020+)

```
Títulos Recientes =
CALCULATE(
    COUNT(tbl_netflix[show_id]),
    tbl_netflix[release_year] >= 2020
)
```

Visual: Card o KPI visual.

### KPI 3: Crecimiento interanual

```
Títulos Año Anterior =
CALCULATE(
    [Total Títulos],
    FILTER(tbl_netflix, tbl_netflix[release_year] = MAX(tbl_netflix[release_year]) - 1)
)
```
```
Crecimiento (%) =
DIVIDE([Total Títulos] - [Títulos Año Anterior], [Títulos Año Anterior], 0)
```

Visual: Indicador KPI o Tarjeta + flecha condicional.

## 📊 Parte 4. Visualizaciones avanzadas

### 1️⃣ Gráfico de Decomposición Jerárquica

- Inserta Gráfico de Decomposición.

- Campos:

    - Analizar: Total Títulos

    - Explicar por: type, country, release_year.

- Este gráfico permite profundizar (drill down) por cada categoría.

### 2️⃣ Treemap

- Inserta Treemap.

- Grupo: listed_in (géneros).

- Valor: show_id (conteo).

- Muestra proporciones de contenido por categoría.

### 3️⃣ Funnel (Embudo)

- Inserta Gráfico de Embudo.

- Datos ejemplo:

    - Etapas: “Total títulos” → “Con director” → “Con reparto”.

Usa medidas filtradas:

```
Con Director =
CALCULATE(COUNT(tbl_netflix[show_id]), NOT(ISBLANK(tbl_netflix[director])))
```

### 4️⃣ Gráfico de Cascada (Waterfall)

- Inserta Cascada.

- Eje: release_year

- Valor: [Total Títulos]

- Muestra la variación entre años (crecimientos y caídas).

### 5️⃣ Mapa de calor o “Filled Map”

- Campo de Ubicación: country.

- Valor: show_id (conteo).

- Personaliza colores para reflejar volumen de títulos.


## 🧭 Parte 5. Visualización de KPIs con objetivos
Ejemplo: Porcentaje de Series vs Meta
```
Meta Series = 0.50
```
```
Cumplimiento Meta =
IF([Porcentaje Series] >= [Meta Series], "Cumple", "No cumple")
```

- Visual sugerido: Gauge (Velocímetro)

- Valor: [Porcentaje Series]

- Mínimo: 0

- Máximo: 1

- Objetivo: [Meta Series]

- Colores:

    - Verde: Cumple meta.

    - Rojo: No cumple meta.

## 🧠 Parte 6. Dashboard “Rendimiento del Catálogo Netflix”
| Zona      | Contenido                                       |
| --------- | ----------------------------------------------- |
| Superior  | KPIs (Títulos totales, % Series, Crecimiento %) |
| Izquierda | Filtros por país, año, género                   |
| Centro    | Treemap (géneros) + Decomposición jerárquica    |
| Derecha   | Funnel o Cascada                                |
| Inferior  | Mapa de países + Comentario narrativo           |


💬 Agrega un texto dinámico DAX:

```
Texto Narrativo =
"El catálogo actual cuenta con " & FORMAT([Total Títulos], "#,0") &
" títulos, con un " & FORMAT([Porcentaje Series], "0.0%") &
" de series, mostrando un crecimiento de " & FORMAT([Crecimiento (%)], "0.0%") & " respecto al año anterior."
```

Inserta un cuadro de texto vinculado a esta medida (fx → valor de campo).

## 🚀 Parte 7. Publicación

- Guarda como Dashboard_KPIs_Netflix.pbix.

- Publica en Power BI Service.

- Configura actualización programada (si usas fuente en la nube).

- Comparte el enlace o incrusta el reporte.

## 🎯 Desafío Final

Crea un dashboard profesional con:

- 3 KPIs principales.

- 2 gráficos avanzados.

- Un texto narrativo dinámico.

- Formato visual consistente y colores corporativos.

- Sube el .pbix junto con una descripción de tus insights en un documento de 1 página.