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
**SQL:** For data extraction, transformation, and cleaning.  
**Power BI:** For creating interactive visualizations.


# Consultas SQL
Below are the SQL queries used to extract, transform, and analyze the data:


1. **Basic Selection:** Returns all rows and columns from the customers table.
   ```sql
   SELECT * FROM customers;
   ```
   - **Purpose:** To get an overview of the `customers` table.
   - **Expected Outcome:** All rows with columns such as `customer_id`, `customer_name`, and `contact`.


2. **Total Customer Count:** Counts the total number of records in the `customers` table.  
   ```sql
   SELECT COUNT(*) FROM customers;
   ```
   - **Purpose:** To check the total number of registered customers.  
   - **Expected Outcome:** A single numeric value, such as `3500 customers`. 


3. **Filter Transactions by Market:** Selects all rows from the `transactions` table where the market code is `Mark001`.  
   ```sql
   SELECT * FROM transactions WHERE market_code = 'Mark001';
   ```
   - **Purpose:** To identify specific transactions made in the Mark001 market.


4. **Unique Product Values in a Specific Market:** Retrieves the unique values of `product_code` in the `Mark001` market.  
   ```sql
   SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';
   ```
   - **Purpose:** To list all products sold in a specific market without duplicates.


5. **Filter Transactions by Currency:** Returns all rows from the `transactions` table where the currency is `USD`.  
   ```sql
   SELECT * FROM transactions WHERE currency = "USD";
   ```
   - **Purpose:** To identify transactions made in US dollars.


6. **Combine Transaction and Date Data:** Performs an INNER JOIN between the `transactions` and `date` tables to include only transactions from the year 2020.  
   ```sql
   SELECT transactions.*, date.*
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020;
   ```
    - **Purpose:** To analyze transactions made during 2020.


7. **Sum of Sales by Currency:** Calculates the total sum of sales (`sales_amount`) in `INR` and `USD` currencies for the year 2020.  
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND (transactions.currency = "INR" OR transactions.currency = "USD");
   ```
  - **Purpose:** To determine the total revenue broken down by `currency`.


8. **Sum of Sales in a Specific Month by Currency:** Calculates the total sum of sales in January 2020 for the currencies `INR` and `USD`.  
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND date.month_name = "January"
   AND (transactions.currency = "INR" OR transactions.currency = "USD");
   ```
   - **Purpose:** To analyze sales made in January 2020.


9. **Sum of Sales by Specific Market:** Calculates the total sum of sales in the `Mark001` market during 2020.  
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND transactions.market_code = "Mark001";
   ```
   - **Purpose:** To evaluate the performance of a specific market.


10. **Handling Invisible Characters in the `currency` Column:** Calculates the total revenue in 2020, accounting for potential carriage return (`\r`) or extra spaces in the data.  
   ```sql
   SELECT SUM(transactions.sales_amount)
   FROM transactions
   INNER JOIN date ON transactions.order_date = date.date
   WHERE date.year = 2020 AND transactions.currency = "INR\r" OR transactions.currency = "USD\r";
   ```
   - **Detected Problem:** The `currency` column contains invisible characters like `\r`, which can cause issues when performing direct comparisons.  
   - **Solution:** Clean the data using the `TRIM` function to remove unnecessary characters:  
     ```sql
     SELECT SUM(transactions.sales_amount)
     FROM transactions
     INNER JOIN date ON transactions.order_date = date.date
     WHERE date.year = 2020 AND TRIM(transactions.currency) IN ("INR", "USD");
     ```

11. **Create a New Column:** Adds a column named `norm_sales_amount` that normalizes sales amounts (`sales_amount`) by converting USD amounts to INR (using a conversion rate of 1 USD = 75 INR). If the currency is not USD, the original value is retained. This transformation also handles data with invisible characters such as `USD#(cr)`.  
     ```sql
     = Table.AddColumn(#"Filtered Rows", "norm_sales_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then[sales_amount]*75 else [sales_amount])
      ```

# Dashboard in Power BI

## Definition of Variables
- **Revenue**: Total amount of money generated from sales (gross income).
- **Sales Qty**: Total quantity of units sold.
- **Markets**: Regions where products are distributed.
- **Revenue by Markets**: Breakdown of revenue by each market.
- **Sales Qty by Markets**: Breakdown of units sold by each market.
- **Top 5 Customers**: The five customers generating the highest revenue.
- **Top 5 Products**: The five products generating the highest revenue.
- **Time Period**: Time range analyzed (e.g., year, quarter, months).

## Dashboard Explanation
1. **KPIs:**
The KPIs on the dashboard were designed using DAX formulas, enabling dynamic and adaptable calculations. Below are the main ones:

   - **Revenue (Total Income):**
     ```dax
     Revenue = SUM('sales transactions'[norm_sales_amount])
     ```
     Muestra el ingreso total generado por las ventas.

 - **Sales Qty (Sales Quantity):**
     ```dax
     Sales Qty = SUM('sales transactions'[sales_qty])
     ```
     Indicates the total number of units sold.


2. **Key Visualizations:**
   - **Revenue by Markets:** Breakdown of revenue by each market.
   - **Sales Qty by Markets:** Breakdown of sales quantity by each market.
   - **Top 5 Customers and Top 5 Products:** Identification of the most important customers and products to prioritize business strategies.

## Dashboard Screenshots
The Power BI dashboard includes interactive visualizations that highlight the main KPIs and sales trends between 2017 and 2020. Below are some representative screenshots of:

**General KPIs, Market Analysis, and Top 5 Customers and Products**

  ![General KPIs Screenshot](https://github.com/sjm-Dev/Sales-Insights/blob/main/Power%20BI%20images/2017%20to%202020%20sales.png)

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
