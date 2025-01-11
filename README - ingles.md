# Sales Insights

## Summary
This project is a sales analysis conducted in two main stages:

SQL Queries: Extraction and preparation of the data required for analysis.
Power BI Dashboard: Interactive visualization of the results to support decision-making.

The analysis focuses on sales data from AtliQ Hardware, a fictional computer hardware company based in India. AtliQ Hardware supplies hardware and computer peripherals to numerous clients across India, with its headquarters located in Delhi, India.

## Problem
The sales director is struggling to track market performance due to a lack of reports supported by concrete data. This results in limited visibility into the key factors affecting sales.

## Objective
Implement a data-driven solution to:

Create a system of key performance indicators (KPIs) to facilitate tracking market and customer performance.
Design interactive visualizations in Power BI using DAX to identify patterns and propose strategies.

## Tools Used
SQL: For data extraction, transformation, and cleaning.
Power BI: For creating interactive visualizations.


# Consultas SQL
Below are the SQL queries used to extract, transform, and analyze the data:

1. **Basic Selection:** Returns all rows and columns from the customers table.
   ```sql
   SELECT * FROM customers;
   ```
   - **Purpose:** To get an overview of the `customers` table.
   - **Expected Outcome:** All rows with columns such as `customer_id`, `customer_name`, and 'contact`.


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

10. **Manejo de caracteres invisibles en la columna `currency`:** Calcula el total de ingresos en 2020, teniendo en cuenta posibles caracteres de retorno de carro (`\r`) o espacios adicionales en los datos.
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND transactions.currency = "INR\r" OR transactions.currency = "USD\r";
   ```
   - **Problema detectado:** La columna `currency` contiene caracteres invisibles como `\r`, lo que puede generar problemas al realizar comparaciones directas.
   - **Solución:** Limpiar los datos utilizando la funcion `TRIM` para eliminar caracteres innecesarios:
     ```sql
     SELECT SUM(transactions.sales_amount)
     FROM transactions
     INNER JOIN date ON transactions.order_date = date.date
     WHERE date.year = 2020 AND TRIM(transactions.currency) IN ("INR", "USD");
     ```
11. **Creación de una nueva columna:** Añade una columna llamada `norm_sales_amount` que normaliza los valores de ventas (`sales_amount`) convirtiendo los montos en USD a INR (utilizando una tasa de conversión de 1 USD = 75 INR). Si la moneda no es USD, se conserva el valor original. Esta transformación también maneja datos con caracteres invisibles como `USD#(cr)`.
     ```sql
     = Table.AddColumn(#"Filtered Rows", "norm_sales_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then[sales_amount]*75 else [sales_amount])
      ```

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

  ![Captura KPIs General](https://github.com/sjm-Dev/Sales-Insights/blob/main/Power%20BI%20images/2017%20to%202020%20sales.png)

Para ver todas las capturas y el archivo Power BI, consultar el repositorio del proyecto.

## Cómo replicar este proyecto
1. Clona este repositorio.
2. Descarga el archivo `.pbix` para ver el tablero en Power BI.
3. Carga los datos en SQL usando las consultas proporcionadas.
4. Ajusta las visualizaciones en Power BI según tus necesidades.

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

## Sobre Mi
Estoy dando mis primeros pasos en el mundo de la ciencia de datos y la analítica, aprendiendo herramientas como SQL, Power BI, R y Python. Este proyecto es parte de mi portafolio y busco que refleje mi interés en resolver problemas empresariales mediante el análisis de datos. Estoy motivado por aprender continuamente y en busca de oportunidades que me permitan desarrollar mis habilidades en proyectos reales.
