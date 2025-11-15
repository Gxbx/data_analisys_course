# 📊 Clase 2.7 — Predicciones Sencillas en Power BI  
## Usando el dataset “Store Sales – Time Series Forecasting” (Kaggle)

> **Nivel:** iniciación/intermedio  
> **Objetivo:** aprender a realizar predicciones simples usando tres métodos en Power BI:  
> 1. Pronóstico automático  
> 2. Parámetros “What-If”  
> 3. Script de Python (modelo simple)

---

## 📦 Dataset sugerido

**Store Sales – Time Series Forecasting (Kaggle):**  
https://www.kaggle.com/competitions/store-sales-time-series-forecasting

### Variables principales:
- `date` → fecha  
- `store_nbr` → número de tienda  
- `family` → categoría de producto  
- `sales` → ventas reales  
- `onpromotion` → productos en promoción  

---

# 1️⃣ PRONÓSTICO AUTOMÁTICO (SIN CÓDIGO)

El método más rápido y accesible para generar predicciones simples en Power BI.

---

## ✔️ Requisitos
- Un **gráfico de líneas**  
- Una **columna de fecha continua**  
- Una **medida numérica** (p. ej., ventas)

---

## 🔧 Paso a paso

1. Inserta un **Gráfico de líneas**.  
2. Eje X → `date`  
3. Eje Y → `sales` (Suma)  
4. En el panel derecho → ve a **Análisis (icono de lupa)**.  
5. Activa **Pronóstico (Forecast)**.  
6. Configura:  
   - **Longitud del pronóstico:** 3, 6 o 12 meses  
   - **Intervalo de confianza:** 95%  
   - **Color del pronóstico**  
7. Observa la línea proyectada al futuro.

---

## 📌 Interpretación
- Power BI usa un modelo **ETS (suavizado exponencial + componentes estacionales)**.  
- No requiere configuración técnica.  
- Perfecto para ventas, series mensuales o semanales.

---

# 2️⃣ PARÁMETROS WHAT-IF (SIMULACIONES)

Permite proyectar escenarios simulando cambios en los valores (crecimiento, incremento, descuentos, etc.)

---

## ✔️ Paso a paso: crear un parámetro

1. Ve a **Modelado → Parámetros → Nuevo parámetro What-If**  
2. Configura:  
   - Nombre: **Crecimiento esperado**  
   - Tipo: **Decimal number**  
   - Mínimo: `0.00`  
   - Máximo: `0.30`  
   - Incremento: `0.05`  
3. Power BI creará automáticamente:  
   - Una tabla del parámetro  
   - Una medida:  
     `Crecimiento esperado Value`

---

## ✔️ Crear una medida proyectada

```DAX
Ventas Proyectadas =
SUM(dataset[sales]) * (1 + 'Crecimiento esperado'[Crecimiento esperado Value])
```

## ✔️ Visualización

1. Inserta una tarjeta para mostrar Ventas Proyectadas.

2. Inserta un gráfico de líneas con:

    - Serie 1 → Ventas reales

    - Serie 2 → Ventas proyectadas

3. Mueve el slider del parámetro para simular escenarios.

📌 Ventajas

- No requiere conocimiento estadístico.

- Útil para análisis de negocio, no necesariamente para predicción real.

- Se usa mucho en empresas para “¿qué pasaría si subimos precios 10%?”