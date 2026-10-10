# Clase 01 — Diseño y personalización de informes interactivos

**Diplomado:** Análisis de Datos e Inteligencia de Negocios — Unimayor  
**Módulo 2 V2.0:** Analítica avanzada, IA y toma de decisiones con Power BI  
**Tema:** 2.1 Diseño y personalización de informes interactivos  
**Duración sugerida:** 4 horas  
**Caso de trabajo:** Oferta de alojamientos de corta estancia en Bogotá  

---

## 1. Propósito de la clase

En esta sesión construiremos el primer borrador de un **dashboard ejecutivo interactivo** en Power BI usando datos reales de **Inside Airbnb para Bogotá**.

La idea central de la clase será:

> Un dashboard no se diseña para mostrar todos los datos disponibles. Se diseña para ayudar a responder una pregunta y tomar una decisión.

El objetivo no es hacer el informe “más bonito”, sino aprender a decidir **qué debe verse primero, qué debe poder filtrarse y qué visualización ayuda mejor a interpretar el caso**.

Al finalizar la sesión, cada estudiante o grupo tendrá una primera página ejecutiva construida sobre un dataset real y trazable.

---

## 2. Objetivos de aprendizaje

Al finalizar la clase podrás:

1. Definir la audiencia y la decisión principal de un dashboard.
2. Cargar y revisar un dataset real en Power BI.
3. Preparar las variables mínimas necesarias para el análisis.
4. Crear medidas básicas para una página ejecutiva.
5. Diseñar una jerarquía visual clara.
6. Incorporar segmentadores e interacciones útiles.
7. Utilizar mapas y visualizaciones comparativas de forma responsable.
8. Usar IA como apoyo para diseñar el informe sin delegar la validación de los datos.

---

## 3. Dataset de trabajo — Inside Airbnb Bogotá

Trabajaremos con información pública compilada por **Inside Airbnb** a partir de información disponible públicamente en Airbnb.

Para que todos los estudiantes trabajen con el mismo corte, esta guía utiliza el dataset de:

**Bogotá, Distrito Capital, Colombia — 14 de septiembre de 2026.**

Inside Airbnb ofrece para Bogotá archivos de alojamientos, calendario, reseñas y barrios. Para esta primera clase utilizaremos principalmente el archivo resumido `listings.csv`, que la propia fuente identifica como apropiado para visualizaciones.

### Fuente oficial

- Página de descarga: <https://insideairbnb.com/get-the-data/>
- Dataset `listings.csv` usado en clase: <https://data.insideairbnb.com/colombia/dc/bogot%C3%A1/2026-09-14/visualisations/listings.csv>
- Reseñas resumidas: <https://data.insideairbnb.com/colombia/dc/bogot%C3%A1/2026-09-14/visualisations/reviews.csv>
- Barrios: <https://data.insideairbnb.com/colombia/dc/bogot%C3%A1/2026-09-14/visualisations/neighbourhoods.csv>
- Barrios GeoJSON: <https://data.insideairbnb.com/colombia/dc/bogot%C3%A1/2026-09-14/visualisations/neighbourhoods.geojson>
- Diccionario de datos: <https://docs.google.com/spreadsheets/d/1iWCNJcSutYqpULSQHlNyGInUvHg2BoUGoNRIGa6Szc4/edit?usp=sharing>
- Supuestos y limitaciones: <https://insideairbnb.com/data-assumptions/>

### Licencia

Inside Airbnb publica estos datos bajo **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

> Importante: Inside Airbnb no está asociado ni respaldado por Airbnb. El dataset es una fotografía de los alojamientos observados en una fecha determinada y las ubicaciones publicadas están anonimizadas aproximadamente alrededor de la dirección real.

---

## 4. ¿Qué contiene `listings.csv`?

El archivo resumido incluye variables suficientes para construir un dashboard atractivo sin necesidad de un modelo complejo.

| Campo | Significado para esta práctica |
|---|---|
| `id` | Identificador único del alojamiento. |
| `name` | Nombre publicado del alojamiento. |
| `host_id` | Identificador del anfitrión. |
| `host_name` | Nombre público del anfitrión. |
| `neighbourhood` | Barrio o zona geográfica asignada. |
| `latitude` | Latitud aproximada. |
| `longitude` | Longitud aproximada. |
| `room_type` | Tipo de alojamiento/habitación. |
| `price` | Precio diario publicado en moneda local. |
| `minimum_nights` | Número mínimo de noches. |
| `number_of_reviews` | Cantidad acumulada de reseñas. |
| `reviews_per_month` | Promedio de reseñas por mes. |
| `calculated_host_listings_count` | Número de alojamientos del anfitrión en el corte. |
| `availability_365` | Días marcados como disponibles durante los próximos 365 días. |
| `number_of_reviews_ltm` | Reseñas registradas durante los últimos 12 meses. |

