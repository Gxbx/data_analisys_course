
## 📝 Guía Práctica: De los Datos a la Inteligencia de Negocio

### 1. Preparación del Dataset

Para este ejercicio, usaremos dos tablas simples que puedes crear en una hoja de Excel o en un archivo de texto separado por comas (CSV).

**Tabla 1: `Ventas` (Tabla de Hechos)**

Esta tabla contiene las transacciones de ventas.

| ID_Venta | ID_Producto | ID_Cliente | Fecha | Cantidad | Monto |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | P-01 | C-101 | 01/01/2024 | 2 | 200 |
| 2 | P-02 | C-102 | 02/01/2024 | 1 | 150 |
| 3 | P-01 | C-101 | 03/01/2024 | 3 | 300 |
| 4 | P-03 | C-103 | 04/01/2024 | 5 | 50 |
| 5 | P-02 | C-102 | 05/01/2024 | 2 | 300 |

**Tabla 2: `Productos` (Tabla de Dimensiones)**

Esta tabla describe los productos.

| ID_Producto | Nombre_Producto | Categoria |
| :--- | :--- | :--- |
| P-01 | Laptop | Electrónica |
| P-02 | Teclado | Accesorios |
| P-03 | Mouse | Accesorios |

---

### 2. Paso a Paso con Power Query

El objetivo de este paso es **limpiar y transformar** los datos antes de cargarlos. Usaremos Power BI o Excel para seguir los pasos.

1.  **Conectar los datos:**
    -   Abre Power BI Desktop o Excel y ve a la pestaña **Obtener datos**.
    -   Selecciona **Excel Workbook** o **Texto/CSV** y carga las dos tablas.
    -   En la ventana del navegador, selecciona `Ventas` y `Productos`, y haz clic en **Transformar datos** para abrir el Editor de Power Query.

2.  **Transformar la tabla `Ventas`:**
    -   En la tabla `Ventas`, selecciona la columna `Fecha`.
    -   En la pestaña **Transformar**, asegúrate de que el tipo de datos sea **Fecha**. Power Query suele detectarlo automáticamente, pero es bueno verificarlo.
    -   **¡Tip!** Si la columna `Fecha` estuviera en un formato de texto incorrecto, aquí es donde lo arreglarías. Por ejemplo, si tuvieras `01-Ene-24`, tendrías que transformarla a un formato de fecha estándar.

3.  **Combinar las tablas:**
    -   Ahora, vamos a agregar la `Categoría` del producto a nuestra tabla de `Ventas`.
    -   Selecciona la tabla `Ventas`.
    -   En la pestaña **Inicio**, haz clic en **Combinar consultas** > **Combinar consultas como nueva**.
    -   En la ventana que aparece:
        -   Selecciona la primera tabla: `Ventas`.
        -   Selecciona la segunda tabla: `Productos`.
        -   Selecciona la columna `ID_Producto` en ambas tablas para establecer la relación.
        -   Elige el tipo de combinación **Left Outer** (Izquierda externa), que es el más común.
    -   Esto creará una nueva tabla. Haz clic en el encabezado de la columna `Productos` con la doble flecha para expandirla.
    -   Selecciona solo la columna `Categoria` y desmarca la opción de **Usar el nombre de la columna original como prefijo**. Haz clic en **Aceptar**. Ahora tu tabla `Ventas` extendida tiene la `Categoría` de cada producto.
    -   Renombra esta nueva tabla a `Ventas_Analisis`.

4.  **Cargar y cerrar:**
    -   Haz clic en **Cerrar y aplicar** en la pestaña **Inicio** para cargar los datos transformados en tu modelo de datos.

---

### 3. Creación de Medidas con DAX

Ahora que los datos están cargados y listos, usaremos DAX para crear cálculos útiles.

1.  **Medida 1: `Total de Ventas`**
    -   En Power BI Desktop, selecciona la tabla `Ventas_Analisis` en la vista de **Datos** o **Reporte**.
    -   Haz clic en **Nueva medida**.
    -   Introduce la siguiente fórmula DAX:

    ```dax
    Total de Ventas = SUM('Ventas_Analisis'[Monto])
    ```
    -   **Explicación:** Esta fórmula suma la columna `Monto` de la tabla `Ventas_Analisis`. Es una medida dinámica que se recalcula según los filtros que apliques en tus visualizaciones.

2.  **Medida 2: `Cantidad Promedio por Venta`**
    -   Crea una nueva medida.
    -   Introduce la siguiente fórmula DAX:

    ```dax
    Cantidad Promedio por Venta = AVERAGE('Ventas_Analisis'[Cantidad])
    ```
    -   **Explicación:** Calcula el promedio de la columna `Cantidad` para cada venta.

3.  **Crear una Columna Calculada (Opcional pero útil):**
    -   Selecciona la tabla `Ventas_Analisis`.
    -   Haz clic en **Nueva columna**.
    -   Introduce la siguiente fórmula:

    ```dax
    Margen = 'Ventas_Analisis'[Monto] - 'Ventas_Analisis'[Costo] // Asume que tienes una columna de costo
    ```
    -   **Explicación:** Esta columna se calcula una sola vez durante la carga de datos. Es útil para cálculos que no necesitan ser dinámicos.
