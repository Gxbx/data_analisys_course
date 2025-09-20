# Guía completa de DAX con Power BI usando el dataset de Netflix (Kaggle)
**Fecha:** 2025-09-20  
**Autor:** Preparado para el Diplomado – Módulo 1.4/1.5 (Power BI + DAX)

> Dataset objetivo: *Netflix Movies and TV Shows* (Kaggle).  
> **Esquema asumido** (columnas típicas): `show_id`, `type`, `title`, `director`, `cast`, `country`, `date_added`, `release_year`, `rating`, `duration`, `listed_in`, `description`.

---

## 1) Preparación del modelo (Power Query + Modelado)

### 1.1 Limpieza rápida en Power Query
1. **Tipo de datos**:  
   - `date_added` → Fecha.  
   - `release_year` → Entero.  
   - `duration` → Texto (luego derivaremos minutos/temporadas).  
   - Demás campos como Texto.
2. **Quitar espacios**: *Transform > Format > Trim* en `title`, `country`, `listed_in`, `rating`.
3. **Duración (minutos o temporadas)**: crear **Columnas condicionales**:
   - **`is_movie`**: si `type = "Movie"` → `1` else `0`.
   - **`is_show`**: si `type = "TV Show"` → `1` else `0`.
   - **`duration_value`** (extraer número de `duration`): *Add Column > Extract > Digits*.
   - **`duration_unit`**: si `Text.Contains(duration,"min")` → `"min"` else `"season"`.
4. **Normalizar `country`**: si hay múltiples países, mantener el primero (opcional) o explotar luego en una tabla puente.

> Cargar como tabla `Netflix`.

### 1.2 Tabla Calendario (DAX)
En **Modeling > New Table**:
```DAX
Calendario =
VAR minFecha = DATE(2014,1,1)  -- o MIN('Netflix'[date_added]) si no hay nulos
VAR maxFecha = TODAY()
RETURN
ADDCOLUMNS(
    CALENDAR(minFecha, maxFecha),
    "Año", YEAR([Date]),
    "MesNum", MONTH([Date]),
    "Mes", FORMAT([Date], "YYYY-MM"),
    "NombreMes", FORMAT([Date], "MMMM"),
    "AñoMes", FORMAT([Date], "YYYY-MM"),
    "Dia", DAY([Date]),
    "Q", "Q" & FORMAT([Date], "Q")
)
```
Relacione `Calendario[Date]` con `Netflix[date_added]`.

---

## 2) Fundamentos de DAX: columnas calculadas vs medidas
- **Columna calculada**: se evalúa fila a fila y se guarda en el modelo.
- **Medida**: cálculo dinámico que depende del filtro de contexto.

### 2.1 Columnas calculadas útiles
```DAX
DuracionNormalizada =
VAR valor = VALUE('Netflix'[duration_value])
VAR unidad = 'Netflix'[duration_unit]
RETURN
IF(unidad = "min", valor, BLANK())

Temporadas =
VAR valor = VALUE('Netflix'[duration_value])
VAR unidad = 'Netflix'[duration_unit]
RETURN
IF(unidad <> "min", valor, BLANK())

Es_Documental = IF(CONTAINSSTRING('Netflix'[listed_in], "Documentary"), 1, 0)
Es_Comedia     = IF(CONTAINSSTRING('Netflix'[listed_in], "Comedies") || CONTAINSSTRING('Netflix'[listed_in], "Comedy"), 1, 0)
Es_Drama       = IF(CONTAINSSTRING('Netflix'[listed_in], "Dramas") || CONTAINSSTRING('Netflix'[listed_in], "Drama"), 1, 0)

Pais_Principal =
VAR c = SUBSTITUTE('Netflix'[country], ",", "|")
VAR pos = FIND("|", c, 1, 0)
RETURN IF(pos>0, LEFT(c, pos-1), 'Netflix'[country])

AnioMes_Added = FORMAT('Netflix'[date_added], "YYYY-MM")
```

### 2.2 Medidas base
```DAX
Titulos = COUNTROWS('Netflix')
Peliculas = CALCULATE(COUNTROWS('Netflix'), 'Netflix'[type] = "Movie")
Series    = CALCULATE(COUNTROWS('Netflix'), 'Netflix'[type] = "TV Show")
% Peliculas = DIVIDE([Peliculas], [Titulos])
% Series    = DIVIDE([Series], [Titulos])
```

---