### Una precaución importante

**`availability_365` no equivale a ocupación.** Un día puede aparecer como no disponible porque fue reservado o porque el anfitrión lo bloqueó. No debemos concluir automáticamente que “menos disponibilidad = más reservas”.

Esta limitación será útil durante la clase para recordar que un dashboard puede ser visualmente correcto y, aun así, producir una interpretación incorrecta si no comprendemos el significado de las variables.

---

## 5. Pregunta detonante

Antes de abrir Power BI responde:

> Si estuvieras evaluando la oferta de alojamientos de corta estancia en Bogotá, ¿qué decisión querrías poder tomar después de observar el dashboard durante 60 segundos?

Algunas posibilidades:

- ¿En qué zonas se concentra la oferta?
- ¿Cómo cambia el precio según la zona y el tipo de alojamiento?
- ¿Qué tipos de alojamiento dominan el mercado observado?
- ¿Qué zonas combinan mayor oferta con precios más altos o bajos?
- ¿Qué alojamientos o segmentos acumulan más reseñas?
- ¿Cómo cambia el panorama cuando filtramos por tipo de habitación?

No existe una única respuesta correcta. Lo importante es **declarar la decisión antes de diseñar la página**.

---

## 6. Actividad 1 — Definir audiencia y decisión

En parejas o grupos, seleccionen una audiencia.

| Audiencia | Ejemplo de decisión |
|---|---|
| Anfitrión | ¿Cómo debería posicionar un nuevo alojamiento frente a la oferta existente? |
| Inversionista | ¿Qué zonas vale la pena estudiar con mayor detalle? |
| Turista | ¿Qué zonas y tipos de alojamiento se ajustan mejor a mi presupuesto? |
| Analista turístico | ¿Cómo se distribuye espacialmente la oferta observada en la ciudad? |

Completen:

| Pregunta | Respuesta del grupo |
|---|---|
| ¿Quién usará el dashboard? |  |
| ¿Qué decisión debe facilitar? |  |
| ¿Qué indicador necesita ver primero? |  |
| ¿Qué comparaciones necesita hacer? |  |
| ¿Qué filtros necesita? |  |
| ¿Qué afirmación NO podremos hacer con estos datos? |  |

---

## 7. Actividad 2 — Descargar el dataset

### Paso 1. Abrir la fuente

1. Abre <https://insideairbnb.com/get-the-data/>.
2. Busca **Bogotá, Distrito Capital, Colombia**.
3. Verifica el corte del **14 de septiembre de 2026**.
4. Descarga **`listings.csv` — Summary information and metrics**.

También puedes utilizar el enlace directo:

<https://data.insideairbnb.com/colombia/dc/bogot%C3%A1/2026-09-14/visualisations/listings.csv>

### Paso 2. Guardar el archivo

Crea una carpeta para la práctica y guarda el archivo como:

```text
listings_bogota_2026-09-14.csv
```

No cambies todavía su contenido desde Excel. Queremos que **Power Query sea el encargado de la preparación**.

---

## 8. Actividad 3 — Cargar el archivo en Power BI

### Paso 1. Abrir Power BI Desktop

1. Abre **Power BI Desktop**.
2. Crea un informe en blanco.
3. Ve a **Inicio > Obtener datos > Texto/CSV**.
4. Selecciona `listings_bogota_2026-09-14.csv`.

### Paso 2. No cargar todavía

Cuando aparezca la vista previa:

1. Comprueba que las columnas se reconocen correctamente.
2. Selecciona **Transformar datos**.

Esto abrirá Power Query.

### Paso 3. Renombrar la consulta

En el panel izquierdo cambia el nombre de la consulta a:

```text
airbnb_listings
```

A partir de este punto utilizaremos ese nombre en toda la guía.

---

## 9. Actividad 4 — Revisar los datos en Power Query

No necesitamos transformar todo el dataset. El objetivo es preparar únicamente lo necesario para el dashboard.

### Paso 1. Revisar tipos de datos

Verifica especialmente:

