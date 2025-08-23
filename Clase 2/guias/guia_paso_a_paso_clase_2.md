# Guía paso a paso – Clase 1.3
**Fundamentos de la Inteligencia de Negocios (BI) con Power BI + Excel Intermedio**  
**Duración sugerida:** 5 horas

---
## 0) Preparación previa (10–15 min)
1. **Archivos de práctica**: `datos_vida_diaria.xlsx` y `caso_restaurante_sabor_cauca.xlsx`.
2. **Instalar/abrir** Power BI Desktop (versión reciente) y Excel (Microsoft 365 o 2019+ recomendado).
3. **Opciones Power BI** → *File > Options > Global > Preview features*: desactive lo no necesario. Asegure *Auto date/time* activo para principiantes (*File > Options > Current File > Data Load*).
4. **Objetivo de la sesión**: Convertir datos en decisiones mediante un flujo básico BI: *obtener datos → transformar → modelar → visualizar → decidir*.

---
## PARTE A — Excel Intermedio (90 min)
### A1) Limpieza y tabulación de datos (Tabla de Excel)
**Archivo:** `datos_vida_diaria.xlsx`
1. Abra el archivo en Excel.
2. Seleccione cualquier celda de la tabla → **Insertar > Tabla** (active “La tabla tiene encabezados”).
3. En **Tabla > Diseño** cambie el **Nombre de la tabla** a `tbl_semana`.
4. Formatee encabezados (negrita), active **Filtro** (si no aparece por defecto).

**Validaciones rápidas**
5. Revise que **no haya celdas en blanco** en columnas clave.
6. Compruebe **tipos**: que “Ventas_Producto_A”, “Visitas_Redes” y “Gastos_Publicidad” sean numéricos.

### A2) Columnas calculadas útiles
En la última columna vacía, inserte nuevas columnas dentro de la **tabla** (se crean automáticamente):
1. **Semana_Num** (extraer número de la cadena "Semana X"):
   - Fórmula: `=VALOR(DERECHA([@Semana],LARGO([@Semana])-7))`
2. **Costo_por_Venta**:
   - Fórmula: `=[@Gastos_Publicidad]/[@Ventas_Producto_A]`
3. **Costo_por_Visita**:
   - Fórmula: `=[@Gastos_Publicidad]/[@Visitas_Redes]`
4. Formatee **2 decimales** y agregue **separador de miles**.

### A3) Funciones intermedias (SUMIFS, AVERAGEIFS, XLOOKUP)
Cree una hoja nueva: **“KPIs”**.
1. **Ventas totales** (SUMA rápida): `=SUMA(tbl_semana[Ventas_Producto_A])`
2. **SUMIFS** – Ventas de una semana dada (parámetro en A2):
   - En **A2** escriba: *Semana 4* (por ejemplo).
   - En **B2**: `=SUMAR.SI.CONJUNTO(tbl_semana[Ventas_Producto_A], tbl_semana[Semana], A2)`
3. **AVERAGEIFS** – Costo por venta promedio en semanas > 4:
   - `=PROMEDIO.SI.CONJUNTO(tbl_semana[Costo_por_Venta], tbl_semana[Semana_Num], ">4")`
4. **XLOOKUP** – Objetivos vs. reales:
   - En **KPIs**, cree una mini tabla manual en **E2:F5** llamada `tbl_objetivo`:
     - Encabezados: `Semana` | `Meta_Ventas`
     - Filas: *Semana 1 → 120*, *Semana 2 → 130*, *Semana 3 → 140*, *Semana 4 → 150*.
   - En **G2**: `=BUSCARX(A2, tbl_objetivo[Semana], tbl_objetivo[Meta_Ventas], "Sin meta")`
   - En **H2**: **Brecha** = `=B2 - G2`

**Tip:** Use **Ctrl+T** para convertir rangos a tabla; facilita referencias estructuradas.

### A4) Tablas dinámicas (Pivot) + Segmentadores
1. Seleccione una celda en `tbl_semana` → **Insertar > Tabla dinámica** → Nueva hoja “Pivot_Semana”.
2. Diseñe **Pivot 1**:
   - **Filas**: `Semana`
   - **Valores**: `Ventas_Producto_A` (Suma), `Gastos_Publicidad` (Suma)
3. **Insertar segmentador** (Slicer) para `Semana` si desea filtrar.
4. **Gráfico**: Inserte **Columnas agrupadas** con Ventas vs Gastos.

