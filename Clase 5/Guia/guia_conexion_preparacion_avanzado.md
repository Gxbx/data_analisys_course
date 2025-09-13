# 📘 Guía de Clase 1.6 – Conexión y Preparación de Datos (Avanzado)

## 🎯 Objetivo de la sesión
Aprender a conectar Power BI a fuentes modernas de datos (Google Drive, OneDrive, Kaggle) y preparar la información de acuerdo al tipo de variable para optimizar las visualizaciones.

---

## 🌐 1. Conexión a Google Drive
1. Sube tu archivo CSV o Excel a Google Drive.
2. Haz clic derecho → **Compartir** → “Cualquiera con el enlace”.
3. Copia el ID del archivo (ejemplo en URL:  
   `https://drive.google.com/file/d/1AbCdEfGhIjKlMnOpQr/view?usp=sharing` → ID es `1AbCdEfGhIjKlMnOpQr`).
4. Construye un enlace de descarga directa:
   ```
   https://drive.google.com/uc?export=download&id=ID_DEL_ARCHIVO
   ```
5. En Power BI: **Inicio → Obtener datos → Web → Pegar enlace**.

👉 Ahora cada vez que actualices el archivo en Drive, podrás refrescar en Power BI.

---

## ☁️ 2. Conexión a OneDrive / SharePoint
1. Guarda tu archivo en OneDrive personal o corporativo.
2. Haz clic derecho → Compartir → Copiar vínculo.
3. Modifica el final de la URL para que termine en `?download=1`.
4. En Power BI: **Obtener datos → Web** → pega la URL.

✅ Ventaja: sincronización automática entre OneDrive y Power BI.

---

## 📂 3. Conexión a Datasets de Kaggle
1. Descarga un dataset desde [Kaggle Datasets](https://www.kaggle.com/datasets).
   - Recomendado: **Superstore Sales Dataset**.
2. Importa el archivo CSV/Excel en Power BI:  
   **Inicio → Obtener datos → Texto/CSV**.
3. (Opcional avanzado) Si usas la **Kaggle API** con Python, puedes automatizar la descarga de datasets.

---

## 🛠️ 4. Preparación de Datos según el Tipo de Variable

### 4.1 Variables Categóricas
- Ejemplos: Segmento de cliente, Región, Categoría de producto.
- Visualizaciones recomendadas:
  - Barras / Columnas para comparar categorías.
  - Pie o Donut solo si son pocas categorías.
- Ejercicio: Ventas por Categoría de Producto.

### 4.2 Variables Numéricas
- Ejemplos: Ventas, Descuento, Ganancia.
- Visualizaciones recomendadas:
  - Tarjetas KPI (totales, promedios).
  - Histogramas (distribución de valores).
- Ejercicio: Monto total de ventas en una tarjeta.

### 4.3 Variables Temporales
- Ejemplos: Fecha de pedido, Año, Mes.
- Buenas prácticas:
  - Crear jerarquías de fecha (Año → Trimestre → Mes → Día).
- Visualizaciones recomendadas:
  - Línea (tendencias).
  - Área apilada (evolución acumulada).
- Ejercicio: Ventas mensuales.

### 4.4 Datos Relacionales
- Combina tablas: Clientes + Ventas.
- Visualizaciones recomendadas:
  - Mapas si hay datos de ubicación.
  - Tablas dinámicas con jerarquías.
- Ejercicio: Ventas por Región y Cliente.

---

## 💡 5. Ejercicio en Clase
1. Conecta un archivo CSV desde Google Drive.
2. Limpia los datos en Power Query:
   - Elimina duplicados.
   - Ajusta tipos de datos (Fecha → Date).
   - Reemplaza nulos en la columna “Ventas”.
3. Construye un mini-dashboard con:
   - Gráfico de barras: Ventas por Categoría.
   - Gráfico de línea: Ventas mensuales.
   - Tarjeta KPI: Ventas totales.

---

## ✅ 6. Tarea / Proyecto
- Descarga un dataset de Kaggle (por ejemplo, **Netflix Titles**, **Superstore Sales** o **Titanic Dataset**).
- Conéctalo a Power BI.
- Prepara:
  - 2 variables categóricas.
  - 1 variable numérica.
  - 1 variable temporal.
- Construye un dashboard de 4 visualizaciones optimizadas.

---
