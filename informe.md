# 📋 Informe del Análisis — Mercado de Vivienda en España

**Proyecto Final | ThePower Business School — Data Analytics**

---

## 1. Introducción y motivación

España atraviesa desde hace años una crisis habitacional que afecta especialmente a jóvenes y familias de rentas medias. Los precios de venta han escalado en las principales ciudades hasta niveles históricamente altos, mientras que los salarios no han crecido al mismo ritmo.

Este análisis toma como punto de partida esa realidad y busca responder preguntas concretas a partir de datos reales:

- ¿En qué provincias son más caras las viviendas?
- ¿Qué tipo de inmueble concentra los precios más elevados?
- ¿Cuánto explica el tamaño (m²) el precio de una vivienda?
- ¿Existe relación entre el tamaño poblacional de una provincia y el precio medio de la vivienda?

---

## 2. Fuentes de datos

### 2.1 Dataset de vivienda — Kaggle

- **Origen:** Kaggle (anuncios de portales inmobiliarios españoles)
- **Cobertura:** 20 provincias españolas
- **Formato:** 20 archivos CSV individuales por provincia
- **Registros brutos:** 100.000 filas × 36 columnas

Las provincias incluidas son: Álava, Albacete, Alicante, Illes Balears, Barcelona, Bizkaia, Cádiz, Ciudad Real, A Coruña, Gipuzkoa, Girona, Huelva, Madrid, Santa Cruz de Tenerife, Segovia, Sevilla, Soria, Valencia, Valladolid y Zamora.

### 2.2 Datos socioeconómicos — INE

- **Origen:** Instituto Nacional de Estadística (INE) — Padrón Municipal
- **Cobertura:** 52 provincias españolas
- **Variable incorporada:** Población total por provincia (`poblacion_total`)

---

## 3. Proceso de preparación de datos

### 3.1 Carga y combinación

Los 20 archivos CSV de vivienda fueron cargados automáticamente mediante `glob` y combinados en un único DataFrame con `pd.concat`. Se verificó que todas las columnas fueran consistentes entre archivos antes de la unión.

### 3.2 Limpieza

Las principales transformaciones realizadas fueron:

- **Tipos de datos:** conversión de columnas a numérico, datetime y booleano según correspondía
- **Separadores de miles:** eliminación del punto como separador en columnas de precio y superficie
- **Columnas nulas:** se calculó el porcentaje de nulos por columna. Se eliminaron **16 columnas** con el 100% de valores nulos, pasando de 36 a 20 columnas útiles
- **Valores categóricos:** estandarización de `condition`, `house_type` y `energetic_certif`

### 3.3 Procesamiento de datos INE

Los datos de población del INE contenían nombres de provincia con tildes, barras (`/`) y denominaciones bilingües (ej. `Araba/Álava`). Se aplicó:

- Extracción del primer nombre antes de `/`
- Sustitución de `Araba` → `Alava`
- Normalización: minúsculas y eliminación de tildes mediante `unicodedata`

### 3.4 Merge de datasets

Se realizó un **left join** entre el dataset de vivienda y el de población del INE, utilizando `provincia` como clave. El resultado incorporó `poblacion_total` y `cod_provincia` a cada registro.

### 3.5 Variable derivada: `price_tramo`

Se creó una columna de segmentación de precio con cuatro categorías:

| Tramo | Descripción |
|---|---|
| `Bajo` | Precio en el cuartil inferior |
| `Medio-Bajo` | Entre Q1 y la mediana |
| `Medio-Alto` | Entre la mediana y Q3 |
| `Alto` | Precio en el cuartil superior |

### 3.6 Dataset final

El dataset resultante cuenta con **100.000 filas y 22 columnas**, cumpliendo los requisitos mínimos del proyecto (≥50.000 filas, ≥20 columnas, 2 fuentes distintas).

---

## 4. Análisis exploratorio (EDA)

### 4.1 Estadísticas descriptivas del precio

| Estadístico | Valor (€) |
|---|---|
| Media | 379.392,874249 |
| Mediana | 212.000,0 |
| Mínimo | — |
| Máximo | - |
| Desviación típica |615.501,088284|

La distribución del precio presenta una **asimetría positiva** marcada: la mayoría de los inmuebles se concentran en rangos de precio moderados, pero existen valores extremos muy elevados (especialmente chalets independientes en provincias con alta demanda) que elevan la media por encima de la mediana.

### 4.2 Precio medio por provincia

