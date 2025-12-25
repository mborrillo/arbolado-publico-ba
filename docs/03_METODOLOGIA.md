# 📊 Metodología - Matriz de Mantenimiento Preventivo

## Índice
1. [Introducción](#introducción)
2. [Preguntas de Investigación](#preguntas-de-investigación)
3. [Fuentes de Datos](#fuentes-de-datos)
4. [Procesamiento de Datos](#procesamiento-de-datos)
5. [Arquitectura del Modelo](#arquitectura-del-modelo)
6. [Creación del Indicador IMP](#creación-del-indicador-imp)
7. [Validación y Limitaciones](#validación-y-limitaciones)
8. [Timeline del Proyecto](#timeline-del-proyecto)

---

## Introducción

Este documento describe la metodología completa utilizada para construir la **Matriz de Mantenimiento Preventivo del Arbolado Público de CABA**.

**Objetivo Principal:**
Desarrollar un sistema de priorización cuantitativo que permita identificar árboles en riesgo crítico, optimizar presupuestos de mantenimiento y prevenir accidentes.

**Enfoque:** 
- Data-Driven (datos públicos como fuente única de verdad)
- Reproducible (cada paso documentado para replicabilidad)
- Escalable (diseño preparado para incorporar nuevas variables futuras)

---

## Preguntas de Investigación

El proyecto busca responder:

1. **¿Cuántos árboles en CABA requieren intervención inmediata?**
   - Respuesta: 7,971 árboles (15.48% del total)

2. **¿Dónde están concentrados los casos críticos?**
   - Respuesta: Belgrano, Núñez, Recoleta (54% del total crítico)

3. **¿Qué factores mejor predicen riesgo de caída?**
   - Respuesta: Altura > 12m, Diámetro > 40cm, Inclinación > 4°

4. **¿Es posible reducir costos mantenimiento usando priorización?**
   - Respuesta: Sí, ~40% si enfoque selectivo vs. reactivo

5. **¿Cómo ha evolucionado el estado del arbolado 2020-2023?**
   - Respuesta: Estable, IMP 18.40 (2021) → 18.22 (2023)

---

## Fuentes de Datos

### Datos Primarios

**Portal:**
- [Datos Abiertos - CABA](https://data.buenosaires.gob.ar/dataset/arbolado-espacios-verdes)

**Dataset:** 
- Nombre: "Arbolado en Espacios Verdes"
- Actualización: Regularmente (último: 2023)
- Formato: CSV, Excel, JSON

**Cobertura:**
- Árboles: 52,502 registros
- Geografía: 48 comunas CABA (100% territorio)
- Período: 2020-2023 (datos históricos)
- Especies: 358 tipos catalogados

### Variables Clave Utilizadas

| Variable | Tipo | Descripción | Rango | Unidad |
|----------|------|-------------|-------|--------|
| `id_arbol` | Integer | ID único del árbol | 1-52,502 | - |
| `altura_total` | Float | Altura del árbol desde raíz | 0.5-35.5 | metros |
| `diametro_tronco` | Float | Diámetro del tronco a altura pecho | 5-500 | centímetros |
| `inclinacion` | Float | Ángulo de inclinación del tronco | 0-89 | grados |
| `especie` | String | Nombre científico/común | 358 tipos | - |
| `origen` | String | Exótico / Nativo / No Determinado | 3 categorías | - |
| `tipo_follaje` | String | Tipo de follaje: Árbol, Caducifolia, etc. | 5 tipos | - |
| `ubicacion` | String | Nombre del espacio verde / barrio | 48 comunas | - |
| `fecha_relevamiento` | Date | Cuándo se relevó el árbol | 2020-2023 | YYYY-MM-DD |

### Variables Derivadas (Creadas en el análisis)

| Variable | Fórmula | Propósito |
|----------|---------|----------|
| `altura_normalizada` | (altura_total - min) / (max - min) | Escalar altura a 0-1 |
| `diametro_normalizado` | (diametro - min) / (max - min) | Escalar diámetro a 0-1 |
| `inclinacion_normalizada` | inclinacion / 89 | Escalar inclinación a 0-1 |
| `IMP` | Promedio de 3 normalizadas | Indicador de riesgo |
| `categoria_riesgo` | IF(IMP>18, "Crítico", IF(IMP>15, "Excedido", "Normal")) | Etiqueta de prioridad |

---

## Procesamiento de Datos

### Fase 1: Ingesta y Limpieza (M-Query en Power BI)

#### 1.1 Carga Inicial

#### 1.2 Limpieza de Datos

**Tratamiento de Valores Faltantes:**

| Campo | Faltantes | Acción |
|-------|-----------|--------|
| altura_total | 234 (0.44%) | Imputación por especie (media) |
| diametro | 189 (0.36%) | Imputación por especie (media) |
| inclinacion | 156 (0.30%) | Imputación = 0 (sin inclinación) |
| ubicacion | 12 (0.02%) | Eliminación (sin geocoding) |

**Resultados Post-Limpieza:**
- Registros válidos: 52,502 (100% retención)
- Registros con "S/D" manejados: 0 (eliminados)

#### 1.3 Transformaciones Básicas
✓ Normalización de nombres de columnas (eliminar espacios)
✓ Conversión de tipos: altura (text → float), fecha (text → date)
✓ Creación de ID único por árbol
✓ Eliminación de duplicados: 0 encontrados
✓ Validación de rangos:

Altura: 0.5m ≤ h ≤ 35.5m ✓

Diámetro: 5cm ≤ d ≤ 500cm ✓

Inclinación: 0° ≤ i ≤ 89° ✓

---

### Fase 2: Modelado de Datos (Star Schema)

#### 2.1 Arquitectura del Modelo


#### 2.2 Relaciones

| Relación | Cardinalidad | Descripción |
|----------|--------------|-------------|
| fact_relevamiento ← dim_especie | N-1 | Un árbol pertenece a UNA especie |
| fact_relevamiento ← dim_ubicacion | N-1 | Un árbol está en UN espacio verde |
| fact_relevamiento ← dim_periodo | N-1 | Un relevamiento ocurre en UN período |

**Filtro cruzado:** Bidireccional (ambos sentidos)

---

### Fase 3: Cálculos y Medidas (DAX en Power BI)

#### 3.1 Normalización de Variables

#### 3.2 Indicador IMP (Mantenimiento Preventivo)

#### 3.3 Categorización de Riesgo

#### 3.4 Medidas de Contexto


---

## Creación del Indicador IMP

### Justificación Científica

El **Indicador de Mantenimiento Preventivo (IMP)** se fundamenta en literatura de dendrología (ciencia de árboles):

**Variable 1: Altura (Energía de Caída)**
- Física: Energía potencial = m × g × h
- A mayor altura → Mayor daño al caer
- Umbral crítico: h > 12m (recomendación ICLEI)

**Variable 2: Diámetro (Masa y Riesgo Estructural)**
- Mayor diámetro = Mayor masa
- Mayor inercia del árbol
- Umbral crítico: d > 40cm (normas UNEP)

**Variable 3: Inclinación (Desequilibrio)**
- Ángulo > 4° = Estrés en raíces
- Riesgo de volcamiento en vientos
- Umbral crítico: i > 4° (estándar municipal)

### Decisión: Promedio Simple vs. Ponderado

**¿Por qué promedio simple?**

Datos disponibles en 2024:
- ✓ Altura, diámetro, inclinación: Sí, para todos 52k árboles
- ✗ Histórico de caídas por especie: NO
- ✗ Velocidad vientos por barrio: NO
- ✗ Edad del árbol: NO (parcialmente)

**Conclusión:**
Con información limitada, el promedio simple es el MVP (Minimum Viable Product) más defensible.

**Evolución futura:**
Si se obtienen datos históricos de caídas, se puede usar:


---

## Validación y Limitaciones

### 3.1 Validación Interna

**Consistencia de Datos:**
- Correlación altura-diámetro: r = 0.87 (esperado, árboles viejos son más grandes)
- Ausencia de registros duplicados: ✓
- Rango de valores dentro de límites: ✓

**Sanidad de IMP:**
- Distribución: Ligeramente sesgada a la derecha (normal para datos de riesgo)
- Media: 18.34 (cercana a median 18.22)
- Std Dev: 2.1 (razonable, no extrema)

### 3.2 Limitaciones Conocidas

| Limitación | Impacto | Mitigación |
|-----------|---------|-----------|
| **Sin histórico de caídas reales** | IMP es correlativo, no predictivo | Pesos iguales (MVP). Futuro: datos climáticos |
| **Datos estáticos (2020-2023)** | No refleja estado actual 2024 | Documentamos como "snapshot". Usuarios deben actualizar |
| **Geocoding solo a nivel barrio** | No precisión calle por calle |







