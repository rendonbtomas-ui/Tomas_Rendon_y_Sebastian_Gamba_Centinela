# Informe Fase 1 · Proyecto Centinela: alertas de repunte de homicidios en Medellín

Nov 3, 2026

## Resumen

El MLP de la Fase 1 no distingue los meses de repunte mejor que el azar: en la prueba (2023-01 a 2023-11) su PR-AUC es 0,188, frente a una prevalencia de alertas de 19,0 %. El código y la partición funcionaron sin errores ni fuga de datos; lo que falta es señal en los rasgos.

- **Datos:** consolidado de MEData/SISC con 21 territorios (16 comunas y 5 corregimientos), 2003-01 a 2023-11; solo agregados, sin datos de personas.
- **Partición cronológica:** entrenamiento 2015–2020, validación 2021–2022, prueba 2023-01 a 2023-11.
- **Modelo elegido:** MLP 16-8 sin Dropout, mejor época 70, umbral 0,23 fijado en validación.
- **Prueba:** recall 0,364, precisión 0,174, F1 0,235 y exactitud 0,550; la clase mayoritaria («sin alerta») alcanza exactitud 0,810.
- **Recomendación:** no usar este sistema para decisiones operativas; queda como línea base documentada para la Fase 2.

## Problema, cliente y etiqueta

El cliente simulado es la Secretaría de Seguridad y Convivencia de Medellín, que necesita anticipar meses con aumentos inusuales de homicidios registrados para apoyar decisiones preventivas verificadas por personas. La unidad de análisis es comuna o corregimiento por mes.

Un territorio `c` tiene alerta en el mes `m` si `H(c,m) ≥ 2` y `H(c,m)` supera el percentil 75 de sus 12 meses anteriores; el modelo predice `m` con información hasta `m−1`. Es un umbral académico: compara territorios de magnitudes distintas y evita que un solo caso dispare alertas, pero pierde la magnitud del aumento y no es una regla oficial.

## Datos y procedencia

La serie sale únicamente del consolidado agregado de la Alcaldía de Medellín (SISC/MEData, datos.gov.co, licencia CC BY-SA 4.0), descargado por el equipo el 2026-11-03. El Excel de víctimas fue retirado: contiene datos de personas y no coincide con el archivo público.

| Elemento | Valor |
| --- | --- |
| Archivo | `consolidado_cantidad_casos_criminalidad_en_comunas_por_anio_mes.csv` |
| SHA-256 | `50965240f8df75bdaa0a225dfa65eaab85ded9d52e52ba85b5347683ee01e23c` |
| Tamaño | 23.164 filas × 4 columnas (`Fecha_hecho`, `Conducta`, `Codigo_comuna`, `Cantidad_casos`) |
| Rango | 2003-01 a 2023-11; 0 % de faltantes en las cuatro columnas |
| Homicidios | 4.071 filas, 18.593 casos, 21 territorios |
| Códigos | el mismo territorio llega como `1` y `1.0` (se unifica); 472 filas «SIN DATO», ninguna de homicidio |
| Ceros | no hay filas con cero: se rellenan 1.200 de 5.271 pares territorio-mes (22,8 %) |

Cero registrado no equivale a cero real: los datos son hechos registrados, con posible subregistro. La radiancia nocturna de NASA VNP46A3 y los polígonos de la DAP no se usan en la Fase 1; entran en la Fase 2 y en la prueba de acoplamiento del Anexo A, que ya tiene registrados el nombre exacto y la fecha de producción de los diez archivos VNP46A3 (falta el SHA-256 de nueve).

## Método

Se filtró `Conducta = Homicidio`, se unificaron los códigos de territorio y se completó el calendario territorio-mes con ceros declarados. Cada rasgo del mes `m` usa solo meses hasta `m−1`; una prueba automática confirma que alterar `H` en `m` no cambia sus rasgos.

