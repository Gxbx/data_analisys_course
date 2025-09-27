# Guía de práctica – Transformación y Modelado de Datos en Power BI
**Fecha:** 2025-09-27

Esta guía resume los temas vistos en las sesiones **1.8 (Transformación de datos)** y **1.9 (Modelado de datos)**, con ejemplos y datasets para práctica.

---

## 📂 Archivos disponibles (CSV)
- `clientes.csv` → Información de clientes.  
- `productos.csv` → Catálogo de productos.  
- `ventas.csv` → Registro de ventas (tabla de hechos).  
- `calendario.csv` → Tabla de calendario para análisis temporal.  

---

## 🔄 Transformaciones Avanzadas (Power Query)

### 1. Combinar datos
- **Merge Queries**: unir `ventas.csv` con `productos.csv` (por `ProductoID`) y con `clientes.csv` (por `ClienteID`).  
- **Append Queries**: simular apilar ventas de diferentes meses (duplicar `ventas.csv` y anexar).  

### 2. Columnas personalizadas
- Crear columna **ValorVenta** = `Cantidad * PrecioUnitario`.  
- Columna condicional: si `Segmento = "Corporativo"` → "Alta prioridad", sino "Normal".  

### 3. Normalización / Pivot-Unpivot
- Si hubiera columnas `Enero, Febrero, Marzo` → **Unpivot** para convertirlas en filas con campo `Mes`.  
- Ejemplo práctico: transformar métricas de ventas mensuales en una sola columna.  

### 4. Limpieza avanzada
- Quitar filas con `Monto` nulo.  
- Reemplazar valores faltantes en `Ciudad` por `"Sin definir"`.  
- Crear jerarquía de fechas desde `Fecha`.  

---

## 🏗 Modelado de Datos en Power BI

### 1. Identificar tablas
- **Hechos**: `ventas` (métricas transaccionales).  
- **Dimensiones**: `clientes`, `productos`, `calendario`.  

### 2. Crear relaciones
- `ventas[ClienteID]` → `clientes[ClienteID]` (1 a muchos).  
- `ventas[ProductoID]` → `productos[ProductoID]` (1 a muchos).  
- `ventas[Fecha]` → `calendario[Date]` (1 a muchos).  

### 3. Medidas DAX básicas
```DAX
Total Ventas = SUM(ventas[Monto])
Total Cantidad = SUM(ventas[Cantidad])
Clientes Únicos = DISTINCTCOUNT(ventas[ClienteID])
Promedio Ticket = DIVIDE([Total Ventas], [Clientes Únicos])
```

### 4. Medidas de tiempo (usando calendario)
```DAX
Ventas YTD = TOTALYTD([Total Ventas], calendario[Date])
Ventas Mes Anterior =
CALCULATE([Total Ventas], PREVIOUSMONTH(calendario[Date]))
Crecimiento YoY =
DIVIDE([Total Ventas] - CALCULATE([Total Ventas], SAMEPERIODLASTYEAR(calendario[Date])),
       CALCULATE([Total Ventas], SAMEPERIODLASTYEAR(calendario[Date])))
```

### 5. Buenas prácticas
- Usar **modelo estrella**: una tabla de hechos + varias dimensiones.  
- Mantener nombres claros y consistentes.  
- Guardar medidas en una tabla especial de medidas.  
- Eliminar columnas innecesarias para optimizar rendimiento.  

---

## 🎯 Ejercicio Final
1. Importar los 4 archivos CSV en Power BI.  
2. Realizar limpieza y transformaciones en Power Query:  
   - Combinar datos de clientes y productos.  
   - Crear columnas condicionales.  
3. Construir un **modelo estrella** con hechos y dimensiones.  
4. Crear al menos **4 medidas DAX** (total ventas, ticket promedio, clientes únicos, crecimiento YoY).  
5. Diseñar un dashboard con:  
   - Ventas por categoría de producto.  
   - Ventas por ciudad.  
   - Ventas acumuladas YTD.  
   - KPI de clientes únicos.  

---

✅ Con esta práctica, el estudiante vivirá el ciclo completo: **transformar datos → modelar → analizar → visualizar.**
