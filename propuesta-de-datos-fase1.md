# Propuesta de Datos · Proyecto Centinela · Fase 1

| Campo | Detalle |
| --- | --- |
| Título del proyecto | Centinela Homicidios Medellín: alertas de repunte mensual con MLP |
| Integrantes | **Tomas Rendon - Juan Sebastian Gamba** |
| Fecha | 2026-11-03 |
| Versión | v1 (ajustes tras retroalimentación del docente) |


# Propuesta de Datos · Proyecto Centinela · Fase 1

| Campo | Detalle |
| --- | --- |
| Título del proyecto | Centinela Homicidios Medellín: alertas de repunte mensual con MLP |
| Integrantes | **[COMPLETAR: nombre completo de cada integrante (máx. 3)]** |
| Fecha  | 2026-11-03 |
| Versión | v1 (ajustes tras retroalimentación del docente) |

## 1. Problema, cliente y objetivo binario

**Cliente:** Secretaría de Seguridad y Convivencia de Medellín (análisis territorial). **Impacto:** anticipar meses con incrementos inusuales de homicidios registrados para apoyar decisiones preventivas verificadas por personas. **Unidad:** comuna/corregimiento–mes.

**Objetivo binario:** `alerta(c,m) = 1` si `H(c,m) ≥ 2` y `H(c,m) > P75[H(c,m−12..m−1)]`; `0` en otro caso. Se predice el mes `m` con información hasta `m−1`. Es un umbral académico: detecta repuntes relativos y pierde la magnitud del aumento; no es una regla oficial. Se eligió para que territorios con magnitudes distintas sean comparables y para que un solo caso no dispare alertas.

## 2. Las dos modalidades del sistema completo

- **Rama temporal:** conteo mensual de homicidios por comuna/corregimiento, registro administrativo de la Alcaldía (SISC), frecuencia mensual.
- **Rama de imagen:** radiancia nocturna mensual (ráster) de NASA Black Marble VNP46A3, medida por el radiómetro VIIRS del satélite Suomi-NPP, recortada al polígono de cada territorio.
- **Pregunta de control, «¿la imagen contiene algo que un número no contenga?»:** sí. El ráster describe la distribución espacial de la luz dentro del territorio, que el conteo administrativo no contiene. **Declaración de honestidad:** las luces nocturnas miden **actividad (iluminación)**, no homicidios; no se afirma relación causal y la ablación de la Fase 2 dirá cuánto aporta.

## 3. Fuentes (con enlaces y licencia)

