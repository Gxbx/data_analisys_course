# 📘 Guía práctica – Navengado en Power BI Desktop

## Objetivo de la sesión
Comprender en detalle el funcionamiento de la interfaz de Power BI Desktop, identificando las diferentes vistas, paneles y menús, para optimizar la creación de informes y análisis de datos.  

---

## 1. Explorando la Interfaz  

### 1.1 La Cinta de Opciones (Ribbon)  
- Identifica las pestañas: **Inicio, Insertar, Modelado, Vista, Ayuda**.  
- Acción práctica:  
  - Localiza la opción **Obtener Datos** y revisa los conectores disponibles.  
  - Refresca los datos de un reporte cargado.  

### 1.2 Vistas Principales  
- **Informe**: espacio para crear dashboards.  
- **Datos**: tabla de datos cargada.  
- **Modelo**: relaciones entre tablas.  
- Acción práctica:  
  - Cambia entre las tres vistas y observa qué cambia en el panel lateral.  

---

## 2. Paneles de Trabajo  

### 2.1 Panel de Visualizaciones  
- Contiene gráficos, tablas, KPIs y objetos.  
- Permite formatear los elementos del reporte.  
- Acción práctica:  
  - Inserta un gráfico de barras y cambia su formato (colores, títulos).  

### 2.2 Panel de Campos  
- Muestra las tablas y columnas disponibles.  
- Permite crear **medidas** y **columnas calculadas**.  
- Acción práctica:  
  - Crea una nueva medida que calcule las **Ventas Totales** con la fórmula:  

```DAX
Ventas Totales = SUM(Ventas[Monto])
```  

### 2.3 Panel de Filtros  
- Aplica filtros a:  
  - Visualización específica  
  - Página del informe  
  - Informe completo  
- Acción práctica:  
  - Filtra los clientes con **ventas mayores a 1000**.  

---

## 3. Vista de Modelo  
- Explora cómo se relacionan las tablas.  
- Tipos de relaciones: 1:1, 1:N, N:N.  
- Acción práctica:  
  - Conecta la tabla **Clientes** con la tabla **Ventas** usando el campo `ID_Cliente`.  

---

## 4. Consejos de Navegación Efectiva  
- Usa **temas predefinidos** para estandarizar colores.  
- Asegúrate de explorar los **atajos de teclado** más usados.  
- Mantén siempre este flujo de trabajo:  
  1. Conexión de datos  
  2. Modelado  
  3. Creación de informes  

---

## Dataset recomendado  

Para esta sesión se recomienda usar el dataset público:  

** "Superstore Sales Dataset" (Kaggle)**  
🔗 [Descargar en Kaggle](https://www.kaggle.com/datasets/abhisheksinghblr/superstore-dataset)  

### Contenido del dataset:  
- **Pedidos**: ID de pedido, fecha, cliente, categoría de producto.  
- **Productos**: nombre, categoría, subcategoría.  
- **Ventas**: cantidad, precio, descuento, ganancia.  
- **Clientes**: ID cliente, segmento, región, país.  

Este dataset es ideal porque:  
- Tiene múltiples tablas → permite practicar la **Vista de Modelo**.  
- Incluye variables numéricas y categóricas → perfectas para practicar **visualizaciones y filtros**.  
- Es muy usado en la comunidad → hay muchos ejemplos y tutoriales disponibles.  

---

## Tarea sugerida  
1. Importa el dataset en Power BI Desktop.  
2. Explora las tres vistas (Informe, Datos, Modelo).  
3. Crea un reporte con:  
   - Una visualización de ventas por región.  
   - Una medida DAX de **Ganancia Total**.  
   - Un filtro para mostrar solo los pedidos del segmento “Corporate”.  
