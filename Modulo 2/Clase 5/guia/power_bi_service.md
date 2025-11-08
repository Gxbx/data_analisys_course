# ☁️ Clase 2.5 — Publicación y Actualización de Informes en Power BI Service

> **Nivel:** intermedio – profesional  
> **Duración estimada:** 4–5 horas  
> **Objetivo:** aprender a publicar, actualizar y compartir informes en Power BI Service (entorno corporativo), aplicando buenas prácticas de conexión, seguridad y automatización.

---

## 🧭 Contenido de la clase

1. Concepto y arquitectura de Power BI Service.  
2. Publicación de reportes desde Power BI Desktop.  
3. Configuración de espacios de trabajo (Workspaces).  
4. Creación de dashboards en la nube.  
5. Programación de actualizaciones automáticas.  
6. Administración de permisos y seguridad.  
7. Buenas prácticas en entornos empresariales.

---

## ☁️ 1. ¿Qué es Power BI Service?

**Power BI Service** es la plataforma en la nube donde se alojan, actualizan y comparten los informes creados en **Power BI Desktop**.

**Flujo general:**
```text
Fuentes de datos → Power BI Desktop → Publicar → Power BI Service → Dashboards compartidos
```

**Ventajas principales:**

- Acceso desde cualquier dispositivo.
- Control de versiones y permisos.
- Actualización automática de datasets.
- Integración con Teams, SharePoint y Excel.

## 🧱 2. Publicar desde Power BI Desktop
**Paso 1️⃣ — Preparar el archivo**

1. Abre tu archivo .pbix en Power BI Desktop.
2. Verifica que los datos se carguen correctamente y que todas las consultas estén sin errores.
3. Guarda los cambios finales.

**Paso 2️⃣ — Iniciar sesión**

1. En Power BI Desktop, ve a Archivo → Opciones y configuración → Cuentas.
2. Inicia sesión con tu cuenta corporativa de Microsoft (Office 365, Azure AD, etc.).
3. Si no tienes una, usa una cuenta educativa o de organización personal que tenga habilitado Power BI Service.

**Paso 3️⃣ — Publicar**

1.  En la cinta superior selecciona Publicar → Mi área de trabajo o el workspace asignado.

2. Espera la confirmación:
```
El informe se publicó correctamente en Power BI Service.
```

3. Haz clic en Abrir en Power BI para ver el reporte en línea.

## 🧩 3. Configuración de espacios de trabajo (Workspaces)

Un Workspace es el entorno colaborativo donde se guardan datasets, reportes y dashboards.

Tipos de Workspaces:

| Tipo                                  | Uso                                             | Permisos                  |
| ------------------------------------- | ----------------------------------------------- | ------------------------- |
| **Mi área de trabajo (My Workspace)** | Personal, sin compartir.                        | Solo tú                   |
| **Workspace compartido**              | Proyectos colaborativos.                        | Editores / Visualizadores |
| **App Workspace**                     | Distribución de apps o dashboards corporativos. | Usuarios finales          |

Recomendación:
Para entornos de empresa, crea un Workspace por proyecto o cliente, y asigna roles específicos.

## 🧠 4. Estructura de objetos en Power BI Service

| Objeto        | Descripción                                          |
| ------------- | ---------------------------------------------------- |
| **Dataset**   | Fuente de datos que alimenta los informes.           |
| **Reporte**   | Visualizaciones interactivas creadas en Desktop.     |
| **Dashboard** | Vista personalizada con tiles de distintos reportes. |
| **App**       | Conjunto empaquetado de dashboards + reportes.       |

Cada publicación de .pbix genera un dataset y un reporte asociados.

## ⚙️ 5. Actualización automática de datos (Scheduled Refresh)
**Paso 1️⃣ — Configurar credenciales**

1. En Power BI Service, abre el reporte publicado.

2. Ve a Configuración → Conjuntos de datos (Datasets).

3. En “Credenciales de origen de datos”, elige el método de autenticación adecuado:

    - Organizacional (Office 365 / Azure AD)

    - Básica (usuario/contraseña)

    - Anónimo (si es un archivo público o CSV local)

*Asegúrate de marcar Mantener las credenciales actualizadas.

