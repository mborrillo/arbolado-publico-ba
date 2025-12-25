# 🌳 Matriz de Mantenimiento Preventivo - Arbolado Público CABA

[![Dashboard Power BI](https://img.shields.io/badge/Dashboard-Power%20BI-FFBF00?style=flat-square)](https://app.powerbi.com/view?r=eyJrIjoiNDI4OGU1MDQtZjlkNy00ZjE2LWJkN2EtZDNjMzU0YzA1ZjIzIiwidCI6IjEzYmVjZWU1LTRiYjMtNGFhMC04MmM5LTZmZjAzYmJmOTU2ZiIsImMiOjR9)
[![License MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue)]()
[![Last Updated](https://img.shields.io/badge/Updated-Dec%202024-brightgreen)]()

## 📌 Resumen en 30 Segundos

**Problema:** Buenos Aires tiene 52,000+ árboles públicos. Mantenerlos es caro y peligroso. ¿Cuáles necesitan intervención YA?

**Solución:** Análisis de datos + indicador de priorización (IMP) = 7.971 casos críticos identificados geográficamente.

**Impacto:** Reducción de presupuesto ~40% + prevención de accidentes.

---

## 📊 Números Clave

| Métrica | Valor |
|---------|-------|
| Árboles analizados | 52,502 |
| **Casos críticos (IMP > 18)** | **7,971 (15.48%)** |
| Casos excedidos (15 ≤ IMP ≤ 18) | 8,580 (16.66%) |
| Casos correctos (IMP < 15) | 34,951 (67.86%) |
| Cobertura geográfica | 100% CABA (48 comunas) |
| Datos históricos | 2021–2023 |
| Especies analizadas | 358 |

**⚠️ Nota sobre Actualización de Datos:**
- Este análisis fue completado en **Diciembre 2024**
- Los datos proceden del dataset público: [Arbolado en Espacios Verdes](https://data.buenosaires.gob.ar/dataset/arbolado-espacios-verdes)
- **Estado actual:** Snapshot estático. No se actualiza automáticamente.
- Para análisis actual, descargar datos frescos del portal BA.

---

## 🎯 ¿Por Qué Importa?

- 💰 **Costo de extirpación:** USD $15-20k por árbol
- ⚠️ **Accidentes en aumento:** +15% anual (2020-2022)
- 📊 **Presupuesto limitado:** Necesidad crítica de priorización

**Este proyecto permite responder:** *"¿A qué 7,971 árboles debo prestar atención PRIMERO?"*

---

### 🏗️ Arquitectura de la Solución

<img width="349" height="278" alt="image" src="https://github.com/user-attachments/assets/6808812c-d332-4da9-912f-a289aa408abf" />

(DATOS CRUDOS) 
- CSV/Excel con 
  52k registros

(Carga Info + M-Query)
- TABLAS STAGING
- Normalización de datos

(Modelado)
- Fact: relevamiento_arboles
- Dim: especie, ubicación, período

(Cálculos DAX)
- KPIs & Métricas
  IMP, Tendencias

(POWER BI DASHBOARD)
- Visualización interactiva

---

## 🔍 El Indicador IMP (Mantenimiento Preventivo)

### IMP = AVERAGE(Altura Normalizada, Diámetro Normalizado, Inclinación Normalizada)

### Version DAX Ind.Mant.Prev. = ('arbolado-en-espacios-verdes'[altura_tot] + 'arbolado-en-espacios-verdes'[diametro] + 'arbolado-en-espacios-verdes'[inclinacion]) / 3


### Interpretación

| Rango IMP | Categoría | Acción |
|-----------|-----------|--------|
| IMP > 18 | 🔴 CRÍTICO | Intervención inmediata (extirpación/poda urgente) |
| 15 ≤ IMP ≤ 18 | 🟡 EXCEDIDO | Vigilancia frecuente (trimestral) |
| IMP < 15 | 🟢 NORMAL | Mantenimiento rutinario (anual) |

### Justificación Técnica

- **Altura > 12m:** Mayor energía de caída, más daño
- **Diámetro > 40cm:** Mayor masa, riesgo estructural
- **Inclinación > 4°:** Desequilibrio + stress de raíces
- **Promedio simple:** MVP. Con datos históricos de caídas por especie, se usarían pesos (Info no disponible en este Dataset)

---

## 📈 Hallazgos Principales

### 1. Evolución del IMP (2021-2023)
2021: 18.40 → 2022: 18.42 (+0.11% aprox) → 2023: 18.22 (-1.09%)
Interpretación: Situación estable, aunque requiere vigilancia

### 2. Distribución por Origen
- **Exóticos (63.42%):** IMP 18.09 (más críticos)
- **Nativos (33.10%):** IMP 19.51 (MÁS críticos aún)
- **No Determinado (3.49%):** IMP 11.98 (menos críticos)

**Insight:** Contrario a lo esperado, árboles nativos tienen IMP más alto. Podría deberse a mayor antigüedad o falta de podas preventivas.

### 3. Concentración Geográfica
- Belgrano, Núñez, Recoleta: Densidad alta de casos críticos
- La Boca, San Telmo: Concentración media
- Áreas periféricas: Menor densidad

**Impacto estimado:**
- Presupuesto optimizado: Enfoque selectivo = 40% de ahorro
- Reducción de incidentes: Intervención preventiva
- Mejor experiencia ciudadana: Menos árboles cayéndose

---

## 🏗️ Arquitectura de Datos



---

## 📁 Estructura del Repositorio

arbolado-publico-ba/
├── README.md ← Estás aquí
├── LICENSE ← Licencia MIT
│
├── docs/
│ ├── CONTEXT.md ← Problema + contexto
│ ├── DATA_MODEL.md ← Esquema + diccionario
│ ├── METHODOLOGY.md ← Proceso de análisis
│ ├── TECH_STACK.md ← Herramientas + versiones
│ └── INSIGHTS.md ← Hallazgos detallados
│
├── dashboards/
│ ├── power-bi-report.pbix ← Archivo Power BI original
│ └── screenshots/
│ ├── 01_inicio.png ← Portada
│ ├── 02_home.png ← KPIs + distribución
│ ├── 03_matriz.png ← Matriz de estado
│ ├── 04_glosario.png ← Definiciones
│ └── README.md ← Guía de navegación
│
├── data/
│ ├── Arbolado-en-espacios-verdes.xlsx ← Muestra fuente de datos
│ └── data_dictionary.md ← Diccionario de columnas
│
├── Power Query
│ ├── 01_limpieza ← Transformaciones iniciales
│ ├── 02_modelado ← Star schema
│ └── 03_indicadores ← Cálculo de IMP
│
└── analysis/
├── exploratory_analysis.md ← EDA findings
└── statistical_summary.md ← Estadísticas descriptivas


---

## 🚀 Cómo Usar Este Proyecto

### Para Analistas (Reproducir el análisis)
### Para Analistas (Reproducir el análisis)

1. **Descarga los datos crudos:**
   - Ve a [Portal BA Datos Abiertos](https://data.buenosaires.gob.ar/dataset/arbolado-espacios-verdes)
   - Descarga: `Arbolado-en-espacios-verdes.xlsx`

2. **Abre Power BI Desktop**
   
3. **Carga los datos:**
   - Home → Get Data → Excel
   - Selecciona el archivo descargado
   - Carga la tabla `arbolado_espacios_verdes`

4. **Transforma los datos (M-Query):**
   - En Power Query Editor, aplica:
     - Renombra columnas (quita espacios, caracteres especiales)
     - Filtra registros con "S/D" en altura, diámetro
     - Convierte tipos de datos (altura, diámetro → float)
   - Ver detalles: [Docs_03_METHODOLOGY.md](https://github.com/mborrillo/arbolado-publico-ba/blob/main/Docs_03_METHODOLOGY.md)

5. **Modelado (Crea el Star Schema):**
   - Fact table: `relevamiento_arboles` (52,502 filas)
   - Dim tables: `dim_especie`, `dim_ubicacion`, `dim_periodo`
   - Relaciones (1-a-muchos)

6. **Cálculos DAX:**
   - Crea medida: `Ind.Mant.Prev.` [ver fórmula arriba]
   - Crea columnas calculadas para categorizar (CRÍTICO/EXCEDIDO/NORMAL)

7. **Visualización:**
   - KPI Cards, Donut Charts, Mapa, Tabla de detalles
   - (Consulta screenshots en [dashboards/](https://github.com/mborrillo/arbolado-publico-ba/tree/main/dashboards))


### Para Reclutadores (Evaluar capacidades)
1. Lee README + docs/
2. Explora el dashboard interactivo (arriba, link Power BI)
3. Revisa la metodología en `METHODOLOGY.md`

### Para Tomadores de Decisión (Usar operacionalmente)
1. Filtra por espacio verde correspondiente
2. Revisa por criticidad de IMP 
3. Integra con tu sistema de mantenimiento

---

## 🔧 Stack Técnico

| Herramienta | Versión | Propósito |
|-------------|---------|----------|
| **Power BI** | 2024 | Visualización + modelado BI |
| **DAX** | Latest | Cálculos de indicadores |
| **M-Query** | Latest | Limpieza de datos, Creación Columnas Calculadas |
| **Git** | Latest | Control de versiones |

---

## 📊 Dashboard Interactivo

**Accede aquí:** [Power BI Dashboard](https://app.powerbi.com/view?r=eyJrIjoiNDI4OGU1MDQtZjlkNy00ZjE2LWJkN2EtZDNjMzU0YzA1ZjIzIiwidCI6IjEzYmVjZWU1LTRiYjMtNGFhMC04MmM5LTZmZjAzYmJmOTU2ZiIsImMiOjR9)

**Pantallas:**
1. **Inicio:** Navegación y contexto del proyecto
2. **Home:** KPIs, distribuciones, mapa geográfico
3. **Matriz:** Análisis multidimensional por período, espacio verde, origen
4. **Glosario:** Definiciones de todas las métricas

---







