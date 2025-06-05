# IKEA Inventory- Forecasting & Risk-Detection - Excel Project
Last year’s stockouts cost IKEA $1.2M in lost sales during Black Friday. This project forecasts product demand 4 weeks in advance and flags which products are at risk of going out of stock - so restock can happen in time.


## Tools & Features Used

| Feature                    | Description |
|---------------------------|-------------|
| **Power Pivot**           | Created relationships between Forecast, Inventory, Date & Surge Control tables |
| **DAX Measures**          | Built logic to forecast demand and detect risk |
| **GETPIVOTDATA**          | Pulled forecasted quantities into visible dashboards |
| **Power Query**           | Built a custom date table with Year, Month, and Week-of-Year |
| **Conditional Formatting**| Visually highlighted "Reorder Now" items |
| **Data Validation**       | Used for Surge Control factor inputs |


## Workflow Steps

1. **Data Cleaning & Transformation**
   - Cleaned all tables (Forecast, Inventory, Surge Control)
   - Built a custom date table with:
     ```m
     = Table.AddColumn(#"Renamed Column to Date", "Year", each Date.Year([Date]), Int64.Type)
     = Table.AddColumn(#"Inserted Year", "Month", each Date.Month([Date]), Int64.Type)
     = Table.AddColumn(#"Inserted Month", "Week of Year", each Date.WeekOfYear([Date]), Int64.Type)
     ```

2. **Power Pivot Model**
   - Linked tables using Power Pivot
   - Created key measures like:

     ```DAX
     Forecast 4weeks :=
     CALCULATE(SUM(Forecast[Forecast Quanity]), DATESINPERIOD('Date'[Date], TODAY(), 28, DAY))

     Surge Adjusted Forecast :=
     [Forecast 4 weeks] * IF(HASONEVALUE(Surge_Control[Surge Factor]), VALUES(Surge_Control[Surge Factor]), 1)

     Risk_Flag :=
     IF([Surge Adjusted Forecast] > SUM(Inventory[Current Stock]), "AT RISK", "OK")
     ```

3. **Dashboard Summary using GETPIVOTDATA**
   - Example:
     ```excel
     =GETPIVOTDATA("[Measures].[Forecast 4 weeks]", $A$3, "[ProductMaster].[Product Name]", "[ProductMaster].[Product Name].&[ANTILOP High Chair]")
     ```

4. **Visualization & Alerts**
   - Conditional formatting to highlight “Reorder Now” items
   - Data validation for surge sensitivity control


## Output Sample

![Risk Flag Table Output](assets/risk-flag-shot.png)

## Key Insights

- Items like **ANTILOP High Chair**, **BILLY Bookcase**, and **FÄRGRIK Dinnerware Set** were forecasted to be out of stock within 4 weeks.
- Real-time risk flags help the inventory team prioritise restocking decisions.
- Excel alone is powerful enough to simulate real-world analytics pipelines.

## File Included

- [IKEA_Inventory_Forecast.xlsx](IKEA_Inventory_Forecast.xlsx)

- `assets/`: Screenshots 