| Componente | Decisión |
| --- | --- |
| Rasgos | rezagos 1–3, sumas y medias móviles de 3, 6 y 12 meses, cambio entre rezagos, mes cíclico (seno y coseno) y territorio (one-hot); 33 entradas |
| Partición por mes objetivo | entrenamiento 2015-01 a 2020-12 (1.512 obs., 285 alertas, 18,8 %); validación 2021-01 a 2022-12 (504 obs., 96 alertas, 19,0 %); prueba 2023-01 a 2023-11 (231 obs., 44 alertas, 19,0 %) |
| Preprocesamiento | escalador ajustado solo con entrenamiento |
| Líneas base | clase mayoritaria y regla del conteo previo (`lag1 − media12`) con umbral elegido en validación |
| MLP | dos capas ocultas con ReLU y Dropout, salida en logit, `BCEWithLogitsLoss`, Adam (tasa 1e-3), lotes de 64, parada temprana con paciencia 15, semilla 42 |
| Umbral de alerta | elegido en validación maximizando F1 (β = 1); la prueba se usó una sola vez |

## Resultados

**El modelo no supera al azar en la prueba.** Con 19,0 % de alertas, un clasificador sin señal obtiene un PR-AUC cercano a ese valor, y el MLP logra 0,188.

### Análisis exploratorio

La mayoría de los conteos mensuales por territorio son pequeños (mediana 3 casos por fila de homicidio, máximo 35). El desbalance es de cerca de 81/19, por lo que la métrica principal es PR-AUC y el recall de la clase de alerta, no la exactitud. El porcentaje de meses con alerta va de cerca de 1 % (territorio 50) a cerca de 25 % (territorio 4).

### Sobreajuste inducido y corregido

Se entrenó con 300 filas y redes grandes para provocar el sobreajuste.

| Corrida | Época del mínimo de validación | Pérdida mínima de validación | Brecha final (val − train) |
| --- | --- | --- | --- |
| Grande (128-64), sin Dropout | 5 | 0,4888 | 2,6479 |
| Grande + Dropout 0,5 | 6 | 0,4886 | 1,6498 |
| Pequeña (16-8) | 28 | 0,4961 | 1,0916 |

En la red grande sin Dropout, la validación sube de ≈ 0,49 a 2,65 mientras el entrenamiento cae a casi 0. El Dropout y una red menor reducen la brecha pero no la eliminan; lo que evita el daño es parar temprano.

### Modelo final

Con el entrenamiento completo y parada temprana, los tres candidatos empatan en la práctica.

| Modelo | Mejor época | Pérdida val. mínima | Brecha final |
| --- | --- | --- | --- |
| 16-8, Dropout 0,2 | 28 | 0,4787 | 0,0268 |
| 16-8, sin Dropout (elegido) | 70 | 0,4748 | 0,0594 |
| 64-32, Dropout 0,2 | 29 | 0,4767 | 0,0637 |

La diferencia entre el elegido y los otros es de 0,002 a 0,004, sin significado práctico. Predecir siempre la prevalencia (≈ 19 %) da una pérdida de ≈ 0,486, así que el modelo mejora apenas un 2 %.

### Evaluación en prueba (2023-01 a 2023-11, 231 observaciones)

Umbral elegido en validación: 0,23.

| Modelo | Exactitud | Precisión (alerta) | Recall (alerta) | F1 (alerta) | PR-AUC |
| --- | --- | --- | --- | --- | --- |
| Clase mayoritaria | 0,810 | 0,000 | 0,000 | 0,000 | — |
| Regla del conteo previo | 0,251 | 0,197 | 0,955 | 0,327 | 0,168 |
| MLP | 0,550 | 0,174 | 0,364 | 0,235 | 0,188 |

Matriz de confusión del MLP: 16 verdaderos positivos, 28 falsos negativos, 76 falsos positivos y 111 verdaderos negativos. La regla del conteo previo alerta en casi todos los meses (recall alto por construcción); su F1 de 0,327 es similar al de alertar siempre (≈ 0,32).

### Errores por territorio

Cada territorio tiene entre 0 y 5 alertas reales en los 11 meses de prueba, por lo que el recall por territorio es muy incierto y se reporta solo como indicio, no como ranking. Por ejemplo, el territorio 8 (5 alertas reales) detectó 3, y los territorios 3 y 6 (4 alertas reales cada uno) no detectaron ninguna; los territorios 12, 50 y 2 no tuvieron alertas reales.

