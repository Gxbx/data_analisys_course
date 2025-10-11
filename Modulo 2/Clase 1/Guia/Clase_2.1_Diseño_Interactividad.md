# 🧩 Módulo 2.1 — Diseño de Informes Interactivos en Power BI

## 🎯 Objetivo
Aprender a diseñar informes y dashboards interactivos en Power BI aplicando buenas prácticas de UX, navegación, bookmarks y storytelling con datos.

---

## 📦 Dataset utilizado
**Fuente:** [Netflix Movies and TV Shows – Kaggle](https://www.kaggle.com/datasets/shivamb/netflix-shows)  
**Archivo:** `netflix_titles.csv`

**Columnas principales:**
- `show_id`  
- `type` (Movie / TV Show)  
- `title`  
- `director`  
- `cast`  
- `country`  
- `date_added`  
- `release_year`  
- `rating`  
- `duration`  
- `listed_in` (categorías o géneros)  
- `description`

---

## 🧭 Parte 1. Conexión de datos

1. Abre **Power BI Desktop**.  
2. En la pestaña **Inicio → Obtener datos → Texto/CSV**.  
3. Selecciona el archivo `netflix_titles.csv`.  
4. Da clic en **Transformar datos** para abrir **Power Query Editor**.  
5. Revisa:
   - Cambia el tipo de datos de `release_year` a **Número entero**.  
   - Convierte `date_added` a formato **Fecha**.  
   - Usa **Dividir columna → Por delimitador** para separar los géneros de `listed_in` si lo deseas.  
6. Cierra y aplica los cambios.

> 💡 *Consejo:* Renombra la tabla como `tbl_netflix` para mantener consistencia en tus medidas DAX.

---

## 📊 Parte 2. Diseño del layout del dashboard

### 🧱 Estructura recomendada:
| Zona | Elemento sugerido |
|------|-------------------|
| Superior | KPIs principales |
| Izquierda | Filtros o botones |
| Centro | Visualizaciones dinámicas |
| Inferior | Segmentadores o detalles |

### 🎨 Crea una página base:
1. Cambia el fondo del lienzo:  
   - **Formato → Fondo → Color suave (gris claro o azul tenue)**  
2. Agrega un **título con texto dinámico**:  
   ```DAX
   Título Página = "Catálogo interactivo de Netflix"
   ```
3. Inserta un **logotipo o icono de Netflix** (Insertar → Imagen).

---

## 🧩 Parte 3. Creación de KPIs principales

Agrega tres **tarjetas visuales**:
1. **Total de títulos:**  
   ```DAX
   Total Títulos = COUNT(tbl_netflix[show_id])
   ```
2. **Total de películas:**  
   ```DAX
   Total Películas = CALCULATE(COUNT(tbl_netflix[show_id]), tbl_netflix[type] = "Movie")
   ```
3. **Total de series:**  
   ```DAX
   Total Series = CALCULATE(COUNT(tbl_netflix[show_id]), tbl_netflix[type] = "TV Show")
   ```

Usa íconos o colores distintos para cada uno (rojo, gris, azul).

---

## 📈 Parte 4. Visualizaciones interactivas

1. **Gráfico de barras:**  
   - Eje X: `country`  
   - Eje Y: `show_id` (conteo)  
   - Filtra los **10 países con más títulos**.  

2. **Gráfico de columnas:**  
   - Eje X: `release_year`  
   - Eje Y: Conteo de `show_id`.  
   - Formato → Líneas suaves → Activado.  

3. **Gráfico circular:**  
   - Campo: `rating`  
   - Valor: Conteo de títulos.

4. **Tabla dinámica:**  
   - Columnas: `title`, `director`, `listed_in`, `release_year`.  
   - Agrega un filtro superior por país.

---

## 🧭 Parte 5. Interactividad y navegación

### 1. **Bookmarks**
1. Crea dos vistas:
   - Vista 1: Filtrada a `type = "Movie"`.  
   - Vista 2: Filtrada a `type = "TV Show"`.  
2. En la pestaña **Ver → Panel de marcadores → Agregar marcador**.  
   - Nómbralos como **Solo Películas** y **Solo Series**.

### 2. **Botones**
1. Inserta dos botones (Insertar → Botón → Forma).  
2. Asigna acción:
   - **Botón 1:** Acción → Bookmark → “Solo Películas”.  
   - **Botón 2:** Acción → Bookmark → “Solo Series”.

🎯 Ahora podrás alternar entre vistas con un clic.

---

## 🧩 Parte 6. Drillthrough y Tooltips

### Drillthrough:
1. Crea una nueva página llamada **Detalle País**.  
2. Agrega una tabla con:
   - `title`, `type`, `rating`, `release_year`.  
3. En el panel de **Drillthrough**, arrastra el campo `country`.  
4. Vuelve al gráfico de países → clic derecho → *Drillthrough → Detalle País*.

### Tooltip personalizado:
1. Crea otra página pequeña (tamaño 300x200px).  
2. Diseña una mini tarjeta con KPIs del país.  
3. Configura la página como **Tooltip** (Formato → Información de página).  
4. Aplica el tooltip al gráfico de países.

---

## 🎛️ Parte 7. Segmentadores sincronizados

1. Inserta un **segmentador** para el campo `rating`.  
2. Copia y pégalo en todas las páginas.  
3. En la pestaña **Ver → Sincronización de segmentadores**, actívalo para todas las páginas.

Esto mantiene los filtros activos al navegar entre vistas.

---

## 📤 Parte 8. Publicación y experiencia final

1. Guarda el archivo como `Dashboard_Interactivo_Netflix.pbix`.  
2. Publica en Power BI Service → Workspace personal.  
3. Ajusta el modo de visualización para “vista de lectura”.  
4. Comparte el enlace público o incrústalo en Microsoft Teams / Moodle / Canvas.

---

## 🧠 Reto adicional

🎯 **Desafío 1:**  
Agrega un botón “Ver tendencias recientes” que muestre solo títulos agregados después de 2020 (usando bookmarks).  

🎯 **Desafío 2:**  
Crea un dashboard con navegación entre páginas tipo “menú lateral” usando imágenes o íconos.  

🎯 **Desafío 3:**  
Agrega una medida dinámica que muestre:
```DAX
Mensaje dinámico =
"Actualmente estás viendo " &
SELECTEDVALUE(tbl_netflix[type], "todos los contenidos")
```
y úsala como subtítulo en tus visuales.

---

## 🏁 Resultado esperado
Un dashboard moderno e interactivo con:
- KPIs principales,  
- Navegación entre vistas,  
- Drillthrough y tooltips personalizados,  
- Segmentadores sincronizados,  
- Diseño limpio y narrativo.

---

## 📚 Referencias
- Microsoft Learn. (2024). *Power BI – Diseño de informes y dashboards interactivos*.  
- Storytelling with Data – Cole Nussbaumer Knaflic. (2015).  
- Kaggle Datasets. *Netflix Movies and TV Shows*.
