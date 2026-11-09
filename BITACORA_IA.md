# Bitácora de IA · Proyecto Centinela · Fase 1 · v2 (revisada 2026-11-08)

**Integrantes:** Tomás Rendón Bustamante · Juan Sebastián Gamba Aguirre · **Herramienta:** Claude (Anthropic), interfaz web · **Sesiones:** una sesión previa a la v1 **[CONFIRMAR fecha]**, 2026-11-03 y 2026-11-08

> Esta versión reemplaza a la bitácora del 2026-11-03, que describía el uso de la IA de forma más limitada que el README, el informe y el anexo C-4. Los textos marcados **[CONFIRMAR]** solo los pueden completar los integrantes: se escriben con lo que realmente ocurrió, antes de firmar.

## 1. Resumen del uso
La IA tuvo un papel sustancial. A partir de lo que pedimos, de la retroalimentación del docente y de las salidas que ejecutamos en Colab, Claude redactó el cuaderno, la propuesta, los anexos, el README y el informe, y en la revisión final también corrigió esos documentos y comentó el cuaderno. Nosotros descargamos el CSV del portal, ejecutamos el cuaderno en Colab y la consulta a NASA con nuestra cuenta Earthdata, revisamos las salidas y somos quienes firmamos y sustentamos. **[CONFIRMAR y completar: qué decisiones metodológicas tomamos nosotros y qué cambios hicimos a lo que la IA propuso.]**

## 2. Datos entregados a la IA
| Dato | Tipo | ¿Permitido por la regla? |
| --- | --- | --- |
| `consolidado_cantidad_casos_criminalidad_en_comunas_por_anio_mes.csv` | Agregado por comuna y mes, sin personas | Sí |
| Propuesta, anexos, README, cuaderno e informe (versiones previas) | Documentos del equipo | Sí |
| Retroalimentación del docente (texto) | Documento institucional | Sí |
| Salidas de Colab (métricas, tablas, nombres y SHA-256 de archivos NASA) | Resultados agregados | Sí |
| PDF del cuaderno `01_centinela_mlp_AUDITADO` de **un compañero del curso** (impreso desde Colab) | Código y salidas de otro trabajo del curso, sin datos de personas | Sí en cuanto a datos personales; **debe declararse** porque inspiró el ajuste v2. **[CONFIRMAR cómo se obtuvo y con permiso de quién se compartió con la IA.]** |
| Salidas de la consulta a NASA (nombres, fechas de producción y SHA-256 de diez archivos) | Metadatos de archivos | Sí |
| `Homicidio 141225.xlsx` | Microdatos de víctimas | **No.** Según el anexo C-4, el archivo se cargó y examinó con un asistente de IA en una sesión previa a la v1, lo que incumplía la regla. **[CONFIRMAR fecha y qué se mostró exactamente.]** Se retiró del proyecto y ninguna cifra derivada de él se usa. |