## Reflexión ética y decisión recomendada

**Se recomienda no usar este modelo para decidir ni para priorizar territorios.** Con el umbral elegido, 82,6 % de las alertas emitidas fueron falsas (76 de 92) y se omitió 63,6 % de los repuntes reales (28 de 44).

| Error | Consecuencia para el cliente | Magnitud en la prueba |
| --- | --- | --- |
| Falso positivo | estigma territorial y gasto de recursos en un territorio sin repunte | 76 de 92 alertas |
| Falso negativo | un repunte no avisado y sin prevención | 28 de 44 repuntes |

Principio del *Marco ético para la IA en Colombia* (Guío Español et al., 2021) aplicado: **transparencia y explicación**. Este informe declara que el sistema no supera al azar, en lugar de presentarlo como herramienta de alerta, y no publica rankings de peligrosidad por territorio. \[Los integrantes pueden añadir otros principios que hayan revisado.\]

Si en el futuro se reutiliza el enfoque, deberían exigirse como mínimo verificación humana de toda alerta, reporte de falsos positivos por territorio y un periodo de prueba más largo; una política de umbral (valor de `BETA`) solo tendría sentido cuando exista un modelo con señal demostrada.

## Límites y trabajo pendiente

**Límites del resultado**

- Hechos registrados no son hechos ocurridos; hay posible subregistro.
- Los ceros rellenados (22,8 % de los pares) no equivalen a ausencia real de homicidios.
- La prueba es corta: 11 meses y 44 alertas reales, con 0 a 5 por territorio; las métricas por territorio son indicios.
- Una sola semilla (42) y una sola partición: no se midió la variación entre ejecuciones.
- La etiqueta (H ≥ 2 y H > P75 de 12 meses) sobre conteos pequeños se parece mucho a ruido, lo que explica en parte la falta de señal.
- La radiancia nocturna no se usa todavía; mide actividad e iluminación, no homicidios, y su aporte lo dirá la ablación de la Fase 2. Las fechas de producción de los archivos descargados (mayo y junio de 2025) reflejan un reprocesamiento y no miden la latencia operativa de publicación.

**Pendiente del equipo**

- [ ] Integrantes y firmas en la propuesta, el README y el anexo C.
- [ ] Calcular el SHA-256 de nueve archivos VNP46A3 del Anexo A, medir la latencia con compuestos recientes (2026-07 a 2026-10) y descargar los polígonos de la DAP.
- [ ] Repetir la búsqueda de réplicas en la búsqueda interna de kaggle.com/datasets (anexo B3).
- [ ] Confirmar el texto sobre cómo se inspeccionó el Excel en la sesión anterior (anexo C-4) y completar la Bitácora de IA.


## Referencias y uso de IA

- Alcaldía de Medellín, Secretaría de Seguridad y Convivencia, SISC. (s. f.). *Consolidado cantidad de casos criminalidad en comunas por año y mes* \[Conjunto de datos\]. Datos Abiertos Colombia. https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif
- NASA VIIRS Land Science Investigator-led Processing System. (2025). *VIIRS/NPP Lunar BRDF-Adjusted Nighttime Lights Monthly L3 Global 15 arc second Linear Lat Lon Grid* (Versión 2) \[Conjunto de datos\]. NASA LAADS DAAC. https://doi.org/10.5067/VIIRS/VNP46A3.002
- Departamento Administrativo de Planeación, Alcaldía de Medellín. (s. f.). *Límite político administrativo: comunas y corregimientos* \[Servicio cartográfico\]. https://www.medellin.gov.co/servidormapas/rest/services/mapas\_nacionales/VC\_Limite\_Politico\_Admtivo/MapServer/1

**Uso de IA.** Claude (Anthropic) ayudó a preparar el cuaderno, los anexos y este informe a partir del CSV agregado y de las salidas que el equipo ejecutó en Colab. Los microdatos del Excel no se entregaron en esta sesión. Cada intervención y la decisión del equipo (aceptada, modificada o rechazada) se registran en la Bitácora de IA, que los integrantes completan con lo que realmente decidieron.

**Integrantes:** \[COMPLETAR\] · **Versión:** v1
