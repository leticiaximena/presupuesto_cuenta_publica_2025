# Concentración del Gasto y Variación Presupuestal 2025
### Análisis de inversión en infraestructura con SQL, Python y Tableau

---

## Descripción del proyecto

Análisis del comportamiento presupuestal (original, modificado y ejercido) de cuatro programas presupuestarios de infraestructura carretera durante el cierre 2025, con el objetivo de identificar dónde se concentra el gasto y en qué etapa del ciclo presupuestal ocurren los ajustes reales.

El proyecto combina **SQL** (extracción y agregación), **Python** (análisis de variación) y **Tableau** (visualización), reproduciendo un flujo de trabajo real de análisis de datos de principio a fin.

## Pregunta de negocio

> ¿Cómo se distribuye la inversión en infraestructura 2025 por programa y estado, y en qué etapa del ciclo presupuestal (original → modificado → ejercido) se concentran los ajustes reales?

## Alcance

El análisis se acota a los cuatro programas presupuestarios bajo mi gestión directa, dentro de una base que abarca la totalidad del ramo de infraestructura:

| Clave | Programa |
|---|---|
| K031 | Construcción y modernización |
| K037 | Conservación y reconstrucción |
| K039 | Estudios y proyectos |
| U004 | Caminos artesanales |

## Fuente de datos

Cifras de cierre de Cuenta Pública 2025, información pública en materia de presupuesto de infraestructura. Se utilizó una base principal de presupuesto (original, modificado y ejercido por clave de proyecto, programa y entidad federativa) junto con dos catálogos auxiliares: nombres de obra y nombres de entidad federativa.

Archivo: `presupuesto_2025.csv`

## Flujo de trabajo

### 1. SQL (Google BigQuery)

- Carga de la tabla principal y los dos catálogos auxiliares (estados y obras)
- Creación de tabla de catálogo de programas presupuestarios (clave → nombre legible)
- Consultas de agregación: concentración de gasto por programa, por programa y estado, y por obra dentro del programa K031
- `JOIN` entre la tabla principal y los catálogos, con conversión de tipos de dato (`CAST`) para resolver incompatibilidades entre claves numéricas y de texto
- Exclusión de registros sin información presupuestal real (original, modificado y ejercido en cero simultáneamente)

Archivo: `sql/consultas_presupuesto.sql`

### 2. Python (Google Colab)

- Carga del detalle exportado de BigQuery (301 registros, 4 programas)
- Validación de calidad de datos: tipos de dato, valores nulos
- Cálculo de variación entre presupuesto modificado y ejercido, en valor absoluto y porcentual, con columnas adicionales expresadas en millones de pesos (MDP)
- Identificación del top de obras con mayor variación
- Exportación del dataset final enriquecido para su conexión en Tableau

Archivo: `python/analisis_variacion_cierre2025.ipynb`
Enlace a Google Colab: https://colab.research.google.com/drive/1nD_zztFxlt9dfw3VAMFFhUqa0dbfOS7o?usp=sharing

**Nota metodológica:** se evaluó también la variación entre presupuesto original y modificado, pero se descartó su inclusión en el análisis final. La mayoría de los registros no contaban con presupuesto original asignado al inicio del ejercicio (recursos incorporados vía adecuaciones presupuestales durante el año), y un caso específico correspondía a una transferencia de recursos de oficinas centrales hacia los estados. Ambos escenarios requieren contexto institucional específico que dificulta una interpretación clara y homogénea para una audiencia externa, por lo que se optó por no incluir este comparativo.

### 3. Tableau Public

Dashboard interactivo con:
- KPIs generales: total modificado, total ejercido, porcentaje de obras con variación real
- Concentración del gasto por programa presupuestario
- Concentración del gasto por estado (mapa de calor)
- Top 15 obras con mayor variación entre modificado y ejercido
- Distribución de obras con y sin variación

Dashboard: *[liga de Tableau Public — pendiente de publicación final]*

## Hallazgos principales

- El **51.8%** de las obras analizadas (156 de 301 registros) presentó reconciliación completa entre presupuesto modificado y ejercido al cierre del año.
- El **48.2%** restante (145 registros) mostró variación real, sumando **928 millones de pesos** en conjunto, concentrados en un número reducido de proyectos con desviaciones significativas — el caso más alto asciende a **299 millones de pesos** en una sola obra.
- La distribución del gasto entre los cuatro programas es desigual: K031 y K037 concentran la mayor parte de los registros y del presupuesto, mientras que K039 y U004 representan un volumen menor de proyectos.

## Estructura del repositorio

```
├── sql/
│   └── consultas_presupuesto.sql
├── python/
│   └── analisis_variacion_cierre2025.ipynb
├── data/
│   └── presupuesto_2025_analisis_final.csv
└── README.md
```

## Herramientas utilizadas

`SQL (BigQuery)` · `Python (pandas)` · `Google Colab` · `Tableau Public`

## Autora

Leticia Ximena Cruz Rodríguez
[LinkedIn](https://linkedin.com/in/leticia-ximena-cr/)