### A5) Power Query (Extracción y Transformación)
**Objetivo:** combinar datos del restaurante para un resumen diario.
1. **Datos > Obtener datos > Desde archivo > Desde libro** y elija `caso_restaurante_sabor_cauca.xlsx`.
2. **Transformar datos** (no “Cargar” todavía) para abrir **Power Query**.
3. En la consulta, verifique **tipos**: `Fecha` (Fecha), `Plato` (Texto), `Ventas` (Número entero), `Pedidos_Cancelados` (Entero), `Costo_Promedio` (Decimal), `Likes_Redes` (Entero).
4. Cambie nombres si es necesario (clic derecho → *Cambiar nombre*).
5. **Quitar filas** con errores si aparecen (*Inicio > Quitar filas > Quitar errores*).
6. **Agrupar por** para crear un resumen diario:
   - *Inicio > Agrupar por* → **Agrupar por** `Fecha`.
   - **Nuevas columnas**:
     - `Ventas_Diarias` = Suma de `Ventas`
     - `Cancelaciones_Diarias` = Suma de `Pedidos_Cancelados`
     - `Likes_Diarios` = Suma de `Likes_Redes`
   - Acepte. Obtendrá una tabla diaria agregada.
7. **Cerrar y Cargar** en una **nueva hoja** como tabla.

### A6) Mini Dashboard en Excel (rápido)
1. Nueva hoja: **Dashboard_Excel**.
2. Inserte **Tarjetas** (celdas con formato) para: Ventas Totales, Gastos Totales, Costo por Venta Promedio.
3. Inserte **Gráfico de columnas**: Ventas por Semana.
4. Inserte **Gráfico de líneas**: Gastos_Publicidad por Semana.
5. Inserte **Gráfico circular** (opcional) con participación de *Ventas* por semana.
6. Agregue **segmentadores** sobre la tabla dinámica para filtrar el dashboard.

**Atajos útiles Excel**
- Crear tabla: **Ctrl+T**
- Navegar a última celda: **Ctrl+Fin**
- Llenar hacia abajo: **Ctrl+D**
- Repetir acción: **F4**

---
## PARTE B — Power BI: de datos a decisiones (120 min)
### B1) Importar datos y Power Query
1. **Home > Get Data > Excel** → seleccione `datos_vida_diaria.xlsx` y `caso_restaurante_sabor_cauca.xlsx`.
2. Marque las hojas/tablas necesarias → **Transform Data** para abrir **Power Query**.
3. **Para cada consulta**:
   - Ajuste **Tipos de datos** correctamente (fecha, texto, número entero/decimal).
   - **Renombre** columnas a nombres claros (sin espacios excesivos).
   - **Quitar errores** y **filtrar valores atípicos** si se requieren.
4. En la tabla del restaurante, **Group By** `Fecha` para obtener: `Ventas_Diarias`, `Cancelaciones_Diarias`, `Likes_Diarios` (igual que en Excel).
5. **Close & Apply** para cargar al modelo.

### B2) Modelado básico
1. Cree una tabla de calendario con DAX:
   - **Modeling > New Table**:
   ```
   Calendario = CALENDAR( MIN('Restaurante'[Fecha]), MAX('Restaurante'[Fecha]) )
   ```
2. Agregue columnas de fecha útiles:
   - **New Column** en `Calendario`:
   ```
   Año = YEAR('Calendario'[Date])
   Mes = FORMAT('Calendario'[Date], "YYYY-MM")
   NombreMes = FORMAT('Calendario'[Date], "MMMM")
   Semana = WEEKNUM('Calendario'[Date], 2)
   ```
3. **Relacione** `Calendario[Date]` (1) con `Restaurante[Fecha]` (*) (activa, dirección simple).

### B3) Medidas DAX esenciales
1. **Ventas Totales**:
   ```DAX
   Ventas Totales = SUM('Restaurante'[Ventas])
   ```
2. **Cancelaciones Totales**:
   ```DAX
   Cancelaciones Totales = SUM('Restaurante'[Pedidos_Cancelados])
   ```
3. **Tasa de Cancelación**:
   ```DAX
   Tasa Cancelacion = DIVIDE([Cancelaciones Totales], [Ventas Totales])
   ```
