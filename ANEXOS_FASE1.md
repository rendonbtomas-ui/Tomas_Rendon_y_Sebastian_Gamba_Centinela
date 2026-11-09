# Anexos de la Propuesta de Datos · Proyecto Centinela Homicidios Medellín · v1

**Integrantes:** Tomás Rendón Bustamante · Juan Sebastián Gamba Aguirre · **Versión:** v2 (revisada 2026-11-08: ajuste del modelo y SHA-256 de NASA completados) · **Ejecución del cuaderno en Colab:** 2026-11-03, 21:37 (hora de Bogotá), sin errores · **Estado (2026-11-08):** 2026-11-08, 18:44 **sin errores** 

---

## Anexo A · Prueba de acoplamiento (10 filas reales) — COMPLETO

**Clave:** `(código de comuna/corregimiento, año-mes)` · **Tipo:** agregada, con salvedad de fuga (la imagen y el conteo del mes `m` no se usan para predecir `m`; para predecir `m` se usa la imagen de `m−2`).

**Lo que sí está establecido:**
- Producto: VNP46A3 versión 2 (DOI 10.5067/VIIRS/VNP46A3.002), temporal desde 2012-01-01, resolución 15 arc-s, formato HDF5, acceso con Earthdata Login (ficha NASA Earthdata y página LAADS, 2026-11-03).
- Nombre de archivo (ficha NASA): `VNP46A3.AYYYYDDD.hXXvYY.CCC.YYYYDDDHHMMSS.h5`; los últimos 13 dígitos son la **2026-11/08**.
- Teselas: Medellín (≈6,25° N, 75,57° O) cae en la tesela **h10v08** (h = ⌊(lon+180)/10⌋ = 10; v = ⌊(90−lat)/10⌋ = 8). 
- Las diez claves y el conteo de homicidios **salen del consolidado real** (celda 3b del cuaderno, semilla 42; solo filas que existen en la fuente, no ceros rellenados; diez meses de imagen distintos).

| N.º | Clave (código, año-mes) | Homicidios del consolidado (`Conducta = Homicidio`, `Cantidad_casos`) | Imagen usada (`m−2`) y prefijo **esperado** del archivo | Nombre exacto del `.h5` (registrado con `earthaccess`, cuaderno 02) y fecha de producción |
| --- | --- | --- | --- | --- |
| 1 | (6, 2021-03) | 1 | 2021-01 · `VNP46A3.A2021001.h10v08.002.` | `VNP46A3.A2021001.h10v08.002.2025147180816.h5` · producción 2025-05-27 (1577 días tras el fin del mes) |
| 2 | (70, 2021-11) | 1 | 2021-09 · `VNP46A3.A2021244.h10v08.002.` | `VNP46A3.A2021244.h10v08.002.2025148155946.h5` · producción 2025-05-28 (1336 días tras el fin del mes) |
| 3 | (7, 2021-12) | 4 | 2021-10 · `VNP46A3.A2021274.h10v08.002.` | `VNP46A3.A2021274.h10v08.002.2025148162844.h5` · producción 2025-05-28 (1305 días tras el fin del mes) |
| 4 | (9, 2022-03) | 2 | 2022-01 · `VNP46A3.A2022001.h10v08.002.` | `VNP46A3.A2022001.h10v08.002.2025155014535.h5` · producción 2025-06-04 (1220 días tras el fin del mes) |
| 5 | (13, 2022-06) | 5 | 2022-04 · `VNP46A3.A2022091.h10v08.002.` | `VNP46A3.A2022091.h10v08.002.2025155031414.h5` · producción 2025-06-04 (1131 días tras el fin del mes) |
| 6 | (8, 2022-08) | 1 | 2022-06 · `VNP46A3.A2022152.h10v08.002.` | `VNP46A3.A2022152.h10v08.002.2025155185700.h5` · producción 2025-06-04 (1070 días tras el fin del mes) |
| 7 | (2, 2022-10) | 2 | 2022-08 · `VNP46A3.A2022213.h10v08.002.` | `VNP46A3.A2022213.h10v08.002.2025155220025.h5` · producción 2025-06-04 (1008 días tras el fin del mes) |
| 8 | (15, 2022-12) | 1 | 2022-10 · `VNP46A3.A2022274.h10v08.002.` | `VNP46A3.A2022274.h10v08.002.2025155195249.h5` · producción 2025-06-04 (947 días tras el fin del mes) |
| 9 | (90, 2023-05) | 1 | 2023-03 · `VNP46A3.A2023060.h10v08.002.` | `VNP46A3.A2023060.h10v08.002.2025160193310.h5` · producción 2025-06-09 (801 días tras el fin del mes) |
| 10 | (6, 2023-10) | 2 | 2023-08 · `VNP46A3.A2023213.h10v08.002.` | `VNP46A3.A2023213.h10v08.002.2025160224217.h5` · producción 2025-06-09 (648 días tras el fin del mes) |

