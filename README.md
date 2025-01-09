# Tablero de Ventas - README

## Resumen
Este proyecto es un Tablero de Análisis de Ventas creado utilizando Power BI. Proporciona una vista detallada de los datos de ventas de AtliQ Hardware, una empresa ficticia de hardware informático con sede en India. AtliQ Hardware suministra hardware y periféricos de computadoras a numerosos clientes en toda India, y su oficina central se encuentra en Delhi, India.

El tablero está diseñado para proporcionar perspectivas basadas en datos. Se construyeron indicadores clave de rendimiento (KPIs), como ingresos totales y cantidades vendidas, utilizando fórmulas DAX para realizar cálculos dinámicos que mejoran la toma de decisiones estratégicas.

## Características
- **KPIs Clave**: Monto total de ingresos y cantidades vendidas.
- **Top 5 Clientes y Productos**: Identifica los clientes y productos con mejor desempeño.
- **Ingresos y Cantidad de Ventas por Mercados**: Desglose detallado por regiones para identificar oportunidades y tendencias.
- **Visualizaciones Dinámicas**: Diseñadas para resaltar patrones y facilitar la toma de decisiones.

## Problema
El director de ventas enfrenta dificultades para rastrear el rendimiento del mercado debido a la falta de reportes respaldados por datos concretos. Esto genera una visibilidad limitada sobre los factores clave que afectan las ventas.

## Objetivo
Implementar una solución basada en datos para:
1. Crear un sistema de métricas clave (KPIs) que facilite el seguimiento del rendimiento de mercados y clientes.
2. Diseñar visualizaciones interactivas en Power BI utilizando DAX para identificar patrones y proponer estrategias.

## Herramientas Utilizadas
- **SQL**: Para la extracción, transformación y limpieza de datos.
- **Power BI**: Para la creación de visualizaciones interactivas.

## Definición de las Variables
- **Revenue**: Monto total de dinero generado por las ventas (ingresos brutos).
- **Sales Qty**: Cantidad total de unidades vendidas.
- **Markets**: Regiones donde se distribuyen los productos.
- **Revenue by Markets**: Desglose de ingresos por cada mercado.
- **Sales Qty by Markets**: Desglose de unidades vendidas por cada mercado.
- **Top 5 Customers**: Los cinco clientes que generan mayor ingreso.
- **Top 5 Products**: Los cinco productos que generan mayor ingreso.
- **Time Period**: Rango de tiempo analizado (e.g., año, trimestre, meses).

## Explicación del Dashboard
1. **KPIs**:
   - Revenue: SUM('Sales transactions'[norm_sales_amount])
   - Sales Qty: SUM('sales transactions'[sales_qty])

2. **Visualizaciones Clave**:
   - Análisis de ingresos y cantidad de ventas desglosados por mercados.
   - Identificación de los principales clientes y productos.

## Capturas del Dashboard
El tablero de Power BI incluye visualizaciones interactivas que destacan los principales KPIs y tendencias. A continuación, se presentan algunas capturas representativas:

1. **KPIs Generales**:
   ![Captura KPIs Generales](ruta/al/archivo/kpis_generales.png)

2. **Análisis por Mercado**:
   ![Captura Análisis por Mercado](ruta/al/archivo/analisis_mercado.png)

3. **Top 5 Clientes y Productos**:
   ![Captura Top Clientes y Productos](ruta/al/archivo/top_clientes_productos.png)

Para ver todas las capturas y el archivo Power BI, consulta el repositorio del proyecto.

## Conclusiones
El análisis de ventas entre 2017 y 2020 revela oportunidades significativas para mejorar la rentabilidad y estabilidad del negocio. Las principales recomendaciones incluyen:
- Priorizar inversiones en mercados secundarios rentables como Bhubaneswar y Hyderabad.
- Optimizar costos operativos y estrategias de precios en mercados principales.
- Aprovechar la estacionalidad para maximizar ingresos en picos y estabilizar meses de baja demanda.

## Mejoras Futuras
- Agregar opciones de filtrado dinámico para analizar datos específicos por regiones o periodos.
- Incorporar una sección de "Revisión de Calidad de Datos" para identificar entradas incompletas o anómalas.
