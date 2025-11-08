# 🗄️ Clase 2.6 — Conexión a bases de datos locales y uso del Power BI Gateway

> **Nivel:** intermedio  
> **Duración estimada:** 4 horas  
> **Objetivo:** aprender a conectar Power BI con una base de datos local (Access o SQLite), publicar el reporte en Power BI Service y configurar el Gateway para actualizar los datos automáticamente.

---

## 🧭 Contenido

1. Introducción al Gateway.  
2. Instalación de una base de datos local (Access o SQLite).  
3. Conexión desde Power BI Desktop.  
4. Creación del reporte y publicación.  
5. Instalación y configuración del Power BI Gateway.  
6. Programación de actualizaciones automáticas.  
7. Buenas prácticas y solución de problemas.

---

## 🧩 1️⃣ ¿Qué es el Power BI Gateway?

El **Power BI Gateway** es un conector que actúa como puente entre tus datos locales (PC, servidor o red privada) y la nube de Power BI Service.

```text
Power BI Desktop → Publicar → Power BI Service ↔ Gateway ↔ Fuente local (Access, SQL, etc.)
```
**Tipos de gateway:**

| Tipo                           | Uso                                     | Ideal para                             |
| ------------------------------ | --------------------------------------- | -------------------------------------- |
| **Personal (modo individual)** | Actualiza datasets del usuario actual   | Trabajo individual, demos, estudiantes |
| **Enterprise (modo estándar)** | Permite múltiples conexiones y usuarios | Empresas y entornos colaborativos      |

* Para esta clase usaremos el Gateway Personal.

## 🧱 2️⃣ Instalar una base de datos local
Opción A — Base de datos Access

1. Descarga el archivo de ejemplo ventas_local.accdb (puedes crear uno con Access).

2. Estructura de tabla recomendada:

| id | fecha      | producto  | categoria  | cantidad | precio_unitario | ciudad   |
| -- | ---------- | --------- | ---------- | -------- | --------------- | -------- |
| 1  | 2024-03-01 | Bicicleta | Deporte    | 2        | 1200000         | Cali     |
| 2  | 2024-03-03 | Casco     | Accesorios | 5        | 95000           | Bogotá   |
| 3  | 2024-03-04 | Guantes   | Accesorios | 10       | 45000           | Medellín |

Si no tienes Access, puedes abrir el archivo .accdb con el driver gratuito “Microsoft Access Database Engine”
Descarga: https://www.microsoft.com/en-us/download/details.aspx?id=54920

Opción B — Base de datos SQLite (alternativa liviana)

1. Descarga DB Browser for SQLite: https://sqlitebrowser.org/

2. Crea un archivo ventas.db con una tabla transacciones:

```sql
CREATE TABLE transacciones (
    id INTEGER PRIMARY KEY,
    fecha TEXT,
    producto TEXT,
    categoria TEXT,
    cantidad INTEGER,
    precio_unitario REAL,
    ciudad TEXT
);
```

3. Inserta algunos registros manualmente o con este script:
```sql
INSERT INTO transacciones (fecha, producto, categoria, cantidad, precio_unitario, ciudad) VALUES
('2024-03-01', 'Bicicleta', 'Deporte', 2, 1200000, 'Cali'),
('2024-03-03', 'Casco', 'Accesorios', 5, 95000, 'Bogotá'),
('2024-03-04', 'Guantes', 'Accesorios', 10, 45000, 'Medellín');
```

## ⚙️ 3️⃣ Conexión desde Power BI Desktop
Conectar a Access

1. Abre Power BI Desktop.
2. Clic en Obtener datos → Base de datos → Access Database (.accdb).
3. Navega hasta el archivo ventas_local.accdb.
4. Selecciona la tabla y haz clic en Cargar.

**Conectar a SQLite**

1. Descarga el controlador ODBC para SQLite desde:
https://www.ch-werner.de/sqliteodbc/

2. Instálalo (elige la versión 64 bits si tu Power BI lo es).

3. En Power BI Desktop:
**Obtener datos → ODBC → DSN = SQLite3 Datasource → Conectar.**

4. Elige la base de datos ventas.db.

## 📊 4️⃣ Crear el reporte

1. Crea una medida DAX:

``` DAX
Total Ventas = SUMX(ventas_local, ventas_local[cantidad] * ventas_local[precio_unitario])
```

2. Inserta un Gráfico de columnas:

    - Eje X → ciudad

    - Eje Y → Total Ventas

3. Inserta un Card visual con Total Ventas.

4. Guarda el archivo como Reporte_Ventas_Local.pbix.

## ☁️ 5️⃣ Publicar el reporte en Power BI Service

1. En la cinta superior → Publicar.

2. Elige tu área de trabajo (My Workspace).

3. Espera la confirmación:
“El informe se publicó correctamente en Power BI Service.”

4. Haz clic en “Abrir en Power BI” para ver el reporte en línea.

## 🔌 6️⃣ Instalar y configurar el Power BI Gateway (modo personal)
**Instalación:**

1. Descarga desde: https://powerbi.microsoft.com/es-es/gateway/

2. Ejecuta el instalador y elige Modo personal (Personal Mode).

3. Inicia sesión con tu misma cuenta de Power BI Service.

**Configuración:**

1. El Gateway se registrará automáticamente.

2. Verifica que esté “En línea” desde Power BI Service:
Configuración → Gateways → Estado: En línea ✅

## 🔁 7️⃣ Programar la actualización automática

1. En Power BI Service, ve a:
```Conjuntos de datos → Configuración (⚙️)```

2. En la sección Gateway de datos, selecciona el gateway disponible.

3. Asocia el origen (Access o SQLite) con el gateway.

4. En la sección Programar actualización (Scheduled refresh):

    - Activa el botón Mantener actualizado el conjunto de datos.

    - Define frecuencia (por ejemplo, diaria a las 8:00 AM).

5. Guarda los cambios.

```
💡 Consejo: asegúrate de que tu PC esté encendido y conectado a internet durante la hora de la actualización.
```