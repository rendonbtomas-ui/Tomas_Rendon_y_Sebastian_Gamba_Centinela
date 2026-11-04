# Anexos de la Propuesta de Datos · Proyecto Centinela Homicidios Medellín · v1

**Integrantes:** [COMPLETAR] · **Versión:** v1 · **Ejecución del cuaderno en Colab:** 2026-11-03, 21:37 (hora de Bogotá), sin errores · **Estado (2026-11-03):** lo marcado **[PENDIENTE]** es trabajo que debe ejecutar un integrante con acceso a las fuentes (cuenta Earthdata, fecha real de descarga, firmas); no son evidencias. Lo que **no** dice [PENDIENTE] se calculó con el archivo real del consolidado (23.164 filas × 4 columnas, SHA-256 en B1).

---

## Anexo A · Prueba de acoplamiento (10 filas reales) — PARCIAL

**Clave:** `(código de comuna/corregimiento, año-mes)` · **Tipo:** agregada, con salvedad de fuga (la imagen y el conteo del mes `m` no se usan para predecir `m`; para predecir `m` se usa la imagen de `m−2`).

**Lo que sí está establecido:**
- Producto: VNP46A3 versión 2 (DOI 10.5067/VIIRS/VNP46A3.002), temporal desde 2012-01-01, resolución 15 arc-s, formato HDF5, acceso con Earthdata Login (ficha NASA Earthdata y página LAADS, 2026-11-03).
- Nombre de archivo (ficha NASA): `VNP46A3.AYYYYDDD.hXXvYY.CCC.YYYYDDDHHMMSS.h5`; los últimos 13 dígitos son la **fecha de producción**.
- Teselas: Medellín (≈6,25° N, 75,57° O) cae en la tesela **h10v08** (h = ⌊(lon+180)/10⌋ = 10; v = ⌊(90−lat)/10⌋ = 8). **[VERIFICAR]** con el shapefile de la grilla Black Marble.
- Las diez claves y el conteo de homicidios **salen del consolidado real** (celda 3b del cuaderno, semilla 42; solo filas que existen en la fuente, no ceros rellenados; diez meses de imagen distintos).

| N.º | Clave (código, año-mes) | Homicidios del consolidado (`Conducta = Homicidio`, `Cantidad_casos`) | Imagen usada (`m−2`) y prefijo **esperado** del archivo (sin verificar) | Nombre exacto del `.h5` y fecha de producción |
| --- | --- | --- | --- | --- |
| 1 | (6, 2021-03) | 1 | 2021-01 · `VNP46A3.A2021001.h10v08.002.` | [PENDIENTE] |
| 2 | (70, 2021-11) | 1 | 2021-09 · `VNP46A3.A2021244.h10v08.002.` | [PENDIENTE] |
| 3 | (7, 2021-12) | 4 | 2021-10 · `VNP46A3.A2021274.h10v08.002.` | [PENDIENTE] |
| 4 | (9, 2022-03) | 2 | 2022-01 · `VNP46A3.A2022001.h10v08.002.` | [PENDIENTE] |
| 5 | (13, 2022-06) | 5 | 2022-04 · `VNP46A3.A2022091.h10v08.002.` | [PENDIENTE] |
| 6 | (8, 2022-08) | 1 | 2022-06 · `VNP46A3.A2022152.h10v08.002.` | [PENDIENTE] |
| 7 | (2, 2022-10) | 2 | 2022-08 · `VNP46A3.A2022213.h10v08.002.` | [PENDIENTE] |
| 8 | (15, 2022-12) | 1 | 2022-10 · `VNP46A3.A2022274.h10v08.002.` | [PENDIENTE] |
| 9 | (90, 2023-05) | 1 | 2023-03 · `VNP46A3.A2023060.h10v08.002.` | [PENDIENTE] |
| 10 | (6, 2023-10) | 2 | 2023-08 · `VNP46A3.A2023213.h10v08.002.` | [PENDIENTE] |

*Verificación:* la celda 3b ejecutada en Colab produjo exactamente esta tabla (semilla 42).

*Nota:* el prefijo esperado supone que el compuesto mensual se nombra con el día del año del primer día del mes; es una suposición a confirmar con el nombre real de cada archivo. Códigos 1–16 = comunas; 50, 60, 70, 80 y 90 = corregimientos.