**Paso 2️⃣ — Programar actualización**

1. En el mismo panel, ve a Programar actualización (Scheduled refresh).

2. Activa Mantener actualizado el conjunto de datos.

3. Define:

    - Frecuencia: diaria, semanal o varias veces al día.

    - Hora exacta de actualización.

4. Guarda los cambios.

*📌 Consejo: Para archivos locales (CSV o Excel), usa Power BI Gateway (personal).

## 🔌 6. Instalar y configurar Power BI Gateway

Gateway es un conector que permite a Power BI Service acceder a datos almacenados localmente (en tu PC o red interna).

**Instalación:**

1. Descarga desde https://powerbi.microsoft.com/es-es/gateway/
2. Instálalo en un equipo que esté encendido cuando se ejecuten las actualizaciones.
3. Inicia sesión con tu cuenta corporativa.

**Configuración:**

1. Abre el Gateway Manager.

2. Registra un nuevo gateway → selecciona Modo personal.

3. En Power BI Service, asocia tu dataset al Gateway:

    - Configuración → Gateway → Seleccionar el gateway disponible.

✅ Si usas fuentes en la nube (Google Sheets, OneDrive, SQL Azure, etc.), no necesitas gateway.

## 📊 7. Crear dashboards en Power BI Service
**Paso 1️⃣ — Fijar visuales**

1. Abre tu reporte publicado.
2. Pasa el cursor sobre una visualización y selecciona el icono 📌 “Fijar en dashboard”.
3. Elige Dashboard nuevo o uno existente.

**Paso 2️⃣ — Personalizar el dashboard**

1. Reorganiza los tiles (arrastrar y soltar).
2. Agrega KPIs de diferentes reportes.
3. Usa el modo Pantalla completa / TV Mode para monitoreo.

## 🔐 8. Administración de permisos
Compartir reportes

1. Abre el reporte o dashboard.
2. Haz clic en Compartir → Invitar usuarios o grupos.
3. Elige el nivel de acceso:
    - Puede ver
    - Puede editar
4. Opcional: agrega un mensaje personalizado.

Roles recomendados:

| Rol         | Permisos             | Uso típico                |
| ----------- | -------------------- | ------------------------- |
| Viewer      | Solo lectura         | Usuarios finales          |
| Contributor | Editar reportes      | Analistas                 |
| Member      | Administrar datasets | Líderes de equipo         |
| Admin       | Control total        | Responsable del Workspace |


## 🔁 9. Actualización manual o desde Power BI Desktop

- Manual:
En Power BI Service → Dataset → botón Actualizar ahora.

- Desde Desktop:
Si el reporte cambió, abre Power BI Desktop → Publicar → Reemplazar versión existente.

💡 El dataset conserva la configuración de actualización automática.

## ⚡ 10. Buenas prácticas de publicación

| Práctica                                            | Descripción                                        |
| --------------------------------------------------- | -------------------------------------------------- |
| **Usar nombres estándar**                           | Evita confusión entre datasets y reportes.         |
| **Separar datasets y reportes**                     | Publica datasets maestros y usa “Live Connection”. |
| **Evitar rutas locales**                            | Prefiere OneDrive, SharePoint o bases en la nube.  |
| **Programar actualizaciones fuera de horario pico** | Mejora el rendimiento del servicio.                |
| **Documentar conexiones y credenciales**            | Facilita mantenimiento.                            |

## 🧠 11. Solución de problemas comunes

| Problema                            | Causa probable                               | Solución                                    |
| ----------------------------------- | -------------------------------------------- | ------------------------------------------- |
| ❌ Error en actualización automática | Falta gateway o credenciales expiradas       | Verifica credenciales y gateway activo      |
| ⚠️ Reporte no se actualiza          | Fuente local no conectada                    | Usa OneDrive o instala Gateway              |
| 🚫 Usuario no ve el dashboard       | Falta de permisos o licencia Pro             | Revisa roles o publica en Workspace Premium |
| 🕒 Actualización lenta              | Exceso de columnas / cálculos en Power Query | Optimiza consultas y modelos                |


## Recursos adicionales

Documentación  Power BI (https://learn.microsoft.com/es-es/power-bi) 