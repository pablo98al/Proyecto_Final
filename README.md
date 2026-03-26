# 🏠 Análisis del Mercado de Vivienda en España

## Proyecto final - Data & Analytics V3

**Proyecto Final — Data Analytics Master's | ThePower Business School**

---

## 📌 Descripción del proyecto

Este proyecto analiza el mercado inmobiliario español cruzando datos de anuncios de vivienda con indicadores socioeconómicos por provincia (población) extraídos del INE.

El objetivo es entender **qué factores influyen en el precio de la vivienda** en España: tamaño, tipo de inmueble, ubicación geográfica y contexto demográfico provincial. El análisis se enmarca en la actual crisis habitacional española, donde el acceso a la vivienda se ha convertido en uno de los problemas sociales más relevantes.

---

## 🗂️ Estructura del repositorio

```
Proyecto_Final/
│
├── 📁 Info_Proyecto/
│   └── Instrucciones.txt          # Requisitos del proyecto
│
├── 📁 Pre - Proyecto/
│   ├── 📁 Data_raw/
│   │   ├── 📁 Casas/              # 20 CSVs de vivienda por provincia (Kaggle)
│   │   └── 📁 Resto fuetes datos/ # Datos socioeconómicos del INE
│   │
│   ├── 📁 Notebook/
│   │   ├── Analisis_Preliminar.ipynb   # Exploración y limpieza inicial, para comprobar si el datasaet es apto
│   │   ├── addinfo.ipynb               # Procesamiento datos INE + merge
│   │   └── Rentas_PV.ipynb             # Procesamiento rentas País Vasco
│   │
│   └── 📁 Output/
│       ├── dataset_final.csv           # Dataset unificado y limpio
│       ├── poblacion.csv               # Datos de población INE procesados
│       └── rentas_PV.csv               # Rentas municipales País Vasco
│
├── 📁 Dashboard/
│   └── dashboard_vivienda.pbix         # Dashboard Power BI
│
├── INFORME.md                          # Informe detallado del análisis
└── README.md                           # Este archivo
```

---

## 📦 Fuentes de datos

| Fuente | Descripción | Registros originales |
|---|---|---|
| [Kaggle — Spanish Houses](https://www.kaggle.com/) | Anuncios de venta de vivienda por provincia | 100.000 filas, 36 columnas |
| [INE — Padrón Municipal](https://www.ine.es/) | Población total por provincia | 52 provincias |

Los datos de vivienda se obtuvieron de 20 archivos CSV individuales (uno por provincia), que fueron combinados en un único dataset mediante `glob` y `pd.concat`.

---

## 🔧 Proceso de trabajo

### 1. Exploración y carga de datos

- Identificación de 20 archivos CSV de vivienda cubriendo 20 provincias españolas
- Verificación de consistencia de columnas entre archivos
- Carga combinada mediante `glob` y `pd.concat` → **100.000 filas, 36 columnas**

### 2. Limpieza y transformación

- Conversión de tipos de datos: numéricos, datetime, booleanos
- Eliminación del separador de miles (`.`) en columnas numéricas
- Cálculo de porcentaje de nulos por columna
- Eliminación de **16 columnas** con nulos al 100%, conservando **20 columnas útiles**
- Estandarización de valores de provincia (minúsculas, sin tildes)

### 3. Integración de datos del INE

- Carga y procesamiento del padrón municipal del INE
- Normalización de nombres de provincia para compatibilidad con el dataset de vivienda
- Filtrado para conservar únicamente registros de población total (`source == 'Poblacion'`)

### 4. Merge de datasets

- Join por columna `provincia` (left join)
- Incorporación de `poblacion_total` y `cod_provincia` al dataset principal
- Creación de columna derivada `price_tramo` con segmentación de precios: `Bajo`, `Medio-Bajo`, `Medio-Alto`, `Alto`

### 5. Dataset final

El dataset resultante cumple todos los requisitos del proyecto:

| Métrica | Valor |
|---|---|
| Filas | 100.000 |
| Columnas | 22 |
| Provincias cubiertas | 20 |
| Fuentes combinadas | 2 (Kaggle + INE) |

---

## 📊 Variables del dataset final

| Columna | Tipo | Descripción |
|---|---|---|
| `ad_last_update` | texto | Última actualización del anuncio |
| `bath_num` | numérico | Número de baños |
| `condition` | categórico | Estado del inmueble |
| `construct_date` | fecha | Año de construcción |
| `energetic_certif` | categórico | Certificación energética |
| `house_id` | numérico | ID único del inmueble |
| `house_type` | categórico | Tipo de inmueble (piso, chalet, etc.) |
| `loc_city` | texto | Municipio |
| `loc_district` | texto | Distrito / calle |
| `loc_full` | texto | Dirección completa |
| `loc_zone` | texto | Zona geográfica |
| `m2_real` | numérico | Superficie total (m²) |
| `m2_useful` | numérico | Superficie útil (m²) |
| `obtention_date` | fecha | Fecha de extracción del anuncio |
| `orientation` | categórico | Orientación del inmueble |
| `price` | numérico | Precio de venta (€) |
| `room_num` | numérico | Número de habitaciones |
| `provincia` | texto | Provincia (clave de join) |
| `source` | texto | Fuente del dato INE |
| `poblacion_total` | numérico | Población de la provincia |
| `cod_provincia` | numérico | Código INE de provincia |
| `price_tramo` | categórico | Segmento de precio |

---

## 📈 Dashboard

El dashboard de Power BI permite explorar de forma interactiva:

- Distribución de precios por provincia y tipo de vivienda
- Relación entre metros cuadrados y precio
- Segmentación por tramo de precio (`price_tramo`)
- Comparativa provincial cruzando precio medio con población

---

## 🛠️ Herramientas utilizadas

- **Python 3.12** — procesamiento y análisis
- **Pandas** — manipulación de datos
- **glob / os** — carga automática de múltiples archivos
- **Visual Studio Code** — entorno de desarrollo
- **Power BI** — visualización y dashboard
- **GitHub** — control de versiones y entrega del proyecto

---

## 📝 Informe del análisis

El informe completo del análisis exploratorio, incluyendo estadísticas descriptivas, correlaciones y conclusiones, está disponible en [`INFORME.md`](./INFORME.md).

---

## 👤 Autor

**Pablo**