**Cómo completar lo pendiente** (fuera del cuaderno; el token de Earthdata va en variable de entorno, nunca en el repositorio). Esquema orientativo, **ajustar tras probarlo**:

```python
import earthaccess            # pip install earthaccess; requiere cuenta Earthdata
earthaccess.login()           # usa variables de entorno / credenciales propias
res = earthaccess.search_data(short_name="VNP46A3",
                              temporal=("2021-01-01", "2021-01-31"),
                              bounding_box=(-75.8, 6.1, -75.4, 6.4))   # Medellín
print(len(res), [r["umm"]["GranuleUR"] for r in res])   # nombre exacto del .h5
files = earthaccess.download(res, "data/raw/vnp46a3")
# fecha de producción = últimos 13 dígitos del nombre (YYYYDDDHHMMSS)
# sha256 de cada archivo → anexo B1; retraso = fecha de producción − fin del mes de la imagen
```

**Declaraciones obligatorias**
- **Retraso de publicación:** [PENDIENTE: medir en las 10 descargas, con la fecha de producción del nombre de archivo, cuántos días después del fin del mes apareció cada compuesto]. Ni la ficha NASA ni la página LAADS (2026-11-03) declaran una latencia del compuesto mensual; mientras tanto rige la regla `m−2`.
- **Qué mide la imagen:** radiancia nocturna, es decir, **actividad/iluminación; no mide homicidios** ni se interpreta como causa de ellos.
- **Aviso de producto:** NASA anuncia que la entrega de nuevas observaciones de Suomi NPP cesa el **2026-11-02** (banner de la ficha). No afecta el periodo histórico 2015–2023 de este proyecto, pero sí un despliegue futuro (Fase 3).

**Resultado de compuerta:** no acreditado hasta registrar el nombre exacto y la fecha de producción de los diez archivos.

---

## Anexo B · Test de procedencia

### B1. Cadena de procedencia

