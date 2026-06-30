# Análisis y Reporte Semanal de Exportación (Pre-semanal)

Este proyecto Python automatiza la limpieza, análisis y visualización de datos de exportación para generar informes detallados en formato Excel y PDF. El script procesa datos de ventas, márgenes e índices, aplicando lógicas de negocio específicas para identificar tendencias y anomalías en diferentes mercados y estados (Inicial, Simulado, Brecha).

## Características

*   **Limpieza de Datos:** Normalización robusta de columnas numéricas con formatos mixtos (europeo, americano, símbolos de moneda, porcentajes).
*   **Cálculo de KPIs:** Generación de tablas de KPIs detalladas y resumidas para análisis por mercado y estado.
*   **Exportación a Excel:**
    *   Creación de un archivo Excel (`resumen_kpis_resultados.xlsx`) con tablas de KPIs.
    *   Formateo de celdas con símbolos de Euro (€) y porcentajes (%).
    *   Auto-ajuste del ancho de las columnas.
    *   Generación de hojas individuales por cada "Mercado", incluyendo datos detallados y ordenados.
    *   Aplicación de formato condicional (escalas de color) para `Margin %` (rojo-blanco-verde) y `Index` (blanco-naranja) bajo condiciones específicas (e.g., estado "Brecha").
    *   Creación de un "Dashboard" en Excel incrustando todos los gráficos generados.
*   **Generación de Gráficos:** Creación de varias visualizaciones clave utilizando `matplotlib`:
    *   Barras: Familias con Índice Positivo (Brecha), Familias con Margen Negativo (Brecha), Media de Index por Estado, Media de Margin % por Estado.
    *   Pastel: Distribución de Ventas por Mercado (Inicial y Simulado).
    *   Boxplot: Distribución de Margin % por Mercado (estado Brecha) con tabla de outliers extremos.
    *   Burbujas: Relación Index vs Margin % por Mercado (estados Inicial y Simulado).
*   **Generación de PDF:** Consolidación de todos los gráficos generados en un único archivo PDF (`dashboard_kpis.pdf`) para una distribución sencilla.

## Requisitos

El script requiere las siguientes librerías de Python. Puedes instalarlas usando `pip`:

```bash
pip install pandas numpy openpyxl matplotlib xlsxwriter
```

## Uso

1.  **Preparar los Datos:**
    Coloca tu archivo de datos de entrada en formato Excel. El script está configurado para leer un archivo llamado `Exportación Pre- Semana 26 (1).xlsx` ubicado en el directorio `/content/`. Si tu archivo tiene otro nombre o ruta, asegúrate de actualizar la línea correspondiente en el script:
    ```python
    datos = pd.read_excel("/content/Exportación Pre- Semana 26 (1).xlsx")
    ```
    Cambia `/content/Exportación Pre- Semana 26 (1).xlsx` por la ruta a tu archivo.

2.  **Ejecutar el Script:**
    Puedes ejecutar el script directamente desde tu entorno Python:

    ```bash
    python exportación_pre_semanal_2_0.py
    ```

## Archivos Generados

Al ejecutar el script, se generarán los siguientes archivos en el mismo directorio donde se ejecuta el script:

*   `resumen_kpis_resultados.xlsx`: Archivo Excel con las tablas de KPIs, hojas detalladas por mercado y un dashboard con los gráficos incrustados.
*   `dashboard_kpis.pdf`: Un archivo PDF que contiene todos los gráficos generados.
*   `fig_index_pos.png`: Gráfico de barras de familias con índice positivo.
*   `fig_neg_margin.png`: Gráfico de barras de familias con margen negativo.
*   `fig_sales_merc.png`: Gráficos de pastel de ventas por mercado.
*   `fig_mean_index.png`: Gráfico de barras de la media de Index.
*   `fig_mean_margin.png`: Gráfico de barras de la media de Margin %.
*   `fig_marg_box_cat.png`: Boxplot de Margin % con outliers.
*   `fig_inicial_bubble.png`: Gráfico de burbujas para el estado "Inicial".
*   `fig_simulado_bubble.png`: Gráfico de burbujas para el estado "Simulado".

## Estructura del Código

El script está organizado en las siguientes secciones lógicas:

1.  **Configuración Inicial e Importaciones:** Carga de librerías y configuración inicial.
2.  **Función de Limpieza Numérica (`clean_numeric_column`, `clean_eu_number`):** Funciones para estandarizar formatos de números y eliminar símbolos de moneda.
3.  **Procesamiento de Datos para KPI Detalle:** Filtrado y preparación del DataFrame `kpis`.
4.  **Procesamiento de Datos para KPI Resumen:** Agregación y preparación del DataFrame `resultado`.
5.  **Exportación Inicial a Excel:** Creación del archivo `resumen_kpis_resultados.xlsx` con las tablas `kpis` y `resultado` en una primera hoja.
6.  **Formateo Básico de Excel:** Aplicación de formatos de número (€, %) y auto-ajuste de columnas en la hoja "Resumen".
7.  **Procesamiento y Exportación Detallada por Mercado:**
    *   Definición de reglas de formato condicional (escalas de color).
    *   Iteración por cada mercado para crear una hoja individual, aplicar formato condicional y formatos de número.
8.  **Generación de Gráficos (`Matplotlib`):**
    *   Definición de funciones para la detección de outliers (`obtener_outliers`).
    *   Creación de los diferentes tipos de gráficos (barras, pastel, boxplot, burbujas).
9.  **Generación de Dashboard en PDF:** Combinación de todos los gráficos PNG en un único PDF.
10. **Generación de Dashboard en Excel:** Incrustación de los gráficos PNG en una nueva hoja "Dashboard" dentro del archivo Excel.

## Notas Adicionales

*   Las rutas de los archivos de entrada y salida están actualmente hardcodeadas. Se recomienda parametrizarlas para mayor flexibilidad (por ejemplo, usando `argparse` para aceptar argumentos de línea de comandos).
*   El formato condicional se aplica de manera iterativa por fila en algunas secciones del código, lo cual podría ser optimizado para aplicar reglas a rangos completos de celdas en `openpyxl` para un mejor rendimiento en datasets muy grandes.