| Campo | Tipo recomendado |
|---|---|
| `id` | Número entero / entero de 64 bits |
| `host_id` | Número entero / entero de 64 bits |
| `latitude` | Número decimal |
| `longitude` | Número decimal |
| `price` | Número decimal |
| `minimum_nights` | Número entero |
| `number_of_reviews` | Número entero |
| `reviews_per_month` | Número decimal |
| `calculated_host_listings_count` | Número entero |
| `availability_365` | Número entero |
| `number_of_reviews_ltm` | Número entero |

### Paso 2. Limpiar `price`

Si Power BI interpreta `price` como texto porque contiene el símbolo `$`:

1. Selecciona la columna `price`.
2. Ve a **Transformar > Reemplazar valores**.
3. Reemplaza `$` por vacío.
4. Si existen separadores de miles con coma, reemplaza `,` por vacío.
5. Cambia el tipo a **Número decimal**.

Si Power BI genera errores por configuración regional:

1. Haz clic derecho en `price`.
2. Selecciona **Cambiar tipo > Usar configuración regional**.
3. Tipo: **Número decimal**.
4. Configuración regional: una que interprete correctamente el formato del archivo, por ejemplo **English (United States)**.

### Paso 3. Revisar nulos

Comprueba si existen valores nulos en:

- `price`
- `reviews_per_month`
- `last_review`

No elimines filas automáticamente. Pregunta primero:

> ¿El nulo significa un error o simplemente ausencia de información?

Por ejemplo, un alojamiento sin `reviews_per_month` puede ser un alojamiento sin reseñas recientes.

### Paso 4. Conservar las columnas necesarias

Para una versión simplificada de la clase puedes trabajar con:

```text
id
name
host_id
host_name
neighbourhood
latitude
longitude
room_type
price
minimum_nights
number_of_reviews
reviews_per_month
calculated_host_listings_count
availability_365
number_of_reviews_ltm
```

Puedes conservar todas las columnas si prefieres explorar libremente.

### Paso 5. Aplicar cambios

Selecciona **Inicio > Cerrar y aplicar**.

---

## 10. Actividad 5 — Conocer el dataset antes de diseñar

Antes de crear el dashboard, responde desde la vista de datos:

1. ¿Cuántos registros tenemos?
2. ¿Cuántos barrios aparecen?
3. ¿Qué valores existen en `room_type`?
4. ¿Existen precios nulos o iguales a cero?
5. ¿Cuál es el rango aproximado de `price`?
6. ¿Hay alojamientos con un número extremadamente alto de `minimum_nights`?

> No construyas visualizaciones todavía. Primero entiende las variables con las que vas a trabajar.

---

## 11. Actividad 6 — Crear medidas básicas

En **Modelado > Nueva medida**, crea:

### Total de alojamientos

```DAX
Alojamientos = DISTINCTCOUNT(airbnb_listings[id])
```

### Precio promedio

```DAX
Precio Promedio = AVERAGE(airbnb_listings[price])
```

### Reseñas totales

```DAX
Reseñas Totales = SUM(airbnb_listings[number_of_reviews])
```

### Disponibilidad promedio

```DAX
Disponibilidad Promedio = AVERAGE(airbnb_listings[availability_365])
```

### Reseñas últimos 12 meses

```DAX
Reseñas Últimos 12 Meses = SUM(airbnb_listings[number_of_reviews_ltm])
```

### Anfitriones

```DAX
Anfitriones = DISTINCTCOUNT(airbnb_listings[host_id])
```

> En esta clase no buscamos profundizar en DAX. Las medidas son deliberadamente sencillas para concentrarnos en diseño e interacción. DAX avanzado será trabajado en clases posteriores.

---

## 12. Actividad 7 — Bocetar antes de construir

Antes de añadir gráficos, dibuja rápidamente esta estructura en una hoja o tablero:

```text
┌──────────────────────────────────────────────────────────────┐
│  Título + contexto + filtros principales                    │
├────────────┬────────────┬────────────┬───────────────────────┤
│ KPI 1      │ KPI 2      │ KPI 3      │ KPI 4                 │
├──────────────────────────┬───────────────────────────────────┤
│ Mapa / distribución      │ Comparación principal             │
├──────────────────────────┼───────────────────────────────────┤
│ Tipo de alojamiento      │ Detalle / ranking                 │
└──────────────────────────┴───────────────────────────────────┘
```

Pregunta antes de continuar:

> Si solo pudiera conservar un elemento de esta página, ¿cuál ayudaría más a la decisión definida por el grupo?