## 3) DAX para *Time Intelligence* (con `Calendario`)
```DAX
Titulos Agregados = COUNTROWS('Netflix')

Titulos YTD = TOTALYTD([Titulos Agregados], 'Calendario'[Date])

Titulos MTD = TOTALMTD([Titulos Agregados], 'Calendario'[Date])

Titulos Últimos 12 Meses =
CALCULATE(
    [Titulos Agregados],
    DATESINPERIOD('Calendario'[Date], MAX('Calendario'[Date]), -12, MONTH)
)

Titulos Año Anterior =
CALCULATE([Titulos Agregados], SAMEPERIODLASTYEAR('Calendario'[Date]))

Crecimiento Interanual % =
DIVIDE([Titulos Agregados] - [Titulos Año Anterior], [Titulos Año Anterior])
```

---

## 4) Agregaciones por país, rating y género

### 4.1 País
```DAX
Titulos por País = COUNTROWS('Netflix')

Top 10 Países por Contenido =
TOPN(10, SUMMARIZE('Netflix', 'Netflix'[Pais_Principal], "Conteo", COUNTROWS('Netflix')), [Conteo], DESC)
```

### 4.2 Rating
```DAX
Titulos por Rating = COUNTROWS('Netflix')
Peliculas por Rating = CALCULATE(COUNTROWS('Netflix'), 'Netflix'[type]="Movie")
Series por Rating = CALCULATE(COUNTROWS('Netflix'), 'Netflix'[type]="TV Show")
```

### 4.3 Género
```DAX
Titulos Documentales = CALCULATE(COUNTROWS('Netflix'), 'Netflix'[Es_Documental] = 1)
Titulos Comedia      = CALCULATE(COUNTROWS('Netflix'), 'Netflix'[Es_Comedia] = 1)
Titulos Drama        = CALCULATE(COUNTROWS('Netflix'), 'Netflix'[Es_Drama] = 1)
```

---

## 5) Medidas con CALCULATE, FILTER y ALL

### 5.1 Participación de país %
```DAX
Titulos Globales = CALCULATE([Titulos], ALL('Netflix'[Pais_Principal]))
Participación País % = DIVIDE([Titulos], [Titulos Globales])
```

### 5.2 Ranking País
```DAX
Ranking País =
VAR t = ADDCOLUMNS(
    SUMMARIZE('Netflix', 'Netflix'[Pais_Principal]),
    "Conteo", [Titulos]
)
RETURN
RANKX(t, [Conteo], , DESC, Dense)
```

---

## 6) Duración y temporadas

```DAX
Duración Promedio (min) =
AVERAGEX(
    FILTER('Netflix', 'Netflix'[type]="Movie"),
    'Netflix'[DuracionNormalizada]
)

Temporadas Promedio =
AVERAGEX(
    FILTER('Netflix', 'Netflix'[type]="TV Show"),
    'Netflix'[Temporadas]
)

Segmento Duración =
VAR d = 'Netflix'[DuracionNormalizada]
RETURN
SWITCH(TRUE(),
    ISBLANK(d), "N/A",
    d < 60, "<60 min",
    d <= 90, "60-90",
    d <= 120, "90-120",
    ">120"
)
```

---

## 7) KPI y tablero sugerido

```DAX
KPI_Titulos = [Titulos]
KPI_Crecimiento_YoY = [Crecimiento Interanual %]
KPI_Peliculas = [Peliculas]
KPI_Series = [Series]
KPI_DuracionPromedio = [Duración Promedio (min)]
KPI_TemporadasPromedio = [Temporadas Promedio]
```

**Visuales recomendados:**
- Tarjetas con KPIs.  
- Gráfico columnas: Títulos por año.  
- Barras: Top países.  
- Treemap: Géneros.  
- Slicer: Tipo (Película/Serie), Rating, País.

---

## 8) Ejercicios prácticos
1. Crear `Titulos YTD` y `Crecimiento Interanual %`.  
2. Ranking de países con `RANKX`.  
3. Medir % de títulos recientes (>=2018).  
4. Analizar distribución de duración en películas.  
5. Crear KPI de temporadas promedio.  

---

## 9) Buenas prácticas
- Prefiera medidas sobre columnas.  
- Tabla calendario única.  
- Consistencia en nombres de campos y medidas.  
- Use `SWITCH(TRUE())` para condiciones múltiples.  
- Agrupe medidas en una tabla “_Measures”.  

---

### ✅ Checklist del estudiante
- [ ] Tabla calendario creada y relacionada.  
- [ ] Medidas básicas (`Titulos`, `Peliculas`, `Series`).  
- [ ] Medidas de tiempo (`YTD`, `YoY`).  
- [ ] Segmentación por país, género y rating.  
- [ ] KPIs en un dashboard con al menos 3 visuales.  
