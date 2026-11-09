# Auditoría a la IA · Proyecto Centinela · Fase 1 · v2

**Integrantes:** Tomás Rendón Bustamante · Juan Sebastián Gamba Aguirre · **Herramienta:** Claude (Anthropic) · **Fecha de la auditoría:** 2026-11-08

Las celdas que reproducen los casos 1 y 2 están en la **sección 10** de `01_centinela_mlp.ipynb` (10a y 10b); el caso 3 se mide en la **sección 8b**. Las cifras de los casos 1 y 2 son las de la v1 (ejecución en Colab del 2026-11-03). Los textos marcados **[CONFIRMAR]** los completan los integrantes.

## Caso 1 · Formato de `Fecha_hecho` mal descrito en los anexos
- **Qué falló:** los anexos B4 y C-1 de la v1 (2026-11-03), redactados con ayuda de la IA, describían la columna como «`Fecha_hecho` (texto `20261103`)», es decir, en formato AAAAMMDD.
- **Evidencia de origen:** el texto literal citado, que estaba en `ANEXOS_FASE1.md` (v1), secciones B4 y C-1-A. 
- **Síntoma en el cuaderno:** la salida de la celda 5 muestra «Rango de fechas: 2003-01 a 2023-11» y `df.head()` trae `2003-01`. En la celda 10a, la versión de la IA (formato AAAAMMDD) lee **0 de 23.164** filas.
- **Corrección y misma medición:** el formato real es `AAAA-MM` (el cuaderno agrega `-01` para obtener una fecha completa). Con ese formato se leen **23.164 de 23.164** filas. Los anexos B4 y C-1 ya están corregidos.
- **Fundamento:** probable origen del error: `FECHA_DESCARGA = "20261103"` (AAAAMMDD) del cuaderno se confundió con el formato de la columna **(hipótesis, no verificada)**. El impacto fue documental: los cálculos nunca fallaron porque el cuaderno usa el formato correcto.
- **Cómo se detectó:** en la revisión del 2026-11-03, al contrastar los anexos con la salida de la celda 5. 
- **Decisión:** aceptada y corregida.

## Caso 2 · La «regla del conteo previo» se vuelve casi «alertar siempre»
- **Qué falló:** la línea base 2 (alertar si `lag1 − media12` supera un umbral elegido por F1 en validación) quedó con un umbral **negativo** (−1,542), así que emite alerta en el **92,2 %** de los meses de prueba. Se presentó como «regla simple», pero su recall alto no es mérito de la regla.
- **Evidencia de origen:** salida de la celda 14 del cuaderno (umbral −1,542; exactitud 0,251; recall 0,955; F1 0,327).
- **Medición (celda 10b, mismas métricas y misma prueba):**

| Línea base | Exactitud | Precisión | Recall | F1 | VP | FN | FP | VN |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Regla del conteo previo | 0,251 | 0,197 | 0,955 | 0,327 | 42 | 2 | 171 | 16 |
| Alertar siempre | 0,190 | 0,190 | 1,000 | 0,320 | 44 | 0 | 187 | 0 |

- **Corrección:** la comparación honesta del MLP incluye «alertar siempre». El MLP (F1 0,235) no supera ninguna de las dos en F1, y su PR-AUC (0,188) apenas supera al de la regla (0,168). El informe y el README ya lo dicen así. No se modificó la regla ni el modelo, para no cambiar los resultados reportados.
- **Decisión:** aceptada y documentada. **Seguimiento (v2):** «alertar siempre» y una regla con igual número de alertas (20 %) se añadieron como líneas base en la sección 8b.

## Caso 3 · La v1 no le daba a la red el umbral que define la etiqueta y tenía entradas redundantes
- **Qué falló:** la etiqueta es `H ≥ 2` y `H >` percentil 75 de los 12 meses anteriores, pero la v1 (escrita con ayuda de la IA) no incluía ese percentil entre las entradas. En cambio incluía `suma3`, `suma6`, `suma12` (iguales a la media por el tamaño de la ventana) y `delta` (igual a `lag1 − lag2`): cuatro entradas sin información nueva.
- **Evidencia de origen:** lista `FEATURES_NUM` de la celda 8 de la v1 (12 variables numéricas, 33 entradas).
- **Cómo se detectó:** al comparar con el cuaderno de otro intento, que sí incluía el percentil, y al revisar qué entradas eran combinaciones exactas de otras. 
- **Medición antes y después** (sección 8b, ejecutada en Colab con PyTorch el 2026-11-08; misma partición; 5 semillas; umbral del 20 % de alertas fijado en validación):

| Configuración | PR-AUC validación | PR-AUC prueba |
| --- | --- | --- |
| v1 (33 entradas) | 0,251 ± 0,017 | 0,183 ± 0,011 |
| v2a: solo los rasgos nuevos (30 entradas) | 0,280 ± 0,010 | 0,214 ± 0,037 |
| v2: rasgos nuevos + `weight_decay` + paciencia 20 | 0,276 ± 0,016 | 0,208 ± 0,034 |

- **Corrección:** el cambio de rasgos explica casi toda la mejora en validación (v2a 0,280 y v2 0,276; `weight_decay` y la paciencia no la cambian). En prueba, la v2 (promedio de semillas 0,204) no se distingue del azar (prevalencia 0,190; valor p de permutación ≈ 0,49; intervalo 0,149 a 0,305). La semilla 42, fijada de antemano como representativa, quedó en 0,180: acertó 3 de 37 alertas cuando por azar se esperaban ≈ 7. El caso no convierte el modelo en uno útil.
- **Fundamento:** la omisión era evitable: el percentil se calcula solo con meses anteriores, por lo que incluirlo no produce fuga.
- **Decisión:** aceptada. Se ajustó en la v2, se conservó la v1 como referencia y se declaró que la prueba se usó dos veces.
