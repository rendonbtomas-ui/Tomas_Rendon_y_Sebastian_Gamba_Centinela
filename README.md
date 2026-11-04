
# Tomas_Rendon_y_Sebastian_Gamba_Centinela
# Proyecto Centinela | Homicidios en Medellín

**Sistema experimental de alerta temprana de repuntes mensuales de homicidios mediante una red neuronal multicapa (MLP).**

**Asignatura:** Redes Neuronales — Deep Learning· Universidad Santo Tomás · Maestría en Ciencia de Datos
**Estado:** Fase 1, **v1** (ajustes tras retroalimentación del docente; pendiente de aprobación)
**Fecha:** 2026-11-03 · **Integrantes:** **Tomas Rendon y Sebastian Gamba** ·

> **Alcance:** anticipar aumentos inusuales en el **número de homicidios registrados** por comuna/corregimiento. No identifica autores ni víctimas ni estima riesgo individual, y sus resultados no justifican actuaciones automáticas de vigilancia.


| Elemento | Propuesta |
| --- | --- |
| Problema | Anticipar con un mes de antelación repuntes de homicidios por comuna/corregimiento de Medellín |
| Cliente | Secretaría de Seguridad y Convivencia de Medellín |
| Unidad | Comuna/corregimiento × mes |
| Evento binario | **Alerta = 1** si `H(c,m) ≥ 2` y `H(c,m) > P75` de los 12 meses anteriores del territorio |
| Fase 1 | MLP (16 → 8, ReLU, Dropout, logit + `BCEWithLogitsLoss`) sobre características tabulares de la serie agregada |
| Fase 2 | GRU/LSTM (serie) + CNN (recorte de radiancia nocturna NASA VNP46A3) + fusión |
| Herramientas | Python, PyTorch, scikit-learn, pandas, Matplotlib, Colab |
| Semilla | `42` |

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

Tamaños y prevalencia de alertas (serie pública): entrenamiento 1.512 obs. / 285 alertas (18,8 %); validación 504 / 96 (19,0 %); prueba 231 / 44 (19,0 %). Escalador e imputación se ajustan solo con entrenamiento; el umbral de alarma se elige solo en validación; la prueba se usa una vez.

## 4. Línea base y evaluación

Clase mayoritaria (exactitud y recall) y regla simple del conteo previo (umbral en validación). Del MLP: matriz de confusión, precisión, recall, F1, PR-AUC, errores por territorio.

**Resultados en prueba** (2023-01 a 2023-11; 231 observaciones, 44 alertas reales = 19,0 %; cuaderno ejecutado en Colab el 2026-11-03). Modelo elegido: MLP 16-8 sin Dropout, mejor época 70, umbral 0,23 fijado en validación.

| Modelo | Exactitud | Precisión (alerta) | Recall (alerta) | F1 (alerta) | PR-AUC |
| --- | --- | --- | --- | --- | --- |
| Clase mayoritaria | 0,810 | 0,000 | 0,000 | 0,000 | — |
| Regla del conteo previo | 0,251 | 0,197 | 0,955 | 0,327 | 0,168 |
| MLP | 0,550 | 0,174 | 0,364 | 0,235 | 0,188 |

Matriz de confusión del MLP: 16 verdaderos positivos, 28 falsos negativos, 76 falsos positivos, 111 verdaderos negativos.

**Conclusión:** el MLP **no supera al azar** (su PR-AUC de 0,188 es casi igual a la prevalencia de 19,0 %) ni a las líneas base en lo que importa; no se recomienda usarlo para decisiones operativas. Los tres tamaños de red empatan en validación (pérdida 0,475–0,479), con mejora de apenas ≈ 2 % frente a predecir la prevalencia. Con 0 a 5 alertas reales por territorio, las métricas por territorio son solo indicios. Detalle en el informe de la Fase 1.

## 5. Imagen y acoplamiento

Clave agregada `(código de comuna, año-mes)`. La imagen mide **actividad (luces)**, no homicidios. Para predecir `m` se usa radiancia de `m−2` o anterior hasta medir el retraso real de publicación. La prueba de diez filas está en [ANEXOS_FASE1.md](./ANEXOS_FASE1.md) (conteos reales listos; **pendiente** descargar los VNP46A3).

## 6. Riesgos y protección de datos

Ver sección 7 de la propuesta y el checklist Ley 1581 (bloques A, B, C, F, H, J) por fuente en el anexo C. Los microdatos no se cargan a Colab, GitHub ni asistentes de IA. Los datos se cargan **directo a Colab** (sin Drive).

## 7. Estructura del repositorio

```text
centinela-homicidios-medellin/
├── README.md
├── propuesta-de-datos-fase1.md
├── ANEXOS_FASE1.md
├── .gitignore              # data/raw y polígonos
├── data/raw/               # no publicar
├── data/processed/         # panel agregado
├── notebooks/01_centinela_mlp.ipynb   # tras aprobación
├── src/
└── results/
```

## 8. Referencias (APA 7)

1. Alcaldía de Medellín, Secretaría de Seguridad y Convivencia, SISC. (s. f.). *Consolidado cantidad de casos criminalidad en comunas por año y mes* [Conjunto de datos]. https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif
2. NASA VIIRS Land Science Investigator-led Processing System. (2025). *VIIRS/NPP Lunar BRDF-Adjusted Nighttime Lights Monthly L3 Global 15 arc second Linear Lat Lon Grid* (Versión 2) [Conjunto de datos]. NASA LAADS DAAC. https://doi.org/10.5067/VIIRS/VNP46A3.002
3. Departamento Administrativo de Planeación, Alcaldía de Medellín. (s. f.). *Límite político administrativo: comunas y corregimientos* [Servicio cartográfico]. https://www.medellin.gov.co/servidormapas/rest/services/mapas_nacionales/VC_Limite_Politico_Admtivo/MapServer/1
4. Universidad Santo Tomás. (s. f.). *Proyecto Centinela · Fase 1* [Guía de la asignatura]. https://jotamao1985.github.io/Deep_Learning_Usta/11_Modulo_1_Actividad_Fase1_Centinela.html
