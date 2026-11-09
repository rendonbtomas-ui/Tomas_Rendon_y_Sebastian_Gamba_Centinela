
# Proyecto Centinela | Homicidios en Medellín

**Sistema experimental de alerta temprana de repuntes mensuales de homicidios mediante una red neuronal multicapa (MLP).**

**Asignatura:** Redes Neuronales — Deep Learning· Universidad Santo Tomás · Maestría en Ciencia de Datos
**Estado:** Fase 1, **v2** (propuesta aprobada el 2026-11-03; v1 + ajuste del 2026-11-08; versión de entrega)
**Fecha:** 2026-11-03 · **Integrantes:** **Tomás Rendón Bustamante · Juan Sebastián Gamba Aguirre**

> **Alcance:** anticipar aumentos inusuales en el **número de homicidios registrados** por comuna/corregimiento. No identifica autores ni víctimas ni estima riesgo individual, y sus resultados no justifican actuaciones automáticas de vigilancia.

## 1. Resumen

| Elemento | Propuesta |
| --- | --- |
| Problema | Anticipar con un mes de antelación repuntes de homicidios por comuna/corregimiento de Medellín |
| Cliente | Secretaría de Seguridad y Convivencia de Medellín |
| Unidad | Comuna/corregimiento × mes |
| Evento binario | **Alerta = 1** si `H(c,m) ≥ 2` y `H(c,m) > P75` de los 12 meses anteriores del territorio |
| Fase 1 | MLP (16 → 8, ReLU, logit + `BCEWithLogitsLoss`) sobre características tabulares de la serie agregada. **v1:** 33 entradas. **v2 (ajuste):** 30 entradas, con el percentil 75 de los 12 meses anteriores |
| Fase 2 | GRU/LSTM (serie) + CNN (recorte de radiancia nocturna NASA VNP46A3) + fusión |
| Herramientas | Python, PyTorch, scikit-learn, pandas, Matplotlib, Colab |
| Semilla | `42` (v2: además 7, 21, 73 y 101 para medir la variación) |

## 2. Datos (solo agregados y públicos)