---

## 13. Actividad 8 — Construir la página ejecutiva

Crea una página llamada:

```text
Vista Ejecutiva
```

### Parte superior — KPI

Crea cuatro tarjetas. Una combinación sugerida es:

1. **Alojamientos**
2. **Precio Promedio**
3. **Reseñas Totales**
4. **Disponibilidad Promedio**

No es obligatorio utilizar estos cuatro. Si la audiencia elegida requiere otros, justifica el cambio.

### Visual 1 — Mapa de oferta

Usa:

- `latitude`
- `longitude`
- tamaño o leyenda según el objetivo del análisis

Ejemplos de campos para tooltip:

- `name`
- `neighbourhood`
- `room_type`
- `price`
- `number_of_reviews`

> Recuerda: las coordenadas publicadas están anonimizadas y no representan la dirección exacta del alojamiento.

#### Si el mapa no está disponible

Algunas instalaciones o cuentas pueden tener las visualizaciones de mapa restringidas. En ese caso utiliza temporalmente un gráfico de barras con `neighbourhood` y `[Alojamientos]`.

### Visual 2 — Precio por zona

Configura un gráfico de barras:

- Eje: `neighbourhood`
- Valor: `[Precio Promedio]`

Ordena de mayor a menor y limita la vista a una cantidad razonable de zonas si el gráfico queda saturado.

### Visual 3 — Tipo de alojamiento

Configura un gráfico de barras o columnas:

- Eje: `room_type`
- Valor: `[Alojamientos]`

### Visual 4 — Actividad de reseñas

Una posibilidad:

- Eje: `neighbourhood`
- Valor: `[Reseñas Últimos 12 Meses]`

La métrica de reseñas **no es equivalente a reservas**, pero puede ayudar a estudiar actividad relativa del mercado observado.

---

## 14. Actividad 9 — Agregar segmentadores

Incluye al menos dos segmentadores.

### Segmentadores recomendados

- `neighbourhood`
- `room_type`
- `minimum_nights`
- `price`

Para esta primera página recomiendo comenzar con:

```text
neighbourhood
room_type
```

Después prueba el informe:

1. Selecciona un barrio.
2. Observa qué KPI cambian.
3. Selecciona un tipo de alojamiento.
4. Comprueba que mapa, precio y reseñas respondan al nuevo contexto.

---

## 15. Actividad 10 — Configurar interacciones

Un dashboard interactivo no significa que absolutamente todo deba filtrar todo.

### Paso a paso

1. Selecciona el gráfico de `room_type`.
2. Ve a **Formato > Editar interacciones**.
3. Observa los iconos que aparecen sobre los demás visuales.
4. Decide para cada visual si debe:
   - filtrar;
   - resaltar;
   - no verse afectado.
5. Repite con el gráfico por barrio.

### Pregunta de control

> ¿La interacción ayuda a responder la pregunta del usuario o solamente produce movimiento en la pantalla?

Si no ayuda, desactívala.

---

## 16. Actividad 11 — Tooltips y contexto

Los tooltips permiten mostrar detalle sin saturar la página.

En el mapa o en el gráfico principal agrega como tooltip:

- barrio;
- tipo de alojamiento;
- precio;
- reseñas;
- disponibilidad.

Luego pasa el cursor por varios puntos y revisa si la información adicional realmente aporta al análisis.

---

## 17. Actividad 12 — Mejorar la jerarquía visual

Aplica estas reglas:

### 1. Lo más importante debe verse primero

El KPI principal y el mensaje de la página deben estar en la zona superior.

### 2. Un color debe tener una función

No uses colores distintos solamente para “hacerlo llamativo”. Usa el color para:

- resaltar;
- diferenciar categorías;
- mostrar estados;
- orientar la lectura.

### 3. Los títulos deben explicar

Evita:

```text
Precio
Barrios
Tipos
```

Prefiere:

```text
Precio promedio por barrio
Distribución de la oferta por zona
Alojamientos según tipo de habitación
```

### 4. Elimina ruido

Revisa:

- bordes innecesarios;
- leyendas redundantes;
- etiquetas repetidas;
- exceso de colores;
- gráficos que responden la misma pregunta.

---

## 18. Actividad 13 — Reto de interpretación

Con el dashboard funcionando, responde solamente con información sustentada por el dataset:

