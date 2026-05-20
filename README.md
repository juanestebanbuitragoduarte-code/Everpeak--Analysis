# Everpeak--Analysis
# 📞 ConnectaTel: Análisis de Datos y Segmentación de Clientes (Latam - 2024)

Este proyecto realiza un proceso completo de **Análisis Exploratorio de Datos (EDA)**, limpieza, ingeniería de variables y segmentación de clientes para **ConnectaTel**, una compañía de telecomunicaciones en Latinoamérica. El objetivo principal es transformar datos brutos de consumo en *insights* ejecutivos listos para la toma de decisiones comerciales y la optimización de planes de suscripción.

---

## 🎯 Objetivo del Proyecto
*   **Limpieza de Datos:** Identificar y corregir anomalías estructurales como valores centinela (`-999`, `?`), fechas imposibles y datos faltantes.
*   **Perfilamiento de Usuarios:** Consolidar el historial de consumos (mensajes, llamadas, minutos) a nivel de usuario único.
*   **Análisis Estadístico y Visual:** Identificar comportamientos de consumo mediante histogramas y diagramas de caja (*boxplots*), determinando el impacto de los valores atípicos (*outliers*).
*   **Segmentación Estratégica:** Clasificar a los usuarios por rangos de edad y niveles de uso técnico para generar recomendaciones de negocio (*upselling*, retención y diseño de nuevos productos).

---

## 📊 Datasets Utilizados
El análisis se basa en la integración de dos conjuntos de datos principales:

1.  **`users`**: Contiene la información demográfica y contractual de la base de clientes.
    *   `user_id`: Identificador único del cliente.
    *   `age`: Edad del usuario.
    *   `city`: Ciudad de residencia en Latam.
    *   `plan`: Tipo de suscripción (ej. Básico/Surf o Premium/Ultimate).
    *   `reg_date`: Fecha de registro en la plataforma.
2.  **`usage`**: Contiene el registro histórico de las transacciones de consumo.
    *   `user_id`: Identificador para vinculación (Llave foránea).
    *   `type`: Tipo de servicio consumido (`call`, `text`, `data`).
    *   `duration`: Duración de la llamada en minutos (si aplica).
    *   `length`: Longitud o metadatos de la transacción.

---

## ⚙️ Etapas del Análisis Realizadas

### 🔹 Paso 1 & 2: Carga y Exploración Inicial
*   Inspección de las dimensiones de los DataFrames, tipos de datos nativos y detección temprana de nulos o anomalías lógicas.

### 🔹 Paso 3: Limpieza Básica de Datos
*   Sustitución de valores centinela numéricos (`-999` en edad) por la mediana del negocio.
*   Estandarización de caracteres inválidos (`?` en ciudad) a valores nulos nativos de Pandas (`pd.NA`).
*   Corrección y acotación de fechas imposibles (posteriores a 2024) utilizando `pd.NaT`.
*   Análisis del mecanismo de pérdida de datos, confirmando que los nulos en duración son **MAR** (*Missing At Random*) debido al tipo de servicio (SMS/Datos).

### 🔹 Paso 4: Summary Statistics e Ingeniería de Variables
*   Creación de banderas booleanas/numéricas de consumo (`is_text`, `is_call`).
*   Agrupación y agregación espacial (`.groupby().agg()`) para calcular: total de mensajes, total de llamadas y total de minutos por cada usuario único.
*   Combinación integradora (`pd.merge(..., how='left')`) para construir la tabla maestra `user_profile` y tratamiento de registros inactivos con `.fillna(0)`.

### 🔹 Paso 5: Visualización de Distribuciones y Outliers
*   Generación de histogramas comparativos mapeados por tipo de plan (`hue='plan'`) usando la paleta `['skyblue', 'green']`.
*   Automatización mediante bucles `for` para graficar *boxplots* de variables de consumo.
*   Cálculo matemático de barreras de tolerancia mediante el método del **Rango Intercuartílico (IQR)** y justificación de negocio para retener los datos extremos.

### 🔹 Paso 6: Segmentación de Clientes
*   **Por Uso:** Clasificación jerárquica en `Bajo uso`, `Uso medio` y `Alto uso` basada en la interacción de llamadas y mensajes utilizando lógica pura de Pandas (`.apply()`).
*   **Por Edad:** Clasificación demográfica en `Joven`, `Adulto` y `Adulto Mayor`.
*   Análisis visual volumétrico de los nuevos segmentos mediante gráficos de barras de frecuencia (`sns.countplot`).

### 🔹 Paso 7: Insight Ejecutivo
*   Traducción de métricas estadísticas a recomendaciones de negocio accionables dirigidas a los *stakeholders* de ConnectaTel.

---

## 🚀 Cómo Ejecutar el Notebook

El proyecto está diseñado para ejecutarse de manera interactiva en la nube o en un entorno local.

### Opción 1: Abrir en Google Colab (Recomendada)
1. Descarga el archivo `.ipynb` de este repositorio.
2. Ve a [Google Colab](https://colab.research.google.com/).
3. Selecciona la pestaña **Subir** (Upload) y arrastra el archivo del notebook.
4. Asegúrate de subir los archivos CSV de los datasets (`users.csv` y `usage.csv` u homólogos) a la sección de archivos de la barra lateral izquierda de Colab antes de correr las celdas.

### Opción 2: Entorno Local (Jupyter Notebook / VS Code)
Si prefieres correrlo en tu máquina, asegúrate de tener instalado Python 3.x y los paquetes requeridos:

```bash
pip install pandas numpy matplotlib seaborn