## 3. Registro de consultas (orden cronológico)
| N.º | Fecha | Qué pedimos | Qué hizo la IA | Archivos afectados | Cómo se verificó | Decisión y justificación |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | **[CONFIRMAR]** (sesión previa) | Explorar el Excel de víctimas y proponer el problema **[CONFIRMAR]** | Examinó la estructura y los conteos del Excel | Propuesta inicial (v0) | El docente indicó que el Excel no coincidía con el archivo público (19.647 × 36 hasta 2023-11-29 frente a 5.711 × 17 hasta 2025-12-14) | **Rechazada.** Se retiró el Excel y sus cifras: contiene datos de personas y no tiene procedencia documentada (anexos B1 y C-4). |
| 2 | 2026-11-03 | Ajustar la propuesta a la retroalimentación del docente (serie agregada de MEData, partición 2015–2020 / 2021–2022 / 2023, checklist Ley 1581, Anexo A, licencias, búsqueda de espejos) | Redactó propuesta v1, anexos y README | `propuesta-de-datos-fase1.md`, `ANEXOS_FASE1.md`, `README.md` | La ejecución en Colab reprodujo las cifras de crudeza del anexo B4 (23.164 × 4, 4.071 filas de homicidio, 1.200 ceros rellenados) y el SHA-256 coincide con B1 | **Aceptada** **[CONFIRMAR si modificamos textos]**. |
| 3 | 2026-11-03 | Cuaderno del MLP para la Fase 1 | Escribió el cuaderno completo (secciones 0 a 9 y Anexo A parte MEData) | `01_centinela_mlp.ipynb` | Lo ejecutamos completo en Colab el 2026-11-03, sin errores; pasó la prueba anti-fuga (celda 8) | **Aceptada tras ejecutar** **[CONFIRMAR cambios del equipo]**. |
| 4 | 2026-11-03 | Código para consultar y descargar VNP46A3 con `earthaccess` | Dio las celdas de búsqueda, descarga, fecha de producción y SHA-256 | `02_anexoA_nasa_earthaccess.ipynb` (celdas A1–A3) | Las ejecutamos con nuestra cuenta Earthdata; el 2026-11-08 se comprobó que el prefijo esperado coincide con el nombre real en los diez casos | **Aceptada tras verificar.** |
| 5 | 2026-11-03 | Informe técnico-ético y búsqueda de espejos en kaggle.com (anexo B3) | Redactó el informe v1 con las salidas del cuaderno; dos búsquedas web con operador `site:` (sin coincidencias) | Informe, anexo B3 | Cifras contrastadas con las salidas del cuaderno; falta repetir la búsqueda en la búsqueda interna de kaggle.com/datasets **[CONFIRMAR quién hizo las búsquedas]** | **Aceptada con pendientes.** |
| 6 | 2026-11-03 | Plantilla de bitácora | Estructuró una plantilla y la bitácora v1 | `BITACORA_IA.md` (v1) | Contraste con README, informe y anexo C-4 (2026-11-08) | **Rechazada.** Se reemplazó por esta versión porque describía un uso de la IA más limitado que el real. |
| 7 | 2026-11-08 | Revisar si faltaba algo en los entregables y explicar los resultados | Leyó los siete archivos y la actividad con su rúbrica; encontró: `Fecha_hecho` mal descrita en los anexos B4 y C-1, bitácora incoherente con el resto, textos pendientes, nombres inconsistentes, falta de auditoría y celdas de NASA que rompían «Ejecutar todo» | Anexos, bitácora, cuaderno | El error de `Fecha_hecho` se comprobó ejecutando código sobre el CSV (cuaderno, sección 10a) **[CONFIRMAR: nuestra revisión]** | **Aceptada.** |
| 8 | 2026-11-08 | Ajustar todos los archivos y comentar el código en primera persona | Corrigió anexos, propuesta y README; reescribió la bitácora; creó la auditoría y el informe en PDF con las figuras de la corrida de Colab; comentó el cuaderno **sin cambiar su lógica** (comparación automática del código), movió NASA al cuaderno 02, completó los textos de interpretación y añadió la sección 10 | Todos | La IA reprodujo la partición y las líneas base con el CSV (1.512/285, 504/96, 231/44, umbral −1,542). **[CONFIRMAR: volvimos a ejecutar «Ejecutar todo» en Colab y las cifras coinciden con el informe]** | **Aceptada** **[CONFIRMAR]**. Los comentarios del cuaderno están en primera persona a pedido nuestro, pero los redactó la IA. |
| 9 | 2026-11-08 | Averiguar por qué el cuaderno de un compañero «funcionó» y el nuestro no; hacer el ajuste, documentarlo en el informe (en Word, no en PDF), completar el SHA-256 de NASA y actualizar todos los archivos | Comparó los dos cuadernos: el modelo era equivalente (pérdida de validación 0,4748 frente a 0,4743) y la diferencia aparente era que el compañero reportaba validación y nosotros prueba; propuso y aplicó el ajuste v2 (percentil 75 como entrada, sin entradas redundantes, `weight_decay`, umbral por presupuesto de alertas, 5 semillas, prueba de permutación, líneas base de igual número de alertas); añadió la sección 8b y el caso 3 de auditoría; completó el SHA-256 de los diez archivos NASA; reescribió el informe en Word | Cuaderno 01, cuaderno 02, anexos, propuesta (enmienda), README, checklist, bitácora, auditoría, informe | La IA reejecutó el pipeline de datos sobre el CSV y reprodujo todas las cifras de la v1 (particiones, umbral −1,542, tabla del Anexo A, SHA-256). Como en su entorno no hay PyTorch, la IA no pudo ejecutar la sección 8b; la ejecutamos nosotros en Colab (PyTorch) el 2026-11-08 y las cifras del informe, del README y de la auditoría son las de esa ejecución. Una réplica previa en NumPy hecha por la IA había llegado a la misma conclusión (prueba ≈ 0,20, sin distinguirse del azar) y no se usa en los documentos. | **Aceptada con pendiente.** Idea del percentil 75 tomada de la comparación con el cuaderno del compañero; el código lo escribió la IA. **[CONFIRMAR decisión del equipo.]** |

## 4. Verificaciones realizadas
- SHA-256 del CSV impreso por el cuaderno, igual al del anexo B1.
- Cifras de crudeza y de partición del cuaderno, iguales a las de los anexos y del informe (contrastadas el 2026-11-08).
- Prueba anti-fuga del cuaderno (celda 8) y `grep` de réplicas de terceros sin coincidencias en los dos cuadernos (2026-11-08).
- Reejecución del pipeline de datos sobre el CSV en el entorno de la IA (2026-11-08): reprodujo las cifras de la v1 y los diez prefijos del Anexo A; nombres, fechas y días de retraso de los diez archivos NASA coinciden con el Anexo A.
- Sección 8b ejecutada en Colab con PyTorch (2026-11-08); sus cifras se copiaron al informe en Word, al README y a la auditoría (caso 3). **[CONFIRMAR]** Reejecución completa («Ejecutar todo») antes de entregar y guardar el cuaderno con las salidas.

## 5. Lo que hizo cada parte
**La IA:** redactó y comentó el cuaderno, los anexos, la propuesta, el README, el informe, esta bitácora y la auditoría. No ejecutó nada en Colab ni en Earthdata; las cifras de la v2 que aparecen en los documentos son las de nuestra ejecución de la sección 8b en Colab.
**Nosotros:** descargamos el CSV del portal oficial, ejecutamos el cuaderno y las consultas a NASA, revisamos las salidas y firmamos. **[CONFIRMAR y completar con nuestras palabras: decisiones propias de datos, arquitectura, regularización, métricas y umbral, y quién sustenta cada parte.]**

## 6. Declaración
Declaramos que esta bitácora describe el uso real de la IA en el proyecto **[CONFIRMAR antes de firmar]**.

| Integrante | Firma | Fecha (AAAA-MM-DD) |
| --- | --- | --- |
| Tomás Rendón Bustamante | | |
| Juan Sebastián Gamba Aguirre | | |