Las provincias con mayor precio medio fueron:

1. Islas Baleares
2. Madrid
3. Gipuzkoa

Las provincias con menor precio medio:

1. Ciudad Real
2. Cadiz
3. Zamora

Se aprecia una diferencia notable entre provincias costeras o con alta demanda (Illes Balears, Madrid, Gipuzkoa) frente a provincias del interior con menor presión de demanda.

### 4.3 Precio por tipo de inmueble

Los tipos de inmueble con precios medianos más elevados fueron los Castillos, las Masias y los Palacios. Dentro de viviendas más asequibles, las fincas rústicas y los chalets son los que tienen los precios medianos más elevados

### 4.4 Relación entre superficie y precio

El análisis de correlación entre `m2_real` y `price` muestra una relación positiva moderada/fuerte. Sin embargo, se observa que la provincia actúa como variable moderadora: inmuebles de igual superficie tienen precios muy distintos según su ubicación.

### 4.6 Distribución de tramos de precio

| Tramo | Nº de inmuebles | % del total |
|---|---|---|
| Bajo | 24,35% |
| Medio-Bajo | 25,52%|
| Medio-Alto | 25,05% |
| Alto | 25,08% |

---

## 5. Visualizaciones realizadas

En el análisis se generaron las siguientes visualizaciones con Python (matplotlib / seaborn):

- Histograma de distribución de precios
- Boxplot de precio por provincia
- Boxplot de precio por tipo de inmueble
- Scatter plot: m² vs precio (coloreado por provincia)
- Heatmap de correlaciones entre variables numéricas
- Gráfico de barras: precio medio por tramo poblacional

---

## 6. Dashboard (Power BI)

El dashboard operativo desarrollado en Power BI permite la exploración interactiva de los datos y responde a las siguientes preguntas de negocio:

- ¿Cuál es el precio medio por provincia? *(barras)*
- ¿Cómo se distribuyen los inmuebles por tramo de precio? *(donut)*
- ¿Qué tipo de vivienda es más cara? *(barras ordenadas)*
- ¿Existe relación entre condición y precio? *(scatter interactivo)*
- ¿Cómo varía el precio según el tipo de vivienda? *(tabla / barras)*

El dashboard incluye filtros por provincia, tipo de inmueble y tramo de precio que permiten explorar los datos de forma segmentada.

---

## 7. Conclusiones

A partir del análisis realizado, se extraen las siguientes conclusiones principales:

**Sobre el precio:**

- El precio de la vivienda en España muestra una alta variabilidad geográfica. Las provincias con mayor presión turística y demanda urbana (Illes Balears, Madrid, provincias vascas) concentran los precios más elevados.
- La distribución de precios es asimétrica: la mayoría de los inmuebles se sitúan en rangos moderados, pero los valores extremos altos elevan significativamente la media.

**Sobre los factores que explican el precio:**

- La **superficie** (m²) es el predictor individual más fuerte del precio.
- El **tipo de inmueble** tiene un impacto significativo: los chalets independientes multiplican el precio respecto a los pisos de similar superficie.
- La **provincia** actúa como variable de contexto determinante: la ubicación geográfica es un factor de precio más potente que las características físicas del inmueble en muchos casos.

**Reflexión final:**
Los datos confirman que la crisis de acceso a la vivienda en España tiene un fuerte componente territorial. No es un problema homogéneo: las brechas de precio entre provincias son enormes, y las dinámicas de cada mercado local requieren análisis específicos. Este dataset ofrece una base sólida para continuar con modelos predictivos de precio o análisis de accesibilidad (precio vs. renta media).

---

## 8. Limitaciones y líneas futuras

**Limitaciones del análisis:**

- Los datos de vivienda corresponden a precios de **oferta** (anuncios), no de transacción real, lo que puede introducir sesgo al alza.
- La cobertura geográfica es parcial: solo 20 de las 52 provincias españolas están representadas.
- Los datos del INE incorporados se limitan a la **población total** por provincia. Un análisis más rico requeriría incorporar renta media por hogar, tasa de desempleo o precio del alquiler.

**Posibles extensiones:**

- Incorporar datos de renta media por provincia (disponibles en INE) para analizar la accesibilidad a la vivienda (ratio precio/renta)
- Construir un modelo de regresión para predecir el precio a partir de las variables disponibles
- Ampliar la cobertura a las 52 provincias
- Añadir evolución temporal del precio si se consiguen series históricas

---
