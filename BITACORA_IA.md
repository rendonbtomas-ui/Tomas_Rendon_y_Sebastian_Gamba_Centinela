
# Bitácora de uso de Inteligencia Artificial · Proyecto Centinela · Fase 1

**Integrantes:** Tomás Rendón Bustamante, Juan Esteban Gamba  
**Fecha de la sesión:** 2026-11-03  
**Herramienta:** Claude (Anthropic), interfaz web, una sola sesión de trabajo continuo

## 1. Resumen del uso
El equipo lideró el diseño metodológico y la ejecución experimental del proyecto. Sin embargo, para agilizar el desarrollo técnico, se utilizó la inteligencia artificial como un asistente de programación y consulta avanzada. La IA apoyó de manera significativa en el diseño del flujo de trabajo para el cargue correcto del archivo CSV, la estructuración de la lógica de limpieza de datos (unificación de códigos territoriales y relleno de ceros en el calendario), la elaboración paso a paso de los bloques de código en Google Colab (ingeniería de características y arquitectura MLP en PyTorch), y los ajustes técnicos de la V2 del modelo para controlar el sobreajuste.

## 2. Datos entregados a la IA
| Dato o recurso | Tipo / Formato | ¿Permitido por la regla? |
| --- | --- | --- |
| `consolidado_cantidad_casos_criminalidad_en_comunas_por_anio_mes.csv` | Datos agregados por comuna y mes (sin información de personas) | Sí |
| Cuaderno de trabajo previo y propuesta metodológica | Documentos de trabajo del equipo | Sí |
| Retroalimentación y directrices del docente | Texto institucional | Sí |
| Salidas métricas y gráficos de validación de Colab | Resultados agregados | Sí |
| Metadatos públicos de imágenes satelitales NASA (VNP46A3 v2) | Estructura de metadatos | Sí |
| `Homicidio 141225.xlsx` | Microdatos detallados de víctimas | **No se entregó.** Manejado de forma local y estrictamente privada por el equipo. |

## 3. Registro de consultas y soporte técnico

| N.º | Qué pidió el equipo | Qué aportó la IA | Componente afectado | Decisión del equipo |
| --- | --- | --- | --- | --- |
| 1 | Consultar la forma óptima de estructurar el cargue del CSV en Pandas, manejando posibles anomalías de formato. | Propuso el esquema de lectura, validación de columnas y manejo inicial de tipos de datos. | Carga de datos (Colab) | **Aceptada y adaptada:** Se integró el código base ajustándolo a las rutas y necesidades específicas del entorno del equipo. |
| 2 | Solicitar el paso a paso lógico para la limpieza de datos (unificación de códigos como `1` y `1.0`, y relleno de pares territorio-mes faltantes con ceros). | Detalló las funciones en Pandas para la indexación completa del calendario y la imputación de ceros declarados. | Limpieza y preprocesamiento | **Aceptada tras pruebas:** El equipo validó la integridad de las filas resultantes para evitar fugas de datos. |
| 3 | Construir paso a paso la estructura del código para la ingeniería de características (rezagos, medias móviles) y el modelo MLP. | Generó la estructura modular en PyTorch (capas, función de pérdida `BCEWithLogitsLoss`, optimizador Adam). | Modelado MLP | **Modificada y depurada:** El equipo ajustó los hiperparámetros (red 16-8) y la lógica de partición cronológica de forma autónoma. |
| 4 | Asesoría técnica para los ajustes de la V2 (control de sobreajuste mediante parada temprana y experimentación con Dropout). | Explicó cómo estructurar el bucle de entrenamiento con *early stopping* y evaluó las métricas de pérdida en validación. | Ajustes del modelo (V2) | **Aceptada:** Se aplicaron las pautas para estabilizar el entrenamiento y obtener las curvas de validación definitivas. |
| 5 | Consultar la interpretación teórica de métricas en escenarios de clases desbalanceadas (PR-AUC vs. exactitud). | Explicó el comportamiento de la curva PR-AUC ante una prevalencia del 19%. | Análisis de resultados | **Aceptada:** Sirvió de base sólida para argumentar el rendimiento real del modelo en el informe. |
| 6 | Apoyo en la redacción preliminar de la propuesta, anexos y el informe técnico. | Redactó borradores iniciales a partir de los apuntes y notas conceptuales provistas por el equipo. | Documentación general | **Modificada sustancialmente:** El equipo reescribió y pulió los textos para reflejar con precisión los hallazgos empíricos obtenidos. |

## 4. Verificaciones realizadas por el equipo
- **Ejecución y pruebas locales:** Todo el código sugerido por la IA fue probado, depurado y ejecutado directamente por los integrantes en Google Colab.
- **Control de privacidad:** Se restringió el uso de la herramienta estrictamente a datos agregados y sintaxis de programación, protegiendo los microdatos sensibles.
- **Criterio analítico:** Las conclusiones sobre el modelo (reconocer que no supera al azar y la alta tasa de falsos positivos) fueron alcanzadas mediante el análisis crítico de los resultados obtenidos por el equipo.

## 5. Lo que la IA no hizo
- No participó en la concepción original de la hipótesis del proyecto ni en la decisión de usar un MLP para la alerta de homicidios.
- No tuvo acceso ni procesó los microdatos del archivo Excel de víctimas.
- No realizó de forma automática la ejecución del modelo ni generó conclusiones automáticas sin la supervisión e interpretación directa del equipo.

## 6. Declaración de autoría
Declaramos que este trabajo es producto de la autoría y el esfuerzo analítico del equipo. Las herramientas de inteligencia artificial se emplearon como un soporte técnico avanzado para optimizar la redacción del código de limpieza, el cargue de datos y los ajustes de la V2, siendo todo el contenido final verificado, comprendido y avalado por los integrantes.

| Integrante | Firma | Fecha |
| --- | --- | --- |
| Tomás Rendón Bustamante |_Tomás Rendón Bustamante_ | 2026-11-08|
| Juan Sebastián Gamba Aguirre | _Juan Sebastián Gamba Aguirre |2026-11-08 |
