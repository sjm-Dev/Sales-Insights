# Sales Insights

## Resumen
Este proyecto es un análisis de ventas realizado en dos etapas principales:
1. **Consultas SQL**: Extracción y preparación de los datos necesarios para el análisis.
2. **Dashboard en Power BI**: Visualización interactiva de los resultados para facilitar la toma de decisiones.

El análisis se centra en los datos de ventas de AtliQ Hardware, una empresa ficticia de hardware informático con sede en India. AtliQ Hardware suministra hardware y periféricos de computadoras a numerosos clientes en toda India, y su oficina central se encuentra en Delhi, India.

## Problema
El director de ventas enfrenta dificultades para rastrear el rendimiento del mercado debido a la falta de reportes respaldados por datos concretos. Esto genera una visibilidad limitada sobre los factores clave que afectan las ventas.

## Objetivo
Implementar una solución basada en datos para:
1. Crear un sistema de métricas clave (KPIs) que facilite el seguimiento del rendimiento de mercados y clientes.
2. Diseñar visualizaciones interactivas en Power BI utilizando DAX para identificar patrones y proponer estrategias.

## Herramientas Utilizadas
- **SQL**: Para la extracción, transformación y limpieza de datos.
- **Power BI**: Para la creación de visualizaciones interactivas.

## Consultas SQL
A continuación, se detallan las consultas SQL utilizadas para extraer, transformar y analizar los datos:

1. **Selección básica:** Retorna todas las filas y columnas de la tabla `customers`.
   ```sql
   SELECT * FROM customers;
   ```

2. **Conteo total de clientes:** Cuenta el número total de registros en la tabla `customers`.
   ```sql
   SELECT COUNT(*) FROM customers;
   ```

3. **Filtrar transacciones por mercado:** Selecciona todas las filas de la tabla `transactions` donde el código de mercado sea `Mark001`.
   ```sql
   SELECT * FROM transactions WHERE market_code = 'Mark001';
   ```

4. **Valores únicos de productos en un mercado específico:** Obtiene los valores únicos de `product_code` en el mercado `Mark001`.
   ```sql
   SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';
   ```

5. **Filtrar transacciones por moneda:** Retorna todas las filas de la tabla `transactions` donde la moneda sea `USD`.
   ```sql
   SELECT * FROM transactions WHERE currency = "USD";
   ```

6. **Combinar datos de transacciones y fechas:** Realiza un INNER JOIN entre las tablas `transactions` y `date` para incluir solo transacciones del año 2020.
   ```sql
   SELECT transactions.*, date.*
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020;
   ```

7. **Suma de ventas por moneda:** Calcula la suma total de ventas (`sales_amount`) en las monedas `INR` y `USD` para el año 2020.
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND (transactions.currency = "INR" OR transactions.currency = "USD");
   ```

8. **Suma de ventas en un mes específico por moneda:** Calcula la suma total de ventas en enero de 2020 para las monedas `INR` y `USD`.
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND date.month_name = "January"
   AND (transactions.currency = "INR" OR transactions.currency = "USD");
   ```

9. **Suma de ventas por mercado específico:** Calcula la suma total de ventas en el mercado `Mark001` durante 2020.
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND transactions.market_code = "Mark001";
   ```

## Dashboard en Power BI

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
El tablero de Power BI incluye visualizaciones interactivas que destacan los principales KPIs y las tendencias de venta entre los años 2017 y 2020. A continuación, se presentan algunas capturas representativas de:

**KPIs Generales**
**Análisis por Mercado**
**Top 5 Clientes y Productos**

  ![Captura KPIs Generales, top 5 clientes y productos](https://github.com/sjm-Dev/Sales-Insights/blob/main/Power%20BI%20images/1.png)

Para ver todas las capturas y el archivo Power BI, consulta el repositorio del proyecto.

## Conclusiones
El análisis de ventas entre 2017 y 2020 revela oportunidades significativas para mejorar la rentabilidad y estabilidad del negocio. Las principales recomendaciones incluyen:
- Priorizar inversiones en mercados secundarios rentables como Bhubaneswar y Hyderabad.
- Optimizar costos operativos y estrategias de precios en mercados principales.
- Aprovechar la estacionalidad para maximizar ingresos en picos y estabilizar meses de baja demanda.

## Mejoras Futuras
- Agregar opciones de filtrado dinámico para analizar datos específicos por regiones o periodos.
- Incorporar una sección de "Revisión de Calidad de Datos" para identificar entradas incompletas o anómalas.
