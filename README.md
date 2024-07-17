# Data-Analysis-Project-AdvantureWorks

## Tools used:
**SQL Server** and **Power BI**.

## Dataset:
**AdvantureWorks** dataset published by [Microsoft](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms).
**Power BI** [Dashboard](https://app.powerbi.com/view?r=eyJrIjoiM2YyNzVmZjYtYTExYi00MThmLWEwMzYtY2M5NTg0OWVmMjE0IiwidCI6ImZkZGIwMWFkLTQ5ODMtNDM2ZS1hYjM1LTFhZjA0M2I4MThjOSIsImMiOjN9).

## Data Cleaning using SQL

**1-Dim_Culender**
```SQL
SELECT
  [DateKey],
  [FullDateAlternateKey] as Date,
  [DayNumberOfWeek] as DayNoOfWeek,
  LEFT([EnglishDayNameOfWeek], 3) as Day,
  [DayNumberOfMonth] as DayNo,
  LEFT([EnglishMonthName], 3) as Month,
  [MonthNumberOfYear] as MonthNo,
  [CalendarQuarter] as Quarter,
  [CalendarYear] As Year
FROM
  [AdventureWorksDW2022]. [dbo]. [DimDate]
WHERE
  CalendarYear >= 2020 and CalendarYear <= 2021; -- filter data to get only 2020 and 2021 records
```

![image](https://github.com/user-attachments/assets/7b2cb1a0-dacf-4dd3-9b21-a09895d68241)

**2-Dim_Customer**
```SQL
SELECT
  c.[CustomerKey],
  c.[FirstName],
  c.[LastName],
  c.[FirstName] + ' ' + C.[LastName] as FullName,
  c.[BirthDate],
  c.[Gender],
  c.[DateFirstPurchase],
  g.[City] as CustomerCity -- Joined from Geography table
FROM
  DimCustomer as c LEFT JOIN DimGeography as g
  ON c.GeographyKey = g.GeographyKey;
```
![image](https://github.com/user-attachments/assets/539d9d2d-8adb-4b31-8f16-3839e2dfeb84)

**3-DIM_Product**
```SQL
SELECT p. [ProductKey]
  ,p. [ProductAlternateKey] as ProductItemCode
  ,p.[EnglishProductName] as ProductName
  ,ps. EnglishProductSubcategoryName as SubCategory -- Joined from sub category table
  ,pc. EnglishProductCategoryName as Category -- Joined from category table
  ,p.[Color]
  ,p.[Size]
  ,p.[ProductLine]
  ,p. [ModelName]
  ,p.[EnglishDescription] as ProductDescription
  ,ISNULL (p. [Status], 'Outdated') as ProductStatus
FROM
  DimProduct as p LEFT JOIN DimProductSubcategory as ps
    ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey
  LEFT JOIN DimProductCategory as pc
    on pc.ProductCategoryKey = ps.ProductCategoryKey
ORDER BY p.ProductKey asc;
```
![image](https://github.com/user-attachments/assets/c2ae2b52-2c51-451e-9861-528ca9f5a054)

**4-FACT_Internet**
```SQL
SELECT
  [ProductKey],
  [OrderDateKey],
  [DueDateKey],
  [ShipDateKey],
  [CustomerKey],
  [SalesOrderNumber],
  [OrderQuantity],
  [UnitPrice],
  [TotalProductCost],
  [SalesAmount]
FROM
  FactInternetSales
WHERE
  LEFT (OrderDateKey, 4) >= 2020 AND LEFT (OrderDateKey, 4) <= 2021 -- To ensure we only get the years included in the budget sheet from extraction.
ORDER BY
  OrderDateKey ASC;
```
![image](https://github.com/user-attachments/assets/3f9752db-03e0-4385-b668-f0b32183aa9f)

## Data Model
![image](https://github.com/user-attachments/assets/8643d0b8-725f-412e-adb4-11236575297d)

## Power BI Dashboard
![image](https://github.com/user-attachments/assets/9a0b321e-5581-4569-982e-dd6ae565f426)