4. **Likes Totales**:
   ```DAX
   Likes Totales = SUM('Restaurante'[Likes_Redes])
   ```
5. **Costo Promedio (restaurante)**:
   ```DAX
   Costo Promedio = AVERAGE('Restaurante'[Costo_Promedio])
   ```
6. **Ventas Totales (semanas)** – si carga `tbl_semana`:
   ```DAX
   Ventas Semana = SUM('tbl_semana'[Ventas_Producto_A])
   Gastos Semana = SUM('tbl_semana'[Gastos_Publicidad])
   Costo por Venta = DIVIDE([Gastos Semana], [Ventas Semana])
   ```

### B4) Visualizaciones clave (Página 1: "Vista Ejecutiva")
1. **Cards** (tarjetas): `[Ventas Totales]`, `[Tasa Cancelacion]`, `[Likes Totales]`, `[Costo Promedio]`.
2. **Column chart**: `Ventas Totales` por `Calendario[Mes]`.
3. **Line chart**: `Cancelaciones Totales` por `Calendario[Mes]`.
4. **Bar chart**: `Ventas Totales` por `Plato` (Top N 5 si aplica).
5. **Scatter (opcional)**: `Costo Promedio` vs `Ventas Totales` por `Plato`.
6. **Slicers**: `Año`, `Plato`.

**Formato rápido**
- Active **Data labels** donde aporte claridad.
- Cambie títulos de visuales: “Ventas por Mes”, “Cancelaciones por Mes”, etc.
- *View > Themes*: escoja un tema sobrio y consistente.

### B5) Interacciones y experiencia de usuario
1. **Editar interacciones**: Seleccione un visual → *Format > Edit interactions* → ajuste cómo afectan a otros (filtrar, resaltar, ninguna).
2. **Tooltips**: Agregue `Costo Promedio` como tooltip en gráficos de ventas.
3. **Drill down**: Active el botón de **Drill** para pasar de **Mes → Día** con la jerarquía de `Calendario`.

### B6) Validación y lectura de insights
1. ¿Crecen las ventas en fines de semana? (use filtro por día de la semana si agrega columna `DiaSemana = WEEKDAY('Calendario'[Date],2)`).
2. ¿Qué plato concentra más ventas y más cancelaciones? (comparar bar chart y tasa).
3. ¿El gasto publicitario está alineado con las ventas semanales? (use `tbl_semana`).

---
## PARTE C — De visual a decisión (20–30 min)
1. Haga que cada equipo enuncie **una decisión accionable** basada en el dashboard.
2. Ejemplos:
   - “Aumentar inventario de *Plato X* los viernes”.
   - “Reducir gasto publicitario en semanas con baja conversión”.
   - “Probar promoción cruzada en el plato con mayor margen”.

---
## Entregables de clase
- **Excel**: hoja *Dashboard_Excel* con al menos 2 gráficos y 1 KPI.
- **Power BI**: archivo `.pbix` con medidas DAX, 1 página ejecutiva con tarjetas y gráficos, y al menos 1 slicer.

## Rúbrica rápida (auto-evaluación)
- **Claridad** (títulos, ejes, unidades): 25%
- **Corrección técnica** (tipos de datos, medidas DAX): 35%
- **Lectura de insights** (decisión respaldada en datos): 25%
- **Diseño/UX** (consistencia visual, filtros útiles): 15%

---
## Troubleshooting (errores comunes)
- **Medidas que no suman correctamente**: verifique tipo de dato y que sea **Measure** (no columna calculada) cuando corresponda.
- **Fechas no ordenan por mes**: Use columna `Mes` = `FORMAT(Date, "YYYY-MM")` y *Sort by Column*.
- **Relación inactiva**: active la relación correcta en *Model view*.
- **Valores en blanco**: revise pasos de Power Query (tipos erróneos o filas con nulos).

---
## Apéndice — Atajos y buenas prácticas
**Excel**
- Nombrar tablas y usar **referencias estructuradas**.
- Usar **Validación de datos** para listas de parámetros.
- Documente supuestos en una hoja "README".

**Power BI**
- Mantenga nombres consistentes `Tabla[Columna]` y **prefijos** para medidas (ej. `m_` si lo prefiere).
- Una tabla de calendario **única** para todas las fechas.
- Guarde versiones del `.pbix` con fecha (ej. `BI_Sesion1_2025-08-22.pbix`).

---
**Fin de la guía.**