| Modalidad | Fuente | Licencia |
| --- | --- | --- |
| Serie | [Consolidado cantidad de casos criminalidad en comunas por año y mes](https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif) (MEData/SISC) | CC BY-SA 4.0 (ficha, 2026-11-03) |
| Imagen | [NASA VNP46A3 v2](https://doi.org/10.5067/VIIRS/VNP46A3.002) | Sin restricciones de uso (EOSDIS Data Use and Citation Guidance); data.gov: *U.S. Government Works*; sin texto de licencia propio en LAADS (2026-11-03) |
| Geometría | [Límite político-administrativo, DAP Medellín](https://www.medellin.gov.co/servidormapas/rest/services/mapas_nacionales/VC_Limite_Politico_Admtivo/MapServer/1) | Términos no localizados: no redistribuir |

**Excel de víctimas retirado.** El archivo `Homicidio 141225.xlsx` no coincide con el archivo público de la ficha (según el docente, 19.647 × 36 hasta 2023-11-29), contiene datos de personas y **no se usa**. Las cifras derivadas de él se eliminaron.

## 3. Etiqueta, partición y fuga de datos

La serie consolidada no trae filas con cero: se completa el calendario territorio–mes y se **declara** que *cero registrado ≠ cero real*. Entradas del modelo para el mes `m`: solo meses `≤ m−1`.

| Partición (mes objetivo) | Intervalo |
| --- | --- |
| Entrenamiento | 2015-01 a 2020-12 |
| Validación | 2021-01 a 2022-12 |
| Prueba | 2023-01 a 2023-11 |

Tamaños y prevalencia de alertas (serie pública): entrenamiento 1.512 obs. / 285 alertas (18,8 %); validación 504 / 96 (19,0 %); prueba 231 / 44 (19,0 %). Escalador e imputación se ajustan solo con entrenamiento; el umbral de alarma se elige solo en validación. La prueba se usó **dos veces** (v1 y v2); la v2 se fijó antes de ver su resultado en prueba y esto se declara en el informe.

## 4. Línea base y evaluación

Clase mayoritaria (exactitud y recall) y regla simple del conteo previo (umbral en validación). Del MLP: matriz de confusión, precisión, recall, F1, PR-AUC, errores por territorio. En la v2 se añaden «alertar siempre», la regla con igual número de alertas y una prueba de permutación contra el azar.

**Resultados en prueba** (2023-01 a 2023-11; 231 observaciones, 44 alertas reales = 19,0 %; cuaderno ejecutado en Colab el 2026-11-03). Modelo elegido: MLP 16-8 sin Dropout, mejor época 70, umbral 0,23 fijado en validación.

| Modelo | Exactitud | Precisión (alerta) | Recall (alerta) | F1 (alerta) | PR-AUC |
| --- | --- | --- | --- | --- | --- |
| Clase mayoritaria | 0,810 | 0,000 | 0,000 | 0,000 | — |
| Regla del conteo previo | 0,251 | 0,197 | 0,955 | 0,327 | 0,168 |
| MLP | 0,550 | 0,174 | 0,364 | 0,235 | 0,188 |

Matriz de confusión del MLP: 16 verdaderos positivos, 28 falsos negativos, 76 falsos positivos, 111 verdaderos negativos.

**Conclusión de la v1:** el MLP **no supera al azar** (su PR-AUC de 0,188 es casi igual a la prevalencia de 19,0 %) ni, en precisión y F1, a la regla del conteo previo (que casi equivale a alertar siempre); no se recomienda usarlo para decisiones operativas. **Conclusión de la v2:** el ajuste mejora de forma modesta en validación, pero en prueba sigue sin distinguirse del azar (y la semilla 42 quedó por debajo de él); la recomendación de no usarlo para decidir se mantiene. Los tres tamaños de red empatan en validación (pérdida 0,475–0,479), con mejora de apenas ≈ 2,5 % frente a predecir la prevalencia (pérdida ≈ 0,487). Con 0 a 5 alertas reales por territorio, las métricas por territorio son solo indicios. Detalle en el informe de la Fase 1.

**Ajuste v2** (sección 8b del cuaderno, ejecutada en Colab con PyTorch el 2026-11-08; 5 semillas; umbral = 20 % de alertas fijado en validación).

| Configuración | PR-AUC validación | PR-AUC prueba |
| --- | --- | --- |
| v1 (33 entradas) | 0,251 ± 0,017 | 0,183 ± 0,011 |
| v2a (30 entradas, solo los rasgos nuevos) | 0,280 ± 0,010 | 0,214 ± 0,037 |
| v2 (30 entradas, con percentil 75, `weight_decay`) | 0,276 ± 0,016 | 0,208 ± 0,034 |

Prevalencia de alertas: 19,0 %. Con la prueba de permutación (promedio de las 5 semillas), el PR-AUC de la v2 se distingue del azar en validación (0,277; p = 0,004) pero **no en prueba** (0,204; p ≈ 0,49; intervalo 95 %: 0,149 a 0,305). Con la semilla 42, la prueba emitió 37 alertas: se esperaban ≈ 7 aciertos por azar y hubo 3 (matriz: 3 VP, 41 FN, 34 FP, 153 VN; PR-AUC 0,180). El resultado en prueba varía mucho entre semillas (PR-AUC de 0,180 a 0,267) y, con las cinco, la precisión promedio (0,173) es casi igual a la de la regla simple con 40 alertas (0,175).

## 5. Imagen y acoplamiento

Clave agregada `(código de comuna, año-mes)`. La imagen mide **actividad (luces)**, no homicidios. Para predecir `m` se usa radiancia de `m−2` o anterior: las fechas de producción medidas (2025-05 y 2025-06) reflejan un reprocesamiento y no la latencia operativa, que sigue sin medirse. La prueba de diez filas está en [ANEXOS_FASE1.md](./ANEXOS_FASE1.md) (diez filas con conteo real, nombre exacto del VNP46A3 y fecha de producción; SHA-256 de los diez archivos en el anexo B1-NASA).

## 6. Riesgos y protección de datos

Ver sección 7 de la propuesta y el checklist Ley 1581 (bloques A, B, C, F, H, J) por fuente en el anexo C. Los microdatos no se cargan a Colab, GitHub ni asistentes de IA. Los datos se cargan **directo a Colab** (sin Drive).

## 7. Estructura del repositorio

```text
centinela-homicidios-medellin/
├── README.md
├── propuesta-de-datos-fase1.md
├── ANEXOS_FASE1.md
├── BITACORA_IA.md
├── AUDITORIA_IA.md
├── Informe_Fase1_Centinela.docx          # informe técnico-ético (Word)
├── .gitignore                            # data/raw, data/processed y polígonos
├── data/raw/                             # no publicar
├── data/processed/                       # panel agregado (lo genera el cuaderno)
├── notebooks/
│   ├── 01_centinela_mlp.ipynb            # cuaderno principal (v1 + ajuste v2 en la sección 8b): corre con «Ejecutar todo»
│   └── 02_anexoA_nasa_earthaccess.ipynb  # descarga NASA (requiere cuenta Earthdata)
└── results/                              # métricas, modelo y tabla del anexo A (los genera el cuaderno)
```

## 8. Referencias (APA 7)

1. Alcaldía de Medellín, Secretaría de Seguridad y Convivencia, SISC. (s. f.). *Consolidado cantidad de casos criminalidad en comunas por año y mes* [Conjunto de datos]. https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif
2. NASA VIIRS Land Science Investigator-led Processing System. (2025). *VIIRS/NPP Lunar BRDF-Adjusted Nighttime Lights Monthly L3 Global 15 arc second Linear Lat Lon Grid* (Versión 2) [Conjunto de datos]. NASA LAADS DAAC. https://doi.org/10.5067/VIIRS/VNP46A3.002
3. Departamento Administrativo de Planeación, Alcaldía de Medellín. (s. f.). *Límite político administrativo: comunas y corregimientos* [Servicio cartográfico]. https://www.medellin.gov.co/servidormapas/rest/services/mapas_nacionales/VC_Limite_Politico_Admtivo/MapServer/1
4. Universidad Santo Tomás. (s. f.). *Proyecto Centinela · Fase 1* [Guía de la asignatura]. https://jotamao1985.github.io/Deep_Learning_Usta/11_Modulo_1_Actividad_Fase1_Centinela.html