| Archivo | Productor primario / URL o DOI | Fecha de descarga | SHA-256 |
| --- | --- | --- | --- |
| `consolidado_cantidad_casos_criminalidad_en_comunas_por_anio_mes.csv` (23.164 filas × 4 columnas; 2003-01 a 2023-11) | Alcaldía de Medellín / SISC · [datos.gov.co](https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif) | 2026-11-03 (declarada por el equipo en `FECHA_DESCARGA` del cuaderno; **[INTEGRANTES: confirmar]**) | `50965240f8df75bdaa0a225dfa65eaab85ded9d52e52ba85b5347683ee01e23c` |
| `VNP46A3.A….h10v08.002.….h5` (10 archivos del Anexo A) | NASA LAADS DAAC · [DOI](https://doi.org/10.5067/VIIRS/VNP46A3.002) | [PENDIENTE] | [PENDIENTE: uno por archivo] |
| Polígonos de comunas y corregimientos (GeoJSON/Shapefile exportado) | Alcaldía de Medellín, DAP · [servicio ArcGIS](https://www.medellin.gov.co/servidormapas/rest/services/mapas_nacionales/VC_Limite_Politico_Admtivo/MapServer/1) | [PENDIENTE] | [PENDIENTE] |

**Sobre el Excel `Homicidio 141225.xlsx`** (retirado de la propuesta): SHA-256 `3d58e3753ee33f13169f4ab1b01eb9cd39ce4f05471563dec447112423ddbf6f`; 5.711 filas × 17 columnas; 2014-01 a 2025-12-14. **Origen: no documentado** (sin enlace ni fecha de descarga). Según el docente, la ficha de datos.gov.co enlaza un archivo de MEData de 19.647 filas × 36 columnas que termina el 2023-11-29: **no es el mismo archivo** y esa fuente no respalda un periodo de prueba 2024-01 a 2025-11. Por eso no se usa ni se cita como fuente.

### B2. Descarga trazable
El cuaderno **no usa Drive**: lee el CSV de `data/raw/` o lo recibe por el botón de subida de Colab, e imprime su SHA-256 (sección 1). Comprobación de réplicas de terceros, ejecutada sobre el cuaderno entregado: `grep -inE "kaggle|kagglehub|/kaggle/input" *.ipynb` → **sin coincidencias** (2026-11-03). El cuaderno se ejecutó completo en Colab con carga directa del CSV (sin Drive) y el SHA-256 impreso coincide con el de B1. **[PENDIENTE]** repetir el `grep` si modifican el `.ipynb` antes de entregar.

### B3. Búsqueda de espejos (2026-11-03)
Dos búsquedas web con operador `site:kaggle.com`: (1) `"Consolidado cantidad de casos criminalidad en comunas por año y mes" Medellín`; (2) `datasets homicidios Medellín comunas SISC`. **Resultado: sin coincidencias en kaggle.com** (los resultados fueron noticias y artículos académicos). Limitación: es búsqueda web, no la búsqueda interna de kaggle.com/datasets. **[PENDIENTE]** repetirla allí y pegar fecha y resultado.

### B4. Huella de crudeza (antes de limpiar; medida en el archivo real, celda 1 del cuaderno; las cifras se reprodujeron en la ejecución en Colab)
- **Tamaño:** 23.164 filas × 4 columnas: `Fecha_hecho` (texto `AAAA-MM`), `Conducta`, `Codigo_comuna` (texto), `Cantidad_casos` (entero). Rango: 2003-01 a 2023-11.
- **Faltantes:** 0 % en las cuatro columnas. Pero el archivo **no trae filas con cero** y el territorio trae «SIN DATO» en 472 filas (ninguna de homicidio).
- **Conductas (filas):** hurto a persona 4.525 · hurto de moto 4.513 · hurto de carro 4.084 · **homicidio 4.071** · hurto a residencia 3.497 · extorsión 2.384 · hurto de semoviente 90. Es decir, el consolidado agrega varios delitos y hay que filtrar `Conducta = Homicidio`.
- **Códigos:** el mismo territorio aparece como `1` y `1.0` (se unifica); 16 comunas (1–16) y 5 corregimientos (50, 60, 70, 80, 90) = **21 territorios** con homicidios. Tras unificar no hay duplicados territorio-mes.
- **Homicidios:** 4.071 filas, 18.593 casos; por fila, media 4,57, mediana 3, mínimo 1, máximo 35.
- **Ceros rellenados:** calendario completo 21 territorios × 251 meses = 5.271 pares; **1.200 (22,8 %)** no tenían fila y se rellenan con 0. Cero registrado ≠ cero real.
- **Balance de la etiqueta de alerta** (con la regla de la propuesta, sobre las observaciones con 12 meses de historia): entrenamiento 285 de 1.512 (18,8 %); validación 96 de 504 (19,0 %); prueba 44 de 231 (19,0 %). Clase mayoritaria: «sin alerta» (≈ 81 %).

### B5. Contraste con espejo
No hay espejo identificado. Contraste con el Excel: archivo distinto (ver B1).

---

## Anexo C · Checklist de protección de datos (Ley 1581 de 2012) — bloques A, B, C, F, H y J, **uno por fuente**

«Ley» = Ley 1581 de 2012; «DUR» = Decreto 1074 de 2015. **Regla de la Fase 1:** sin personas identificables. Casillas marcadas [x] = sustentadas en la documentación de la fuente o verificadas en el archivo real; [ ] = requieren que un integrante lo compruebe.

### C-1. Fuente: Consolidado de criminalidad por comuna, año y mes (MEData/SISC)

**Resultado:** [x] Datos agregados por la fuente oficial (sin personas identificables) · [x] Sin datos de personas · [ ] No admisible

**A. Datos personales**
- [x] Revisadas todas las columnas del CSV descargado: `Fecha_hecho` (AAAA-MM), `Conducta`, `Codigo_comuna`, `Cantidad_casos`. *(Verificado con el archivo real, SHA-256 en B1.)*
- [x] No hay unidad individual: la fuente publica conteos por comuna y mes.
- [x] Ninguna columna contiene fecha exacta (solo año-mes), barrio, edad, sexo ni coordenadas.

**B. Datos sensibles**
- [x] Un conteo de homicidios por comuna-mes no revela por sí solo salud, origen étnico u otros datos del art. 5.

**C. Niños, niñas y adolescentes**
- [x] Sin registros individuales; no se usa ninguna variable de edad.

**F. Anonimización / reidentificación**
- [x] Solo agregados por comuna–mes. Prueba escrita: *¿podría alguien identificar a una persona combinando estas columnas con información pública?* **No**, porque no hay atributos de persona; riesgo residual: conteos de 1 en un territorio pequeño (al menos el 25 % de las filas de homicidio tiene 2 casos o menos), que son cifras oficiales ya publicadas; **no se publicarán desagregaciones adicionales**.

**H. Transferencia internacional (Colab, GitHub, asistentes de IA)**
- [x] Se sabe que esos servicios procesan datos fuera de Colombia (Ley art. 26).
- [x] A Colab/GitHub se sube solo este agregado (directo a Colab, sin Drive); a asistentes de IA, esquema y agregados. Este CSV agregado se compartió con un asistente de IA en la sesión de preparación de esta v1: es un agregado sin personas, permitido por la regla.

**J. Publicación**
- [ ] Repositorio privado compartido con el docente, o público con script de descarga. Licencia CC BY-SA 4.0: atribuir y mantener CompartirIgual en derivados redistribuidos. **[INTEGRANTES: marcar la opción elegida.]**

### C-2. Fuente: NASA VNP46A3 versión 2

**Resultado:** [x] Sin datos de personas

**A.** [x] Ráster de radiancia a ~500 m, sin identificadores. [ ] Revisados los metadatos del primer archivo descargado.
**B.** [x] No hay datos sensibles.
**C.** [x] No hay titulares individuales.
**F.** [x] Un píxel de 500 m no identifica a una persona; no se cruza con datos de víctimas.
**H.** [x] Archivos sin personas: pueden usarse en Colab/GitHub. [ ] Token Earthdata en variable de entorno, nunca en el repositorio.
**J.** [x] Uso sin restricciones según la *EOSDIS Data Use and Citation Guidance*; citar la fuente con el DOI (la página LAADS no declara una licencia propia, revisada 2026-11-03). [ ] Decidir si se publican recortes o solo el script de descarga.

### C-3. Fuente: Polígonos de comunas y corregimientos (DAP)

**Resultado:** [x] Sin datos de personas

**A.** [x] Geometrías territoriales. [ ] Verificar los campos de la capa (código y nombre de comuna) tras exportarla; el código debe casar con `Codigo_comuna` (1–16, 50–90).
**B.** [x] No aplica. **C.** [x] No aplica.
**F.** [x] Polígonos de territorios completos, sin ubicación de víctimas.
**H.** [x] Sin personas: uso permitido en servicios externos.
**J.** [ ] **Términos de redistribución no localizados**: no subir los polígonos a un repositorio público; el repositorio lleva un script que los descarga del servicio oficial y la carpeta va en `.gitignore`.

### C-4. Fuente descartada: Excel `Homicidio 141225.xlsx`

**Resultado:** ☑ **Datos personales/sensibles → no admisible en la Fase 1; fuente retirada.**

- **A.** Contiene identificadores indirectos (fecha y hora, barrio, edad, sexo, ocupación, identidad de género) y, según el docente, coordenadas exactas de cada víctima.
- **B.** Incluye atributos sensibles de víctimas.
- **C.** Hay edades menores de 18 años.
- **F.** La combinación fecha + barrio + edad + sexo puede reidentificar víctimas. No es agregado.
- **H.** No se subirá a Colab, GitHub ni a un asistente de IA a partir de ahora.
- **J.** No se publica ni se incluye en ningún repositorio.
- **Declaración de cómo se inspeccionó (según el README anterior):** el archivo fue cargado y examinado con un asistente de IA en una sesión previa a esta versión para obtener su estructura y conteos, lo cual incumple la regla de no entregar microdatos a un asistente de IA. **[INTEGRANTES: confirmar o corregir este texto para que describa exactamente lo ocurrido, y registrarlo en la Bitácora de IA.]** Ninguna cifra derivada del Excel se usa en esta v1.

### Declaración y firmas

Lo consignado en este checklist es verdadero y se revisó para cada fuente de la Propuesta.

| Integrante | Firma | Fecha (AAAA-MM-DD) |
| --- | --- | --- |
| [COMPLETAR nombre] | | |
| [COMPLETAR nombre] | | |
| [COMPLETAR nombre] | | |
