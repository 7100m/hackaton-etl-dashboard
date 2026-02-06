
# HACKATÓN DATA I — ETL + Dashboard Comercial (Databricks)

## Resumen
Proyecto de integración y transformación de datos (ETL) en **Databricks Community Edition** para convertir datos transaccionales de e‑commerce en un dataset analítico 
y un dashboard gerencial (ventas, clientes, devoluciones y análisis por país/producto).  
Se enriquece el dataset **UCI Online Retail** (precios en GBP) con tipo de cambio **GBP→USD** usando la API **Frankfurter**, integrando tasas diarias por fecha de factura 
para estandarizar el análisis en USD.
La solución sigue el enfoque **Medallion** (Bronze/Silver/Gold) para trazabilidad, limpieza progresiva y consumo optimizado en BI. 

## Objetivo
Implementar una ETL completa y entregar un dashboard comercial con insights accionables para soporte gerencial. 

## Fuentes de datos
- **UCI — Online Retail** (dataset transaccional: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country).
- **Frankfurter — Currency Data API** (tipos de cambio, endpoints latest/histórico/series). 

## Arquitectura (Medallion)
La arquitectura organiza el pipeline en tres capas: **Bronze** (crudo), **Silver** (limpio/enriquecido) y **Gold** (agregados de negocio listos para BI). 

### Tablas del proyecto
**Bronze**
- `bronze_online_retail`
- `bronze_fx_gbp_usd` 

**Silver**
- `silver_online_retail` (limpieza + reglas de negocio, incluye tratamiento de cancelaciones “C” en InvoiceNo)
- `silver_fx_gbp_usd` (tipos de cambio normalizados por fecha)
- `silver_online_retail_usd` (transacciones convertidas a USD por fecha) 

**Gold**
- `gold_kpi_daily` (KPIs por día)
- `gold_kpi_country` (KPIs por país)
- `gold_kpi_daily_country` (KPIs por día y país)
- `gold_kpi_product_top` (ranking/Top productos)

## KPIs y métricas (Dashboard)
- Ventas brutas (USD), Devoluciones (USD), Ventas netas (USD). 
- Órdenes totales, Clientes activos. 
- Tasa de devoluciones (%). 

## Dashboard
Dashboard gerencial con dos pestañas:
- **Ejecutivo**: KPIs principales + tendencia de ventas netas.
- **Diagnóstico**: análisis por país (Top por ventas netas, devoluciones en el tiempo y Top por tasa de devoluciones).

## Ejecución (cómo correrlo)
1. Cargar el dataset UCI y crear la tabla `bronze_online_retail`. 
2. Consumir Frankfurter API para obtener tasas GBP→USD por fecha y guardar en `bronze_fx_gbp_usd`. 
3. Generar tablas Silver:
   - `silver_online_retail` (limpieza, tipificación, reglas de cancelación).
   - `silver_fx_gbp_usd` (normalización de tasas).
   - `silver_online_retail_usd` (join por fecha + conversión a USD). 
4. Generar tablas Gold: `gold_kpi_daily`, `gold_kpi_country`, `gold_kpi_daily_country`, `gold_kpi_product_top`. 
5. Construir el dashboard consumiendo únicamente tablas Gold para mejor rendimiento y consistencia. 

## Autor
Marilyn Diana Ulloa Marcial — Genius Lab II — Febrero 2026. 

## Link Dashboard
https://dbc-692b6911-3563.cloud.databricks.com/dashboardsv3/01f10319bec3133f88909c013c181ee0/published?o=7474652008712159

> Uso académico (Hackatón). Datos de fuentes públicas. Sin información personal sensible. 