| Modalidad | Fuente (productor primario) | Enlace o DOI | Licencia y versión | Dónde se verificó la licencia | ¿Datos de personas? |
| --- | --- | --- | --- | --- | --- |
| Temporal | Alcaldía de Medellín, SISC (MEData) · *Consolidado cantidad de casos criminalidad en comunas por año y mes* | [datos.gov.co/…/gcyu-chif](https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif) | **Creative Commons Atribución-CompartirIgual 4.0 (CC BY-SA 4.0)** | Ficha del conjunto en datos.gov.co (campo *License*), consultada el 2026-11-03 | No: conteos agregados por comuna, año y mes |
| Imagen | NASA LAADS DAAC · VNP46A3 versión 2 (SNPP VIIRS, mensual) | [doi.org/10.5067/VIIRS/VNP46A3.002](https://doi.org/10.5067/VIIRS/VNP46A3.002) | **Sin restricciones de uso**, según la *EOSDIS Data Use and Citation Guidance* (política abierta de datos EOSDIS); el catálogo data.gov registra el conjunto como *U.S. Government Works*. La página del producto en LAADS DAAC **no declara un texto de licencia propio** (revisada 2026-11-03); por eso no se afirma una licencia CC en concreto: se cita la fuente con el DOI | [Ficha NASA Earthdata del producto](https://www.earthdata.nasa.gov/data/catalog/laads-vnp46a3-2), [página LAADS del producto](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/VNP46A3) y [EOSDIS Data Use Policy](https://www.earthdata.nasa.gov/earth-observation-data/data-use-policy) (consultadas 2026-11-03) | No (radiancia a 15 arc-s ≈ 500 m) |
| Geometría auxiliar | Alcaldía de Medellín, DAP · límite político-administrativo (comunas y corregimientos) | [Servicio ArcGIS, capa 1](https://www.medellin.gov.co/servidormapas/rest/services/mapas_nacionales/VC_Limite_Politico_Admtivo/MapServer/1) | **No localicé una licencia explícita**; el servicio declara a la DAP como titular. Se trata como *términos de redistribución no verificados* | Metadatos del servicio (DAP); sin texto de licencia | No |

**Fuente retirada:** el Excel `Homicidio 141225.xlsx` (5.711 × 17, hasta 2025-12-14) **ya no es fuente de esta propuesta**: contiene datos de víctimas y no coincide con el archivo público (ver anexos B y C).

## 4. Por qué NO es un reto resuelto

- **Procedencia:** las dos fuentes de datos se obtienen del productor primario (portal oficial y repositorio NASA); ningún dato viene de Kaggle ni de una réplica. Cadena y huella SHA-256 del consolidado (`50965240…e23c`) en el anexo B1; las huellas de NASA y polígonos se calculan al descargarlos.
- **Búsqueda de espejos (2026-11-03):** dos búsquedas web con operador `site:kaggle.com` (nombre del conjunto y «homicidios Medellín comunas SISC») no devolvieron ningún conjunto de Kaggle; detalle en B3. La repetición directa en kaggle.com/datasets queda a cargo de los integrantes.
- **Crudeza (medida en el archivo real, 23.164 filas × 4 columnas):** el consolidado mezcla 7 tipos de conducta; los homicidios son 4.071 filas (18.593 casos, 2003-01 a 2023-11) en 21 territorios (16 comunas + 5 corregimientos). El código de territorio llega duplicado como `1` y `1.0` y hay 472 filas con «SIN DATO» (ninguna de homicidio). **No trae filas con cero**: al completar el calendario territorio–mes se rellenan 1.200 de 5.271 pares (22,8 %), y se declara que *cero registrado ≠ cero real*. La radiancia exige recorte por polígono, máscara de calidad y manejo de nubes/luz lunar.
- **No hay solución pública que copiar:** no encontré un empaquetado público de este problema (etiqueta de repunte + radiancia por comuna).

## 5. Plan de la Fase 1, clave de alineación y acoplamiento

- **Modalidad aplanada en la Fase 1:** la **serie** (conteos rezagados 1–3 meses, sumas/medias móviles 3/6/12, mes cíclico, territorio). **Fase 2:** GRU/LSTM para la serie y CNN para el recorte de radiancia, con fusión.
- **Fuente de la serie:** únicamente el consolidado agregado de MEData (CC BY-SA 4.0), con datos hasta 2023-11 según el docente. **Se rellenan con 0 los meses sin fila** y se declara en el cuaderno.
- **Partición cronológica por mes objetivo:** entrenamiento **2015–2020**, validación **2021–2022**, prueba **2023-01 a 2023-11**. El historial de 12 meses sale de años anteriores a 2015 de la misma serie. Escalado solo con entrenamiento; umbral de decisión solo con validación. Tamaños y prevalencia de la etiqueta, calculados con la serie pública (no con el Excel): entrenamiento 1.512 observaciones con 285 alertas (18,8 %); validación 504 con 96 (19,0 %); prueba 231 con 44 (19,0 %).
- **Clave de alineación (agregada):** `(código de comuna/corregimiento, año-mes)`. Salvedad de fuga: la imagen del mes `m` no entra al modelo que predice `m`.
- **Retraso de publicación:** el nombre de cada archivo VNP46A3 contiene la fecha de producción (`YYYYDDDHHMMSS`). En los diez archivos del anexo A esa fecha cae entre 2025-05-27 y 2025-06-09 (648 a 1.577 días después del mes de la imagen): refleja un reprocesamiento de la serie histórica, no la latencia operativa. Se mantiene la regla conservadora de usar para predecir `m` solo imágenes del mes `m−2` o anterior, y queda pendiente medir la latencia con compuestos recientes. **No he encontrado en las fuentes consultadas una cifra oficial de latencia del compuesto mensual.**
- **Prueba de acoplamiento:** anexo A: las diez claves y los conteos de homicidio ya salen del consolidado real; los diez nombres exactos de VNP46A3 y sus fechas de producción ya están registrados; **queda pendiente** el SHA-256 de nueve de los diez archivos.

## 6. Línea base comprometida

En el conjunto de prueba (2023-01 a 2023-11): **clase mayoritaria** (exactitud y recall de la clase de alerta, que vale 0 si la mayoritaria es «sin alerta») y **regla del conteo previo** `H(c,m−1)` contra su histórico, con umbral elegido en validación (escalar crudo). MLP: 16 y 8 neuronas ReLU, Dropout, salida logit, `BCEWithLogitsLoss`, Adam, parada temprana; semilla 42. Aún no hay métricas del modelo.

## 7. Riesgos éticos y protección de datos

| Riesgo | Daño para el cliente | Mitigación |
| --- | --- | --- |
| Sesgo de muestreo / subregistro | Subestimar territorios con menor denuncia o captura | Declarar que los datos son hechos registrados; no interpretar ceros como ausencia; informar incertidumbre |
| Falsa alarma | Estigma territorial y gasto de recursos | Verificación humana; no publicar rankings de peligrosidad; reportar tasa de falsos positivos por territorio |
| Alarma omitida | Daño sin prevención | Reportar recall y falsos negativos por territorio; no presentar el sistema como garantía |
| Reidentificación | Exposición de víctimas | Solo agregados oficiales; el Excel con datos de víctimas queda fuera del proyecto |

**Checklist Ley 1581 (bloques A, B, C, F, H y J):** uno por fuente (consolidado, NASA, polígonos) y firma de cada integrante en el anexo C.

## 8. Referencias (APA 7)

- Alcaldía de Medellín, Secretaría de Seguridad y Convivencia, SISC. (s. f.). *Consolidado cantidad de casos criminalidad en comunas por año y mes* [Conjunto de datos]. Datos Abiertos Colombia. https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif
- NASA VIIRS Land Science Investigator-led Processing System. (2025). *VIIRS/NPP Lunar BRDF-Adjusted Nighttime Lights Monthly L3 Global 15 arc second Linear Lat Lon Grid* (Versión 2) [Conjunto de datos]. NASA Level 1 and Atmosphere Archive and Distribution System Distributed Active Archive Center. https://doi.org/10.5067/VIIRS/VNP46A3.002
- Departamento Administrativo de Planeación, Alcaldía de Medellín. (s. f.). *Límite político administrativo: comunas y corregimientos* [Servicio cartográfico]. https://www.medellin.gov.co/servidormapas/rest/services/mapas_nacionales/VC_Limite_Politico_Admtivo/MapServer/1

**Anexos A, B y C:** [ANEXOS_FASE1.md](./ANEXOS_FASE1.md) (se entregan en la misma carpeta).
