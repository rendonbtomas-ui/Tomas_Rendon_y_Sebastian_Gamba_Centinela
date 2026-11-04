# Bitácora de IA · Proyecto Centinela · Fase 1 · v1

**Integrantes:** [COMPLETAR] · **Fecha de la sesión:** 2026-11-03 · **Herramienta:** Claude (Anthropic), interfaz web, una sola conversación

## 1. Resumen del uso
La mayor parte del código del cuaderno (versión v1 ajustada), de los anexos, de la propuesta, del README, del informe y de esta bitácora fue redactada por la IA a partir de lo que el equipo le entregó. El equipo aportó los archivos y la retroalimentación del docente, ejecutó el cuaderno en Colab, ejecutó la búsqueda de NASA con `earthaccess` y devolvió los resultados a la IA. La IA ejecutó por su cuenta solo las celdas sin PyTorch (no pudo instalarlo); el MLP se entrenó únicamente en Colab, por el equipo.

## 2. Datos entregados a la IA
| Dato | Tipo | ¿Permitido por la regla? |
| --- | --- | --- |
| `consolidado_cantidad_casos_criminalidad_en_comunas_por_anio_mes.csv` | Agregado por comuna y mes, sin personas | Sí |
| Propuesta, anexos, README y cuaderno (versión previa) | Documentos del equipo | Sí |
| Retroalimentación del docente (texto) | Documento | Sí |
| PDF con las salidas del cuaderno en Colab | Resultados agregados | Sí |
| Nombres, tamaños, fechas de producción y SHA-256 de archivos VNP46A3 | Metadatos de archivos de NASA | Sí |
| `Homicidio 141225.xlsx` | Microdatos de víctimas | **No se entregó en esta sesión.** Lo ocurrido en la sesión anterior: [INTEGRANTES: describir exactamente qué pasó] |

## 3. Registro de consultas (orden cronológico)
La columna «Decisión del equipo» solo está llenada donde la propia conversación lo respalda. En las demás, el equipo debe marcar **Aceptada / Modificada / Rechazada** con el motivo, después de revisar el resultado.

| N.º | Qué pidió el equipo | Qué hizo la IA | Archivos afectados | Decisión del equipo |
| --- | --- | --- | --- | --- |
| 1 | Completar anexos y código según la retroalimentación del docente, y adaptar el cuaderno a Colab sin Drive. | Leyó el CSV real (23.164 × 4), calculó su SHA-256 y la huella de crudeza; ajustó el cuaderno (columnas reales, códigos `1`/`1.0`, carga directa); ejecutó las celdas sin PyTorch; completó anexos B y C con cifras reales y propuso las 10 claves del Anexo A. | Cuaderno; anexos A, B, C; propuesta; README | Aceptada: el equipo ejecutó el cuaderno en Colab. Modificada: el equipo puso su propia `FECHA_DESCARGA`. [COMPLETAR el resto] |
| 2 | Qué archivos cargar en `data/raw`. | Indicó que solo el CSV del consolidado y que el Excel de víctimas no se sube. | — | [COMPLETAR] |
| 3 | Redactar la declaración de IA. | Entregó una plantilla con lo que hizo la IA y la decisión del equipo en blanco. | Esta bitácora (versión previa) | [COMPLETAR] |
| 4 | Qué pasa con las otras dos fuentes (NASA y polígonos). | Explicó que no entran al modelo de la Fase 1, solo al Anexo A y a la Fase 2, y dio un orden de trabajo. | — | [COMPLETAR] |
| 5 | Interpretar las salidas de Colab. | Revisó el PDF: sin errores técnicos; el MLP no supera al azar (PR-AUC 0,188 frente a prevalencia 19,0 %); señaló el formato de `FECHA_DESCARGA` y la línea base débil. | — | [COMPLETAR: ¿adoptan esta interpretación?] |
| 6 | Hacer el informe. | Creó el informe en Docs con resultados, reflexión ética (principio de transparencia), límites y pendientes. | Informe | [COMPLETAR] |
| 7 | Cómo quedaría el anexo. | Actualizó `ANEXOS_FASE1.md` con la fecha de descarga y la confirmación de la ejecución en Colab. | Anexos | [COMPLETAR] |
| 8 | Cómo nombrar los archivos. | Propuso la estructura del repositorio y reglas de nombres. | — | [COMPLETAR] |
| 9 | Si el README quedaba igual con los resultados. | Añadió la tabla de prueba y la conclusión al README. | README | [COMPLETAR] |
| 10 | Revisar la salida de `earthaccess`. | Explicó que los identificadores `LAADS:…` no son nombres de archivo y dio celdas para listar nombres, fechas de producción y SHA-256. | — | Aceptada: el equipo ejecutó las celdas y devolvió los resultados. |
| 11 | Completar con los nombres reales de VNP46A3. | Rellenó las 10 filas del Anexo A, calculó los retrasos (648 a 1.577 días), señaló que reflejan un reprocesamiento y ajustó propuesta, README y anexos. | Anexos; propuesta; README | [COMPLETAR] |
| 12 | Estado de la propuesta. | Resumió su contenido y lo pendiente. | — | [COMPLETAR] |
| 13 | Revisar el informe. | Verificó que no hubiera cambios y actualizó tres puntos desfasados. | Informe | [COMPLETAR] |
| 14 | Esta bitácora (versión final). | Redactó este registro. | Esta bitácora | [COMPLETAR] |

## 4. Verificaciones que ayudan a documentar su propio criterio
Si las hacen de verdad, regístrenlas en la tabla como «Modificada» o «Aceptada tras verificar»:
- Abrir el CSV y comprobar las columnas y la ausencia de fecha exacta, barrio, edad, sexo y coordenadas (anexo C-1).
- Comprobar que el SHA-256 de su CSV coincide con el de B1.
- Comparar las cifras del informe con las que imprimió su propia ejecución en Colab.
- Revisar la licencia de NASA en la fuente y decidir si el texto les parece correcto.

## 5. Lo que la IA no hizo
- No firmó, no puso nombres de integrantes ni ejecutó el MLP.
- No descargó datos de NASA ni de la DAP; no verificó la latencia operativa de VNP46A3.
- No repitió la búsqueda dentro de kaggle.com/datasets.

## 6. Declaración
Declaramos que esta bitácora describe fielmente el uso de IA en este trabajo y que revisamos el contenido generado antes de entregarlo.

| Integrante | Firma | Fecha |
| --- | --- | --- |
| [COMPLETAR] | | |
| [COMPLETAR] | | |
| [COMPLETAR] | | |