1. ¿Qué zonas concentran más alojamientos en el corte observado?
2. ¿Cómo cambia el precio promedio según `room_type`?
3. ¿Qué ocurre con los KPI al seleccionar una zona concreta?
4. ¿Qué zonas concentran más reseñas durante los últimos 12 meses?
5. ¿Qué hallazgo merece ser investigado en una segunda página?

Después identifica una afirmación que **NO** deberías hacer.

Ejemplo:

> “Este barrio tiene mayor ocupación porque presenta menos disponibilidad.”

No podemos concluirlo directamente porque el calendario no diferencia entre noches reservadas y noches bloqueadas por el anfitrión.

---

## 19. Actividad 14 — IA como copiloto de diseño

Utiliza IA para recibir propuestas de diseño, pero entrégale primero el contexto del dataset.

### Prompt sugerido

```text
Actúa como analista de Business Intelligence.

Estoy trabajando con el archivo resumido listings.csv de Inside Airbnb para Bogotá, corte 14 de septiembre de 2026.

Las variables principales son:
id, host_id, neighbourhood, latitude, longitude, room_type, price, minimum_nights, number_of_reviews, reviews_per_month, calculated_host_listings_count, availability_365 y number_of_reviews_ltm.

Mi audiencia es [INDICAR AUDIENCIA] y la decisión que quiero facilitar es [INDICAR DECISIÓN].

Propón una página ejecutiva en Power BI con:
- máximo 4 KPI;
- máximo 4 visualizaciones;
- 2 o 3 filtros;
- una jerarquía visual clara;
- una pregunta que cada visual debería responder.

No inventes resultados ni valores. No interpretes availability_365 como ocupación. Si necesitas conocer un valor del dataset, indícame qué debo calcular o verificar.
```

### Después de usar IA responde

1. ¿Qué sugerencia fue útil?
2. ¿Qué sugerencia descartaste?
3. ¿Qué recomendación requería validar los datos?
4. ¿La IA intentó inferir algo que el dataset no permite afirmar?

### Regla del curso

> IA propone → analista verifica → datos sustentan → humano decide.

---

## 20. Checklist de calidad

Antes de entregar comprueba:

- [ ] La audiencia está explícitamente definida.
- [ ] La página responde una decisión concreta.
- [ ] El KPI principal se entiende rápidamente.
- [ ] Los títulos explican qué muestra cada visual.
- [ ] El mapa o su alternativa aportan al análisis.
- [ ] Los filtros cambian el contexto de forma útil.
- [ ] Las interacciones no generan ruido.
- [ ] Los colores son consistentes.
- [ ] El dashboard no interpreta `availability_365` como ocupación.
- [ ] Las conclusiones pueden rastrearse a los datos.
- [ ] La fuente del dataset está documentada.
- [ ] La página se puede comprender en aproximadamente 60 segundos.

---

## 21. Entregable de la clase

Cada grupo debe entregar:

1. Archivo `.pbix`.
2. Captura o PDF de la **Vista Ejecutiva**.
3. Una explicación breve que incluya:
   - audiencia;
   - decisión que facilita;
   - KPI principales;
   - un hallazgo sustentado por los datos;
   - una limitación del dataset;
   - una mejora pendiente para la siguiente clase.

### Requisitos mínimos del dashboard

- 4 KPI como máximo en la parte superior;
- al menos 3 visualizaciones;
- mínimo 2 segmentadores;
- al menos una interacción revisada manualmente;
- fuente de datos visible en el informe o en la entrega.

---

## 22. Rúbrica rápida

| Criterio | Nivel esperado |
|---|---|
| Propósito | El dashboard facilita una decisión claramente formulada. |
| Datos | La preparación respeta el significado de las variables. |
| Diseño | Existe jerarquía visual y el informe evita saturación. |
| Interactividad | Los filtros e interacciones aportan al análisis. |
| Indicadores | Los KPI son coherentes con la audiencia. |
| Interpretación | Los hallazgos están sustentados y reconocen limitaciones. |
| Trazabilidad | La fuente y fecha del dataset están documentadas. |

---

## 23. Cierre reflexivo

Responde individualmente:

> ¿Qué cambia cuando dejamos de diseñar “gráficos” y empezamos a diseñar una decisión?

---

## 24. Próxima clase

En la siguiente sesión trabajaremos:

**Uso de gráficos avanzados, indicadores clave de desempeño y KPI.**

El dashboard de Bogotá creado hoy será utilizado como punto de comparación para discutir cuándo una métrica se convierte realmente en un KPI y cuándo una visualización avanzada aporta valor —o solamente complejidad—.
