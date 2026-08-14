# Reposición Inteligente de Inventario (RII)

Trabajo Final — Aprendizaje Automático aplicado a Problemas Reales
Maestría en Ciencia de Datos, Universidad Católica del Uruguay

Autor: Walter D. Soriano Vaio

## Descripción del proyecto

Solución de Machine Learning para apoyar la decisión trimestral de reposición de inventario de un comercio retail con más de 8.000 productos activos, bajo una restricción real de logística (camión de 8 toneladas de capacidad por envío). El proyecto se organiza en tres ejes:

- **Eje 1 — Segmentación estratégica**: agrupamiento no supervisado (K-Means) de productos según margen y rotación, contrastado con la clasificación clásica A-B-C del negocio.
- **Eje 2 — Predicción de demanda**: modelo de regresión que predice la demanda mensual de cada producto a partir de su comportamiento histórico.
- **Eje 3 — Propuesta de reposición**: prioriza qué reponer cada trimestre según rentabilidad del inventario (GMROI), respetando la capacidad del camión.

## Estructura del repositorio

```
.
./README.md
./notebooks/InventoryAnalytics_RII.ipynb
./reports/
./reports/Informe_Tecnico.pdf                 
./reports/Formulacion_Problema_Impacto.pdf   
./reports/Evaluacion_Negocio_Interpretabilidad.pdf  
./outputs/
./outputs/propuesta_reposicion_3meses.csv

## Cómo ejecutar el pipeline

1. Abrir 'notebooks/InventoryAnalytics_RII.ipynb' en Jupyter Notebook, JupyterLab o Google Colab (recomendado)
2. Ejecutar todas las celdas en orden. El notebook:
   - descarga el dataset directamente desde la URL configurada en la celda de carga
   - realiza el análisis exploratorio
   - construye las features con ventanas temporales fijas (evita data leakage)
   - entrena y compara los modelos del Eje 2
   - genera la segmentación del Eje 1
   - arma la propuesta de reposición del Eje 3
   - exporta el CSV final
3. Al llegar a la última celda, se genera 'propuesta_reposicion_3meses.csv'. Si el notebook se ejecuta en Google Colab, la descarga del archivo se dispara automáticamente. En otros entornos el archivo queda guardado en la carpeta de ejecución.

### Fuente de datos

El notebook carga el dataset real de ventas/stock (enero-julio 2026) desde:

https://raw.githubusercontent.com/wdsoriano/InventoryAnalytics/main/dataset.csv

## Resultados principales

- **Eje 2 (demanda)**: el modelo Random Forest mejora al baseline ingenuo y a la regresión lineal (MAE 2,95 vs. 3,67 y 3,15 respectivamente).
- **Eje 1 (segmentación)**: se valida un agrupamiento en 3 segmentos (A/B/C), correspondiente al análisis ABC clásico de gestión de inventario, usando margen y rotación calculados sobre una ventana temporal fija. No se excluye ningún segmento de la reposición.
- **Eje 3 (reposición)**: la propuesta trimestral usa el 62,6% de la capacidad del camión (8 toneladas) y captura el 100% del valor GMROI-ponderado de la demanda a reponer.

El detalle completo de la formulación, el EDA, el diseño del pipeline, los resultados comparativos y el análisis de costos está documentado en 'reports/Informe_Tecnico.pdf'.

## Limitaciones conocidas

- El modelo predice demanda mensual y se proyecta a 3 meses asumiendo demanda estable en el trimestre.
- Los productos sin historial de ventas previo al mes de corte quedan fuera del alcance del modelo predictivo.