*Verificación:* la celda 3b ejecutada en Colab produjo exactamente esta tabla (semilla 42). Una reejecución independiente del código sobre el CSV (SHA-256 de B1) reprodujo las diez claves y los prefijos.

*Nota:* el prefijo esperado supone que el compuesto mensual se nombra con el día del año del primer día del mes; es una suposición a confirmar con el nombre real de cada archivo. Códigos 1–16 = comunas; 50, 60, 70, 80 y 90 = corregimientos.


**Declaraciones obligatorias**
- **Retraso de publicación:** medido en los diez nombres de archivo (fecha de producción menos fin del mes de la imagen): entre **648 y 1.577 días** (producción entre 2025-05-27 y 2025-06-09). Es el reprocesamiento de la serie histórica, **no** la latencia operativa del compuesto mensual, que sigue sin medirse (se necesitan compuestos recientes, 2026-07 a 2026-10). Ni la ficha NASA ni la página LAADS (2026-11-03) declaran esa latencia; mientras tanto rige la regla `m−2`.
- **Qué mide la imagen:** radiancia nocturna, es decir, **actividad/iluminación; no mide homicidios** ni se interpreta como causa de ellos.
- **Aviso de producto:** NASA anuncia que la entrega de nuevas observaciones de Suomi NPP cesa el **2026-11-02** (banner de la ficha). No afecta el periodo histórico 2015–2023 de este proyecto, pero sí un despliegue futuro (Fase 3).

**Resultado de compuerta:** nombre exacto, fecha de producción y **SHA-256 de los diez archivos** registrados (celdas A4 y A2 del cuaderno `02_anexoA_nasa_earthaccess.ipynb`, ejecutadas el 2026-11-08; los SHA-256 están en la tabla B1-NASA). Los nombres, las fechas de producción y los días de retraso de la salida coinciden con la tabla de este anexo.

---

## Anexo B · Test de procedencia

### B1. Cadena de procedencia

