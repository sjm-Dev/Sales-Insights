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

## Herramientas utilizadas
- **SQL**: Para la extracción, transformación y limpieza de datos.
- **Power BI**: Para la creación de visualizaciones interactivas.

# Consultas SQL
A continuación, se detallan las consultas SQL utilizadas para extraer, transformar y analizar los datos:

1. **Selección básica:** Retorna todas las filas y columnas de la tabla `customers`.
   ```sql
   SELECT * FROM customers;
   ```
   - **Propósito:** Obtener una vista general de la tabla `customers`.
   - **Resultado esperado:** Todas las filas con columnas como `customer_id`, `customer_name`, y `contact`.

2. **Conteo total de clientes:** Cuenta el número total de registros en la tabla `customers`.
   ```sql
   SELECT COUNT(*) FROM customers;
   ```
   - **Propósito:** Verificar la cantidad total de clientes registrados.
   - **Resultado esperado:** Un único valor numérico, como `3500 clientes`.

3. **Filtrar transacciones por mercado:** Selecciona todas las filas de la tabla `transactions` donde el código de mercado sea `Mark001`.
   ```sql
   SELECT * FROM transactions WHERE market_code = 'Mark001';
   ```
   - **Propósito:** Identificar transacciones específicas realizadas en el mercado `Mark001`.

4. **Valores únicos de productos en un mercado específico:** Obtiene los valores únicos de `product_code` en el mercado `Mark001`.
   ```sql
   SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';
   ```
   - **Propósito:** Listar todos los productos vendidos en un mercado determinado sin duplicados.

5. **Filtrar transacciones por moneda:** Retorna todas las filas de la tabla `transactions` donde la moneda sea `USD`.
   ```sql
   SELECT * FROM transactions WHERE currency = "USD";
   ```
   - **Propósito:** Identificar transacciones realizadas en dólares estadounidenses.

6. **Combinar datos de transacciones y fechas:** Realiza un INNER JOIN entre las tablas `transactions` y `date` para incluir solo transacciones del año 2020.
   ```sql
   SELECT transactions.*, date.*
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020;
   ```
   - **Propósito:** Analizar las transacciones realizadas durante 2020.

7. **Suma de ventas por moneda:** Calcula la suma total de ventas (`sales_amount`) en las monedas `INR` y `USD` para el año 2020.
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND (transactions.currency = "INR" OR transactions.currency = "USD");
   ```
   - **Propósito:** Determinar el total de ingresos desglosado por moneda.

8. **Suma de ventas en un mes específico por moneda:** Calcula la suma total de ventas en enero de 2020 para las monedas `INR` y `USD`.
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND date.month_name = "January"
   AND (transactions.currency = "INR" OR transactions.currency = "USD");
   ```
   - **Propósito:** Analizar las ventas realizadas en enero de 2020.

9. **Suma de ventas por mercado específico:** Calcula la suma total de ventas en el mercado `Mark001` durante 2020.
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND transactions.market_code = "Mark001";
   ```
   - **Propósito:** Evaluar el rendimiento de un mercado específico.

# Dashboard en Power BI

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
1. **KPIs:**
Los KPIs del tablero fueron diseñados utilizando fórmulas DAX, que permiten cálculos dinámicos y adaptables. A continuación, se presentan los principales:
   - **Revenue (Ingresos Totales):**
     ```dax
     Revenue = SUM('sales transactions'[norm_sales_amount])
     ```
     Muestra el ingreso total generado por las ventas.
   - **Sales Qty (Cantidad de Ventas):**
     ```dax
     Sales Qty = SUM('sales transactions'[sales_qty])
     ```
     Indica el número total de unidades vendidas.

2. **Visualizaciones Clave:**
   - **Revenue by Markets:** Desglose de ingresos por cada mercado.
   - **Sales Qty by Markets:** Desglose de cantidad de ventas por mercado.
   - **Top 5 Customers y Top 5 Products:** Identificación de los clientes y productos más importantes para priorizar estrategias comerciales.

## Capturas del Dashboard
El tablero de Power BI incluye visualizaciones interactivas que destacan los principales KPIs y las tendencias de venta entre los años 2017 y 2020. A continuación, se presentan algunas capturas representativas de:

**KPIs Generales, Análisis por Mercado y Top 5 de Clientes y Productos**

  ![Captura KPIs General](https://github.com/sjm-Dev/Sales-Insights/blob/main/Power%20BI%20images/1.png)

Para ver todas las capturas y el archivo Power BI, consultar el repositorio del proyecto.

## Conclusiones
El análisis de ventas entre 2017 y 2020 revela oportunidades significativas para mejorar la rentabilidad y estabilidad del negocio. Las principales recomendaciones incluyen:
- Priorizar inversiones en mercados secundarios rentables como Bhubaneswar y Hyderabad.
- Optimizar costos operativos y estrategias de precios en mercados principales.
- Aprovechar la estacionalidad para maximizar ingresos en picos y estabilizar meses de baja demanda.

## Mejoras Futuras
- **Agregar métricas adicionales:** Como márgenes de ganancia y costos operativos.
- **Incluir más filtros:** Por ejemplo, rangos de precios o regiones específicas.
- **Automatización del flujo de datos:** Implementar un flujo ETL para actualizar los datos en tiempo real.
- **Análisis predictivo:** Usar Machine Learning para predecir ingresos futuros y tendencias del mercado.

## Sobre el Autor
Soy un apasionado de la ciencia de datos y la analítica. Este proyecto refleja mis habilidades en SQL, Power BI y análisis de datos, y es parte de mi portafolio profesional.