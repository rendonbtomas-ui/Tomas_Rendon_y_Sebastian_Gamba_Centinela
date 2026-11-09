# Checklist antes de entregar (para Tomás y Juan Sebastián)

**Admisibilidad (sin esto la entrega se devuelve sin calificar)**
- [ ] Abrir `01_centinela_mlp.ipynb` en Colab → *Entorno de ejecución → Reiniciar y ejecutar todo*. Debe correr de principio a fin sin errores (sube el CSV cuando lo pida).
- [ ] Comparar las salidas de la **v1** (secciones 4 a 8) con el informe: umbral 0,23, matriz 16/28/76/111, PR-AUC 0,188. Si algo cambia, avisen antes de entregar.
- [x] **Sección 8b (ajuste v2):** ejecutada en Colab el 2026-11-08; sus cifras ya están en el informe en Word, el README, la auditoría (caso 3) y la bitácora. Si al reejecutar cambia algún decimal, actualizar esos cuatro sitios.
- [ ] Descargar el cuaderno de Colab **con las salidas** de la 8b. Si entregan ese archivo y no el del zip, copien en él la celda de texto «Interpretación de la sección 8b» (viene en el cuaderno del zip, justo después de la celda 8b-3).
- [ ] Guardar el cuaderno **con las salidas** (*Archivo → Descargar → .ipynb*).
- [ ] Adjuntar la Propuesta aprobada con su fecha (2026-11-03).

**Completar a mano (solo ustedes lo saben)**
- [ ] `BITACORA_IA.md`: todos los **[CONFIRMAR]** y firmas; la sesión previa; qué decidieron ustedes.
- [ ] `AUDITORIA_IA.md`: captura del texto original del caso 1 y los **[CONFIRMAR]**.
- [ ] `ANEXOS_FASE1.md`: fecha real de descarga de NASA (B1), fecha real de descarga de cada archivo NASA (los diez SHA-256 ya están en B1-NASA), opción de publicación (C-1, J), texto de C-4 y firmas del Anexo C.
- [ ] Repetir la búsqueda de espejos en la búsqueda interna de kaggle.com/datasets (B3) y anotar fecha y resultado.
- [ ] Confirmar la escritura exacta de los nombres (Gamba Aguirre).
- [ ] `BITACORA_IA.md`, fila 9: confirmar cómo se obtuvo y se usó el cuaderno del compañero (de ahí vino la idea de agregar el percentil 75) y declararlo con sus palabras.

**Empaque**
- [ ] Zip `Rendon_Gamba_Centinela_Fase1.zip` (o repositorio privado compartido con el docente) con: README, propuesta, anexos, bitácora, auditoría, `Informe_Fase1_Centinela.docx` y los dos cuadernos. Sin `data/raw` ni polígonos.

**Sustentación (no va en el zip)**
- [ ] Cada uno debe poder explicar con sus palabras una parte distinta: datos y partición, etiqueta y rasgos, arquitectura y bucle, regularización, métricas y umbral. Lean los comentarios del cuaderno y prueben explicar cada celda sin mirarlos.