| Archivo | Productor primario / URL o DOI | Fecha de descarga | SHA-256 |
| --- | --- | --- | --- |
| `consolidado_cantidad_casos_criminalidad_en_comunas_por_anio_mes.csv` (23.164 filas × 4 columnas; 2003-01 a 2023-11) | Alcaldía de Medellín / SISC · [datos.gov.co](https://www.datos.gov.co/dataset/Consolidado-cantidad-de-casos-criminalidad-en-comu/gcyu-chif) | 2026-11-03 (declarada por el equipo en `2026-11-08` del cuaderno;) | `50965240f8df75bdaa0a225dfa65eaab85ded9d52e52ba85b5347683ee01e23c` |
| `VNP46A3.A….h10v08.002.….h5` (10 archivos del Anexo A; nombres y SHA-256 en la tabla B1-NASA) | NASA LAADS DAAC · [DOI](https://doi.org/10.5067/VIIRS/VNP46A3.002) | SHA-256 de los diez calculado el 2026-11-08 con `earthaccess` (cuaderno 02). | Ver tabla B1-NASA |
| Polígonos de comunas y corregimientos (GeoJSON/Shapefile exportado) | Alcaldía de Medellín, DAP · [servicio ArcGIS](https://www.medellin.gov.co/servidormapas/rest/services/mapas_nacionales/VC_Limite_Politico_Admtivo/MapServer/1) | [PENDIENTE] | [PENDIENTE] |

### B1-NASA. SHA-256 de los diez archivos VNP46A3 del Anexo A

Salida de las celdas A4 y A2 del cuaderno 02 (2026-11-08). El prefijo coincide con el esperado del Anexo A en los diez casos.

| N.º | Archivo `.h5` | Producción | Días tras fin de mes | SHA-256 |
| --- | --- | --- | --- | --- |
| 1 | `VNP46A3.A2021001.h10v08.002.2025147180816.h5` | 2025-05-27 | 1577 | `10f2868c00c8cf362c9668235300ae7ad1307600c8062d73a5b56439b2b71181` |
| 2 | `VNP46A3.A2021244.h10v08.002.2025148155946.h5` | 2025-05-28 | 1336 | `62d005cdd5a8fa2a52ad2210d5fb055034184bb7012991b0762c294fba3bd21f` |
| 3 | `VNP46A3.A2021274.h10v08.002.2025148162844.h5` | 2025-05-28 | 1305 | `2ae7e134d9b593aaf07f03e0607b5c348121a059928d1e21cce83f3418a268d5` |
| 4 | `VNP46A3.A2022001.h10v08.002.2025155014535.h5` | 2025-06-04 | 1220 | `33accc5238bc88fb84f3f39b33d30094839f11e156097abbc5d043b7bf266160` |
| 5 | `VNP46A3.A2022091.h10v08.002.2025155031414.h5` | 2025-06-04 | 1131 | `2f5d468a9c3991c86c19be6ba60c8a9cb5d6b55da7b773f10fcaa5e7a04ff104` |
| 6 | `VNP46A3.A2022152.h10v08.002.2025155185700.h5` | 2025-06-04 | 1070 | `c9209a0e8fd8fc3a9d792b6d37b51d41f450d601afeec24067c3602362fab218` |
| 7 | `VNP46A3.A2022213.h10v08.002.2025155220025.h5` | 2025-06-04 | 1008 | `45c28c094db85164a35ed76fd151643e2f70def4c8cbf21928b74e12cba9bd29` |
| 8 | `VNP46A3.A2022274.h10v08.002.2025155195249.h5` | 2025-06-04 | 947 | `fd1fb36026f30ce31018c1619f6ff1e038abdb7049bc70180c88e9c6ef061c60` |
| 9 | `VNP46A3.A2023060.h10v08.002.2025160193310.h5` | 2025-06-09 | 801 | `c6d1a9bdbe0e2e9490b444cac2a1a4ed67276d13ac11f28e3a7483b6d24e9fb6` |
| 10 | `VNP46A3.A2023213.h10v08.002.2025160224217.h5` | 2025-06-09 | 648 | `692007019a4e20544e0e6ca53091cfc8128650dd240cc4a083dbf29a9df993eb` |

*Archivo adicional.* La búsqueda del mes 2021-01 devolvió también un archivo vecino, `VNP46A3.A2020336.h10v08.002.2025134001101.h5` (producción 2025-05-14; SHA-256 `74e4682ac5e7c84fff5b91d2290a8afa7bd937762d9e93e14f6fb8f77891b937`), que **no** forma parte de las diez claves del Anexo A y no se usa; se registra su huella solo por transparencia.

**Sobre el Excel `Homicidio 141225.xlsx`** (retirado de la propuesta): SHA-256 `3d58e3753ee33f13169f4ab1b01eb9cd39ce4f05471563dec447112423ddbf6f`; 5.711 filas × 17 columnas; 2014-01 a 2025-12-14. **Origen: no documentado** (sin enlace ni fecha de descarga). Según el docente, la ficha de datos.gov.co enlaza un archivo de MEData de 19.647 filas × 36 columnas que termina el 2023-11-29: **no es el mismo archivo** y esa fuente no respalda un periodo de prueba 2024-01 a 2025-11. Por eso no se usa ni se cita como fuente.

### B2. Descarga trazable
El cuaderno **no usa Drive**: lee el CSV de `data/raw/` o lo recibe por el botón de subida de Colab, e imprime su SHA-256 (sección 1). Comprobación de réplicas de terceros, ejecutada sobre el cuaderno entregado: `grep -inE "kaggle|kagglehub|/kaggle/input" *.ipynb` → **sin coincidencias** (2026-11-03). El cuaderno se ejecutó completo en Colab con carga directa del CSV (sin Drive) y el SHA-256 impreso coincide con el de B1. El `grep` se repitió el 2026-11-08 sobre los dos cuadernos entregados (`01_centinela_mlp.ipynb` y `02_anexoA_nasa_earthaccess.ipynb`): **sin coincidencias**. Tras el ajuste v2 (cuaderno 01 con la sección 8b y cuaderno 02 con textos actualizados) se repitió el 2026-11-08: `grep -inE "kaggle|kagglehub|/kaggle/input" *.ipynb` → **sin coincidencias**. repetirlo si vuelven a modificar algún `.ipynb`.

### B3. Búsqueda de espejos (2026-11-03)
Dos búsquedas web con operador `site:kaggle.com`: (1) `"Consolidado cantidad de casos criminalidad en comunas por año y mes" Medellín`; (2) `datasets homicidios Medellín comunas SISC`. **Resultado: sin coincidencias en kaggle.com** (los resultados fueron noticias y artículos académicos). Limitación: es búsqueda web, no la búsqueda interna de kaggle.com/datasets.

### B4. Huella de crudeza (antes de limpiar; medida en el archivo real, celda 1 del cuaderno; las cifras se reprodujeron en la ejecución en Colab)
- **Tamaño:** 23.164 filas × 4 columnas: `Fecha_hecho` (texto `AAAA-MM`, por ejemplo `2003-01`), `Conducta`, `Codigo_comuna` (texto), `Cantidad_casos` (entero). Rango: 2003-01 a 2023-11.
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
- [x] Revisadas todas las columnas del CSV descargado: `Fecha_hecho` (texto `AAAA-MM`), `Conducta`, `Codigo_comuna`, `Cantidad_casos`. *(Verificado con el archivo real, SHA-256 en B1.)*
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
- **Declaración de cómo se inspeccionó (según el README anterior):** el archivo fue cargado y examinado con un asistente de IA en una sesión previa a esta versión para obtener su estructura y conteos, lo cual incumple la regla de no entregar microdatos a un asistente de IA. 
### Declaración y firmas

Lo consignado en este checklist es verdadero y se revisó para cada fuente de la Propuesta.

| Integrante  Fecha 2026-11-08 | 
| Tomás Rendón Bustamante      |
| Juan Sebastián Gamba Aguirre |
